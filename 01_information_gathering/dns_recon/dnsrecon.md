# DNSRecon

Es un script de python que proporciona la hablidad de:

- Descubrir subdominios y registros ocultos (A, AAAA, CNAME).
- Identificar transferencias de zona (AXFR) mal configuradas (hallazgo de alta criticidad).
- Mapear la infraestructura de red (MX, NS, SOA, TXT).
- Detectar configuraciones inseguras o mal implementadas (DNSSEC, TXT/SPF/DMARC).

## Guía de uso

```bash
$ dnsrecon -d preview.owasp-juice.shop
[*] std: Performing General Enumeration against: preview.owasp-juice.shop...
[-] DNSSEC is not configured for preview.owasp-juice.shop
[*]      MX smtpin.rzone.de 81.169.145.97
[*]      MX smtpin.rzone.de 2a01:238:20a:202:50f0::1097
[*]      A preview.owasp-juice.shop 81.169.145.156
[*]      AAAA preview.owasp-juice.shop 2a01:238:20a:202:1156::
[*]      TXT _domainkey.preview.owasp-juice.shop o=~; t=y; r=dkim@rzone.de
[*] Enumerating SRV Records
[-] No SRV Records Found for preview.owasp-juice.shop
```

### Tipo de enumeración

- std: SOA, NS, A, AAAA, MX and SRV.
- rvl: búsqueda inversa para un CIDR o rango IP.
- brt: fuerza bruta usando un diccionario (activa).
- srv: Registros SRV.
- axfr: Prueba todos los servidores NS para una transferencia de zona (activa).
- bing: Realiza una búsqueda en Bing de subdominios y hosts.
- yand: Realiza una búsqueda en Yandex de subdominios y hosts.
- crt: Realiza una búsqueda en crt.sh de subdominios y hosts.
- snoop: Realiza cache snooping contra todos los servidores NS de un dominio dado, probando todos con un archivo que contiene los dominios, archivo proporcionado con la opción -D.

```bash
$ dnsrecon -d preview.owasp-juice.shop -t brt
[*] No dictionary file has been specified.
[*] Using the dictionary file: /usr/share/dnsrecon/dnsrecon/data/namelist.txt (provided by tool)
[*] brt: Performing host and subdomain brute force against preview.owasp-juice.shop...
[+] 0 Records Found

```

## Recursos

- [Realizar una Enumeración Estándar utilizando dnsrecon](https://www.youtube.com/watch?v=yDHvlXTn-gQ)
- [Realizar una Enumeración por Fuerza Bruta utilizando dnsrecon](https://www.youtube.com/watch?v=3GWBGFIYreo)

[⟵ Anterior](../02_pasiva.md#reconocimiento-dns)
