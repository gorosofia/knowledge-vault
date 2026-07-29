---
title: "DeepSeek"
source: "https://chat.deepseek.com/a/chat/s/9ebea0ab-8b37-4cef-b6a2-5b2694f3d1d1"
author:
published:
created: 2026-07-29
description: "Chat with DeepSeek AI – your intelligent assistant for coding, content creation, file reading, and more. Upload documents, engage in long-context conversations, and get expert help in AI, natural language processing, and beyond. | 深度求索（DeepSeek）助力编程代码开发、创意写作、文件处理等任务，支持文件上传及长文本对话，随时为您提供高效的AI支持。"
tags:
  - "clippings"
---
markdown

Copiar

Descargar

\---
tags: \[proyecto, agente-ia, job-search, research, obsidian, scraping, MCP\]
fecha-creacion: 2026-07-29
version: 3.0
estado: research-completo
\---
# 📘 SISTEMA DE BÚSQUEDA DE TRABAJO 100% AGÉNTICO — MASTER RESEARCH (v3.0)
\> \*\*Objetivo de este documento\*\*: Recopilar TODA la información técnica y no técnica necesaria para perfilar un sistema agéntico de búsqueda de empleo. Esto incluye \*\*portales de empleo, APIs oficiales, scraping de webs corporativas (páginas de carrera), formatos de CV europeos, sistemas ATS, mejores prácticas por industria, ejemplos de cartas de presentación y una estrategia de descubrimiento activo mediante MCP\*\*. Es el único documento fuente para el desarrollo.
\---
## 1. 🕵️ ESTRATEGIA DE IDENTIFICACIÓN DE FUENTES (PORTALES + WEB CORPORATIVA)
El sistema debe operar en dos capas de descubrimiento: \*\*agregadores conocidos\*\* y \*\*exploración web activa\*\* para encontrar ofertas directamente en los sitios de las empresas.
### 1.1 Descubrimiento en Portales de Empleo (Estático + Dinámico)
\- \*\*Lista base (semilla)\*\*: InfoJobs, Indeed, LinkedIn, StepStone, Apec, Reed, EURES, Monster, Glassdoor.
\- \*\*Actualización dinámica\*\*: El agente lanza consultas en buscadores como: \`"top job boards Europe 2026"\`, \`"bolsas de trabajo en Alemania"\` o \`"mejores portales de empleo en Francia"\` para añadir nuevas fuentes de forma autónoma.
### 1.2 🆕 Descubrimiento de Páginas de Carrera Corporativas (Web Corporativa)
El agente no debe esperar a que las empresas publiquen en portales; debe \*\*ir a buscarlas directamente a sus dominios\*\*. Estrategia detallada:
| Técnica | Herramienta MCP / API | Ejemplo de acción |
|\---------|\------------------------|\-------------------|
| \*\*Google Dorks / Búsqueda avanzada\*\* | \`mcp-server-search\` (SerpAPI) o \`mcp-server-playwright\` | Consultas: \`site:microsoft.com careers\`, \`site:apple.com jobs\`, \`"join our team" site:es\`, \`"careers" site:.de\` |
| \*\*Detección de ATS embebidos\*\* | \`mcp-server-fetch\` + análisis de HTML | Identificar si la URL redirige a \`myworkday.com\`, \`greenhouse.io\`, \`lever.co\`, \`icims.com\`. Si es así, usar sus APIs específicas o adaptar el scraper. |
| \*\*Sitemaps de empleo\*\* | \`mcp-server-fetch\` | Probar rutas comunes: \`https://\[empresa\]/sitemap-jobs.xml\`, \`https://\[empresa\]/jobs-sitemap.xml\`, \`https://\[empresa\]/sitemap.xml\` y filtrar por URLs que contengan \`job\` o \`career\`. |
| \*\*JSON-LD / Schema.org (Microdatos)\*\* | \`mcp-server-fetch\` + parser de JSON-LD | El 70% de las grandes empresas marcan sus ofertas con \`\<script type="application/ld+json">\` y \`@type: JobPosting\`. Extraer campos: \`title\`, \`description\`, \`datePosted\`, \`hiringOrganization\`, \`jobLocation\`, \`directApply\`, \`salary\`. |
| \*\*RSS / Feeds OCULTOS\*\* | \`mcp-server-fetch\` | Buscar \`/feed\`, \`/jobs.rss\`, \`/careers.xml\`, \`/vacancies.xml\` en el dominio raíz o en subcarpetas. |
| \*\*Scraping con navegador headless\*\* | \`mcp-server-playwright\` | Para webs que renderizan ofertas con JavaScript (React/Vue). El agente debe hacer clic en "Ver más ofertas", simular scroll infinito o interactuar con filtros. |
\*\*Pipeline de descubrimiento corporativo\*\*:
1. \*\*Semilla de empresas\*\*: Partir de listas de Fortune 500, IBEX 35, DAX 40, CAC 40, FTSE 100, o listas sectoriales (ej. "top tech companies in Berlin").
2. \*\*Búsqueda en buscador\*\*: Para cada empresa, lanzar 3-5 queries específicas para hallar su portal de carrera (ej. \`"Netflix careers"\`, \`"Spotify jobs"\`).
3. \*\*Fingerprinting\*\*: Clasificar el portal encontrado (ATS nativo, Workday, Greenhouse, Lever, SmartRecruiters, o desarrollado a medida).
4. \*\*Extracción de ofertas\*\*: Usar el parser adecuado (API oficial si existe, o scraping estructurado con base en el fingerprint).
5. \*\*Normalización\*\*: Mapear los campos al esquema unificado del agente (título, empresa, ubicación, fecha, descripción, salario, tipo de contrato, enlace de aplicación).
### 1.3 MCP Servers Recomendados para esta Capa
\- \*\*\`mcp-server-fetch\`\*\*: Para descarga de HTML, sitemaps y feeds.
\- \*\*\`mcp-server-playwright\`\*\*: Para interacción con SPAs y extracción tras renderizado.
\- \*\*\`mcp-server-search\`\*\* (ej. implementación con SerpAPI o Brave Search API): Para consultar motores de búsqueda de forma programática.
\- \*\*\`mcp-server-jsonld\`\*\* (custom o usando LangChain): Para extraer y validar Schema.org.
\- \*\*\`mcp-server-reasoning\`\*\* (LLM): Para clasificar fuentes y decidir si usar API o scraping.
\---
## 2. 🔌 APIs DE BOLSAS DE TRABAJO (OFICIALES Y AGREGADORES)
### 2.1 LinkedIn APIs
\- \*\*Productos disponibles\*\*: Marketing API, Jobs API, Profile API, Talent Solutions API, Learning API.
\- \*\*Apply Connect / Job Posting API\*\*: Permite crear y gestionar ofertas. \*\*Requisito\*\*: ser partner aprobado de LinkedIn Talent Solutions y firmar acuerdo de API.
\- \*\*Versión actual\*\*: \`202603\`. URL base: \`https://api.linkedin.com/rest/\`.
\- \*\*Simple Job Posting API\*\*: Para crear, actualizar, renovar y cerrar ofertas.
\- \*\*Job Sync API\*\*: Para sincronizar requisiciones desde un ATS.
\- \*\*Alternativa no oficial\*\*: \`Apify LinkedIn Jobs API\` o scraping del guest API (que devuelve HTML con datos estructurados para usuarios no autenticados).
\- \*\*Docs oficiales\*\*: \[LinkedIn Talent APIs\](https://learn.microsoft.com/zh-tw/linkedin/talent/apply-connect/create-apply-connect-jobs)
### 2.2 Indeed APIs
\- \*\*Job Sync API\*\* (GraphQL): Crear, actualizar, expirar y obtener estado de ofertas. Incluye cualificaciones, horario, salario, beneficios.
\- \*\*Job Update API\*\*: Actualización y listado de ofertas.
\- \*\*Sponsored Jobs API\*\*: Patrocinar ofertas indexadas.
\- \*\*Candidate Sync API\*\*: Sincronización de candidatos.
\- \*\*Tiempo de integración\*\*: Aprox. 6 semanas.
\- \*\*Docs\*\*: \[Indeed API Docs\](https://docs.indeed.com)
### 2.3 InfoJobs API (España e Italia)
\- \*\*RESTful\*\* (JSON/XML). Disponible para España (\`api.infojobs.net\`) e Italia (\`api.infojobs.it\`).
\- \*\*Entidades\*\*: \`Candidate\`, \`Offer\`, \`Curriculum\` (hasta 5 CVs), \`Application\`, \`Cover Letter\`, \`Dictionary\` (provincias, rangos salariales).
\- \*\*Autenticación\*\*: Credenciales de desarrollador para operaciones básicas; \*\*OAuth2\*\* para operaciones que requieren autorización del usuario.
\- \*\*Docs\*\*: \[InfoJobs Developer\](http://developer.infojobs.net)
### 2.4 Agregadores Multi-Fuente (Recomendados para acelerar desarrollo)
| Agregador | Cobertura | Enlace |
|\-----------|\-----------|\--------|
| \*\*Ever Jobs\*\* | 160+ fuentes (REST, GraphQL, MCP) | \[GitHub\](https://github.com/ever-jobs/ever-jobs) |
| \*\*Sherlock\*\* | 80+ países en paralelo | \[GitHub\](https://github.com/Gorghs/Project\_Sherlock) |
| \*\*Jobo-enterprise\*\* | 45+ plataformas ATS (Workday, Greenhouse, etc.) | \[NPM\](https://www.npmjs.com/package/jobo-enterprise) |
| \*\*Apify Jobs API\*\* | Indeed, LinkedIn, Glassdoor, ZipRecruiter | \[Apify\](https://apify.com) |
| \*\*European Job Search\*\* | 20 bolsas europeas en una ejecución | \[Apify\](https://apify.com) |
| \*\*Agent47\*\* | Capa unificada con MCP server | \[GitHub\](https://github.com/espadaw/Agent47) |
### 2.5 Nota sobre Normalización
El mayor desafío técnico es normalizar esquemas dispares entre plataformas. Por ejemplo, solo el \*\*16%\*\* de las ofertas extraídas de ATS (como Workday) incluyen rangos salariales estructurados. El sistema debe implementar un \*\*mapeador de campos flexible\*\* y un \*\*validador de esquemas\*\* que pueda adaptarse a cada fuente.
\---
## 3. 📄 FORMATOS DE CV EUROPEOS Y ESTÁNDARES ATS
### 3.1 Europass CV (Estándar UE)
\- \*\*Idiomas\*\*: Disponible en +29 idiomas. \*\*Países\*\*: Reconocido en +35 europeos.
\- \*\*Estructura de 6 secciones\*\*:
  1. Información Personal (nombre, contacto, ciudad, país, foto opcional)
  2. Experiencia Laboral (orden cronológico)
  3. Educación y Formación
  4. Idiomas (con niveles CEFR A1-C2)
  5. Habilidades Digitales (valoración explícita)
  6. Información Adicional
\- \*\*Extensión\*\*: 1-2 páginas (máx 3 para perfiles senior con 15+ años de experiencia).
\- \*\*Cuándo usarlo\*\*: Sector público de la UE, universidades, investigación, programas de movilidad internacional, posiciones financiadas por la UE.
\- \*\*Cuándo NO usarlo\*\*: Sector privado en países donde los CVs modernos y personalizados son la norma (a menos que el empleador lo solicite explícitamente).
\- \*\*Recurso oficial\*\*: \[Europass CV Creator\](https://europass.europa.eu/en/create-europass-cv)
\- \*\*Implementación no oficial\*\*: Plantillas en LaTeX (\`europasscv2025\`).
### 3.2 Reglas de Oro para ATS (Applicant Tracking Systems)
Los ATS leen en texto plano. \*\*El 38.6%\*\* de las Fortune-500 usan \*\*Workday\*\*. Otros ATS relevantes: Taleo (Oracle), Greenhouse, iCIMS, Lever, Avature, Breezy, JazzHR, Jobvite, ADP, UKG, Rippling, Pinpoint, Freshteam.
| ✅ HACER (ATS‑Friendly) | ❌ NO HACER (Riesgo de filtrado) |
|\--------------------------|\----------------------------------|
| Fuentes simples: Arial, Calibri, Times New Roman, Inter, Helvetica (10-12pt) | Tablas, gráficos, imágenes, logotipos |
| Encabezados de sección estándar: "EXPERIENCE", "EDUCATION", "SKILLS", "CERTIFICATIONS" | Nombres creativos: "Mi Trayectoria", "Career Journey" |
| Guardar como \*\*PDF o .docx\*\* (según lo que pida la oferta) | Columnas o cuadros de texto (los ATS los tratan como espacios en blanco) |
| Viñetas estándar: •, -, o \* | Encabezados o pies de página con información importante (se pierde) |
| Fechas en formato estándar: Month Year (ej. April 2026) | Fuentes extravagantes, colores o formatos .jpg/.png |
\*\*Estructura ATS‑friendly básica\*\*:
\`\`\`text
\[NOMBRE COMPLETO\]
\[Teléfono\] | \[Email\] | \[Ciudad, País\] | \[LinkedIn\]
SUMMARY
\[2-3 frases que resuman tu perfil y objetivo profesional\]
SKILLS
\[Keywords separadas por comas: "Python, SQL, Project Management, Agile, SAP"\]
EXPERIENCE
\[Título del puesto\] | \[Empresa\] | \[Ciudad, País\] | \[Fechas\]
\- Logro cuantificable con métricas (ej. "Aumenté ventas un 20% en 6 meses")
\- Logro cuantificable con métricas
EDUCATION
\[Título\] | \[Institución\] | \[Fecha de graduación\]
CERTIFICATIONS
\[Nombre de la certificación\] – \[Fecha\]

---

## 4\. 🧠 OPTIMIZACIÓN PARA ATS Y KEYWORDS

### 4.1 Funcionamiento del Scoring ATS

1. **Parsing**: El sistema extrae todo el texto del CV.
2. **Keyword Matching**: Compara el contenido con las palabras clave de la oferta (título, requisitos, habilidades).
3. **Scoring**: Asigna un porcentaje de coincidencia.
4. **Filtering**: Los CVs por debajo de un umbral (ej. 70%) nunca llegan a un reclutador humano.

**Factor crítico**: Las keywords deben reflejar **exactamente** la terminología de la oferta (incluyendo siglas, nombres de herramientas, certificaciones, etc.). Un CV con las mismas habilidades pero redactado de forma distinta puede obtener una puntuación mucho más baja.

### 4.2 Técnica de Extracción de Keywords para el Agente

1. **Analizar la descripción del puesto** y resaltar términos que aparecen más de 2 veces.
2. **Identificar sinónimos y variantes** (ej. "Gestión de proyectos" → "Project Management", "PM").
3. **Mapear verbos de acción** usados en las responsabilidades (ej. "liderar", "coordinar", "implementar").
4. **Incluir herramientas específicas** (ej. "Salesforce", "SAP", "AutoCAD", "Figma").
5. **Revisar los requisitos obligatorios vs. deseables**: priorizar los obligatorios.

**Recomendación 2026**: Anteponer keywords de alto impacto directamente en los títulos de los puestos y en el resumen, no solo en las viñetas de experiencia.

### 4.3 Uso de Embeddings Semánticos (Complemento al Keyword Matching)

Además del matching exacto, el agente puede usar modelos de embeddings (ej. `text-embedding-3-small` o `all-MiniLM-L6-v2`) para comparar la similitud semántica entre el CV y la oferta. Esto permite encontrar coincidencias incluso cuando no se usan las mismas palabras literales.

---

## 5\. 📊 MEJORES PRÁCTICAS DE CV POR INDUSTRIA (2026)

### 5.1 Tendencias Generales 2026

- **Cambio fundamental**: Los empleadores ya no se fijan solo en dónde has trabajado, sino en **qué puedes hacer y qué tan rápido pueden verlo**. Las decisiones de contratación comienzan con un **vistazo rápido de 15-20 segundos**.
- **El CV cronológico largo** está siendo reemplazado por un enfoque híbrido: liderar con habilidades y logros, respaldado por un historial laboral conciso.
- **Contratación basada en habilidades**: Se está utilizando para ampliar los pools de talento más allá de "debe tener X años" o "debe tener un título".

### 5.2 Retail, Hostelería e Industria

**Qué buscan los empleadores**:

- Habilidades medibles y resultados concretos.
- CV que comunique valor en los primeros 15-20 segundos.
- Formato limpio y coincidencia de keywords con la oferta.

**Habilidades duras a destacar (cerca de la parte superior)**:

- Software de punto de venta (POS) y manejo de efectivo.
- Control de stock y gestión de inventario.
- Sistemas de gestión de almacenes (WMS) y escáneres portátiles.
- Seguridad alimentaria y formación en salud y seguridad (ej. OSHA, IOSH).
- Manipulación manual, operación de equipos o certificaciones específicas del rol.

**Ratio recomendado**: **60:40 de habilidades duras a blandas**.  
**Al menos 5 habilidades relevantes** deben ser fáciles de identificar en el tercio superior del CV.

**Las 4 preguntas que los managers de contratación buscan en el tercio superior**:

1. ¿Para qué rol te postulas? (target role)
2. ¿Qué impacto has tenido? (impact)
3. ¿Tienes las habilidades adecuadas? (skills fit)
4. ¿Puedes demostrar que entregas resultados? (proof you can deliver)

**Alfabetización digital**: Los empleadores prestan más atención a la alfabetización digital. No necesitas afirmar que eres un experto, pero debes mostrar comodidad con las herramientas del trabajo (sistemas de reservas, escáneres de almacén, portales de formación online, herramientas de reporting básicas).

### 5.3 Sector Tecnológico y Startups

**Transición de academia a industria**:

- En academia, un CV de 6+ páginas indica meticulosidad; en industria, indica lo contrario.
- Poner un **resumen profesional** en la parte superior.
- Añadir una **sección de habilidades** a continuación.
- Listar la **experiencia profesional** en primer lugar (no la educación, a menos que seas recién graduado).

**Estructura recomendada para tech**:

1. Header + resumen de 1 línea (ej. "Ingeniero de Software con 5 años en backend y nube").
2. Experiencia — sección más importante, con logros cuantificables.
3. Proyectos — demuestra, no solo digas (incluye enlaces a GitHub, repositorios o demos).
4. Habilidades técnicas (lenguajes, frameworks, herramientas).

**Tendencias 2026 para CVs tecnológicos**:

- Enfocarse en reputación y relaciones (contribuciones a open source, comunidades).
- Agudizar instintos creativos y fluidez en IA (menciona si usas Copilot, ChatGPT, o has desarrollado modelos).
- Demostrar "talento, agilidad y resistencia".

**Uso de IA en CVs**: Usar IA para brainstorming o edición, pero **no enviar CVs genéricos escritos por IA** — los reclutadores ya detectan este patrón.

### 5.4 Longitud Recomendada por Perfil

| Perfil | Páginas recomendadas |
| --- | --- |
| Recién graduado / Junior | 1 página |
| Experiencia profesional consolidada (3-10 años) | 1-2 páginas |
| Senior / Directivo (15+ años) | Máximo 3 páginas |

### 5.5 Errores Comunes a Evitar en Cualquier Industria

- CVs genéricos (no adaptados a la oferta) → "pierden siempre".
- Afirmaciones vagas como "Advanced Excel" — sé específico (ej. "Power Query, Macros, Tablas Dinámicas").
- Frases largas y farragosas — usa frases cortas y ejemplos concretos.
- No incluir métricas — los logros sin números no son creíbles.

---

## 6\. ✉️ CARTAS DE PRESENTACIÓN (COVER LETTERS)

### 6.1 Formato Estándar Europeo

- **Extensión**: 1 página, 250-400 palabras, 3-4 párrafos.
- **Saludo**: "Dear Mr. Müller" (formal) o "Dear Hiring Team" si no se conoce el nombre. En entornos menos formales (startups), se puede usar "Dear \[Nombre\]".
- **Fecha**: Usar formato local (ej. "Milano, 20 maggio 2026" para Italia, o "April 24, 2026" para UK).
- **Estructura de 6 secciones**:
	1. **Header**: Datos de contacto (nombre, teléfono, email, ubicación, enlace a LinkedIn).
		2. **Saludo**: Dirigido a la persona o equipo.
		3. **Apertura**: Establecer nombre, rol/estudios recientes y un gancho auténtico (no genérico). Evita "Me dirijo a ustedes para postularme a la oferta...".
		4. **Cuerpo (tesis principal)**: Explicar por qué eres la persona adecuada para el puesto, vinculando tu experiencia con las necesidades de la empresa. Usa ejemplos concretos.
		5. **Cierre**: Resumir tu interés y agradecer la consideración.
		6. **Sign-off**: Despedida profesional (ej. "Yours sincerely" o "Atentamente") + firma y nombre.

### 6.2 Consejos Clave para 2026

- **Personalización absoluta**: Nada de plantillas genéricas — los ATS y reclutadores detectan y descartan cartas masivas.
- **Propuesta comercial**: La carta es una propuesta de valor; tú eres el producto. Enfócate en cómo vas a resolver los problemas de la empresa, no en lo que esperas obtener.
- **Apertura creativa y auténtica**: Capta la atención desde la primera línea. Ejemplo: "Cuando vi que buscaban un especialista en logística para optimizar su cadena de suministro en Europa, supe que mi experiencia en SAP y en reducción de costes encajaba perfectamente."
- **Keywords estratégicas**: Los ATS también escanean cartas de presentación, así que incluye las mismas keywords que en el CV.
- **Formalidad europea**: En países como Alemania o Francia, mantener un tono formal y estructurado. En UK o Países Bajos, se permite un estilo ligeramente más directo.

### 6.3 Ejemplos por Sector

**Sector financiero/contable**: Destacar habilidades duras como presupuestos, análisis de estados financieros, declaraciones de impuestos y cumplimiento normativo.

**Sector tecnológico europeo**: Formato limpio, formal y directo. Usar "Dear Mr. Müller" o "Dear Hiring Team" — evitar "Hi" o "Hey" en entornos corporativos.

**Sector público / UE**: Europass CV obligatorio y carta formal con énfasis en la ciudadanía/estatus de visado y alineación con los valores de la UE.

### 6.4 Recursos de Ejemplos

- [Zety - Ejemplos por industria](https://zety.com/blog/cover-letter-examples)
- [Central Washington University - Guía PDF](https://www.cwu.edu)
- [Imperial College London - Guía Europa](https://www.imperial.ac.uk)
- [University of Rhode Island - Ejemplos de aperturas creativas](https://www.uri.edu)

---

## 7\. ⚙️ RECOMENDACIONES ARQUITECTÓNICAS PARA EL AGENTE

### 7.1 Stack Tecnológico Sugerido

1. **Orquestación de Agentes**: LangGraph o CrewAI + MCP Servers (para la capa de herramientas).
2. **Base de datos vectorial**: Pinecone o Qdrant para almacenar embeddings de ofertas y perfiles.
3. **Base de datos relacional**: PostgreSQL para guardar el inventario de empresas, URLs de carrera, tipo de ATS detectado, y ofertas normalizadas.
4. **Cola de tareas**: Celery o BullMQ para gestionar scraping asíncrono y rate-limiting.
5. **Generación de CVs**: Python con `python-docx` (para Word) o `LaTeX` (para Europass). También se puede usar `weasyprint` para generar PDFs desde HTML.
6. **Proxies y rotación de IP**: Integrar con servicios como SmartProxy, Oxylabs o residential proxies para evitar bloqueos.

### 7.2 Flujo de Trabajo del Agente (Actualizado con Web Corporativa)

1. **Discovery Agent (Portales)**: Busca y valida bolsas de trabajo usando MCP Fetch/Search.
2. **Discovery Agent (Corporativo)**:
	- Carga listas de empresas objetivo (por sector, país, tamaño).
		- Lanza queries de búsqueda para hallar `https://[empresa]/careers` o `/jobs`.
		- Escanea sitemaps, JSON-LD y feeds RSS para extraer URLs de ofertas.
