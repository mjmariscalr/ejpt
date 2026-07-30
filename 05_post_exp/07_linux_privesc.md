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
openssl passwd -1 -salt <texto> <pass>
```

- `passwd`: Subcomando de OpenSSL que permite generar el hash de una contraseña.
- `-1`: Indica que se utilice el algoritmo **MD5-Crypt** para generar el hash. El resultado tendrá un formato similar a: `$1$abc$Qm3KjY...`
- `-salt`: evita que una misma contraseña genere siempre el mismo hash y dificulta el uso de tablas precalculadas.

Por último, para modificar la contraseña del usuario, modificamos la segunda columna de `/etc/shadow` para añadir nuestra contraseña:

```text
root:$y$j9T$J9kP8mQv...:19850:0:99999:7:::
usuario:$y$j9T$Q8wErTyU...:19850:0:99999:7:::
```
