# Dumping y cracking de hashes

Windows almacena los hashes de las contraseñas de los usuarios de forma local en la base de datos **SAM (Security Accounts Manager)**. No se puede copiar mientras el sistema operativo está en ejecución, ya que el kernel de Windows lo mantiene bloqueado. Es necesario usar técnicas y herramientas en memoria para realizar dumping. En las versiones modernas de Windows, la base de datos SAM está cifrada mediante una syskey.

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

## Dumping de hashes NTLM

Podemos realizar dumping de los hashes de contraseñas de Windows mediante herramientas, como: `hashdump` de Meterpreter o `Mimikatz`.

[⟵ Anterior](08_persistencia.md) | [Siguiente ⟶]()
