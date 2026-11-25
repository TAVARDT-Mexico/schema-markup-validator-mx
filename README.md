# Validador de Marcado Schema y Datos Estructurados

[![Validador Schema](https://img.shields.io/badge/TAVARDT-Schema.org%20Validator-5b3cf5.svg)](https://tools.tavardt.com/mx/validador-schema-markup/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Motor técnico de validación de datos estructurados para Schema.org en formatos JSON-LD y Microdatos, comprobando relaciones de entidades, campos requeridos y elegibilidad para fragmentos enriquecidos.

La solución extrae estructuras semánticas de entornos web productivos para detectar fallos de sintaxis en JSON, esquemas incompletos y omisión de propiedades exigidas por buscadores e interfaces de IA. Diseñada sin dependencias externas, se ejecuta de forma nativa en consola Python y flujos de CI/CD.

***

## Capacidades de Validación Semántica y Modelos Admitidos

Los motores de búsqueda y los sistemas de recuperación neuronal dependen de datos estructurados explícitos para desambiguar organizaciones, verificar autorías y habilitar rich snippets. Los errores de sintaxis o la falta de propiedades clave conllevan la descalificación inmediata de estos formatos.

* **Extracción Automatizada de Datos Estructurados**: Análisis sistemático de bloques JSON-LD (`application/ld+json`) y detección de marcado heredado en Microdatos (`itemscope`, `itemtype`).
* **Soporte para Grafos Semánticos**: Procesamiento transparente de jerarquías basadas en `@graph`, resolviendo nodos de entidades entrelazadas en aplicaciones web modernas.
* **Inspección de Entidades Schema.org**: Verificación de atributos requeridos en los modelos más utilizados: Organization, LocalBusiness, WebSite, Article, NewsArticle, BlogPosting, Product, BreadcrumbList y FAQPage.
* **Control de Sintaxis e Integridad**: Penalización dedutiva ante JSON malformado (-20 puntos), ausencia de JSON-LD (-40 puntos), falta de entidades (-30 puntos) y omisión de propiedades recomendadas (-5 puntos cada una).
* **Puntuación Cuantitativa de Calidad Semántica**: Calificación objetiva de 0 a 100 puntos que categoriza la salud técnica en OPTIMAL, WARNING o CRITICAL con indicaciones prioritarias.

***

## Matriz de Inspección de Entidades Semánticas

<table>
  <thead>
    <tr>
      <th align="left">Clase de Entidad</th>
      <th align="left">Propiedades Obligatorias</th>
      <th align="left">Regla de Deducción</th>
      <th align="left">Impacto en Rich Results y Recuperación</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Organization</strong></td>
      <td><code>name</code>, <code>url</code></td>
      <td>-5 Puntos por propiedad ausente</td>
      <td>Identifica la entidad corporativa y establece la autoridad de la marca en grafos semánticos.</td>
    </tr>
    <tr>
      <td><strong>LocalBusiness</strong></td>
      <td><code>name</code>, <code>address</code></td>
      <td>-5 Puntos por propiedad ausente</td>
      <td>Fundamental para posicionamiento local, presencia en mapas y consultas de proximidad.</td>
    </tr>
    <tr>
      <td><strong>WebSite</strong></td>
      <td><code>name</code>, <code>url</code></td>
      <td>-5 Puntos por propiedad ausente</td>
      <td>Habilita la caja de búsqueda de sitelinks y delimita el entorno del dominio principal.</td>
    </tr>
    <tr>
      <td><strong>Article / NewsArticle</strong></td>
      <td><code>headline</code>, <code>author</code></td>
      <td>-5 Puntos por propiedad ausente</td>
      <td>Habilita la inclusión en Noticias Destacadas y referencia formal de fuentes en LLMs.</td>
    </tr>
    <tr>
      <td><strong>BlogPosting</strong></td>
      <td><code>headline</code>, <code>author</code></td>
      <td>-5 Puntos por propiedad ausente</td>
      <td>Registra autoría y cronología de publicación en artículos editoriales.</td>
    </tr>
    <tr>
      <td><strong>Product</strong></td>
      <td><code>name</code>, <code>offers</code></td>
      <td>-5 Puntos por propiedad ausente</td>
      <td>Activa la visualización de precios, disponibilidad de catálogo y opiniones de compra.</td>
    </tr>
    <tr>
      <td><strong>BreadcrumbList</strong></td>
      <td><code>itemListElement</code></td>
      <td>-5 Puntos por propiedad ausente</td>
      <td>Sustituye URLs complejas por rutas de migas de pan legibles en las páginas de resultados.</td>
    </tr>
    <tr>
      <td><strong>FAQPage</strong></td>
      <td><code>mainEntity</code></td>
      <td>-5 Puntos por propiedad ausente</td>
      <td>Muestra módulos interactivos de preguntas frecuentes directamente bajo el resultado orgánico.</td>
    </tr>
  </tbody>
</table>

***

## Instalación y Guía Rápida de Uso

Requiere Python 3.8 o superior utilizando únicamente librerías estándar (`sys`, `re`, `json`, `ssl`, `argparse`, `urllib.request`, `urllib.error`). No requiere instalación de módulos mediante pip.

Clone el repositorio y ejecute la validación sobre un dominio objetivo:

```bash
git clone https://github.com/TAVARDT-Mexico/schema-markup-validator-mx.git
cd schema-markup-validator-mx
python schema_validator.py ejemplo.mx
```

Para generar la salida estructurada en formato JSON para flujos automatizados:

```bash
python schema_validator.py ejemplo.mx -j
```

Inspeccione dominios configurando un tiempo de espera personalizado:

```bash
python schema_validator.py https://ejemplo.mx -j -t 12
```

Ejemplo de payload JSON devuelto:

```json
{
  "target": "https://ejemplo.mx",
  "score": 95,
  "status": "OPTIMAL",
  "json_ld_blocks": 1,
  "entities": [
    "Organization",
    "WebSite"
  ],
  "microdata_detected": false,
  "issues": [
    "Entity 'Organization' is missing recommended property: url"
  ],
  "recommendations": [
    "Add missing property 'url' to 'Organization' entity."
  ]
}
```

Escala de clasificación de calidad:

* **Puntuación 85 a 100 (OPTIMAL)**: Marcado estructurado de alta calidad con cobertura completa de entidades y sin errores de sintaxis.
* **Puntuación 65 a 84 (WARNING)**: Omisiones menores de propiedades o cobertura incompleta que pueden limitar los fragmentos enriquecidos.
* **Puntuación 0 a 64 (CRITICAL)**: Sintaxis JSON rota, ausencia de datos estructurados o esquemas dañados que exigen corrección inmediata.

***

## Preguntas Frecuentes (FAQ Técnico)

### ¿Por qué se prefiere el formato JSON-LD frente a los Microdatos para fragmentos enriquecidos e IA?
JSON-LD mantiene los datos semánticos en scripts independientes, permitiendo su actualización sin alterar el diseño visual en HTML. Además, los motores de búsqueda líderes y rastreadores de IA recomiendan JSON-LD porque puede inyectarse dinámicamente y procesarse sin el coste computacional del renderizado.

### ¿Cómo procesa el validador las estructuras `@graph` en implementaciones avanzadas?
Al detectar la propiedad `@graph`, el motor recorre la lista de subentidades declaradas y valida los atributos de cada una de ellas de forma recursiva. Esto garantiza la integridad de modelos de datos vinculados entre sí.

### ¿Qué penalizaciones cuantitativas se aplican ante fallos en los datos estructurados?
La herramienta deduce 40 puntos si no detecta bloques JSON-LD, 20 puntos por cada bloque con sintaxis JSON corrupta y 30 puntos si no localiza entidades reconocibles. Cada propiedad recomendada ausente resta 5 puntos sobre la base inicial de 100.

### ¿De qué forma evalúa el script los datos estructurados sin recurrir a un navegador headless?
El código efectúa peticiones HTTP GET directas con un User-Agent específico, obteniendo el código fuente de la página. Expresiones regulares optimizadas extraen los bloques de script, que el analizador de JSON procesa en milisegundos.

### ¿Cómo pueden los equipos de ingeniería incorporar esta validación en cadenas de CI/CD?
Al invocar `schema_validator.py` con el parámetro `-j`, los sistemas de integración continua pueden recoger el JSON devuelto y evaluarlo con jq. Es posible definir reglas automáticas que cancelen el despliegue si la puntuación desciende por debajo de 85 o si existen advertencias sintácticas.

***

## Consultoría en Arquitectura Semántica y Grafos de Conocimiento

Para diseño de datos estructurados, ingeniería de grafos de conocimiento y cumplimiento de estándares W3C en México, contacte con los especialistas de **TAVARDT México**.

***

## Enlaces Útiles

* **Portal Corporativo:** https://ag.tavardt.com/mx/
* **Suite de Diagnósticos:** https://tools.tavardt.com/mx/
* **Contacto Técnico:** https://ag.tavardt.com/mx/contacto/

***

### Licencia
Distribuido bajo la Licencia MIT.

***

© 2026 TAVARDT. Todos los derechos reservados.
