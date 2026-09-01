# Ofuscación de código PowerShell

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
