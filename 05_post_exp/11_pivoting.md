# Pivoting

El pivoting es una técnica de post-explotación que consiste en utilizar un host comprometido que está conectado a múltiples redes para obtener acceso a sistemas dentro de otras redes. Después de obtener acceso a un host, podemos utilizar el host comprometido para explotar otros hosts de una red interna privada a la que anteriormente no podíamos acceder.

Meterpreter nos proporciona la capacidad de añadir una ruta de red hacia la subred de la red interna, realizar **port forwarding (redirección de puertos)** y, como consecuencia, escanear y explotar otros sistemas de la red. El port forwarding es el proceso de redirigir el tráfico desde un puerto específico de un sistema objetivo hacia un puerto específico de nuestro sistema.

En pivoting, podemos redirigir un puerto remoto de un host al que anteriormente no teníamos acceso hacia un puerto local de nuestro sistema Kali Linux, de modo que podamos interactuar remotamente con el servicio que se está ejecutando en ese puerto o explotarlo.
