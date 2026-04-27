# Enumeración *SMB*

SMB (Server Message Block) es un protocolo que permite compartir archivos, usuarios y periféricos en una red local. Este servicio está disponible tanto para Windows (SMB) como para Linux (SAMBA). El puerto habitual es el 445, aunque originalmente sobre NetBIOS usaba el 139.

La enumeración SMB permite obtener:

- Versión SMB.
- Archivos y recursos compartidos.
- Usuarios.
- Fuerza bruta para obtener contraseñas.

## Enumeración SMB con Metasploit Framework

Los principales módulos para SMB son:

### ***smb_version***

Se usa para obtener la versión que está ejecutando el sistema objetivo. En este módulo solo es necesario configurar el host y el puerto del objetivo.

```bash
msf > show options

Module options (auxiliary/scanner/smb/smb_version):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   RHOSTS                    yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT                     no        The target port (TCP)
   THREADS  1                yes       The number of concurrent threads (max one per host)
```

### ***smb_enumusers***

Módulo dedicado a la enumeración de usuarios SMB. En este caso tampoco necesitamos configurar nada más allá de la IP y el puerto, por lo que podemos ejecutar directamente.

```bash
msf auxiliary(scanner/smb/smb_enumusers) > run

[*] 192.168.1.15:445 - Enumerating users using SAMR
[+] 192.168.1.15:445 - BUILTIN\Administrator (RID: 500)
[+] 192.168.1.15:445 - BUILTIN\Guest (RID: 501)
[+] 192.168.1.15:445 - WORKGROUP\john (RID: 1000)
[+] 192.168.1.15:445 - WORKGROUP\mary (RID: 1001)
[+] 192.168.1.15:445 - WORKGROUP\backup (RID: 1002)
[*] 192.168.1.15:445 - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

### ***smb_enumshares***

Lista los recursos compartidos SMB. Para este módulo podemos activar la opción *ShowFiles* para obtener información más detallada

```bash
msf auxiliary(scanner/smb/smb_enumshares) > run

[*] 192.168.1.10:445    - Enumerating shares
[+] 192.168.1.10:445    - ADMIN$       - (DISK) Remote Admin
[+] 192.168.1.10:445    - C$           - (DISK) Default share
[+] 192.168.1.10:445    - IPC$         - (IPC)  Remote IPC
[+] 192.168.1.10:445    - Public       - (DISK) Public shared folder
[+] 192.168.1.10:445    - Backups      - (DISK) Backup storage
[*] 192.168.1.10:445    - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

### ***smb_login***

Prueba de fuerza bruta de credenciales contra el servicio SMB. En este caso las opciones importantes son: *RHOSTS, RPORT, PASS_FILE y USER_FILE* (SMBUser para un usuario especifico, por ejemplo un usuario admin). También es interesante la opción *STOP_ON_SUCCESS* para detener las pruebas en caso de obtener una contraseña correcta.

## Enumeración con NSE

La enumeración SMB con Nmap Scripting Engine (NSE) utiliza scripts para listar recursos compartidos, usuarios, políticas y vulnerabilidades. 

**Enumerar recursos compartidos (Shares):**

```bash
nmap -p <Puerto> --script smb-enum-shares <IP>
```

**Enumerar usuarios:**

```bash
nmap -p <Puerto> --script smb-enum-users <IP>
```

**Detectar SO y versión SMB:**

```bash
nmap -p <Puerto> --script smb-os-discovery <IP>
```

**Enumerar dominios:**

```bash
nmap -p <Puerto> --script smb-enum-domains <IP>
```

## Enumeración con enum4linux

Enum4linux es una herramienta de automatización que combina información obtenida de smbclient, rpclient, net y nmblookup. Extrae información sobre: usuarios, grupos, recursos compartidos, políticas de contraseñas y sistema operativo.

**Enumerar usuarios:**

```bash
enum4linux -U <IP>
```

**Enumerar grupos:**

```bash
enum4linux -G <IP>
```

**Enumerar recursos compartidos:**

```bash
enum4linux -S <IP>
```

**Información sobre la política de contraseñas:**

```bash
enum4linux -P <IP>
```

**Sistema operativo:**

```bash
enum4linux -O <IP>
```

**Enumerar nombres de dominio:**

```bash
enum4linux -N <IP>
```

**Obtener información sobre impresoras:**

```bash
enum4linux -I <IP>
```

**Información adicional de dominio via LDAP:**

```bash
enum4linux -L <IP>
```

**Enumeración completa de todo lo anterior:**

```bash
enum4linux -A <IP>
```

[⟵ Anterior](../03_activa.md#323-enummeración-de-servicios)
