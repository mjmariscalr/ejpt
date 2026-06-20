# 5. Ataques basados en hosts

Los ataques basados en sistema o host son ataques dirigidos a un sistema o equipo específico que ejecuta un sistema operativo determinado, por ejemplo, Windows o Linux y suelen entrar en juego después de haber obtenido acceso a una red objetivo, momento en el que será necesario explotar servidores, estaciones de trabajo o portátiles dentro de la red interna.

Se centran principalmente en explotar vulnerabilidades inherentes del sistema operativo objetivo. A diferencia de los ataques basados en red, son mucho más especializados y requieren comprender el sistema operativo objetivo y las vulnerabilidades que lo afectan. Este tipo de ataques implican la explotación de configuraciones incorrectas y vulnerabilidades inherentes dentro del sistema operativo.

## 5.1 Explotación Windows

Windows es uno de los principales objetivos en pentesting por su amplia implantación en entornos corporativos y su papel central en redes empresariales. Su uso frecuente como sistema de usuario, servidor y controlador de dominio hace que concentre una gran superficie de ataque dentro de una misma infraestructura.

Si bien esta sección no explora todos los posibles vectores de ataque, ofrece una idea básica de como actuar en una explotación Windows. Principalmente podemos atacar: servicios que actúan en puertos abiertos y vulnerabilidades nativas de Windows que pueden estar presentes en equipos de usuario y no solo en servidores. Algunos de los vectores de ataque son:

- **Servicios**
	- [**Microsoft IIS FTP**](win/ftp.md)
	- [**Microsoft IIS WebDAV**](win/webdav.md)
	- [**OpenSSH**](win/ssh.md)
	- [**SMB**](win/smb.md)
	- [**MySQL**](win/mysql.md)
	- [**HTTP File Server (HFS)**](win/hfs.md)
	- [**Apache Tomcat**](win/tomcat.md)
- **Vulnerabilidades**
	- [**Explotación de la vulnerabilidad MS17-010 de SMB (EternalBlue)**](win/eternalblue.md)
	- [**Explotación de la vulnerabilidad CVE-2019-0708 de RDP (BlueKeep)**](win/rdp.md)
- **Protocolos**
	- [**Remote Desktop Protocol (RDP)**](win/rdp.md)
	- [**WinRM**](win/winrm.md)

## 5.2. Explotación Linux

Linux es uno de los sistemas operativos más habituales en entornos de servidor, infraestructuras cloud y dispositivos de red. Su estabilidad, flexibilidad y naturaleza abierta han favorecido su adopción en una gran variedad de servicios críticos, convirtiéndolo en un objetivo frecuente durante las fases de enumeración y explotación.

A diferencia de otros sistemas, gran parte de la superficie de ataque en Linux suele estar asociada a servicios expuestos a la red y aplicaciones de terceros instaladas sobre el sistema. Por este motivo, la identificación de versiones, configuraciones inseguras y vulnerabilidades conocidas adquiere una gran importancia.

Esta sección presenta algunos de los servicios, protocolos y vulnerabilidades más comunes que pueden encontrarse en sistemas Linux. El objetivo no es cubrir todos los escenarios posibles, sino proporcionar una base práctica sobre técnicas de explotación aplicables a entornos reales y laboratorios de aprendizaje.

- **Servicios**
- **Vulnerabilidades**
- **Protocolos**

[⟵ Anterior](04_payloads.md) | [Siguiente ⟶](06_redes.md)