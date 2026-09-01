# Evasión AV

La evasión de defensas consiste en técnicas para evitar ser detectados a lo largo del ataque. Las técnicas utilizadas para la evasión de defensas incluyen:

- desinstalar o deshabilitar el software de seguridad
- la ofuscación o el cifrado de datos y scripts 
- procesos de confianza para ocultar y enmascarar malware

**Métodos de detección del antivirus:**

1. ***Detección basada en firmas:*** Una firma de AV es una secuencia única de bytes que identifica un malware de forma inequívoca. El exploit ofuscado no debe coincidir con ninguna firma conocida en la base de datos del AV.
2. ***Detección basada en heurística:*** Se basa en reglas o decisiones para determinar si un binario es malicioso. También busca patrones específicos dentro del código o de las llamadas del programa.
3. ***Detección basada en comportamiento:*** Se basa en identificar el malware mediante el monitoreo de su comportamiento.

**Técnicas de evasión On-disk:**

- ***Ofuscación:*** proceso de ocultar algo. Reorganiza el código para hacer que sea más difícil de analizar o de aplicar ingeniería inversa.
- ***Encoding:*** cambia los datos a un nuevo formato utilizando un esquema. Es un proceso reversible; los datos se pueden codificar a un nuevo formato y decodificar a su formato original.
- ***Packing:*** Genera un ejecutable con una nueva estructura binaria de menor tamaño y proporciona una nueva firma.
- ***Crypters:*** Cifran el código o los payloads y descifran el código cifrado en la memoria.

**Técnicas de evasión In-Memory:**

Se centran en la manipulación de la memoria y no escribe archivos en el disco. Se inyecta el payload en un proceso aprovechando varias API de Windows, luego, se ejecuta en la memoria en un hilo (thread) separado.

## Ofuscación de código PowerShell

La ofuscación se refiere al proceso de ocultar algo importante, valioso o crítico. La ofuscación reorganiza el código para dificultar su análisis o ingeniería inversa (RE). La mayoría de las soluciones antivirus detectarán de inmediato el código malicioso de PowerShell y es necesario ofuscar/codificar el código para evitar la detección.

[Invoke-Obfuscation](https://github.com/danielbohannon/Invoke-Obfuscation) es un ofuscador de comandos y scripts de PowerShell de código abierto, compatible con PowerShell v2.0+.

Podemos instalar powershell en kali desde los repositorios oficiales:

```console
usr@host# apt install -y powershell
usr@host# pwsh
PS /path>
```

Para usar esta herramienta, debemos navegar al directorio donde se almacena e importar el módulo:

```console
PS /path/Invoke-Obfuscation> Import-Module Invoke-Obfuscation.psd1 
```

Ofuscar un script:

```console
Invoke-Obfuscation> SET SRPITPATH /path/script.ps1
```

A continuación se eligen las opciones de ofuscación y se copia el resultado a un nuevo script.

[⟵ Anterior](05_sistema.md) | [Siguiente ⟶](../05_post/01_intro.md)
