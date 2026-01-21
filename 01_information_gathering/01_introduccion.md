# 1. Introducción a la recolección de información

Es la primera fase de un pentesting e implica obtener información de cualquier individuo, compañía, web o sistema objetivo. La información que se obtenga en esta fase es esencial, ya que cuanta más información tengamos, más fácil será tener éxito en las fases finales de la intrusión.

Las fases de un pentesting son: recolección de información, análisis de vulnerabilidades, explotación, post-explotación y elaboración de informes. Dependiendo del contexto, se puede incluir una fase previa de planificación en la que se establecen los objetivos, el alcance y las cuestiones legales. 

## 1.1. Objetivos

El objetivo principal de la fase de Information Gathering en un pentest es recopilar la mayor cantidad de información posible sobre el sistema evaluado para preparar y orientar las fases posteriores. A partir de este, podemos establecer tres objetivos secundarios:

- Comprender al objetivo, es decir, conocer cuanta información puede obtener un atacante externo.
- Identificar posibles vectores de ataque. Si estamos realizando un pentest, CTF o resolviendo un laboratorio, es posible que tengamos cierta información previa, pero es recomendable realizar esta fase para obtener información nueva.
- Reducir la incertidumbre antes de atacar, validando la información necesaria antes de ejecutar pruebas más invasivas y reduciendo así errores, falsos supuestos o pasos innecesarios.


## 1.2. Tipos de reconocimiento

Esta fase se divide habitualmente en dos tipos: 

- Recolección pasiva. En esta fase no se interactúa con el objetivo y generalmente se obtiene la información de fuentes públicas. Puede llevarse a cabo mediante buscadores, redes sociales, consultas a DNS públicos, herramientas OSINT, etc.
- Recolección activa. Obtiene la información directamente del objetivo y es fácilmente detectable.

## 1.3. Consideraciones éticas y legales

### Consideraciones éticas

Existen dos aspectos a señalar en cuanto a las consideraciones éticas de un pentest. En primer lugar, es fundamental minimizar los riesgos, evitando dañar los sistemas y asegurarse de no afectar la disponibilidad de los servicios. En segundo lugar, es altamente recomendable registrar todas las actividades y documentar las fuentes y métodos para que el pentest sea auditable y demostrar que no se han cometido abusos.

Por otro lado es importante mantener una buena ética profesional, actuando con la intención de mejorar la seguridad y evitar acciones que puedan beneficiar al auditor.

### Consideraciones legales

En cuanto a las consideraciones legales de la recolección de información, es importante tener en cuenta cómo se obtiene la información, qué tipo de información estamos tratando y el uso que se le va a dar. En cualquier caso es importante obtener un consentimiento explícito y no recolectar información sin autorización. Aunque la información sea pública, conviene documentar fuentes y limitar la recolección a lo necesario para evitar conflictos legales.

Hay que destacar que incluso teniendo consentimiento explícito y por escrito, es necesario cumplir con el Reglamento General de Protección de Datos si tratamos con datos sensibles y no se justifican ciertas pruebas como ataques a terceros.





[⟵ Anterior](../README.md) | [Siguiente ⟶](02_pasiva.md)

