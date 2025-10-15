# Recolección de información

Es la primera fase de un pentesting e implica obtener información de cualquier individuo, compañía, web o sistema objetivo. La información que se obtenga en esta fase es esencial, ya que cuanta más información tengamos, más fácil será tener éxito en las fases finales de la intrusión. Esta fase se divide habitualmente en dos tipos: recolección pasiva y recolección activa.

## Recolección de información pasiva

Consiste en obtener la información sin interactuar con el objetivo, normalmente haciendo uso de recursos accesibles de forma pública. Algunas de las principales técnicas son:

- Motores de búsqueda para buscar información sobre empresas, personas o tecnologías.
- Analizar perfiles en redes sociales.
- Búsqueda de dominios para obtener información sobre la propiedad de un dominio o IP. Se pueden usar herramientas como *[Whois](https://who.is)*
- Herramientas especializadascomo *[Shodan](https://www.shodan.ioth 	)*, que indexa dispositivos conectados a Internet, o *The Harvester*, que busca información en varias fuentes públicas. Esta última se encuentra disponible en distribuciones como *Kali* o *Parrot*, y tambien se puede descargar de su [repositorio en github](https://github.com/laramies/theHarvester)

La información más importante en este caso es:

- Direcciones IP e información DNS.
- Nombres de dominio y su propiedad.
- Direcciones de correo y perfiles en redes sociales.
- Tecnologías web.
- Subdominios.

### Reconocimiento web y footprinting



## Recolección de información activa

En este caso sí debemos interactuar directamente con el objetivo, por ejemplo realizando un escaneo de puertos con el que podremos buscar vulnerabilidades asociadas a los servicios activos en el sistema. La información más importante en este caso es:

- Puertos abiertos.
- Estructura interna de la red.
- Enumeración de la información.

[⟵ Anterior](README.md) <!-- | [Siguiente ⟶](pagina-siguiente.md) -->

