# Microsoft IIS WebDAV

IIS (Internet Information Services) es un software propietario y extensible de servidor web desarrollado por Microsoft para su uso con la familia Windows NT. Puede utilizarse para alojar sitios web y aplicaciones web, y proporciona a los administradores una interfaz gráfica (GUI) robusta para la gestión de sitios web. Puede alojar tanto páginas web estáticas como dinámicas desarrolladas en ASP, .NET y PHP. Normalmente se configura para ejecutarse en los puertos 80 y 443.

Extensiones de archivos ejecutables compatibles:

- .asp
- .aspx
- .config
- .php

## WebDAV

WebDAV (Web-based Distributed Authoring and Versioning) es un conjunto de extensiones del protocolo HTTP que permite a los usuarios editar y gestionar archivos de forma colaborativa en servidores web remotos. Gracias a estas extensiones, un servidor web puede funcionar también como un servidor de archivos, facilitando la creación, modificación y administración de contenido de manera remota por varios usuarios.

WebDAV suele ejecutarse sobre Microsoft IIS (Internet Information Services) utilizando los puertos 80 (HTTP) y 443 (HTTPS). Para conectarse a un servidor WebDAV es necesario disponer de credenciales válidas, ya que este servicio implementa mecanismos de autenticación basados en nombre de usuario y contraseña para controlar el acceso a los recursos y garantizar que únicamente los usuarios autorizados puedan gestionarlos.

Pasos para la explotación de WebDAV:

- Identificar si WebDAV está configurado para ejecutarse en el servidor web IIS.
- Ataque de fuerza bruta para identificar credenciales legítimas.
- Autenticarnos con el servidor WebDAV y cargar un payload malicioso .asp que puede usarse para ejecutar comandos arbitrarios u obtener una shell inversa en el objetivo.


### DAVTest

Se utiliza para escanear, autenticar y explotar un servidor WebDAV. Está preinstalado en la mayoría de las distribuciones de pruebas de penetración ofensivas como Kali y Parrot OS.



### Cadaver

Cadaver admite la carga y descarga de archivos, la visualización en pantalla, la edición in situ, las operaciones de espacio de nombres (mover/copiar), la creación y eliminación de colecciones, la manipulación de propiedades y el bloqueo de recursos en servidores WebDAV. Preinstalado en la mayoría de las distribuciones de pruebas de penetración ofensivas como Kali y Parrot OS.

[⟵ Anterior](../../05_sistema.md#explotación-windows)
