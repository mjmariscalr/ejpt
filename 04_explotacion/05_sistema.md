# 5. Ataques basados en hosts

Los ataques basados en sistema o host son ataques dirigidos a un sistema o equipo específico que ejecuta un sistema operativo determinado, por ejemplo, Windows o Linux y suelen entrar en juego después de haber obtenido acceso a una red objetivo, momento en el que será necesario explotar servidores, estaciones de trabajo o portátiles dentro de la red interna.

Se centran principalmente en explotar vulnerabilidades inherentes del sistema operativo objetivo. A diferencia de los ataques basados en red, son mucho más especializados y requieren comprender el sistema operativo objetivo y las vulnerabilidades que lo afectan. Este tipo de ataques implican la explotación de configuraciones incorrectas y vulnerabilidades inherentes dentro del sistema operativo.

## 5.1 Explotación Windows

Windows es uno de los principales objetivos en pentesting por su amplia implantación en entornos corporativos y su papel central en redes empresariales. Su uso frecuente como sistema de usuario, servidor y controlador de dominio hace que concentre una gran superficie de ataque dentro de una misma infraestructura.

Si bien esta sección no explora todos los posibles vectores de ataque, ofrece una idea básica de como actuar en una explotación Windows. Principalmente podemos atacar: servicios que actúan en puertos abiertos y vulnerabilidades nativas de Windows que pueden estar presentes en equipos de usuario y no solo en servidores. Algunos de los vectores de ataque son:

- **Servicios**
	- [**Microsoft IIS FTP**](win/serv/ftp.md)
	- [**Microsoft IIS WebDAV**](win/vulns/webdav.md)
	- [**OpenSSH**](win/serv/ssh.md)
	- [**SMB**](win/serv/smb.md)
	- [**MySQL**](win/serv/mysql.md)
	- [**HTTP File Server (HFS)**](win/serv/hfs.md)
- **Vulnerabilidades**
	- [**Explotación de la vulnerabilidad MS17-010 de SMB (EternalBlue)**](win/vulns/eternalblue.md)
	- [**Explotación de la vulnerabilidad
	- **CVE-2019-0708 de RDP (BlueKeep)**](win/vulns/rdp.md)
- **Protocolos**
	- [**Remote Desktop Protocol (RDP)**](win/vulns/rdp.md)
	- [**WinRM**](win/vulns/winrm.md)


## 5.2. Explotación Linux

[⟵ Anterior](04_payloads.md) | [Siguiente ⟶](06_redes.md)