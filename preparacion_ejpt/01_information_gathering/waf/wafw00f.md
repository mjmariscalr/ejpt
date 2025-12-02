# WAFW00F

Wafw00f es una herramienta de reconocimiento usada para identificar si un sitio web está protegido por un WAF y, en caso afirmativo, determinar qué WAF es. Wafw00f realiza solicitudes web y analiza la respuesta buscando patrones característicos como:

- Encabezados HTTP típicos de algunos WAF
- Códigos de estado inusuales
- Comportamientos específicos ante solicitudes modificadas.

Con esos datos compara contra una base de firmas para identificar la tecnología, por ejemplo: Cloudflare.

Durante la fase de reconocimiento es importante detectar si una web está protegida por un WAF ya que no podemos detectar la IP real del servidor que la aloja. Esta detección nos puede ayudar a realizar una mejor estrategia para los siguientes pasos.

## Guía de uso

Para ver los WAF que soporta usamos:

```bash
$ wafw00f -l         

                ______
               /      \
              (  W00f! )
               \  ____/
               ,,    __            404 Hack Not Found
           |`-.__   / /                      __     __
           /"  _/  /_/                       \ \   / /
          *===*    /                          \ \_/ /  405 Not Allowed
         /     )__//                           \   /
    /|  /     /---`                        403 Forbidden
    \\/`   \ |                                 / _ \
    `\    /_\\_              502 Bad Gateway  / / \ \  500 Internal Error
      `_____``-`                             /_/   \_\\

                        ~ WAFW00F : v2.3.2 ~
        The Web Application Firewall Fingerprinting Toolkit
    
[+] Can test for these WAFs:

  WAF Name                        Manufacturer
  --------                        ------------

  360PanYun                        360 Technologies                 
  360WangZhanBao                   360 Technologies                 
  ACE XML Gateway                  Cisco                            
  ASP.NET Generic                  Microsoft                        
  ASPA Firewall                    ASPA Engineering Co.             
  AWS Elastic Load Balancer        Amazon                           
  AireeCDN                         Airee                            
  Airlock                          Phion/Ergon                      
  Alert Logic                      Alert Logic                      
  AliYunDun                        Alibaba Cloud Computing          
  AnYu                             AnYu Technologies                
  Anquanbao                        Anquanbao
  .
  .
  .
```

Para usar esta herramienta simplemente le indicamos el nombre de dominio:

```bash
$ wafw00f preview.owasp-juice.shop                     

                   ______
                  /      \
                 (  Woof! )
                  \  ____/                      )
                  ,,                           ) (_
             .-. -    _______                 ( |__|
            ()``; |==|_______)                .)|__|
            / ('        /|\                  (  |__|
        (  /  )        / | \                  . |__|
         \(_)_))      /  |  \                   |__|

                    ~ WAFW00F : v2.3.2 ~
    The Web Application Firewall Fingerprinting Toolkit
    
[*] Checking https://preview.owasp-juice.shop
[+] Generic Detection results:
[-] No WAF detected by the generic detection
[~] Number of requests: 7
```

## Riesgo de detección

En general, wafw00f lanza peticiones HTTP normales y analiza la respuesta para detectar el posible WAF. En caso de que esto no funcione, lanza una pequeña cantidad de peticiones HTTP potencialmente maliciosas. Este funcionamiento simula trafico bastante normal. Si bien es posible levantar sospechas en los sistemas de detección, el riesgo es bastante bajo.

## Recursos

- [WAFW00F](https://github.com/EnableSecurity/wafw00f)
- [Identification of Web Application Firewall using WAFW00F in Kali Linux](https://www.geeksforgeeks.org/linux-unix/identification-of-web-application-firewall-using-wafw00f-in-kali-linux/)
