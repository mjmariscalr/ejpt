# libssh Authentication Bypass (CVE-2018-10933)

La vulnerabilidad CVE-2018-10933 afecta a determinadas versiones de libssh, una biblioteca ampliamente utilizada para implementar funcionalidades del protocolo SSH en aplicaciones cliente y servidor.

El fallo se encontraba en la máquina de estados utilizada por libssh en modo servidor para gestionar el proceso de autenticación. Debido a un error lógico en el tratamiento de determinados mensajes del protocolo SSH, un cliente malicioso podía hacer que el servidor considerase autenticada una conexión sin haber validado previamente ninguna credencial.

En condiciones normales, un cliente SSH debe demostrar su identidad mediante un proceso de autenticación. Sin embargo, la vulnerabilidad permitía alterar la secuencia esperada de mensajes del protocolo y forzar al servidor vulnerable a aceptar la sesión como legítima. Como resultado, un atacante remoto podía acceder a los servicios ofrecidos por la aplicación sin necesidad de conocer usuarios, contraseñas o claves válidas.

La vulnerabilidad afectaba únicamente al código de libssh ejecutándose en modo servidor; las aplicaciones que utilizaban la biblioteca exclusivamente como cliente no estaban expuestas al problema. Las versiones vulnerables incluían las ramas 0.6.x, 0.7.x anteriores a la versión 0.7.6 y 0.8.x anteriores a la versión 0.8.4.

[⟵ Anterior](../05_sistema.md#explotación-linux)
