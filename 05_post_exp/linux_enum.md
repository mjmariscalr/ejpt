# 2. Enumeración Linux

La fase de enumeración permite identificar las características del sistema operativo, su configuración y los componentes instalados, información que servirá como base para las siguientes etapas de la post-explotación. Conocer estos aspectos ayuda a comprender mejor el objetivo y permite determinar qué técnicas de escalada de privilegios, persistencia o movimiento lateral pueden ser viables.

## Enumeración del sistema

Después de obtener acceso inicial a un sistema objetivo, siempre es importante recopilar más información sobre él, como qué sistema operativo está ejecutando y cuál es su versión. Esta información es muy útil, ya que nos da una idea de lo que podemos hacer y del tipo de exploits que podrían ser compatibles.

Buscamos:

- Nombre del equipo.
- Distribución del sistema operativo y su versión.
- Versión del kernel y arquitectura.
- Información de la CPU.
- Información de los discos y unidades montadas.
- Paquetes o software instalados.


