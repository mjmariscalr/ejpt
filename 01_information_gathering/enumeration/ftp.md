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

```bash
msf > use auxiliary/scanner/ftp/ftp_version
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

msf auxiliary(scanner/ftp/ftp_version) > set rhosts 192.168.1.1
rhosts => 192.168.1.1
msf auxiliary(scanner/ftp/ftp_version) > run
[+] 192.168.1.1:21        - FTP Banner: '220 (vsFTPd 2.3.4)'
[*] 192.168.1.1:21        - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

### ***ftp_login***

Prueba múltiples combinaciones de nombres de usuario y contraseñas[^1] contra el servicio, permitiendo identificar cuentas con credenciales débiles. Para este módulo necesitamos especificar un usuario y contraseña o al menos una lista[^2].


### ***ftp_anonymous***

Verifica si el servidor permite el acceso con el usuario anonymous o ftp sin necesidad de una contraseña real (o usando un formato de correo electrónico genérico como contraseña).

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

[⟵ Anterior](../03_activa.md#322-msf-módulos-auxiliares)

---
## Notas

[^1]: Hay que tener en cuenta que en general, este tipo de herramientas pertenecen a la fase de explotación, ya que buscan romper la autenticación y obtener credenciales. Se consideran enumeración cuando el objetivo es la enumeración de usuarios sin intentar obtener acceso al sistema.
[^2]: Podemos encontrar las listas predefinidas de *metasploit* en: /usr/share/metasploit-framework/data/wordlists
