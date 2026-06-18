# Remote Desktop Protocol (RDP)

El Protocolo de Escritorio Remoto (RDP) desarrollado por Microsoft permite acceso remoto con interfaz gráfica y se utiliza para conectarse e interactuar de forma remota con un sistema Windows. Utiliza por defecto el puerto TCP 3389, aunque puede configurarse en cualquier otro puerto TCP. 

La autenticación de RDP requiere una credenciales legítimas en el sistema objetivo. Se puede realizar un ataque de fuerza bruta contra RDP para identificar unas credenciales válidas que puedan utilizarse para obtener acceso remoto al objetivo.

## Enumeración

En este ejemplo podemos ver que no aparece el puerto 3389, sino el 3333. Aun así, no aparece información del servicio que está ejecutando:

```bash
nmap -sS -T4 -sV demo.ine.local
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-06-19 02:29 IST
Nmap scan report for demo.ine.local (10.2.16.76)
Host is up (0.0039s latency).
Not shown: 991 closed tcp ports (reset)
PORT      STATE SERVICE        VERSION
3333/tcp  open  ssl/dec-notes?
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows
```

Para comprobar si este RDP está corriendo sobre este puerto, podemos usar el módulo `rdp_scanner` de metasploit:

```bash
msf > use auxiliary/scanner/rdp/rdp_scanner
msf auxiliary(scanner/rdp/rdp_scanner) > set rport 3333
msf auxiliary(scanner/rdp/rdp_scanner) > set rhosts demo.ine.local
msf auxiliary(scanner/rdp/rdp_scanner) > run
[*] 10.2.16.76:3333       - Detected RDP on 10.2.16.76:3333       (name:WIN-OMCNBKR66MN) (domain:WIN-OMCNBKR66MN) (domain_fqdn:WIN-OMCNBKR66MN) (server_fqdn:WIN-OMCNBKR66MN) (os_version:6.3.9600) (Requires NLA: Yes)
[*] demo.ine.local:3333   - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

## Explotación

**Fuerza bruta con hydra**

```bash
hydra -L usr_file -P pass_file <HOST> rdp -s 3333
```

**Conexión con xfreerdp**

xfreerdp es un cliente de RDP de código abierto que forma parte del proyecto FreeRDP. Su función es permitir que un equipo Linux, macOS o incluso Windows se conecte a otro sistema mediante RDP, de forma similar a cómo funciona la aplicación Conexión a Escritorio Remoto de Microsoft.

Para conectarte a un servidor Windows:

```bash
xfreerdp /u:usuario /p:contraseña /v:192.168.1.10:3333
```

![xfreerdp](../../../img/xfreerdp.png)

[⟵ Anterior](../../05_sistema.md#explotación-windows)