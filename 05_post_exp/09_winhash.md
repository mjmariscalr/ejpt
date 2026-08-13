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

[⟵ Anterior](08_persistencia.md) | [Siguiente ⟶](10_linuxhash.md)
