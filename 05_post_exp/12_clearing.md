# Limpieza del rastro

Las fases de **explotación y post-explotación** de una prueba de penetración implican interactuar activamente con los sistemas objetivo y con los datos almacenados en dichos sistemas. Como resultado, es posible que tengas que **eliminar o revertir cualquier cambio** que hayas realizado en los sistemas objetivo que hayas comprometido, siguiendo las directrices especificadas en las **reglas de compromiso**.

## Windows

Si has transferido algún archivo a los sistemas objetivo que hayas comprometido, **lleva un registro de dónde se han guardado** para poder eliminarlos cuando hayas terminado. Una buena práctica es almacenar todos tus **scripts, exploits y binarios** en el directorio `C:/Temp` en Windows y en el directorio `/tmp` en Linux.

También es importante tener en cuenta el framework de explotación que estés utilizando. Un ejemplo de ello es **MSF**, que genera y almacena artefactos en el sistema objetivo al utilizar exploits o en post-explotación. Algunos módulos bien diseñados proporcionan instrucciones y scripts de recursos que ofrecen información sobre dónde se almacenan los artefactos y cómo eliminarse.

Una técnica típica consiste en eliminar el registro de eventos de Windows (Windows Event Log), pero debe evitarse durante una prueba de penetración, ya que almacena una gran cantidad de información importante para el cliente.
