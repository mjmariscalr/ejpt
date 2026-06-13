# Microsoft IIS FTP

Microsoft IIS (Internet Information server) funciona principalmente como servidor web, pero para analizar un servicio FTP es necesario escanear tanto el puerto 21 como el 80. Esto se debe a que Microsoft FTP es un paquete de Microsoft IIS. Cuando, durante un escaneo con `nmap` u otra situación, vemos *Microsoft ftpd (demon)*, significa que el servidor FTP se usa para permitir a usuarios autenticados modificar, subir o descargar archivos del directorio del servidor web.

### Acceso anonimo

Esta es una configuracón de FTP que permite a cualquiera acceder al servidor sin proporcionar credenciales. Esto se puede comprobar de varias formas:

**Mediante un script `nmap`:**

```bash
nmap -p <puerto> --script ftp-anon <IP>
```

**Usando el cliente ftp:**

```bash
ftp <IP> <puerto>
Name: anonymous
Password: <pulsamos enter sin escribir>
```

### Fuerza bruta

**Metasploit:**

```bash
msf auxiliary(scanner/ftp/ftp_login) > set rhosts <IP>
msf auxiliary(scanner/ftp/ftp_login) > set user_file <WORDLIST>
msf auxiliary(scanner/ftp/ftp_login) > set pass_file <WORDLIST>
msf auxiliary(scanner/ftp/ftp_login) > run
```

**Hydra:**

```bash
hydra -L <USR_WORDLIST> -P <PASS_WORDLIST> <IP> <SERVICE>
```

```bash

```
