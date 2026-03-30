# Enumeración *SSH*

*SSH* (Secure Shell) es un protocolo de administración remota que ofrece encriptación. Por defecto usa el puerto 22, pero como todos los servicios puede configurarse para escuchar en cualquier otro puerto.

*SSH* es una alternativa segura a una conexión remota a una shell mediante *Telnet*, ya que cifra la conexión. Usando *Telnet*, los datos se envían en texto plano, lo que hace este protocolo sensible frente a ataques *MITM (Man In The Middle)*. 

Podemos enumerar con distintas herramientas para obtener la versión *SSH* o realizar fuerza bruta sobre la autenticación.

## Enumeración con *Metasploit*

*SSH* es un servicio dificil de enumerar por su propio diseño, pero hay algunos módulos con los que podemos obtener cierta información.

### *ssh_version*

Con este script obtenemos la versión *ssh* que está instalada en el sistema. Además, podemos llegar a obtener la versión del sistema operativo. No es necesaria una configuración más allá de la IP o el puerto si fuera distinto al 22.

### *ssh_login*

Script para realizar fuerza bruta a los usuarios y credenciales de ssh. Es recomendable enumerar antes los usuarios para agilizar el proceso y tratar de explotar directamente a usuarios que sí existen.

Configurar: 
- BRUTEFORCE_SPEED: reduce el riesgo de detección al reducir la velocidad a la que se envían consultas.
- UNSERNAME/USER_FILE: Usuario específico o lista de usuarios.
- USERPASS_FILE: lista de posibles contraseñas.
- VERBOSE: muestra todos los intentos.

Si ha encontrado credenciales se abre una sesión que se puede ver con el comando *sessions* y se establece la conexion con *sessions n*, siendo n el numero establecido por metasploit para esa sesión.

### *ssh_enumusers*

Similar a *ssh_login*, pero solo enumera usuarios sin extraer credenciales.

```bash
msf auxiliary(scanner/ssh/ssh_enumusers) > show options

Module options (auxiliary/scanner/ssh/ssh_enumusers):

   Name          Current Setting  Required  Description
   ----          ---------------  --------  -----------
	CHECK_FALSE   true             no        Check for false positives (random username)
	DB_ALL_USERS  false            no        Add all users in the current database to the list
	Proxies                        no        A proxy chain of format type:host:port[,type:host:port][...]. Supported proxies:
                                        	socks5h, sapni, socks4, http, socks5
	RHOSTS                         yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/
                                        	using-metasploit.html
	RPORT         22               yes       The target port
	THREADS       1                yes       The number of concurrent threads (max one per host)
	THRESHOLD     10               yes       Amount of seconds needed before a user is considered found (timing attack only)
	USERNAME                       no        Single username to test (username spray)
	USER_FILE                      no        File containing usernames, one per line


Auxiliary action:

	Name              Description
	----              -----------
	Malformed Packet  Use a malformed packet
```

## Enumeración con *nmap*

```bash
ls /usr/share/nmap/scripts | grep ssh
ssh-auth-methods.nse
ssh-brute.nse
ssh-hostkey.nse
ssh-publickey-acceptance.nse
ssh-run.nse
ssh2-enum-algos.nse
sshv1.nse
```

### ssh-hostkey

### 
