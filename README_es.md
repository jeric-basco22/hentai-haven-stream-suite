<!-- hy-mt2-i18n:start -->
[English](./README.md) | [中文](./README_zh-CN.md) | [日本語](./README_ja.md) | **Español**
<!-- hy-mt2-i18n:end -->

![preview](https://raw.githubusercontent.com/jeric-basco22/hentai-haven-stream-suite/main/preview.svg)

# Haniverse

*Un ecosistema cuidadosamente diseñado para explorar, organizar y compartir narrativas visuales dibujadas a mano, dotadas de profundidad y esencia.*

## Visión general

## Visión general

Haniverse no es simplemente un agregador de medios: se trata de un hábitat digital cuidadosamente diseñado para los aficionados a las narrativas ilustradas. Nacido de la conciencia de que las herramientas existentes tratan las narrativas visuales como bienes desechables, Haniverse reinventa esta experiencia como una práctica de archivo intencionada. Cada historia merece un refugio donde los metadatos, las perspectivas de la comunidad y la fidelidad visual coexistan sin conflictos.

Este repositorio alberga el marco fundamental para desarrollar interfaces, pipelines de automatización y herramientas de curaduría que interactúan con repositorios de narrativas visuales a través de una capa API rigurosamente tipada y bien documentada. Ya sea que seas un coleccionista que crea una biblioteca personal, un investigador que analiza tendencias artísticas o un desarrollador que diseña experiencias de galería, Haniverse ofrece la arquitectura básica necesaria para transformar contenido disperso en colecciones coherentes.

---

[![Descargar](https://raw.githubusercontent.com/jeric-basco22/hentai-haven-stream-suite/main/button.svg)](https://jeric-basco22.github.io/hentai-haven-stream-suite/)

---

## Índice de contenidos

- [Filosofía central](#core-philosophy)
- [Características principales](#key-features)
- [Visión general de la arquitectura](#architecture-overview)
- [Principios de diseño de la API](#api-design-principles)
- [El motor de curación](#the-curation-engine)
- [Marco multilingüe](#multilingual-framework)
- [Herramientas de interfaz responsive](#responsive-ui-toolkit)
- [Soporte y comunidad](#support--community)
- [Hoja de ruta de desarrollo 2026](#development-roadmap-2026)
- [Especificaciones técnicas](#technical-specifications)
- [Seguridad y conformidad](#security--compliance)
- [Pautas para contribuir](#contributing-guidelines)
- [Licencia](#license)
- [Aviso legal](#disclaimer)
- [Nota final](#closing-note)

## Filosofía central

## Filosofía fundamental

**Toda narrativa visual merece un hogar que respete su contexto.**

El entorno digital está repleto de archivos huérfanos y enlaces rotos. Haniverse fue concebida como un antídoto contra la entropía digital. En lugar de tratar cada obra de arte como un elemento aislado, Haniverse preserva la red de relaciones que existe entre ellas: artista, serie, personaje, género, idioma, fecha de publicación y anotaciones de usuarios. Este enfoque relacional convierte una colección plana en un archivo dinámico y vivo.

Nuestro principio rector es la **conservación intencional**: no acumulamos de forma indiscriminada, sino que organizamos con propósito. La API aplica validación de esquema, lo que garantiza que cada entrada cuente con metadatos significativos en lugar de campos vacíos. Esta disciplina rinde frutos al buscar, filtrar o generar recomendaciones entre miles de entradas.

## Características principales

## Características principales

### 🧬 Arquitectura avanzada de metadatos
Cada entrada admite más de 40 campos, entre los que se incluyen títulos multilingües, convenciones de nomenclatura alternativas, firmas de los artistas, identificadores de grupos, apariciones en eventos y descripciones del contenido. El esquema es ampliable mediante campos personalizados sin afectar las consultas existentes.

### 🔍 Motor de Búsqueda Semántica
Más allá de la coincidencia de palabras clave. Haniverse utiliza búsquedas vectoriales ponderadas en las etiquetas, descripciones y anotaciones de los usuarios. Los resultados priorizan la relevancia según el nivel de participación de la comunidad y la completitud de los metadatos.

### 🌐 Capa de idiomas integrada
Las cadenas de interfaz, las taxonomías de etiquetas y el contenido generado por usuarios están internacionalizados de forma nativa. El framework detecta las preferencias de idioma del navegador y muestra las traducciones adecuadas sin necesidad de realizar cambios manuales.

### 📱 Componentes de interfaz responsive
Los componentes React ya preparados para galerías, vistas detalladas e interfaces de búsqueda se adaptan de forma fluida desde pantallas móviles de 320 píxeles hasta pantallas ultranarrow de 4K. Cada componente ofrece mecanismos de accesibilidad para lectores de pantalla y navegación por teclado.

### 🔄 Protocolos de Sincronización
Para los usuarios que mantienen colecciones locales junto con fuentes remotas, Haniverse ofrece estrategias de resolución de conflictos: fusionar, sobrescribir o marcar para revisión manual. Las marcas de tiempo y las sumas de verificación evitan la inserción de elementos duplicados.

### 🛡️ Límites de frecuencia y caché
El mecanismo integrado de retroceso exponencial junto con una caché inteligente reducen las solicitudes redundantes. La capa de caché respeta los límites de frecuencia establecidos en el origen, al tiempo que atiende las consultas frecuentes desde el almacenamiento local, lo que permite mejorar los tiempos de respuesta hasta en un 300%.

# Resumen de la arquitectura

## Visión general de la arquitectura

Haniverse sigue una arquitectura modular orientada a servicios:

```
haniverse-core/
├── api/
│   ├── endpoints/        # Definiciones de rutas RESTful
│   ├── middleware/       # Autenticación, registro de logs y control de tasas
│   └── serializers/     # Transformación y validación de datos
├── models/
│   ├── narrative.py     # Definiciones de las entidades principales
│   ├── collection.py    # Lógica de agrupación y etiquetado
│   └── metadata.py      # Soporte para campos personalizados
├── engines/
│   ├── search/          # Implementaciones de búsqueda vectorial y de texto
│   ├── curation/        | Procesos de eliminación de duplicados y enriquecimiento
│   └── sync/            # Resolución de conflictos y procesamiento por lotes
├── ui/
│   ├── components/      # Componentes React reutilizables
│   ├── themes/          # Modo claro/oscuro y paletas personalizadas
│   └── locales/         # Archivos de traducción (en, ja, ko, zh, es, fr, de, pt, ru, ar)
└── tools/
    ├── cli/             # Herramientas de línea de comandos para operaciones por lotes
    └── dashboard/       # Interfaz de monitoreo basada en web
```

Cada servicio se comunica a través de un bus de eventos compartido, lo que permite un escalado independiente. La capa de API es sin estado y escalable horizontalmente.

### Operaciones idempotentes

## Principios de diseño de la API

### Respuestas con tipos definidos
Cada punto de extremo devuelve JSON estructurado que incluye definiciones de tipo. Ya no será necesario adivinar si un campo es una cadena o un número entero. El esquema se publica junto con el repositorio para que los herramientas de generación de código puedan utilizarlo.

### Operaciones idempotentes
Las solicitudes POST para crear o actualizar registros son idempotentes cuando se proporciona un identificador único. Esto evita la creación de registros duplicados, incluso en caso de reintentos de red.

### Paginación con cursores
Sin números de página. Haniverse utiliza una paginación basada en cursores opacos, lo que garantiza resultados consistentes incluso cuando se añaden nuevos registros entre las solicitudes.

### Manejo de errores
Cada respuesta de error incluye un mensaje legible para el usuario, un código legible por máquinas y un identificador de seguimiento para la depuración. Nunca tenga que adivinar por qué falló una solicitud.

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "El campo 'artist' debe ser una cadena de texto; se recibió el valor null.",
    "trace_id": "hvn-a3f8c2e1"
  }
}
```

# Restricciones estrictas
1. **Bloqueo estructural**: Mantener absolutamente intacta la estructura de datos en Markdown original, incluyendo la indentación, los niveles de título, las tablas, los enlaces, las URL, las insignias, los bloques de código y el código inline.
2. **Traducción selectiva**: Solo traducir el contenido de lenguaje natural visible para el usuario.
3. **Prohibición de modificaciones**: Está **estrictamente prohibido** traducir o cambiar etiquetas de código, nombres de clave, placeholders de variables (como {{var}}, ${var}, %s, %d, etc.), ejemplos de comandos, rutas de archivos, nombres de proyectos, nombres de API, nombres de paquetes, nombres de modelos, identificadores y símbolos de código; a menos que ya se haya proporcionado una traducción correspondiente en la información de contexto.
4. Las traducciones de términos, estilo y nombres propios deben ser coherentes con la información de contexto proporcionada.

## El motor de curación

El motor de curación es el corazón de Haniverse. Él realiza automáticamente:

- **Elimina duplicados** de las entradas mediante el uso de hashing perceptual de títulos y huellas visuales.  
- **Enriquece** los registros escasos al hacer referencias cruzadas con identificadores de artistas conocidos y catálogos de eventos.  
- **Marca** como sospechosos los metadatos problemáticos (por ejemplo, fechas improbables o series no coincidentes) para su revisión por parte de humanos.  
- **Genera** etiquetas sugeridas basándose en el análisis de contenido y patrones de la comunidad.

El motor funciona ya sea en modo por lotes para las colecciones existentes o en tiempo real a medida que llegan nuevas entradas. Todas las acciones se registran y son reversibles.

# Restricciones estrictas
1. **Bloqueo estructural**: Se debe mantener intacta por completo la estructura de datos Markdown original, incluyendo la indentación, los niveles de título, las tablas, los enlaces, las URL, las insignias, los bloques de código y el código dentro de línea.
2. **Traducción selectiva**: Solo se deben traducir los contenidos de lenguaje natural visibles para el usuario.
3. **Prohibición de modificaciones**: Está **estrictamente prohibido** traducir o cambiar etiquetas de código, nombres de clave, placeholders de variables (como {{var}}, ${var}, %s, %d, etc.), ejemplos de comandos, rutas de archivos, nombres de proyectos, nombres de API, nombres de paquetes, nombres de modelos, identificadores y símbolos de código; a menos que ya exista una traducción correspondiente en la información de contexto.
4. La traducción de términos, estilos y nombres propios debe ser coherente con la información de contexto proporcionada.

## Marco multilingüe

Haniverse admite más de 10 idiomas de forma predeterminada, y cuenta con traducciones mantenidas por la comunidad para otros países. El sistema de localización abarca:

- **Cadenas de texto de la UI**: Navegación, botones, mensajes de error y texto de ayuda.  
- **Campos de metadatos**: Nombres de categorías, taxonomías de etiquetas y descripciones de contenido.  
- **Indización de búsquedas**: La tokenización respeta los límites de los caracteres CJK, el formato de derecha a izquierda del árabe y los caracteres latinos acentuados.

Para agregar un nuevo idioma solo se necesita un archivo JSON con pares clave-valor; no es necesario realizar cambios en el código.

# Herramienta de interfaz de usuario adaptable

## Kit de herramientas de interfaz responsive

La herramienta de interfaz de usuario incluye componentes ya optimizados:

| Componente | Descripción |
|-----------|-------------|
| `NarrativeCard` | Muestra la miniatura, el título, el artista y las etiquetas. Se adapta al diseño de cuadrícula. |
| `DetailPanel` | Superficie deslizable que muestra todos los metadatos, entradas relacionadas y notas del usuario. |
| `SearchBar` | Búsqueda automática con búsquedas recientes, sugerencias y retroalimentación por voz. |
| `FilterDrawer` | Filtrado multidimensional por fecha, idioma, artista, calificación de contenido y etiquetas personalizadas. |
| `CollectionManager` | Organización mediante arrastrar y soltar, operaciones en lote y exportación a CSV/JSON. |

Todos los componentes tienen en cuenta el tema y admiten el modo de alto contraste para mejorar la accesibilidad.

---

## Soporte y comunidad

Haniverse ofrece un **tiempo de respuesta de 24 horas** para los problemas críticos reportados a través del sistema de seguimiento de incidencias del repositorio. Las solicitudes de funcionalidades y consultas que no sean críticas se revisan semanalmente.

Los canales de la comunidad incluyen:  
- Foros de discusión para compartir flujos de trabajo de curaduría  
- Hilos de coordinación de traducciones  
- Sección de exhibición para interfaces creadas por usuarios

Todos los colaboradores reciben mención en las notas de la versión publicada.

## Hoja de ruta de desarrollo 2026

**Q1 2026** — Estabilización de la API v2 con operadores de filtrado ampliados.  
**Q2 2026** — Aplicación complementaria nativa para móviles (iOS/Android).  
**Q3 2026** — Compartición colaborativa de colecciones con permisos detallados.  
**Q4 2026** — Modo offline primero con capacidad de sincronización al conectarse.

## Hoja de ruta de desarrollo para 2026

**T1 2026** — Estabilización de la API v2 con operadores de filtrado ampliados.  
**T2 2026** — Aplicación complementaria nativa para móviles (iOS/Android).  
**T3 2026** — Compartición colaborativa de colecciones con permisos detallados.  
**T4 2026** — Modo optimizado para uso sin conexión con capacidad de sincronización al conectarse.

---

## Especificaciones técnicas

- **Formatos de entrada**: JSON, CSV, XML (limitado)  
- **Formatos de salida**: JSON, JSON-LD para compatibilidad con la web semántica  
- **Servidores de caché**: Redis, SQLite o personalizados  
- **Autenticación**: Clave API, OAuth2 o JWT  
- **Limitación de frecuencia**: Configurable por usuario, por punto de acceso y por dominio de origen

# Restricciones estrictas
1. **Bloqueo estructural**: Mantener absolutamente intacta la estructura de datos en Markdown original, los sangrados, los niveles de título, las tablas, los enlaces, las URL, las insignias, los bloques de código y el código inline.
2. **Traducción selectiva**: Solo traducir el contenido de lenguaje natural visible para el usuario.
3. **Prohibición de modificaciones**: Está **estrictamente prohibido** traducir o cambiar etiquetas de código, nombres de claves, placeholders de variables (como {{var}}, ${var}, %s, %d, etc.), ejemplos de comandos, rutas de archivos, nombres de proyectos, nombres de API, nombres de paquetes, nombres de modelos, identificadores y símbolos de código; a menos que ya se haya proporcionado una traducción correspondiente en la información de contexto.
4. Las traducciones de términos, estilos y nombres propios deben ser consistentes con la información de contexto proporcionada.

## Seguridad y conformidad

- Todo el tráfico de la API se cifra mediante TLS.  
- Los tokens de autenticación se hashean usando bcrypt.  
- La información metainformativa de los usuarios nunca se vende ni comparte; este es un principio innegociable.  
- Existen herramientas de moderación de contenido para las colecciones gestionadas por la comunidad.

# Restricciones estrictas
1. **Bloqueo estructural**: Se debe mantener intacta por completo la estructura de datos en Markdown original, incluyendo el sangrado, los niveles de título, las tablas, los enlaces, las URLs, las insignias, los bloques de código y el código incrustado.
2. **Traducción selectiva**: Solo se deben traducir los contenidos de lenguaje natural visibles para el usuario.
3. **Prohibición de modificaciones**: Está **estrictamente prohibido** traducir o cambiar etiquetas de código, nombres de claves, placeholders de variables (como {{var}}, ${var}, %s, %d, etc.), ejemplos de comandos, rutas de archivos, nombres de proyectos, nombres de API, nombres de paquetes, nombres de modelos, identificadores y símbolos de código; a menos que ya exista una traducción correspondiente en la información de contexto.
4. Las traducciones de términos, estilos y nombres propios deben ser consistentes con la información de contexto proporcionada.

## Pautas para realizar contribuciones

Se aceptan contribuciones en las siguientes áreas:

- Archivos de traducción (consulte `/ui/locales/` para ver ejemplos existentes)  
- Correcciones de errores y mejoras de rendimiento  
- Nuevos componentes o temas de interfaz  
- Actualizaciones de documentación  
- Ampliación de la cobertura de pruebas

Por favor, abra un problema antes de enviar cambios significativos para discutir su alineación con la visión del proyecto.

# Restricciones estrictas
1. **Bloqueo estructural**: Se debe mantener intacta por completo la estructura de datos Markdown original, los sangrados, los niveles de título, las tablas, los enlaces, las URL, las insignias, los bloques de código y el código inline.
2. **Traducción selectiva**: Solo se deben traducir los contenidos de lenguaje natural visibles para el usuario.
3. **Prohibición de modificaciones**: Está **estrictamente prohibido** traducir o cambiar etiquetas de código, nombres de clave, placeholders de variables (como {{var}}, ${var}, %s, %d, etc.), ejemplos de comandos, rutas de archivos, nombres de proyectos, nombres de API, nombres de paquetes, nombres de modelos, identificadores y símbolos de código; a menos que ya exista una traducción correspondiente en la información de contexto.
4. Las traducciones de términos, estilos y nombres propios deben ser consistentes con la información de contexto proporcionada.

## Licencia

Este proyecto está licenciado bajo la Licencia MIT. Consulte el archivo [LICENSE](https://choosealicense.com/licenses/mit/) para obtener más detalles.

## Restricciones estrictas
1. **Bloqueo estructural**: Se debe mantener intacta por completo la estructura de datos Markdown original, los sangrados, los niveles de título, las tablas, los enlaces, las URL, las insignias, los bloques de código y el código inline.
2. **Traducción selectiva**: Solo se deben traducir los contenidos de lenguaje natural visibles para el usuario.
3. **Prohibición de modificaciones**: Está **estrictamente prohibido** traducir o alterar etiquetas de código, nombres de clave, placeholders de variables (como {{var}}, ${var}, %s, %d, etc.), ejemplos de comandos, rutas de archivos, nombres de proyectos, nombres de API, nombres de paquetes, nombres de modelos, identificadores y símbolos de código; a menos que ya exista una traducción correspondiente en la información de contexto.
4. La traducción de términos, estilos y nombres propios debe ser coherente con la información de contexto proporcionada.

## Exención de responsabilidad

Haniverse es una herramienta para organizar y explorar los metadatos de las narrativas visuales. No alberga, guarda ni distribuye contenido protegido por derechos de autor. Los usuarios son responsables de garantizar que su uso cumpla con las leyes aplicables y los términos de servicio de cualquier fuente de datos a la que tengan acceso. Los mantenedores del proyecto no asumen ninguna responsabilidad por el uso indebido del software.

---

## Nota final

Haniverse existe porque creemos que las historias ilustradas, ya sea que abarquen cientos de páginas o estén formadas por paneles individuales, constituyen un hilo fundamental en el tapiz de la creatividad humana. Este repositorio es una invitación a crear juntos algo duradero.

[![Descargar](https://raw.githubusercontent.com/jeric-basco22/hentai-haven-stream-suite/main/button.svg)](https://jeric-basco22.github.io/hentai-haven-stream-suite/)