3. **Scraping/API Agent**: Extrae ofertas en crudo desde portales + webs corporativas usando el método adecuado (API oficial, scraping HTML, o Playwright) y las normaliza al esquema unificado.
4. **Matching Agent**: Compara el perfil del usuario (CV vectorizado) contra cada oferta usando una combinación de keyword matching y similitud semántica (embeddings). Asigna una puntuación.
5. **Generation Agent**: Personaliza CV y Cover Letter según la oferta seleccionada, aplicando las mejores prácticas ATS y de industria.
6. **Application Agent**: Postula a través de APIs (ej. InfoJobs Application API) o mediante automatización de navegador (Playwright) cuando no exista API. Alternativamente, redirige al usuario al enlace directo para que complete la postulación manualmente.

### 7.3 Cumplimiento Legal (GDPR y Términos de Servicio)

- **LinkedIn**: Restringido a partners aprobados. No hacer scraping agresivo; respetar los términos de uso.
- **Indeed**: Respetar `robots.txt` y los términos de la API. No automatizar postulaciones si está prohibido.
- **Corporativas**: Respetar `robots.txt`, limitar la tasa de peticiones (delay de 1-2 segundos entre requests) y no sobrecargar los servidores.
- **GDPR**: No almacenar datos personales (CVs, emails, nombres) sin consentimiento explícito del usuario. Implementar política de retención de datos.
- **Transparencia**: El sistema debe informar al usuario que se está utilizando IA para buscar, personalizar y postular en su nombre. Ofrecer siempre la opción de revisar y aprobar las postulaciones antes de enviarlas.

