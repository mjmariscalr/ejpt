# Enumeración web

Un servidor web es un software que sirve como almacenamiento de los datos de un sitio web. Algunos ejemplos de servidores web son: *Apache, Nginx y Microsoft IIS*. Utiliza *HTTP (Hypertext Transfer Protocol)* para establecer la conexión entre cliente y servidor, por defecto en el puerto 80, aunque como todos los servicios, puede configurarse en cualquier otro puerto.

Cuando escribimos un nombre de dominio en el navegador, este envía una petición *DNS* para obtener la IP. Una vez que la tiene, envía una petición *HTTP* al servidor web alojado en esa IP, que devolverá una respuesta *HTTP* que normalmente incluirá el sitio web e información asociada.

Podemos enumerar información como: versión del servidor web, cabeceras HTTP, directorios, etc.

## *Metasploit*

Una vez configurados los parámetros básicos del objetivo, se procede al uso de los módulos orientados a la recopilación de información, que permiten interactuar directamente con el servicio web y obtener datos relevantes para las siguientes fases del pentesting.

### http_version

Obtiene la versión del protocolo HTTP que soporta el servidor web objetivo mediante el envío de una petición y el análisis de la respuesta. Este módulo permite identificar si el servidor utiliza, por ejemplo, HTTP/1.0, HTTP/1.1 o versiones más recientes.

En caso de estar enumerando HTTPS, será necesario cambiar los valores de *RPORT* Y *SSL* a 443 y true respectivamente.

```bash
msf > use auxiliary/scanner/http/http_version
msf auxiliary(scanner/http/http_version) > run

[+] 192.168.1.10:80 - HTTP version detected: HTTP/1.1
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

### http_header

Obtiene y muestra las cabeceras de la respuesta *HTTP* del servidor objetivo tras realizar una petición. Esto permite identificar información relevante como tecnologías utilizadas, políticas de seguridad (por ejemplo, cabeceras de protección) o configuraciones del servidor.

Para ejecutar este módulo, es necesario configurar *SSL* en true si el servicio utiliza *HTTPS*. Opcionalmente, se puede ajustar *TARGETURI* para especificar una ruta concreta del servidor (por defecto /), y *HTTP_METHOD* para definir el tipo de petición *HTTP* que se enviará (por ejemplo, *GET* o *HEAD*).

```bash
msf > use auxiliary/scanner/http/http_header
msf auxiliary(scanner/http/http_header) > run

[+] 192.168.1.10:80        : HTTP/1.1 200 OK
[+] 192.168.1.10:80        : Server: Apache/2.4.41 (Ubuntu)
[+] 192.168.1.10:80        : X-Powered-By: PHP/7.4.3
[+] 192.168.1.10:80        : Content-Type: text/html; charset=UTF-8
```

### robots_txt

Recupera y muestra el contenido del archivo robots.txt de un servidor web objetivo. Su función es identificar rutas declaradas en las directivas Disallow y Allow, destacando posibles directorios o endpoints que el propio sitio intenta ocultar del rastreo automatizado. Permite obtener ciertas rutas que pueden ser sensibles sin necesidad de realizar fuerza bruta.

```bash
msf6 > use auxiliary/scanner/http/robots_txt
msf6 auxiliary(scanner/http/robots_txt) > run

[+] 192.168.1.10:80 - robots.txt found
[+] Contents of robots.txt:
User-agent: *
# Directories
Disallow: /data/
Disallow: /backup/
Disallow: /secure/
```

### dir_scanner

Sirve para identificar directorios y rutas ocultas en aplicaciones web. Su objetivo principal es descubrir recursos que no están enlazados directamente en la interfaz del sitio, pero que pueden ser accesibles si se conoce su ruta exacta. Funciona mediante técnicas de fuerza bruta de directorios, utilizando wordlists.

```bash
msf auxiliary(scanner/http/dir_scanner) > show options

