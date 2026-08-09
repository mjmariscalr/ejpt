# Escalada de privilegios en sistemas Windows

Para elevar losprivilegios en Windows, primero debemos identificar las vulnerabilidades de relacionadas que existen en el sistema objetivo. 

Este proceso variará considerablemente según el tipo de sistema objetivo al que hayas obtenido acceso. La escalada de privilegios en Windows puede llevarse a cabo mediante una gran variedad de técnicas, dependiendo de la versión de Windows y de la configuración específica del sistema.

**PrivescCheck**: este script tiene como objetivo enumerar problemas comunes de configuración en Windows que pueden aprovecharse para realizar una escalada local de privilegios. Además, recopila diversa información que puede resultar útil durante las fases de explotación y/o postexplotación. Disponible en su repositorio [GitHub](https://github.com/itm4n/PrivescCheck).

```bash
C: > powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck"
```

## Credenciales en texto plano en Winogn

Winlogon (Windows Logon Application) es un proceso esencial de Microsoft Windows encargado de gestionar el inicio y el cierre de sesión de los usuarios.

Entre sus funciones principales están:

- Gestionar el inicio de sesión (cuando introduces tu contraseña o PIN).
- Cargar el perfil del usuario al iniciar sesión.
- Bloquear y desbloquear el equipo (por ejemplo, con Windows + L).
- Gestionar la secuencia de atención segura (Ctrl + Alt + Supr).
- Coordinar el cierre de sesión y algunas tareas relacionadas con la seguridad.

El fallo se encuentra en **AutoAdminLogon**. Cuando se configura el inicio de sesión automático, Windows guarda la contraseña en el Registro y queda almacenada en texto claro para que Winlogon pueda iniciar sesión automáticamente.

Si durante el paso anterior hemos obtenido las credenciales de un usuario administrador, podemos usar [PSExec](../04_explotacion/win/smb.md#ejecución-remota-de-comandos-con-psexec) para obtener acceso con este usuario.

[⟵ Anterior](05_mejora_shell.md) | [Siguiente ⟶](07_linux_privesc.md)
