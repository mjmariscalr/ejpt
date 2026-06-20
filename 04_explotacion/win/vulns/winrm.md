# Windows Remote Management (WinRM)

WinRM es un protocolo de administración remota de Windows a través de HTTP(S). Microsoft implementó WinRM en Windows para facilitar el trabajo de los administradores de sistemas.

Se utiliza normalmente para:

- Acceder de forma remota e interactuar con equipos Windows en una red local.
- Acceder de forma remota y ejecutar comandos en sistemas Windows.
- Administrar y configurar sistemas Windows de forma remota.

Normalmente utiliza los puertos TCP 5985 y 5986 (HTTPS). 

WinRM implementa control de acceso y seguridad para la comunicación entre sistemas mediante diversas formas de autenticación:

- usuario:contraseña
- usuario:hash
- kerberos
- NTLM
- Otras formas

Podemos usar `CrackMapExec` para obtener usuarios y contraseñas mediante fuerza bruta y un script llamado `evil-winrm` para obtener una shell remota.

## Enumeración

El escaneo por defecto de `nmap` solo analiza los 1000 puertos más comunes y no incluye los específicos de WinRM, así que será necesario analizar los puertos por defecto, un rango más amplio o incluso el rango completo de puertos para encontrar esta utilidad.

![winrm_nmap](../../../img/winrm_nmap.png)

## Explotación

**Obtención de credenciales con `CrackMapExec`**

`CrackMapExec` es una herramienta de seguridad ofensiva utilizada para la evaluación y auditoría de redes Windows y entornos Active Directory. Su función principal es automatizar la interacción con sistemas remotos que usan protocolos como SMB, WinRM, MSSQL o SSH.

[⟵ Anterior](../../05_sistema.md#explotación-windows)