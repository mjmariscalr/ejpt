# Enumeración *SMTP*

SMTP (Simple Mail Transmission Protocol) es un protocolo de comunicación usado para transmisión de correo electrónico. Por defecto usa el puerto 25 de TCP, pero puede configurarse en el 465 o 587 con cifrado SSL/TLS para garantizar una conexión cifrada extremo a extremo. Durante la fase de enumeración activa podemos ovtener:

- La versión de *SMTP*, que puede indicarnos vulnerabilidades del sistema.
- Usuarios. Si el sistema tiene activo SSH puede darnos acceso.
- Errores de configuración.

## Enumeración *SMTP* con *metasploit*

Abajo podemos ver todos los módulos disponibles, de los cuales los mas importantes y en los que nos vamos a centrar son: smtp_version, 

```bash
msf > search type:auxiliary name:smtp

Matching Modules
================

	#  Name                                             Disclosure Date  Rank    Check  Description
	-  ----                                             ---------------  ----    -----  -----------
	0  auxiliary/server/capture/smtp                    .                normal  No     Authentication Capture: SMTP
	1  auxiliary/client/smtp/emailer                    .                normal  No     Generic Emailer (SMTP)
	2  auxiliary/scanner/smtp/smtp_version              .                normal  No     SMTP Banner Grabber
	3  auxiliary/scanner/smtp/smtp_ntlm_domain          .                normal  No     SMTP NTLM Domain Extraction
	4  auxiliary/scanner/smtp/smtp_relay                .                normal  No     SMTP Open Relay Detection
	5  auxiliary/fuzzers/smtp/smtp_fuzzer               .                normal  No     SMTP Simple Fuzzer
	6  auxiliary/scanner/smtp/smtp_enum                 .                normal  No     SMTP User Enumeration Utility
	7  auxiliary/dos/smtp/sendmail_prescan              2003-09-17       normal  No     Sendmail SMTP Address prescan Memory Corruption
	8  auxiliary/scanner/http/wp_easy_wp_smtp           2020-12-06       normal  No     WordPress Easy WP SMTP Password Reset
	9  auxiliary/admin/http/wp_post_smtp_acct_takeover  2024-01-10       normal  Yes    Wordpress POST SMTP Account Takeover
```

### *smtp_version*

Este módulo simplemente nos dice qué versión de *SMTP* está instalada en el sistema. Podemos ver que no tiene muchas opciones para configurar, por lo que si tenemos claro cual es la IP del objetivo y el puerto en el que está actuando, podemos ejecutar el módulo.

```bash
msf auxiliary(scanner/smtp/smtp_version) > options 

Module options (auxiliary/scanner/smtp/smtp_version):

	Name     Current Setting  Required  Description
	----     ---------------  --------  -----------
	RHOSTS                    yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using
	                                   -metasploit.html
	RPORT    25               yes       The target port (TCP)
	THREADS  1                yes       The number of concurrent threads (max one per host)
```

Vemos en el ejemplo de abajo que podemos obtener tanto la versión de *SMTP*, como el dominio usado para el correo.

```bash
msf auxiliary(scanner/smtp/smtp_version) > run

[+] 192.82.13.3:25        - 192.82.13.3:25 SMTP 220 openmailbox.xyz ESMTP Postfix: Welcome to our mail server.\x0d\x0a
[*] 192.82.13.3:25        - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

### *smtp_enum*

Este módulo se encarga de enumerar los usuarios del sistema usando para ello una wordlist. Por defecto usa la lista unix_users.txt, pero podemos cambiarla en caso de que el objetivo sea windows o de necesitar una lista más completa o que se ajuste mejor a la situación.

```bash
msf auxiliary(scanner/smtp/smtp_enum) > options

Module options (auxiliary/scanner/smtp/smtp_enum):

	Name       Current Setting                      Required  Description
	----       ---------------                      --------  -----------
	RHOSTS                                          yes       The target host(s), see https://docs.metasploit.com/docs/using-m
	                                                         etasploit/basics/using-metasploit.html
	RPORT      25                                   yes       The target port (TCP)
	THREADS    1                                    yes       The number of concurrent threads (max one per host)
	UNIXONLY   true                                 yes       Skip Microsoft bannered servers when testing unix users
	USER_FILE  /opt/metasploit/data/wordlists/unix  yes       The file that contains a list of probable users accounts.
	          _users.txt

