# Conexión remota

Durante una auditoría es necesario interactuar con sistemas remotos ya que muchas fases de una evaluación requieren obtener o mantener acceso a un sistema objetivo de forma controlada. Para ello existen diferentes mecanismos que permiten establecer canales de comunicación entre dos equipos a través de una red.

## Fundamentos de `Netcat`

**`Netcat`** (también conocida como la *navaja suiza de TCP/IP*) es una utilidad de red utilizada para leer y escribir datos a través de conexiones de red utilizando los protocolos TCP o UDP. Está disponible tanto para sistemas operativos \*NIX como para Windows, lo que la convierte en una herramienta extremadamente útil en entornos de pentesting multiplataforma.

`Netcat` utiliza una arquitectura de comunicación cliente-servidor con dos modos de funcionamiento:

- **Modo cliente:** permite conectarse a cualquier puerto TCP/UDP o a un listener de `Netcat` (servidor).
- **Modo servidor:** permite escuchar conexiones entrantes de clientes en un puerto específico.

Puede utilizarse para:

- **Banner Grabbing**
- **Escaneo de puertos**
- **Transferencia de archivos**
- **Bind Shells y Reverse Shells**

### Uso básico

#### Establecer una conexión TCP con un host remoto

Puede usarse para realizar *banner grabbing*.

Parámetros:

- **-n:** evita la resolución DNS.
- **-v:** modo verbose.

```bash
nc -nv <IP> <Puerto>
```

#### Conexión *UDP*

```bash
nc -u <IP> <Puerto>
```

#### Habilitar el modo servidor (*listener*)

Esto es un ejemplo de como establecer la conexión entre dos equipos mediante `Netcat`. Para comprender su uso durante un pentesting es necesario leer las siguientes secciones.

El primer paso es transferir `netcat` a *Windows*. Para ello podemos usar `nc.exe`, la versión precompilada disponible en kali que se encuentra en `/usr/share/windows-binaries`.

Para transferir este ejecutable hay dos formas:

- Crear un servidor web que aloje el ejecutable y descargarlo desde navegador del objetivo.
- Crear un servidor web que aloje el ejecutable y descargarlo en el objetivo mediante comandos.

Podemos lanzar el servidor con el módulo `SimpleHTTPServer` de *Python*:

```bash
python -m SimpleHTTPServer 80
```

Una vez descargado, podemos poner a `Netcat` en modo escucha (*listener*), esperando conexiones entrantes en el puerto especificado.

Parámetros:

- **-l:** (listen) indica que Netcat debe escuchar conexiones entrantes.
- **-p:** especifica el puerto en el que escuchará.

```bash
nc -nvlp <Puerto>
```

#### Transferir archivos desde el cliente al servidor

Servidor:

```bash
nc -nvlp <Puerto> > <archivo>
```

Cliente:

```bash
nc -nv <IP> <Puerto> < <archivo>
```

## Bind shell

Una bind shell o de enlace es un tipo de shell remota en la que un sistema remoto abre un puerto específico, espera conexiones entrantes y le otorga al atacante control directo sobre la línea de comandos. Se puede configurar un listener de Netcat para lanzar un ejecutable específico al realizar una conexión, como cmd.exe o /bin/bash.

![img](../img/bindshell.png)

## Reverse shell
