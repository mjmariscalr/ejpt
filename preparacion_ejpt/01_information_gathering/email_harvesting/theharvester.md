# theHarvester

theHarvesteres una herramienta para recopilar subdominios, direcciones de correo electrónico, hosts virtuales, puertos abiertos y nombres de empleados de distintas fuentes. Para ello, consulta servicios públicos como motores de búsqueda. Dependiendo de la configuración y APIs disponibles, puede consultar:

- Motores de búsqueda (Bing, Yahoo, DuckDuckGo, Baidu).
- Servidores PGP.
- Shodan.
- Censys.
- Certificados (CT logs).
- Redes sociales o sitios especializados.

Si bien esta herramienta se puede usar para obtener distintos tipos de información, en esta parte de la guía nos vamos a centrar en el correo electrónico.

## Guía de uso

El uso es sencillo y aunque hay varias opciones para este comando, las más importantes son *-d* para indicar el dominio y *-b* para indicar las fuentes que se van a usar. Algunas de ellas, como por ejemplo *dnsdumpster*, necesitan una *API key* para poder usarlos.

```bash
$ theHarvester -d zonetransfer.me -b duckduckgo,bing,yahoo,urlscan,hunter,netlas,crtsh,criminalip
Read proxies.yaml from /etc/theHarvester/proxies.yaml
*******************************************************************
*  _   _                                            _             *
* | |_| |__   ___    /\  /\__ _ _ ____   _____  ___| |_ ___ _ __  *
* | __|  _ \ / _ \  / /_/ / _` | '__\ \ / / _ \/ __| __/ _ \ '__| *
* | |_| | | |  __/ / __  / (_| | |   \ V /  __/\__ \ ||  __/ |    *
*  \__|_| |_|\___| \/ /_/ \__,_|_|    \_/ \___||___/\__\___|_|    *
*                                                                 *
* theHarvester 4.8.2                                              *
* Coded by Christian Martorella                                   *
* Edge-Security Research                                          *
* cmartorella@edge-security.com                                   *
*                                                                 *
*******************************************************************

[*] Target: zonetransfer.me 

Read api-keys.yaml from /etc/theHarvester/api-keys.yaml
Read api-keys.yaml from /etc/theHarvester/api-keys.yaml

[!] Missing API key for criminalip. 
Read api-keys.yaml from /etc/theHarvester/api-keys.yaml

[!] Missing API key for Hunter. 
Read api-keys.yaml from /etc/theHarvester/api-keys.yaml

[!] Missing API key for netlas. 
[*] Searching Duckduckgo. 
        Searching 0 results.
[*] Searching Bing. 
[*] Searching CRTsh. 
[*] Searching Urlscan. 
[*] Searching Yahoo. 

[*] ASNS found: 2
--------------------
AS16276
AS16509

[*] Interesting Urls found: 14
--------------------
http://alltcpportsopen.firewall.test.zonetransfer.me/
http://asfdbbox.zonetransfer.me/
http://canberra-office.zonetransfer.me/
http://dc-office.zonetransfer.me/
http://deadbeef.zonetransfer.me/
http://email.zonetransfer.me/
http://home.zonetransfer.me/
http://intns1.zonetransfer.me/
http://intns2.zonetransfer.me/
http://ipv6actnow.org.zonetransfer.me/
http://office.zonetransfer.me/
http://owa.zonetransfer.me/
http://staging.zonetransfer.me/
http://vpn.zonetransfer.me/

[*] IPs found: 8
-------------------
2600:9000:206f:4200:7:60:4d00:93a1
2600:9000:206f:ba00:7:60:4d00:93a1
2600:9000:206f:c200:7:60:4d00:93a1
2600:9000:266e:1a00:7:60:4d00:93a1
2600:9000:277c:aa00:7:60:4d00:93a1
5.196.105.14
54.192.51.114
54.192.51.120

[*] Emails found: 2
----------------------
customer-service@zonetransfer.me
pippa@zonetransfer.me

[*] No people found.

[*] Hosts found: 22
---------------------
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me
alltcpportsopen.firewall.test.zonetransfer.me
asfdbbox.zonetransfer.me
canberra-office.zonetransfer.me
contact.zonetransfer.me
dc-office.zonetransfer.me
deadbeef.zonetransfer.me
email.zonetransfer.me
home.zonetransfer.me
intns1.zonetransfer.me
intns2.zonetransfer.me
ipv6actnow.org.zonetransfer.me
office.zonetransfer.me
owa.zonetransfer.me
robin.zonetransfer.me
robinwood.zonetransfer.me
rp.zonetransfer.me
sip.zonetransfer.me
sqli.zonetransfer.me
staging.zonetransfer.me
tcp.zonetransfer.me
vpn.zonetransfer.me
```

## Recursos

[theHarvester](https://github.com/laramies/theHarvester)
[The Harvester](https://www.osintux.org/documentacion/the-harvester)

[⟵ Anterior](../01_information_gathering/.md#)
