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



### Metasploit:

[⟵ Anterior](../../05_sistema.md#explotación-windows)