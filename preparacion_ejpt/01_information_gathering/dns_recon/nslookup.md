# Nslookup

Comando similar a *host* usado principalmente en entornos Windows, aunque se encuentra disponible para Linux. El comando *host* es más moderno, por lo tanto suele ser la opción más usada.

## Guía de uso

```bash
$ nslookup google.com 
Server:         9.9.9.9
Address:        9.9.9.9#53

Non-authoritative answer:
Name:   google.com
Address: 142.250.200.131
Name:   google.com
Address: 2a00:1450:4003:80f::2003
```

### Búsqueda inversa

```bash
$ nslookup 142.250.200.131
131.200.250.142.in-addr.arpa    name = mad41s14-in-f3.1e100.net
```

### Consultar tipos de registro

Para especificar el tipo de registro usamos la opción *type*:

```bash
$ nslookup -type=MX google.com    
Server:         9.9.9.9
Address:        9.9.9.9#53

Non-authoritative answer:
google.com       mail exchanger = 0 smtp.google.com.
```
### Especificar servidor DNS

Para especificar el servidor dns que se usará para consultar:

```bash
$ nslookup google.com 8.8.8.8
Server:         8.8.8.8
Address:        8.8.8.8#53

Non-authoritative answer:
Name:   google.com
Address: 142.250.200.78
Name:   google.com
Address: 2a00:1450:4003:80d::200e
```

## Riesgo de detección

Los riesgos de detección usando nslookup son esencialmente los mismos que con el comando [host](host.md#riesgo-de-detección), ya que funcionan de forma similar.

[⟵ Anterior](../01_information_gathering.md#reconocimiento-dns)
