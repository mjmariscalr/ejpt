# dig

Permite consultas DNS más avanzadas y con mayor funcionalidad. Para las consultas con *dig* seguimos el formato: **dig @SERVER DOMAIN_NAME TYPE**.

## Guía de uso

De la misma forma que las demás opciones estudiadas, para hacer una consulta general basta con indicarle un dominio al comando dig.

```bash
$ dig preview.owasp-juice.shop    

; <<>> DiG 9.20.13 <<>> preview.owasp-juice.shop
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 65234
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;preview.owasp-juice.shop.      IN      A

;; ANSWER SECTION:
preview.owasp-juice.shop. 150   IN      A       81.169.145.156

;; Query time: 43 msec
;; SERVER: 9.9.9.9#53(9.9.9.9) (UDP)
;; WHEN: Tue Nov 18 16:55:54 CET 2025
;; MSG SIZE  rcvd: 69
```

### Consultar tipos de registro

Para consultar el tipo de registro basta con escribirlo al final de la consulta.

```bash
$ dig preview.owasp-juice.shop SOA

; <<>> DiG 9.20.13 <<>> preview.owasp-juice.shop SOA
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 6056
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;preview.owasp-juice.shop.      IN      SOA

;; AUTHORITY SECTION:
owasp-juice.shop.       150     IN      SOA     docks10.rzone.de. hostmaster.strato-rz.de. 2020051979 86400 7200 604800 300

;; Query time: 9 msec
;; SERVER: 9.9.9.9#53(9.9.9.9) (UDP)
;; WHEN: Tue Nov 18 17:03:16 CET 2025
;; MSG SIZE  rcvd: 126
```

### Especificar servidor DNS

El servidor DNS al que hacemos la consulta se puede indicar incluyendo su dirección IP o nombre de dominio con un @.

```bash
$ dig preview.owasp-juice.shop @1.1.1.1    

; <<>> DiG 9.20.13 <<>> preview.owasp-juice.shop @1.1.1.1
;; global options: +cmd

.
.
.

;; Query time: 16 msec
;; SERVER: 1.1.1.1#53(1.1.1.1) (UDP)
;; WHEN: Tue Nov 18 17:10:21 CET 2025
;; MSG SIZE  rcvd: 69

```

### Búsqueda inversa

Para la búsqueda inversa indicamos la opción -x.

```bash
$ dig -x 81.169.145.156                

; <<>> DiG 9.20.13 <<>> -x 81.169.145.156
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 63338
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;156.145.169.81.in-addr.arpa.   IN      PTR

;; ANSWER SECTION:
156.145.169.81.in-addr.arpa. 43200 IN   PTR     w9c.rzone.de.

;; Query time: 106 msec
;; SERVER: 9.9.9.9#53(9.9.9.9) (UDP)
;; WHEN: Tue Nov 18 17:12:00 CET 2025
;; MSG SIZE  rcvd: 82
```


## Riesgo de detección

Los riesgos de detección usando dig son esencialmente los mismos que con el comando [host](host.md#riesgo-de-detección), ya que funcionan de forma similar.

## Recursos

[Footprinting usando DIG](https://noticiasseguridad.com/tutoriales/18034/)

[⟵ Anterior](../01_information_gathering.md#reconocimiento-dns)
