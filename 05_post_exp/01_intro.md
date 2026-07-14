# 1. Introducción a la post-explotación

La post-explotación es la fase final del proceso de pruebas de penetración y consiste en las tácticas, técnicas y procedimientos que los atacantes/adversarios llevan a cabo después de obtener acceso inicial a un sistema objetivo. En otras palabras, implica lo que haces o debes hacer una vez que consigues un punto de apoyo inicial en el sistema objetivo. Variará en función del sistema operativo objetivo, así como de su infraestructura.

Las técnicas y herramientas dependerán del tipo de acceso al sistema comprometido, así como del nivel de sigilo que a mantener. Esto significa, que será necesario utilizar diferentes técnicas y herramientas según el sistema operativo objetivo y su configuración. Además, las técnicas de post-explotación deberán cumplir con las reglas de compromiso acordadas con el cliente.

> Nota: Es necesario asegurarse de contar con los permisos y privilegios necesarios para modificar servicios, configuraciones del sistema, realizar escalada de privilegios, eliminar registros (logs), etc.

## 1.1. Metodología

Para llevar a cabo una fase de post-explotación completa y exhaustiva, es necesario utilizar una metodología estructurada que abarque las etapas más importantes de la post-explotación y que pueda aplicarse durante los distintos compromisos o evaluaciones de seguridad.

Este enfoque estructurado y metodológico garantiza que no se omitan ni se pasen por alto fases importantes de la post-explotación, además de proporcionarnos objetivos claros y medibles para cada etapa del proceso.

1. Enumeración local.
	- Sistemas de información.
	- Usuarios y grupos.
	- Información de red.
	- Servicios.
	- Automatización de la enumeración local.
2. Transferencia de archivos.
	- Crear un servidor web con python.
	- Transferencia de archivos a un objetivo Windows.
	- Transferencia de archivos a un objetivo Linux.
3. Mejora de shells.
	- Mejora a meterpreter.
	- Generación de shells TTY.
4. Escalada de privilegios.
5. Persistencia.
6. Extracción y descifrado de hashes.
7. Pivoting.
	- Reconocimiento de la red interna.
	- Pivoting.
8. Limpiar el rastro.
