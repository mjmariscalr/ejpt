# Enumeración SMB

SMB (Server Message Block) es un protocolo que permite compartir archivos, usuarios y periféricos en una red local. Este servicio está disponible tanto para Windows (SMB) como para Linux (SAMBA). El puerto habitual es el 445, aunque originalmente sobre NetBIOS usaba el 139.

La enumeración SMB permite obtener:

- Versión SMB.
- Archivos y recursos compartidos.
- Usuarios.
- Fuerza bruta para obtener contraseñas.

## Enumeración SMB con Metasploit Framework

Los principales módulos para SMB son:

### ***smb_version***

Para obtener la versión que está ejecutando el sistema objetivo.

```bash

```

### ***smb_enumusers***

Módulo dedicado a la enumeración de usuarios SMB.

```bash

```

### ***smb_enumshares***

Para listar los recursos compartidos SMB.

```bash

```

### ***smb_login***

Prueba credenciales contra el servicio SMB.


```bash

```

## Enumeración con NSE



## Enumeración con enum4linux

[⟵ Anterior](../03_activa.md#322-msf-módulos-auxiliares)
