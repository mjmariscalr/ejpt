# Escalada de privilegios en sistemas Linux

El proceso de análisis de vulnerabilidades referentes a la escalada de privilegios es más simple que en Windows, ya que no necesitamos una herramienta extra para hacerlo. Toda esta información la obtenemos en el paso de enumeración con [LinEnum](03_linux_enum.md#linenum)

## Error de configuración en los permisos

Para buscar este fallo en archivos del sistema que nos puedan permitir una escalada de privilegios, podemos hacerlo con:

```bash
usr@hostname$ find / -not -type l -perm -o+w
```

- `-not -type l`: significa que no sea un enlace simbólico.
- `-perm`: filtra por permisos.
- `o+w`:
	- `o` = others (otros usuarios).
	- `+w` = permiso de escritura.

Si el archivo `/etc/shadow`, encargado de almacenar las contraseñas de usuario, puede ser modificado por cualquier usuario, significa que un usuario sin privilegios podría modificar la contraseña del usuario root y obtener acceso completo al sistema.

Para generar una contraseña en el formato de hash que usa este archivo podemos hacer lo siguiente:

```bash
usr@hostname$ openssl passwd -1 -salt <texto> <pass>
```

- `passwd`: Subcomando de OpenSSL que permite generar el hash de una contraseña.
- `-1`: Indica que se utilice el algoritmo **MD5-Crypt** para generar el hash. El resultado tendrá un formato similar a: `$1$abc$Qm3KjY...`
- `-salt`: evita que una misma contraseña genere siempre el mismo hash y dificulta el uso de tablas precalculadas.

Por último, para modificar la contraseña del usuario, modificamos la segunda columna de `/etc/shadow` para añadir nuestra contraseña:

```text
root:$y$j9T$J9kP8mQv...:19850:0:99999:7:::
usuario:$y$j9T$Q8wErTyU...:19850:0:99999:7:::
```

## Errores de configuración en sudo

`sudo` hace lo siguiente:

1. Comprueba el archivo de políticas (sudoers y los ficheros incluidos).
2. Verifica si el usuario puede ejecutar ese comando concreto como otro usuario (normalmente root).
3. Si la política exige autenticación, solicita la contraseña (o reutiliza un ticket de autenticación válido).
4. Si la autorización tiene éxito, crea un nuevo proceso con el UID/GID del usuario de destino y ejecuta el binario autorizado con esos privilegios.

Si ese programa incorpora una función para ejecutar otros comandos (por ejemplo, un editor, un paginador o un depurador), esos comandos se lanzan como procesos hijos del programa y, salvo restricciones adicionales, heredan sus credenciales efectivas. Es un comportamiento normal de los sistemas Unix: un proceso hijo hereda el contexto de ejecución de su padre.

La primera comprobación que podemos hacer es listar los comandos que puede ejecutar el usuario:

```bash
usr@hostname$ sudo -l
```

Uno de los programas que puede facilitarnos la escalada de privilegios es `man`. Si podemos ejecutar este comando como sudo sin indicar ningún tipo de contraseña, una vez dentro podemos escribir `!/bin/bash` para obtener una nueva sesión. Si hemos ejecutado `man` con permisos root, todo lo que ejecutemos dentro los hereda.

Otros comandos que permiten la ejecución de comandos son:

| Programa                   | Sintaxis                   | Ejemplo         |
| -------------------------- | -------------------------- | --------------- |
| `man` (a través de `less`) | `!comando`                 | `!ls -l`        |
| `less`                     | `!comando`                 | `!pwd`          |
| `more`                     | `!comando`                 | `!date`         |
| `vi` / `vim`               | `:!comando`                | `:!gcc main.c`  |
| `nvi`                      | `:!comando`                | igual que Vim   |
| `ed`                       | `!comando`                 | `!ls`           |
| `ex`                       | `!comando`                 | `!make`         |
| `gdb`                      | `shell comando`            | `shell ls`      |
| `lldb`                     | `platform shell` o `shell` | `shell id`      |
| `ftp`                      | `!comando`                 | `!ls`           |
| `lftp`                     | `!comando`                 | `!cat archivo`  |
| `sftp` (OpenSSH)           | `!comando`                 | `!pwd`          |
| `psql`                     | `\! comando`               | `\! ls`         |
| `sqlite3`                  | `.shell comando`           | `.shell whoami` |
| `mysql`                    | `system comando`           | `system ls`     |
| `gnuplot`                  | `!comando`                 | `!date`         |
| `expect`                   | `exec comando`             | `exec ls`       |
| `python` (REPL/IPython)    | `!comando` (IPython)       | `!ls`           |
| `R`                        | `system("comando")`        | `system("ls")`  |

Y comandos basados en `less` como:

- `git log`
- `git diff`
- `git show`
- `git blame`
- `systemctl status`
- `journalctl`
- `ps aux | les`

[⟵ Anterior](06_win_privesc.md.md) | [Siguiente ⟶](08_win_pers.md)