### 7.4 🆕 Gestión de Proxies y Rotación de IP

Dado que el agente buscará en cientos de dominios corporativos, es esencial implementar:

- **Pool de proxies rotativos** (residenciales o datacenter con rotación automática).
- **User-Agent aleatorios** (simular distintos navegadores y dispositivos).
- **Uso de cabeceras HTTP** como `Accept-Language` para países específicos (ej. `de-DE` para Alemania).
- **Respetar los tiempos de espera** entre peticiones para no activar sistemas anti-bot (ej. Cloudflare).

### 7.5 Monitorización y Logging

- Implementar logs estructurados para cada acción del agente (búsqueda, extracción, matching, generación, postulación).
- Métricas clave: número de ofertas descubiertas, tasa de éxito de extracción, tiempo de proceso, tasa de coincidencia promedio.

---

## 8\. 🔗 LISTA MAESTRA DE ENLACES (TODOS)

### APIs y Agregadores

- [LinkedIn Apply Connect](https://learn.microsoft.com/zh-tw/linkedin/talent/apply-connect/create-apply-connect-jobs)
- [LinkedIn Simple Job Posting](https://learn.microsoft.com/en-us/linkedin/talent/job-postings/api-create-jobs)
- [Indeed Job Sync API](https://docs.indeed.com)
- [Indeed Job Update API](https://docs.indeed.com)
- [InfoJobs Developer (ES)](http://developer.infojobs.net)
- [InfoJobs Developer (IT)](https://developer.infojobs.it)
- [Ever Jobs (Agregador 160+ fuentes)](https://github.com/ever-jobs/ever-jobs)
- [Sherlock (Agregador 80+ países)](https://github.com/Gorghs/Project_Sherlock)
- [Jobo-enterprise (45+ ATS)](https://www.npmjs.com/package/jobo-enterprise)
- [Agent47 (MCP unificado)](https://github.com/espadaw/Agent47)
- [State of ATS 2026 (Datos de mercado)](https://github.com/Kayvan-Zahiri/state-of-ats-2026)

### CV y Formato Europeo

- [Europass Oficial - CV Creator](https://europass.europa.eu/en/create-europass-cv)
- [Indeed ATS Guide](https://indeedflex.com/career-hub/guides/ats-resume-tips)

### Cartas de Presentación

- [Zety Cover Letter Examples](https://zety.com/blog/cover-letter-examples)
- [Imperial College London - Career Guide](https://www.imperial.ac.uk)
- [Central Washington University - Cover Letter Guide](https://www.cwu.edu)

### Portales Europeos Clave (Base de Conocimiento)

- [EURES - European Job Mobility](https://eures.europa.eu)
- [StepStone (Alemania)](https://www.stepstone.de)
- [Apec (Francia - directivos)](https://www.apec.fr)
- [France Travail (antiguo Pôle emploi)](https://www.francetravail.fr)
- [Reed (Reino Unido)](https://www.reed.co.uk)
- [InfoJobs Italia](https://www.infojobs.it)
- [Tecnoempleo (España - tecnología)](https://www.tecnoempleo.com)
- [Jobatus (España)](https://www.jobatus.es)
- [Interamt (Alemania - sector público)](https://www.interamt.de)
- [Monsterboard (Países Bajos)](https://www.monsterboard.nl)

### 🆕 Recursos para Scraping Corporativo y JSON-LD

- [Schema.org JobPosting](https://schema.org/JobPosting) — Esquema estándar para ofertas en webs corporativas.
- [Google Job Posting Guidelines](https://developers.google.com/search/docs/appearance/structured-data/job-posting) — Cómo Google indexa ofertas; útil para emular su lógica de extracción.
- [Robots.txt especificación](https://developers.google.com/search/docs/crawling-indexing/robots/intro) — Para respetar límites de rastreo.
- [Playwright MCP Server](https://github.com/executeautomation/mcp-playwright) — Para interacción con SPAs y formularios.
- [Fetch MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch) — Para descarga de HTML y sitemaps.

---

## 📌 NOTA FINAL PARA EL VAULT

Este documento es la **única fuente de verdad** para el desarrollo del sistema de búsqueda de trabajo 100% agéntico.

**Puntos clave a recordar**:

1. El agente debe combinar **APIs oficiales** (portales) con **técnicas de descubrimiento web activo** (webs corporativas) usando herramientas MCP.
2. La **capa de normalización** es el componente más crítico: unifica ofertas de decenas de fuentes con esquemas diferentes.
3. La **generación de CVs** debe ser ATS-friendly (una columna, fuentes simples, sin tablas) y adaptarse a los estándares europeos (Europass cuando sea requerido).
4. La **gestión de proxies y rate-limiting** es esencial para no ser bloqueado por los dominios corporativos.
5. Cumple siempre con **GDPR** y los términos de servicio de cada plataforma; ofrece transparencia al usuario.

Revisa periódicamente los enlaces de APIs, ya que versionan cada año (ej. LinkedIn ya usa `202603`). Con esta base, el desarrollo puede comenzar con un **proof of concept** que integre 2-3 fuentes (ej. Indeed + InfoJobs + scraping de una empresa grande) y luego escalar a todo el ecosistema europeo.