# Microsoft IIS FTP

Microsoft IIS (Internet Information server) funciona principalmente como servidor web, pero para analizar un servicio FTP es necesario escanear tanto el puerto 21 como el 80. Esto se debe a que Microsoft FTP es un paquete de Microsoft IIS. Cuando, durante un escaneo con `nmap` u otra situación, vemos *Microsoft ftpd (demon)*, significa que el servidor FTP se usa para permitir a usuarios autenticados modificar, subir o descargar archivos del directorio del servidor web.

