# SAMBA

**Samba** es la implementación de SMB para Linux, y permite que los sistemas Windows accedan a recursos compartidos (archivos, carpetas y dispositivos) alojados en ellos.

### Obtención de credenciales con hydra

**Samba** utiliza autenticación mediante **nombre de usuario y contraseña** para obtener acceso al servidor o a un recurso compartido de red. Es posible realizar un **ataque de fuerza bruta** contra un servidor Samba con el objetivo de obtener credenciales válidas.

```bash
hydra -l <USR> -P <PASS_WORDLIST> <IP> smb
hydra -L <USR_WORDLIST> -P <PASS_WORDLIST> <IP> smb
```

### SMBMap

Una vez obtenidas credenciales legítimas, se puede utilizar **SMBMap** para enumerar los recursos compartidos de Samba, listar el contenido de dichos recursos, descargar archivos y ejecutar comandos remotos en el sistema objetivo.

```bash
smbmap -H host -u usr -p pass
```

### smbclient

También se puede utilizar una herramienta llamada **smbclient**. Es un cliente que forma parte del conjunto de herramientas de Samba. Se comunica con un servidor **LAN Manager** y ofrece una interfaz similar a la de un programa **FTP**. Puede utilizarse para:

- Descargar archivos desde el servidor al equipo local.
- Subir archivos desde el equipo local al servidor.
- Obtener información sobre directorios y recursos compartidos del servidor.

[⟵ Anterior](../05_sistema.md#explotación-linux)
