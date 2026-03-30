# Enumeración *DNS*

DNS (Domain Name System) es el sistema que traduce los nombres de dominio (google.com) en direcciones IP. 

## Transferencias de zona DNS

La técnica que usamos para obtener las transferencias de zona se conoce como *interrogación DNS*. Es el proceso de enumerar los registros DNS de un dominio. Esta es la misma técnica que usan herramientas como *DNSDumpster*, el problema con ellas es que no permiten acceder a mucha información más allá de registros MX.

Las transferencias de zona se utilizan en DNS para replicar la información de una zona desde un servidor DNS maestro a uno secundario. El servidor maestro autoriza a los secundarios, el secundario solicita la zona y se copia la base de datos completa o parcial de registros DNS.

Si una transferencia de zona está mal configurada, un atacante puede obtener:

- Todos los nombres de host
- Direcciones IP internas
- Subdominios.
- Direcciones de correo.

LA transferencia de zona en un pentesting consiste en solicitar a un servidor DNS que envíe todos los registros de una zona. Se envía una consulta DNS de tipo AXFR al servidor. El servidor DNS: acepta la solicitud si el cliente está autorizado y rechaza la solicitud si no lo está. Si es aceptada, el servidor responde con todos los registros de la zona (A, AAAA, MX, NS, TXT, etc.).

Cuando esto se hace desde un cliente no autorizado (kali), los posibles resultados son:

- **Servidor bien configurado:** responde con REFUSED o Transfer failed y no se obtiene información
- **Servidor mal configurado:** acepta la solicitud y devuelve todos los registros DNS de la zona

Herramientas: dnsenum, dig, dnsrecon, fierce.

### Transferencia de zona con dig

```bash
$ dig axfr @nsztm2.digi.ninja zonetransfer.me
```

### Transferencia de zona con dnsenum

```bash
$ dnsenum zonetransfer.me
```

### Transferencia de zona con dnsrecon

```bash
$ dnsrecon -d zonetransfer.me -t axfr
```

### Transferencia de zona con fierce

```bash
$ fierce -dns zonetransfer.me
```
























Módulos de MSF para enumeración DNS

En Metasploit Framework, estos módulos se usan mucho para recopilar información DNS:

1️⃣ auxiliary/gather/enum_dns

Enumeración básica de DNS.

Qué hace

Bruteforce de subdominios

Consultas DNS

Descubre hosts asociados al dominio

Ejemplo

use auxiliary/gather/enum_dns
set DOMAIN example.com
set RHOSTS 8.8.8.8
run
2️⃣ auxiliary/gather/dns_enum

Similar a la herramienta dnsenum.

Funciones

Subdomain brute force

MX records

NS records

Transferencias de zona

Ejemplo

use auxiliary/gather/dns_enum
set DOMAIN example.com
run
3️⃣ auxiliary/scanner/dns/dns_amp

Detecta servidores DNS vulnerables a amplificación.

Uso

use auxiliary/scanner/dns/dns_amp
set RHOSTS target_dns
run
4️⃣ auxiliary/scanner/dns/dns_bruteforce

Bruteforce de subdominios.

Ejemplo

use auxiliary/scanner/dns/dns_bruteforce
set DOMAIN example.com
set WORDLIST /usr/share/wordlists/subdomains.txt
run
🔎 Scripts NSE de Nmap para enumeración DNS

Nmap usa NSE (Nmap Scripting Engine) para automatizar enumeración DNS.

1️⃣ dns-brute

Bruteforce de subdominios.

nmap --script dns-brute example.com

Opciones:

nmap --script dns-brute --script-args dns-brute.domain=example.com
2️⃣ dns-zone-transfer

Comprueba si el DNS permite AXFR (zone transfer).

nmap --script dns-zone-transfer -p 53 ns1.example.com
3️⃣ dns-recursion

Detecta servidores DNS que permiten recursión abierta.

nmap --script dns-recursion -p 53 target
4️⃣ dns-cache-snoop

Detecta entradas cacheadas en el servidor DNS.

nmap --script dns-cache-snoop -p 53 target
5️⃣ dns-service-discovery

Descubre servicios asociados a DNS.

nmap --script dns-service-discovery target
🧰 Comando completo de enumeración DNS con Nmap

Muy usado en reconocimiento inicial:

nmap -p53 --script dns-brute,dns-zone-transfer,dns-recursion target




