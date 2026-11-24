# Validador de Schema Markup y Datos Estructurados

[![Validador Schema](https://img.shields.io/badge/TAVARDT-Schema.org%20Validator-5b3cf5.svg)](https://tools.tavardt.com/mx/validador-schema-markup/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)

Auditoria de datos estructurados Schema.org JSON-LD y Microdata, validacion de entidades corporativas y elegibilidad para rich snippets.

Desarrollado y mantenido por el equipo de infraestructura semantica de **TAVARDT**.

***

## Capacidades de Validacion Semantica

* **Extraccion de Datos Estructurados**: Ingestion automatizada de bloques JSON-LD y etiquetas semanticas Microdata.
* **Reconocimiento de Entidades Schema.org**: Auditoria de tipos clave (Organization, LocalBusiness, WebSite, Article, Product, BreadcrumbList).
* **Validacion de Sintaxis y Atributos**: Localizacion de errores de formato JSON, atributos obligatorios faltantes y enlaces invalidos.
* **Metrica de Calidad Semantica**: Evaluacion integral de 0 a 100 puntos con recomendaciones tecnicas de estructuracion.

***

## Instalacion y Ejecucion

Requiere Python 3.8 o superior con la biblioteca estandar.

```bash
git clone https://github.com/TAVARDT-Mexico/schema-markup-validator-mx.git
cd schema-markup-validator-mx
python schema_validator.py ejemplo.mx
```

Para salida en formato JSON:

```bash
python schema_validator.py ejemplo.mx -json
```

***

## Herramienta en Linea

Para validaciones inmediatas y auditorias de datos estructurados:

* **Validador de Schema Markup Oficial**
* **[Portal Corporativo TAVARDT](https://ag.tavardt.com/mx/)**

***

## Consultoria en Marcado Semantico

Para estructuracion de datos corporativos y mejores practicas segun Estandares Web y Buenas Practicas SEO, contacte a **TAVARDT**.

***

© 2026 TAVARDT. Todos los derechos reservados.
