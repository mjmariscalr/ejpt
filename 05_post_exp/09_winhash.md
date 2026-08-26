# Dumping y cracking de hashes Windows

Windows almacena los hashes de las contraseñas de los usuarios de forma local en la base de datos **SAM (Security Accounts Manager)**. No se puede copiar mientras el sistema operativo está en ejecución, ya que el kernel de Windows lo mantiene bloqueado. Es necesario usar técnicas y herramientas en memoria para realizar dumping desde el proceso **LSASS**. En las versiones modernas de Windows, la base de datos SAM está cifrada mediante una syskey.

La autenticación y verificación de las credenciales de los usuarios se llevan a cabo mediante la **Autoridad de seguridad local (LSA)**, un componente del sistema encargado de aplicar y gestionar aspectos de la seguridad del sistema. Participa en:

- **Autenticación de usuarios:** comprobar las credenciales cuando un usuario inicia sesión.
- **Aplicación de políticas de seguridad:** por ejemplo, las relacionadas con contraseñas o permisos.
- **Gestión de tokens de seguridad:** determinar qué usuario y qué privilegios tiene una sesión.
- **Gestión de credenciales** del sistema.

## NTLM (NTHash)

El hashing es el proceso de convertir unos datos en un valor de longitud determinada. Para generar este nuevo valor se utiliza un algoritmo de hash. En Windows, desde su versión 2003, existen dos tipos de hashes: **LM** y **NTLM**; pero desde Windows Vista, deshabilita LM y usa por defecto NTLM.

NTLM es una colección de protocolos de autenticación que usa Windows para facilitar la autenticación entre dos equipos. Usa un algoritmo MD4 desechando la contraseña original, y mejora LM en varios aspectos:

- No separa el hash en dos trozos.
- Case sensitive.
- Permite el uso de símbolos y caracteres unicode.

### Dumping de hashes NTLM

Podemos realizar dumping de los hashes de contraseñas de Windows mediante herramientas, como: `hashdump` de Meterpreter o `Mimikatz`.

#### `hashdump`

Antes de comenzar el dumping es recomendable migrar a LSASS para migrar la sesión a 64 bits y tener mayor estabilidad al trabajar directamente en el proceso LSASS.

```bash
meterpreter > pgrep lsass
meterpreter > migrate <process_id>
```

Ahora podemos simplemente usar el comando `hashdump`, que nos proporcionará una lista con usuarios y los hashes de sus contraseñas.

```bash
meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:8846f7eaee8fb117ad06bdd830b7586c:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
```

| Columna            | Qué representa                                                                                                                                                                                                  |
| ------------------ | ------------------------------------------------------------------------------------------- |
| **Usuario**        | Nombre de la cuenta de Windows.                                                             |
| **RID** o *Relative Identifier* | Identificador relativo de la cuenta dentro de ese equipo/dominio. `500` suele corresponder a la cuenta Administrador integrada y `501` a Guest.                                                  |
| **LM hash**        | Suele estar deshabilitado y aparece un valor conocido que indica que no se está utilizando. |
| **NTLM hash**      | Hash **NT** asociado a la contraseña.                                                       |
| **Campos finales** | Campos adicionales del formato de credenciales de Windows.                                  |

#### `Mimikatz`

Mimikatz es una herramienta de post-explotación de Windows que permite extraer credenciales en texto plano de la memoria y hashes de contraseñas de las bases de datos SAM locales, entre otros datos.

Podemos utilizar el ejecutable de Mimikatz o, si tenemos acceso a una sesión Meterpreter en un objetivo Windows, podemos utilizar la extensión integrada Kiwi de Meterpreter, que permite ejecutar Mimikatz dinámicamente en el sistema objetivo sin escribirlo en el disco, lo que se conoce como ejecución fileless.

```bash
meterpreter > load kiwi
meterpreter > creds_all        # Obtiene todas las credenciales
meterpreter > lsa_dump_sam     # Obtiene hashes de cuentas locales de la SAM
meterpreter > lsa_dump_secrets # Obtiene secretos almacenados por LSA
meterpreter > 
```

### Cracking de hashes NTLM

Una vez que hayamos extraído los hashes, podemos realizar su cracking usando: `John the Ripper` o `Hashcat`

#### `John the Ripper`

Para listar todos los hashes soportados: `john --list=formats`.

Esta herramienta usa una wordlist por defecto en caso de no indicarle una. Podemos usarla de la siguiente forma:

```console
usr@hostname:~# john --format=NT hashes.txt --wordlist=wordlist.txt
```

#### `Hashcat`

```console
usr@hostname:~# hashcat -a3 -m 1000 hashes.txt wordlist.txt
```

Siendo:

- `-a`: indica el modo de ataque, siendo 3 fuerza bruta.
- `-m`: tipo de hash, siendo 1000 NTLM.

Para buscar el código correspondiente a un tipo de hash podemos filtrar de la siguiente forma:

```console
usr@hostname:~# hashcat -hh | grep -i ntlm
```

## Windows configuration files

Windows puede automatizar una tareas repetitivas, como el despliegue masivo o la instalación de Windows en muchos sistemas. Esto se hace mediante la utilidad Unattended Windows Setup. Usa archivos de configuración que contienen configuraciones específicas y credenciales de cuentas de usuario, concretamente la contraseña de la cuenta de Administrador.

Si los archivos de configuración de Unattended Windows Setup se dejan en el sistema objetivo después de la instalación, pueden revelar credenciales de cuentas de usuario que los atacantes pueden utilizar para autenticarse legítimamente en el sistema Windows objetivo.

Normalmente utilizará uno de los siguientes archivos de configuración, que contienen información sobre las cuentas de usuario y la configuración del sistema:

- C:\Windows\Panther\Unattend.xml
- C:\Windows\Panther\Autounattend.xml

Como medida de seguridad, las contraseñas almacenadas en el archivo de configuración de Unattended Windows Setup pueden estar codificadas en Base64.

[⟵ Anterior](08_persistencia.md) | [Siguiente ⟶](10_linuxhash.md)
