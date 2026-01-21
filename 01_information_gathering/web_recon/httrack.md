# Nombre

HTTrack es una herramienta legal y legítima cuyo propósito es clonar o descargar un sitio web para verlo offline. Permite descargar HTML, CSS, JavaScript, imágenes y estructura completa de enlaces. Esto puede ser útil para revisar el código sin hacer solicitudes continuas al servidor, además de entender la lógica, rutas, validaciones, etc.

## Guía de uso

Esta herramienta puede usarse tanto en modo gráfico como en línea de comandos. En línea de comando, el uso básico es el que se muestra a continuación. Con la opción *-O* se indica el directorio donde se descarga el sitio web.

```bash
$ httrack https://preview.owasp-juice.shop  -O "/home/mmr23/websites/adf"
```

### Limitar el número de conexiones

Evita que HTTrack abra muchas conexiones simultáneas que podrían sobrecargar el servidor y levantar sospechas. Para ello usamos la opcion *%cN*, donde N es el límite de conexiones por segundo.

```bash
$ httrack https://preview.owasp-juice.shop  -O "/home/mmr23/websites/adf" -%c1
```

## Riesgo de detección

HTTrack recorre todo el sitio descargando páginas y recursos, por lo que se generan muchas solicitudes HTTP y puede realizar solicitudes paralelas, lo que puede levantar sospechas. Si se usa con: velocidad baja, sin paralelizar demasiadas conexiones y con un comportamiento similar al de un navegador, se reduce el riesgo de disparar alertas.

## Recursos

[HTTrack](www.httrack.com/)

[⟵ Anterior](../02_pasiva.md#reconocimiento-web)
