# 3.2. Transferencias de zona DNS

La técnica que usamos para obtener las zonas de transferencia se conoce como *interrogación DNS*. Es el proceso de enumerar los registros DNS de un dominio. Esta es la misma técnica que usan herramientas como *DNSDumpster*, el problema con ellas es que no permiten acceder a mucha información más allá de registros MX.

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