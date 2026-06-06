# 4. Payloads

Un payload es la carga útil que se ejecuta en un sistema objetivo una vez que una vulnerabilidad ha sido explotada con éxito. Mientras que un exploit se encarga de aprovechar una debilidad de seguridad para obtener acceso o ejecutar código, el payload representa la acción que se llevará a cabo tras dicha explotación.

Pueden distintos objetivos, siendo las más habituales:

- la apertura de una consola remota
- la ejecución de comandos en el sistema comprometido
- la recopilación de información
- la elevación de privilegios o el establecimiento de mecanismos de acceso persistente

Dentro de los ataques dirigidos al cliente (client-side attacks), los payloads suelen distribuirse mediante archivos o aplicaciones aparentemente legítimas que son ejecutadas por el usuario objetivo. Por este motivo debemos conocer técnicas relacionadas con la generación, codificación y empaquetado de payloads, así como los mecanismos empleados por las soluciones de seguridad para detectarlos y bloquearlos.

### Client-side attacks

Son un vector de ataque con el objetivo de conseguir que un usuario interactúe con contenido diseñado para ejecutar acciones controladas por el atacante en el sistema objetivo. Pueden incluir la ejecución de código, la recopilación de información, el acceso remoto al sistema o el establecimiento de comunicaciones con la infraestructura del atacante.

Suelen apoyarse en técnicas de ingeniería social para persuadir al usuario de abrir documentos, ejecutar aplicaciones o visitar recursos aparentemente legítimos. Los formatos más utilizados son: documentos ofimáticos, archivos PDF, instaladores y ejecutables portables. Los ataques del lado del cliente se centran en la interacción del usuario y en las aplicaciones que usa habitualmente.

Debido a que estos ataques suelen requerir la entrega y, en muchos casos, el almacenamiento de un payload en el sistema objetivo, es necesario considerar los mecanismos de detección y protección.

## 4.1. Generación de payloads con `Msfvenom`

Msfvenom es una herramienta de línea de comandos incluida en Metasploit que permite generar y codificar (encode) payloads para distintos sistemas operativos y plataformas.

Permite generar payloads compatibles con sistemas Windows, Linux, macOS y Android, así como exportarlos en múltiples formatos, incluyendo ejecutables, bibliotecas dinámicas, scripts y otros tipos de archivos

Con `Msfvenom` se pueden payloads que, una vez ejecutados en el sistema objetivo, establecen una comunicación con un listener o handler configurado previamente por el auditor. Esta conexión permite gestionar la sesión obtenida y evaluar el nivel de acceso alcanzado durante la prueba.

## 4.2. Codificación  de payloads con `Msfvenom`

## 4.3. Inyección de payloads en ejecutables Windows

[⟵ Anterior](03_shells.md) | [Siguiente ⟶](05_windows.md)
