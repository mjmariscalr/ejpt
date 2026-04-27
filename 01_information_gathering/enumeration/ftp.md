# Enumeración FTP

FTP (File Transfer Protocol) es un protocolo de red estándar utilizado para la transferencia de archivos entre un cliente y un servidor. Por defecto, opera sobre el puerto 21. FTP presenta dos debilidades críticas:

- **Transmisión en texto plano:** Las credenciales y los datos no se cifran, lo que permite ataques de intercepción (sniffing).
- **Configuraciones inseguras:** No es un problema propio del software, sino de configuraciones que permiten el acceso anónimo o que ejecutan versiones de software obsoletas con vulnerabilidades conocidas.

En el caso del protocolo FTP, la enumeración nos permite obtener información sensible como pueden ser: versiones del sistema, estructura de directorios y archivos sensibles, acceso anónimo y lista de usuarios.

## Enumeración con Metasploit Framework

Igual que en la sección sobre escaneo de puertos con *Metasploit*, para buscar los modulos relacionados con la enumeración FTP, usamos el comando search. En este caso el número de resultados es bastante elevado y gran parte de ellos son exploits. Para limitar los resultados, podemos filtrar de la siguiente forma:

```bash
msf > search type:auxiliary name:ftp

Matching Modules
================

   #   Name                                                          Disclosure Date  Rank    Check  Description
   -   ----                                                          ---------------  ----    -----  -----------
   0   auxiliary/scanner/ftp/anonymous                               .                normal  No     Anonymous FTP Access Detection
   1   auxiliary/server/capture/ftp                                  .                normal  No     Authentication Capture: FTP
   2   auxiliary/scanner/ftp/bison_ftp_traversal                     2015-09-28       normal  Yes    BisonWare BisonFTP Server 3.5 Directory Traversal Information Disclosure
   3   auxiliary/scanner/ssh/cerberus_sftp_enumusers                 2014-05-27       normal  No     Cerberus FTP Server SFTP Username Enumeration
   4   auxiliary/scanner/snmp/cisco_config_tftp                      .                normal  No     Cisco IOS SNMP Configuration Grabber (TFTP)
   5   auxiliary/scanner/snmp/cisco_upload_file                      .                normal  No     Cisco IOS SNMP File Upload (TFTP)
   .
   .
   .
```

Algunos módulos relevantes para la enumeración FTP son:

### ***ftp_version***

Permite descubrir la versión FTP para posteriormente buscar vulnerabilidades.

Ejemplo:

Seleccionamos el módulo con el comando '*use*':

```bash
msf > use auxiliary/scanner/ftp/ftp_version
```
Podemos ver las opciones de configuración con *show options*.

```bash
msf auxiliary(scanner/ftp/ftp_version) > show options

Module options (auxiliary/scanner/ftp/ftp_version):

   Name     Current Setting      Required  Description
   ----     ---------------      --------  -----------
   FTPPASS  mozilla@example.com  no        The password for the specified username
   FTPUSER  anonymous            no        The username to authenticate as
   RHOSTS                        yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT    21                   yes       The target port (TCP)
   THREADS  1                    yes       The number of concurrent threads (max one per host)


View the full module info with the info, or info -d command.
```

Con *set* establecemos un valor para una variable concreta. Con *setg* hacemos que sea global y pueda usarse en otros módulos. Esto es util si ejecutamos distintos módulos auxiliares en un mismo objetivo.

```bash
msf auxiliary(scanner/ftp/ftp_version) > setg rhosts 192.168.1.1
rhosts => 192.168.1.1
```
Para ejecutar el modulo usamos el comando *run*

```bash
msf auxiliary(scanner/ftp/ftp_version) > run
[+] 192.168.1.1:21        - FTP Banner: '220 (vsFTPd 2.3.4)'
[*] 192.168.1.1:21        - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

### ***ftp_login***

Prueba múltiples combinaciones de nombres de usuario y contraseñas[^1] contra el servicio, permitiendo identificar cuentas con credenciales débiles.

```bash
msf auxiliary(scanner/ftp/ftp_login) > show options

