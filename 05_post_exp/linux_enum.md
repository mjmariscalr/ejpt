# 2. Enumeración Linux

La fase de enumeración permite identificar las características del sistema operativo, su configuración y los componentes instalados, información que servirá como base para las siguientes etapas de la post-explotación. Conocer estos aspectos ayuda a comprender mejor el objetivo y permite determinar qué técnicas de escalada de privilegios, persistencia o movimiento lateral pueden ser viables.

## Enumeración del sistema

Después de obtener acceso inicial a un sistema objetivo, siempre es importante recopilar más información sobre él, como qué sistema operativo está ejecutando y cuál es su versión. Esta información es muy útil, ya que nos da una idea de lo que podemos hacer y del tipo de exploits que podrían ser compatibles.

Buscamos:

- Nombre del equipo.
- Distribución del sistema operativo y su versión.
- Versión del kernel y arquitectura.
- Información de la CPU.
- Información de los discos y unidades montadas.
- Paquetes o software instalados.

**Información del sistema:**

```bash
meterpreter > sysinfo
Computer        : 192.168.10.25
OS              : Debian 9.5 (Linux 5.4)
Architecture    : x64
Meterpreter     : x86/linux
```

**Obtener una sesion bash interactiva desde meterpreter:**

```bash
meterpreter > shell
/bin/bash -i
usr@hostname:
```

**Hostname:**

Al obtener una shell interaciva con el paso anterior podemos ver el nombre del equipo, pero tambien podemos hacerlo con el comando hostname:

```bash
usr@hostname: hostname
```

**Comprobar la distribución:**

```bash
usr@hostname: cat /etc/*release
```

**Información del sistema:**

```bash
usr@hostname: uname -a                                                                        
Linux hostname 5.14.28-1-generic #1 SMP PREEMPT_DYNAMIC Sat, 04 Jul 2026 20:51:33 +0000 x86_64 GNU/Linux
```

**Variables de entorno del usuario:**

```bash
usr@hostname: env
```

**Información sobre la CPU:**

```bash
usr@hostname: lscpu
lscpu
Arquitectura:                            x86_64
  modo(s) de operación de las CPUs:      32-bit, 64-bit
  Tamaños de las direcciones:            39 bits physical, 48 bits virtual
  Orden de los bytes:                    Little Endian
CPU(s):                                  8
  Lista de la(s) CPU(s) en línea:        0-7
.
.
.
```

**Memoria libre:**

```bash
usr@hostname: free -h                                           
               total       usado       libre  compartido   búf/caché  disponible
Mem:            15Gi       6,7Gi       4,0Gi       818Mi       4,8Gi       8,8Gi
Inter:            0B          0B          0B
```

**Discos instalados:**

```bash
usr@hostname: df -h
usr@hostname: lsblk
```

**Listar los paquetes instalados:**

| Distribuciones                                                        | Comando                              |
| --------------------------------------------------------------------- | ------------------------------------ |
| Debian, Ubuntu, Kali, Linux Mint                                      | `dpkg -l` (o `apt list --installed`) |
| Fedora, RHEL 8/9, CentOS Stream, Rocky Linux, AlmaLinux, Oracle Linux | `dnf list installed`                 |
| CentOS 7, RHEL 7                                                      | `yum list installed`                 |
| Arch Linux, Manjaro                                                   | `pacman -Q`                          |
| openSUSE, SUSE Linux Enterprise                                       | `zypper se --installed-only`         |
| Alpine Linux                                                          | `apk info`                           |
| Gentoo                                                                | `qlist -I` (o `equery list '*'`)     |
| Void Linux                                                            | `xbps-query -l`                      |

## Enumeración de grupos y usuarios

**Comprobar los privilegios de un usuario en Linux:**

| Información                                  | Comando                            |
| -------------------------------------------- | ---------------------------------- |
| Usuario actual                               | `whoami`                           |
| UID, GID y grupos                            | `id`                               |
| Grupos del usuario                           | `groups`                           |
| Privilegios sudo del usuario actual          | `sudo -l`                          |
| Privilegios sudo de otro usuario (como root) | `sudo -l -U <usuario>`             |
| Comprobar si es root                         | `id -u` (si devuelve `0`, es root) |

**Listar todos los usuarios:**

Para diferenciar una cuenta de servicio de una de usuario podemos comprobar la shell asociada. Si tiene algo como `/bin/bash` o `/bin/sh`, será una cuenta de usuario.

```bash
usr@hostname: cat /etc/groups
usr@hostname: cat /etc/groups | grep -v /nologin # Excluye la cadena /nologin y por tanto cuentas de servicio.
usr@hostname: ls /home
usr@hostname: w    # Muestra los usuarios conectados actualmente y la actividad que están realizando.
usr@hostname: who  # Muestra qué usuarios tienen una sesión iniciada en el sistema.
usr@hostname: last # Muestra el historial de inicios y cierres de sesión de los usuarios.
```
