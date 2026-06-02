# 1. Introducción a la explotación

La explotación consiste en las técnicas y herramientas utilizadas por los adversarios o los pentesters para obtener un acceso inicial a un sistema o red objetivo. El éxito de la explotación dependerá de la naturaleza y calidad de la recopilación de información y de la enumeración de servicios ya que solo podemos explotar un objetivo si sabemos que es vulnerable.

Según *PTES*, podemos definir las fases de un pentest según el siguiente diagrama:

```mermaid
flowchart TD
    A[Pre-engagement] --> A1[Definición del alcance]
    A --> A2[Reglas de compromiso]
    A --> A3[Objetivos]

    B[Recopilación de información] --> B1[OSINT]
    B --> B2[Reconocimiento activo]
    B --> B3[Enumeración de servicios]

    C[Modelado de amenazas] --> C1[Identificación de activos]
    C --> C2[Análisis de riesgos]
    C --> C3[Vectores de ataque]

    D[Análisis de vulnerabilidades] --> D1[Escaneo]
    D --> D2[Validación manual]
    D --> D3[Priorización]

    E[Explotación] --> E1[Obtención de acceso]
    E --> E2[Escalada de privilegios]
    E --> E3[Movimiento lateral]

    F[Post-explotación] --> F1[Persistencia]
    F --> F2[Impacto]
    F --> F3[Recolección de evidencias]

    G[Reporte] --> G1[Hallazgos]
    G --> G2[Recomendaciones]
    G --> G3[Presentación ejecutiva]

    A --> B --> C --> D --> E --> F --> G
``` 
