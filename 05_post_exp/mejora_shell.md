# Mejora de shell no interactiva

Tras obtener acceso inicial a un sistema, es habitual que la primera consola disponible sea una **shell no interactiva**, con funcionalidades limitadas y poco adecuada para realizar tareas de post-explotación. Por ello, una práctica común consiste en mejorar o **actualizar la shell** a una más completa que proporcione mayores capacidades de administración, interacción con el sistema y ejecución de técnicas de post-explotación de forma más eficiente.

Una shell no interactiva es una consola con funcionalidades limitadas, en la que puedes ejecutar comandos, pero no dispones de todas las características de una terminal normal iniciada por un usuario y suele obtenerse tras explotar una vulnerabilidad.

Sus limitaciones más comunes son:

- No permite utilizar correctamente las teclas de dirección, Tab o el historial de comandos.
- No muestra un prompt completo o es muy básico.
- Muchas aplicaciones interactivas (vim, nano, top, htop, su, sudo, etc.) no funcionan correctamente.
- No dispone de control de trabajos y combinaciones como `Ctrl+C` pueden cerrar la conexión en lugar de comportarse como en una terminal normal.

Para actualizarla usamos el comando `/bin/bash -i`, aunque no es obligatorio que `bash` esté instalado en el sistema. Para comprobar las shell instaladas usamos `cat /etc/shells`. Otras formas son:

```bash
/bin/sh -i
python -c 'import pty; pty.spawn("/bin/bash")'
perl -e 'exec "/bin/bash";'
ruby: exec "/bin/bash"
```
