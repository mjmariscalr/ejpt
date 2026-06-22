# vsftpd 2.3.4 Backdoor

Uno de los servidores FTP más conocidos en sistemas Linux es **vsftpd (Very Secure FTP Daemon)**, que ha sido ampliamente utilizado en distribuciones como Ubuntu, CentOS y Fedora.

Sin embargo, la versión vsftpd 2.3.4 es conocida por estar asociada a la vulnerabilidad **CVE-2011-2523**, un caso histórico de ataque a la cadena de suministro. En este incidente, el archivo de distribución del software fue comprometido y se introdujo una backdoor en el binario distribuido. Como resultado, sistemas que instalaban esta versión podían quedar expuestos a acceso no autorizado y posible ejecución remota de comandos.

Este caso demuestra que una vulnerabilidad no siempre proviene de un fallo en el código o en la configuración, sino que puede originarse en la propia cadena de distribución del software. Por ello, la verificación de integridad de paquetes y la procedencia del software son aspectos críticos en la seguridad de sistemas.
