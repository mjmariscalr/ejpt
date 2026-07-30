# Escalada de privilegios en sistemas Linux

El proceso de análisis de vulnerabilidades referentes a la escalada de privilegios es más simple que en Windows, ya que no necesitamos una herramienta extra para hacerlo. Toda esta información la obtenemos en el paso de enumeración con [LinEnum](03_linux_enum.md#linenum)

## Error de configuración en los permisos

Para buscar este fallo en archivos del sistema que nos puedan permitir una escalada de privilegios, podemos hacerlo con:

```bash
usr@hostname$ find / -not -type l -perm -o+w
```
