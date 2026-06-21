# FTP

**FTP (File Transfer Protocol)** es un protocolo que utiliza el puerto **TCP 21** y se emplea para facilitar el intercambio de archivos entre un servidor y uno o varios clientes. También se usa como medio para transferir archivos hacia y desde el directorio de un servidor web.

La autenticación en FTP requiere una combinación de **nombre de usuario y contraseña**. Como resultado, un atacante podría intentar realizar un **ataque de fuerza bruta** contra el servidor FTP para identificar credenciales válidas. En algunos casos, los servidores FTP pueden estar configurados para permitir **acceso anónimo**, lo que permite a cualquier persona acceder al servidor FTP sin proporcionar credenciales.

La forma que vamos a ver en esta sección de obtener acceso a FTP es mediante un ataque de fuerza bruta. Existen vulnerabilidades que afectan a FTP, pero generalmente solo afectan a un software específico y hay multitud de ellos disponibles para linux: vsftpd, ProFTPD, Pure-FTPd, WU-FTPD, etc.

```bash
hydra -L usr_file -P pass_file host ftp
```

[⟵ Anterior](../05_sistema.md#explotación-linux)