Module options (auxiliary/scanner/ftp/ftp_login):

   Name              Current Setting  Required  Description
   ----              ---------------  --------  -----------
   ANONYMOUS_LOGIN   false            yes       Attempt to login with a blank username and password
   BLANK_PASSWORDS   false            no        Try blank passwords for all users
   BRUTEFORCE_SPEED  5                yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS      false            no        Try each user/password couple stored in the current database
   DB_ALL_PASS       false            no        Add all passwords in the current database to the list
   DB_ALL_USERS      false            no        Add all users in the current database to the list
   DB_SKIP_EXISTING  none             no        Skip existing credentials stored in the current database (Accepted: none, use
                                                r, user&realm)
   PASSWORD                           no        A specific password to authenticate with
   PASS_FILE                          no        File containing passwords, one per line
   Proxies                            no        A proxy chain of format type:host:port[,type:host:port][...]. Supported proxi
                                                es: socks5h, sapni, socks4, http, socks5
   RECORD_GUEST      false            no        Record anonymous/guest logins to the database
   RHOSTS                             yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/bas
                                                ics/using-metasploit.html
   RPORT             21               yes       The target port (TCP)
   STOP_ON_SUCCESS   false            yes       Stop guessing when a credential works for a host
   THREADS           1                yes       The number of concurrent threads (max one per host)
   USERNAME                           no        A specific username to authenticate as
   USERPASS_FILE                      no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false            no        Try the username as the password for all users
   USER_FILE                          no        File containing usernames, one per line
   VERBOSE           true             yes       Whether to print output for all attempts
```

Para este módulo necesitamos especificar un usuario y contraseña o al menos una lista[^2].

```bash
msf auxiliary(scanner/ftp/ftp_login) > set user_file /path/to/wordlist.txt
msf auxiliary(scanner/ftp/ftp_login) > set pass_file /path/to/wordlist.txt
msf auxiliary(scanner/ftp/ftp_login) > run
```

### ***ftp_anonymous***

Verifica si el servidor permite el acceso con el usuario anonymous o ftp sin necesidad de una contraseña real (o usando un formato de correo electrónico genérico como contraseña). No es necesario establecer ninguna configuración más allá de la IP y el puerto FTP del objetivo.

## Enumeración con NSE

La enumeración FTP con NSE permite identificar vectores de ataque reales de forma estructurada y eficiente. La enumeración con NSE puede combinar detección de versión (-sV), scripts por defecto (-sC) y scripts específicos (--script ftp-\*).

### ftp-anon

Comprueba si el acceso anónimo está habilitado y lista archivos accesibles.

```bash
nmap -p 21 --script ftp-anon <IP>
```

### ftp-brute

Ataque de fuerza bruta contra credenciales FTP.

```bash
nmap -p 21 --script ftp-brute <IP>
```

Con diccionario personalizado:

```bash
nmap -p 21 --script ftp-brute --script-args userdb=users.txt,passdb=pass.txt <IP>
```

### ftp-syst

Obtiene información del sistema remoto (tipo de SO).

```bash
nmap -p 21 --script ftp-syst <IP>
```

### ftp-vsftpd-backdoor

Detecta la vulnerabilidad backdoor en versiones antiguas de vsftpd (2.3.4).

```bash
nmap -p 21 --script ftp-vsftpd-backdoor <IP>
```

### ftp-proftpd-backdoor

Detecta backdoor en versiones vulnerables de ProFTPD (caso famoso 1.3.3c).

```bash
nmap -p 21 --script ftp-proftpd-backdoor <IP>
```

[⟵ Anterior](../03_activa.md#323-enummeración-de-servicios)

---
## Notas

[^1]: Hay que tener en cuenta que en general, este tipo de herramientas pertenecen a la fase de explotación, ya que buscan romper la autenticación y obtener credenciales. Se consideran enumeración cuando el objetivo es la enumeración de usuarios sin intentar obtener acceso al sistema.
[^2]: Podemos encontrar las listas predefinidas de *metasploit* en: /usr/share/metasploit-framework/data/wordlists
