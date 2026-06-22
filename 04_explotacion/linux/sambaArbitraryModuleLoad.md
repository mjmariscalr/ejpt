# Samba Arbitrary Module Load (CVE-2017-7494)

La vulnerabilidad **CVE-2017-7494** afecta a diversas versiones de Samba El problema se origina en la forma en que Samba gestionaba determinadas solicitudes relacionadas con named pipes. Un usuario autenticado que dispusiera de permisos de escritura sobre un recurso compartido podía subir una biblioteca compartida maliciosa (.so) a una ubicación accesible por el servidor y posteriormente provocar que Samba la cargara. Como consecuencia, el código contenido en dicha biblioteca se ejecutaba con los privilegios del proceso de Samba.

La vulnerabilidad fue catalogada como una vulnerabilidad de ejecución remota de código (Remote Code Execution, RCE), ya que permitía a un atacante ejecutar código arbitrario en el sistema afectado sin necesidad de acceso interactivo previo al servidor. Aunque normalmente era necesario disponer de credenciales válidas y permisos de escritura en algún recurso compartido, en muchas configuraciones reales estos requisitos podían cumplirse con relativa facilidad.

El impacto potencial incluía:

- Ejecución remota de comandos en el servidor.
- Compromiso completo del sistema afectado.
- Instalación de malware o puertas traseras.
- Robo o modificación de información almacenada en recursos compartidos.
- Movimiento lateral dentro de una red corporativa.

### Explotación con MSF

[⟵ Anterior](../05_sistema.md#explotación-windows)