msf auxiliary(scanner/smtp/smtp_enum) > run

[*] 192.82.13.3:25        - 192.82.13.3:25 Banner: 220 openmailbox.xyz ESMTP Postfix: Welcome to our mail server.
[+] 192.82.13.3:25        - 192.82.13.3:25 Users found: , _apt, admin, administrator, backup, bin, daemon, games, gnats, irc, list, lp, mail, man, news, nobody, postfix, postmaster, proxy, sync, sys, uucp, www-data
[*] 192.82.13.3:25        - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed

```

## Enumeración *SMTP* con *nmap*

Los scripts específicos para *SMTP* disponibles actualmente en *nmap* son los que vemos más abajo, aunque hay otros como *banner* o *ssl-cert*, que sin ser orientados a este servicio, pueden ser bastante útiles.

```bash
ls /usr/share/nmap/scripts | grep smtp  
smtp-brute.nse
smtp-commands.nse
smtp-enum-users.nse
smtp-ntlm-info.nse
smtp-open-relay.nse
smtp-strangeport.nse
smtp-vuln-cve2010-4344.nse
smtp-vuln-cve2011-1720.nse
smtp-vuln-cve2011-1764.nse
```

### *smtp-enum-users*

Permite descubrir usuarios válidos en el servidor *SMTP* usando comandos como VRFY, EXPN o RCPT. Utiliza una lista de nombres para ver cuáles reconoce el servidor que puede personalizarse.

```bash
nmap --script smtp-enum-users --script-args 'userdb=/path/wordlist.txt' <IP>
```

### *smtp-brute*

Realiza ataques de fuerza bruta para encontrar credenciales válidas.

```bash
nmap --script smtp-brute --script-args userdb=users.txt,passdb=pass.txt <IP>
```

### *smtp-open-relay*

Comprueba si el servidor permite enviar correo sin autenticación (open relay).

```bash
nmap --script smtp-open-relay <IP>
```

### *smtp-commands*

Muestra los comandos soportados por el servidor (EHLO).

```bash
nmap --script smtp-commands <IP>
```

### *banner*

No es específico de SMTP, sino un script genérico de enumeración que intenta leer el banner que devuelve el servicio al conectarse.

```bash
nmap --script banner <IP>
```

### *ssl-cert*

Muy útil si SMTP usa TLS (puertos 465 o 587). Muestra: CN del certificado, dominio, organización y fechas de validez. Además, puede revelar subdominios o nombres internos.

```bash
nmap -p465,587 --script ssl-cert <IP>
```

## Otros comandos

### Comando nc

Es una herramienta de línea de comandos para TCP/IP que lee/escribe datos, escanea puertos, transfiere archivos y crea conexiones remotas. Es ideal para depuración de red y administración. Con el comando de abajo iniciamos una conexión en el puerto indicado del dominio o IP. Ésto nos permite comprobar el banner, que nos puede dar información como versión del servicio o el nombre de dominio para el correo electrónico.

```bash
nc ejemplo.local 25
```

### Comando VRFY

Es una instrucción del protocolo SMTP utilizada para verificar si una dirección de correo electrónico específica existe en un servidor de correo. Para usarlo es necesario abrir antes una conexión *SMTP*, por ejemplo con nc o con telnet.

```bash
VRFY admin@openmailbox.xyz
```

### *smtp-user-enum*

Herramienta para enumeración de usuarios *SMTP*, usada principalmente contra el servicio de Solaris. Puede usar *EXPN*[^1], *VRFY* y *RCPT*[^2].

```bash
smtp-user-enum -U /path/wordlist.txt -t ejemplo.local
```

---
## Notas

[^1]: *EXPN*: Se usa para expandir una lista de correo (alias o grupo) y ver qué usuarios contiene.
[^2]: *RCPT*: Se usa normalmente al enviar correos, pero también sirve para comprobar si un destinatario existe.