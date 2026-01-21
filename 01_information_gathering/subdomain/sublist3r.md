# Sublist3r

Sublist3r es una herramienta de enumeración de subdominios asociados a un dominio principal utilizando técnicas de recolección de información públicas y no intrusivas. Hay que destacar que incluye un método de fuerza bruta que no veremos en esta sección porque implica recolección activa. Ayuda a identificar:

- Subdominios expuestos.
- Infraestructura adicional de una empresa.
- Servicios olvidados o no documentados.
- Posibles vectores de ataque (solo en contextos autorizados).

Sublist3r no ataca, solo consulta fuentes públicas: motores de búsqueda como Google, Yahoo, Bing, Baidu y Ask, y otras herramientas como Netcraft, Virustotal, ThreatCrowd, DNSdumpster y ReverseDNS.

## Guía de uso

| Short Form | Long Form      | Description                                         |
|------------|----------------|-----------------------------------------------------|
| -d         | --domain       | Domain name to enumerate subdomains of              |
| -b         | --bruteforce   | Enable the subbrute bruteforce module               |
| -p         | --ports        | Scan the found subdomains against specific ports    |
| -v         | --verbose      | Enable verbose mode and display results in realtime |
| -t         | --threads      | Number of threads to use for subbrute bruteforce    |
| -e         | --engines      | Specify a comma-separated list of search engines    |
| -o         | --output       | Save the results to a text file                     |
| -h         | --help         | Show the help message and exit                      |

Para usar todos los buscadores no es necesario especificar ninguno:

```bash
$ sublist3r -d wordreference.com
.
.
.
[-] Enumerating subdomains now for wordreference.com
[-] Searching now in Baidu..
[-] Searching now in Yahoo..
[-] Searching now in Google..
[-] Searching now in Bing..
[-] Searching now in Ask..
[-] Searching now in Netcraft..
.
.
.
[!] Error: Virustotal probably now is blocking our requests
[-] Total Unique Subdomains Found: 22
www.wordreference.com
ac.wordreference.com
cdn77f.wordreference.com
daily.wordreference.com
```
En la prueba anterior vemos como Virustotal bloquea las peticiones. Esto también puede pasar con google y puede deberse a que algunos de estos buscadores limitan el numero depeticiones que reciben y las bloquean con captcha.

En caso de querer usar solo algunos de los buscadores usamos la opción *-e* de la siguiente forma:

```bash
$ sublist3r -d wordreference.com -e google,yahoo

                 ____        _     _ _     _   _____
                / ___| _   _| |__ | (_)___| |_|___ / _ __
                \___ \| | | | '_ \| | / __| __| |_ \| '__|
                 ___) | |_| | |_) | | \__ \ |_ ___) | |
                |____/ \__,_|_.__/|_|_|___/\__|____/|_|

                # Coded By Ahmed Aboul-Ela - @aboul3la
    
[-] Enumerating subdomains now for wordreference.com
[-] Searching now in Google..
[-] Searching now in Yahoo..
```

## Recursos

- [Sublist3r](https://github.com/aboul3la/Sublist3r)
- [Sublist3r: Herramienta para enumerar Subdominios](https://esgeeks.com/sublist3r-herramienta-enumeracion-subdominios/)

[⟵ Anterior](../02_pasiva.md#enumeración-de-subdominios)