Module options (auxiliary/scanner/http/dir_scanner):

	Name        Current Setting                       Required  Description
	----        ---------------                       --------  -----------
	DICTIONARY  /opt/metasploit/data/wmap/wmap_dirs.  no        Path of word dictionary to use
	           txt
	PATH        /                                     yes       The path  to identify files
	Proxies                                           no        A proxy chain of format type:host:port[,type:host:port][...]. Sup
	                                                           ported proxies: socks4, socks5, socks5h, http, sapni
	RHOSTS                                            yes       The target host(s), see https://docs.metasploit.com/docs/using-me
	                                                           tasploit/basics/using-metasploit.html
	RPORT       80                                    yes       The target port (TCP)
	SSL         false                                 no        Negotiate SSL/TLS for outgoing connections
	THREADS     1                                     yes       The number of concurrent threads (max one per host)
	VHOST                                            no        HTTP server virtual host

msf > run

[*] Starting DirScanner against 192.168.1.10
[+] Found: /admin/ (Status: 200)
[+] Found: /login/ (Status: 200)
[+] Found: /backup/ (Status: 403)
[+] Found: /config/ (Status: 403)
```

### file_dir

Escáner HTTP diseñado para enumerar archivos y directorios en servidores web mediante peticiones automatizadas.

```bash
msf auxiliary(scanner/http/files_dir) > show options

Module options (auxiliary/scanner/http/files_dir):

	Name        Current Setting                       Required  Description
	----        ---------------                       --------  -----------
	DICTIONARY  /opt/metasploit/data/wmap/wmap_files  no        Path of word dictionary to use
	           .txt
	EXT                                               no        Append file extension to use
	PATH        /                                     yes       The path  to identify files
	Proxies                                           no        A proxy chain of format type:host:port[,type:host:port][...]. Sup
	                                                           ported proxies: socks4, socks5, socks5h, http, sapni
	RHOSTS                                            yes       The target host(s), see https://docs.metasploit.com/docs/using-me
	                                                           tasploit/basics/using-metasploit.html
	RPORT       80                                    yes       The target port (TCP)
	SSL         false                                 no        Negotiate SSL/TLS for outgoing connections
	THREADS     1                                     yes       The number of concurrent threads (max one per host)
	VHOST                                             no        HTTP server virtual host

msf > run

[*] Starting files_dir scan against 192.168.1.10
[*] Using HTTP method GET

[+] Found file: /index.php (Status: 200)
[+] Found file: /robots.txt (Status: 200)
[+] Found dir:  /admin/ (Status: 403)
[+] Found dir:  /backup/ (Status: 403)
[+] Found dir:  /private/ (Status: 403)

[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

### http_login

Realiza fuerza bruta sobre un formulario de autenticación. Las principales opciones a configurar son: AUTH_URI con la ruta del formulario y, por otro lado debemos tener en cuenta que por defecto están configurados USER_FILE, PASS_FILE y USERPASS_FILE. Podemos cambiarlos pero no necesitamos las dos opciones (archivos separados vs un unico archivo).

### apache_userdir_enum

permite enumerar usuarios válidos en servidores Apache con la función UserDir habilitada en Apache HTTP Server. Funciona probando rutas del tipo /~usuario y analizando las respuestas del servidor para determinar qué usuarios existen

## *Nmap*

Hay muchos scripts de *nmap* para el protocolo http, por lo que nos vamos a centrar en lo esencial.

### http-methods

Sirve para identificar qué métodos HTTP están permitidos en un servidor web. Envía peticiones al servidor y detecta si acepta métodos como: GET, POST, HEAD, OPTIONS, PUT, DELETE, TRACE o CONNECT

Algunos métodos pueden ser peligrosos si están habilitados sin control:

- PUT o DELETE: podrían permitir subir o borrar archivos en el servidor
- TRACE: puede usarse en ataques como Cross-Site Tracing (XST)
- OPTIONS: revela información sobre la configuración del servidor

```bash
nmap --script http-methods --script-args http-methods.test=all -p80 -T4 -Pn -sV nmap.scanme.org
```

### http-enum



## Otros métodos



### gobuster



### dirbuster



[⟵ Anterior](../03_activa.md#323-enummeración-de-servicios)
