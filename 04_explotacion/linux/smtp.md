# SMTP: Ejecución de código remoto en Haraka (CVE-2016-1000282)

SMTP (Simple Mail Transfer Protocol) es un protocolo de comunicación utilizado para la transmisión de correos electrónicos. Utiliza por defecto el puerto TCP 25. También puede configurarse para funcionar en los puertos TCP 465 y 587.

Haraka es un servidor SMTP de alto rendimiento y de código abierto desarrollado en Node.js que incluye un plugin para procesar archivos adjuntos. Las versiones de Haraka anteriores a la v2.8.9 son vulnerables a una inyección de comandos.

**CVE-2016-1000282** es una vulnerabilidad crítica de **inyección de comandos** presente en versiones de **Haraka** anteriores a la **2.8.9**. Un atacante remoto puede enviar un correo electrónico con un archivo adjunto especialmente manipulado para ejecutar comandos arbitrarios en el servidor, lo que puede derivar en una **ejecución remota de código** y el compromiso completo del sistema.

[⟵ Anterior](../05_sistema.md#explotación-linux)
