# Exploiting Bash CVE-2014-6271 Vulnerability (Shellshock)

**Shellshock (CVE-2014-6271)** es el nombre que recibe una familia de vulnerabilidades en Bash (desde la versión 1.3) que permiten ejecutar comandos arbitrarios de forma remota a través de Bash y permite obtener acceso remoto al sistema objetivo mediante una shell inversa. Bash es un intérprete de comandos de sistemas \*Nix que forma parte del proyecto GNU Project y es la shell predeterminada en la mayoría de las distribuciones de Linux.

Shellshock se debe a un fallo en Bash, por el cual ejecuta erróneamente comandos que aparecen después de una secuencia de caracteres como `() { :; };`. Solo afecta a sistemas Linux y otros sistemas tipo Unix, ya que Windows no utiliza Bash de forma nativa.

Los servidores web Apache HTTP Server configurados para ejecutar scripts CGI o archivos .sh también son vulnerables a este ataque. Los scripts CGI (Common Gateway Interface) son utilizados por Apache para ejecutar comandos en el sistema Linux; posteriormente, la salida de esos comandos se muestra al cliente que realizó la solicitud. Un atacante puede aprovechar Shellshock para inyectar comandos maliciosos a través de variables de entorno que son procesadas por Bash, logrando así la ejecución remota de código en sistemas vulnerables.

## Enumeración

Para comprobar si un sistema es vulnerable, podemos usar nmap:

```bash
nmap --scirpt http-shellshock --scritp-args "http-shellshock.uri=/ruta/scirpt.cgi" <IP>
```





[⟵ Anterior](../05_sistema.md#explotación-linux)
