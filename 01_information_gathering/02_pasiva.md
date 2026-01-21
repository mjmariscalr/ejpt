# 2. Recolección de información pasiva

Consiste en obtener la información sin interactuar con el objetivo, normalmente haciendo uso de recursos accesibles de forma pública. Algunas de las principales fuentes de información son:

- Motores de búsqueda para buscar información sobre empresas, personas o tecnologías.
- Analizar perfiles en redes sociales.
- Bases de datos públicas de distintos propósitos: *[Whois](https://who.is), [Shodan](https://www.shodan.io/), etc.*
- Archivos públicos: robots.txt, sitemap.xml.
- Metadatos en documentos públicos.

La información más importante que podemos encontrar durante un reconocimiento pasivo es:

- Direcciones IP e información DNS.
- Nombres de dominio y su propiedad.
- Direcciones de correo y perfiles en redes sociales.
- Tecnologías web.
- Subdominios.

## 2.1. Herramientas

A continuación se da una lista de herramientas que se pueden usar durante la fase de reconocimiento pasivo distribuidas según su finalidad.

### Reconocimiento web

En esta sección se explora el proceso de reconocimiento pasivo y footprinting sobre una web. El footprinting es esencialmente lo mismo que el reconocimiento pasivo con la diferencia de que se identifica información más importante del objetivo. En esta fase la información que podemos encontrar es:

- Direcciones IP
- Directorios ocultos a motores de búsqueda
- Nombres
- Direcciones de correo
- Números de teléfono
- Direcciones físicas
- Tecnologías que se están usando en la web.

Algunas herramientas importantes son: 

- [BuiltWith](web_recon/BuiltWith.md)
- [Wappalyzer](web_recon/Wappalyzer.md)
- [WhatWeb](web_recon/WhatWeb.md)
- [robots.txt](web_recon/robots.md)
- [sitemap.xml](web_recon/sitemap.md)
- [Netcraft](web_recon/Netcraft.md)

### Enumeración Whois

Proceso de usar el protocolo Whois para recopilar información sobre un dominio web o una dirección IP. Whois es un protocolo de Internet utilizado para consultar bases de datos y obtener información sobre quién ha registrado un nombre de dominio (o dirección IP). Los datos de Whois constituyen una colección de información sobre el nombre de dominio registrado, sus servidores de nombre y registrador, la fecha de creación del nombre de dominio, la fecha de vencimiento del nombre de dominio, la información de contacto del titular del nombre registrado, el contacto técnico y el contacto administrativo, entre otros.

- [WhoIs](whois/whois.md)

### Reconocimiento DNS

Dado que aun nos encontramos en la fase de reconocimiento pasivo, no se va a interactuar directamente con el servidor DNS. Teniendo esto en cuenta, la principal información a obtener en esta fase es sobre registros DNS de un dominio particular. Los principales registros DNS son:

- ***A***: contiene la dirección IPv4 del dominio.
- ***AAAA***: contiene la dirección IPv6.
- ***CNAME***: redirige el subdominio al dominio principal. Este registro puede revelar subdominios o infraestructura externa.
- ***MX***: muestra los servidores de correo para el dominio.
- ***NS***: servidores autoritativos de la zona, que incluyen la información oficial. Una consulta a estos servidores se puede considerar reconocimiento activo.
- ***TXT***: registros de texto que a veces contienen información sensible.
- ***SOA***: almacena información importante sobre un dominio o una zona, como la dirección de correo electrónico del administrador, cuándo se actualizó el dominio por última vez y cuánto tiempo debe esperar el servidor entre actualizaciones.
- ***PTR***: actúa de forma inversa al registro *A*, es decir, almacena una dirección IP y contiene información sobre el dominio/nombre de host para esa IP.

A continuación se muestra una lista con algunas herramientas interesantes.

- [host](dns_recon/host.md)
- [nslookup](dns_recon/nslookup.md)
- [dig](dns_recon/dig.md)
- [dnsrecon](dns_recon/dnsrecon.md)
- [DNSDumpster](dns_recon/dnsdumpster.md)

### WAF

Un WAF es un cortafuegos de aplicaciones web. Es un sistema que protege aplicaciones web filtrando, monitoreando y bloqueando tráfico HTTP malicioso. Analiza las solicitudes que llegan a un servidor web para detectar y bloquear ataques como:

- SQL Injection (SQLi)
- Cross-Site Scripting (XSS)
- Cross-Site Request Forgery (CSRF)
- Path Traversal
- File Inclusion
- Ataques automatizados (bots, bruteforce)
- Exploits de vulnerabilidades conocidas

Es necesario identificar si existe un WAF y que reglas utiliza porque puede:

- Detectar o bloquear payloads comunes.
- Introducir falsos positivos que afectan los resultados.
- Modificar tus pruebas.
- Generar logs que pueden alertar.

Una herramienta interesante para esta tarea es [WAFW00F](waf/wafw00f.md).

### Enumeración de subdominios

La enumeración de subdominios es el proceso de descubrir subdominios asociados a un dominio principal (por ejemplo: api.ejemplo.com, mail.ejemplo.com, admin.ejemplo.com). Es una etapa clave en reconocimiento durante un pentest.

- [Sublist3r](subdomain/sublist3r.md)

### Google dorks

- [Google dorks](google_dorks/google_dorks.md)

### Recolección de correo electrónico

En este caso vamos a ver dos herramientas que se pueden usar de forma combinada. La primera la usaremos para obtener direcciones de correo electrónico, aunque tambien puede mostrar subdominios, direcciones IP y más datos que pueden ser interesantes durante un pentest. La segunda es una base de datos que nos muestra si una dirección de correo se ha filtrado junto con su contraseña en una base de datos.

- [theHarvester](email_harvesting/theharvester.md)
- [Have i been pwned?](email_harvesting/pwned.md)

[⟵ Anterior](01_introduccion.md) | [Siguiente ⟶](03_activa.md)

