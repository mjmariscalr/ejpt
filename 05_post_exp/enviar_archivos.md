# Transferencia de archivos a objetivos Windows y Linux

Después de obtener acceso inicial a un sistema objetivo, será transferir archivos al sistema de destino. En algunos casos, no tendremos acceso mediante una sesión de Meterpreter, por lo que debemos utilizar las utilidades integradas del sistema para facilitar la transferencia.

Este proceso utiliza un enfoque de dos pasos, en el que primero deberás alojar los archivos que deseas transferir en un servidor web y, posteriormente, descargar los archivos alojados en ese servidor web al sistema objetivo.

## Configuración de un servidor web con Python

Python incluye un módulo integrado conocido como SimpleHTTPServer (Python 2) y http.server (Python 3), que puede utilizarse para implementar un servidor HTTP sencillo que proporciona controladores estándar para las solicitudes GET y HEAD.

Este módulo puede utilizarse para alojar archivos en cualquier directorio de tu sistema y puede ponerse en funcionamiento mediante un único comando en la terminal.

**Con python 2**

Crea un servidor web con acceso al directorio donde ejecutamos el comando.

```bash
python -m SimpleHTTPServer 80
```

**Con python 3**

Crea un servidor web con acceso al directorio donde ejecutamos el comando.

```bash
python -m http.server 80
```
