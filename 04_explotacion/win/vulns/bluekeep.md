# Explotación de la vulnerabilidad CVE-2019-0708 de RDP (BlueKeep)

BlueKeep (CVE-2019-0708) es el nombre de una vulnerabilidad de RDP permitía ejecutar código arbitrario de forma remota y obtener acceso al sistema Windows y, en consecuencia, a la red de la que forma parte el sistema.

El exploit aprovecha una vulnerabilidad en el protocolo RDP de Windows que permite a los atacantes acceder a una porción de la memoria del kernel, lo que les posibilita ejecutar código arbitrario de forma remota con privilegios de sistema y sin necesidad de autenticación.

La vulnerabilidad BlueKeep afecta a varias versiones de Windows:

- Windows XP
- Windows Vista
- Windows 7
- Windows Server 2008 y Windows Server 2008 R2.

La vulnerabilidad BlueKeep tiene diversas PoC (Pruebas de Concepto) y códigos de explotación no legítimos que podrían ser de naturaleza maliciosa. Por lo tanto, se recomienda utilizar únicamente código de explotación y módulos verificados para la explotación.

## Enumeración

MSF cuenta con un módulo auxiliar de BlueKeep que puede utilizarse para comprobar si un sistema objetivo es vulnerable al exploit.


## Explotación

Metasploit dispone de un módulo de explotación que puede utilizarse para explotar la vulnerabilidad en sistemas sin parchear y, en consecuencia, proporcionarnos una sesión privilegiada de Meterpreter en el sistema objetivo.

> Nota: Apuntar a la memoria del espacio del kernel y a las aplicaciones puede provocar fallos del sistema.

[⟵ Anterior](../../05_sistema.md#explotación-windows)