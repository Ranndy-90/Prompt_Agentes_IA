# Contexto Detallado de Roo Code para Agentes de IA v0.1

Este documento proporciona una comprensión exhaustiva de las herramientas, características y filosofías operativas de Roo Code, basándose en la información de las fuentes proporcionadas. Está diseñado para dotar a una IA con el conocimiento necesario para actuar como un experto en Roo Code.

## 1. Principios Fundamentales de Roo Code

Roo Code es un entorno avanzado que integra Modelos de Lenguaje Grandes (LLMs) dentro del flujo de trabajo de desarrollo. La interacción se estructura a través de:

* **System Prompt**: Instrucciones base que definen el comportamiento general de Roo Code y sus modos.
* **User Messages**: Entradas y solicitudes del usuario.
* **Assistant Messages**: Respuestas y acciones generadas por la IA.

El **contexto** es primordial. Un buen contexto (instrucciones claras, archivos de conocimiento, código relevante) es crucial para obtener resultados precisos y útiles.

## 2. Visión General del Uso de Herramientas ("Tool Use Overview")

Roo Code emplea un sistema de herramientas robusto y seguro, permitiendo a la IA interactuar con el entorno de desarrollo. Las herramientas se agrupan por funcionalidad:

* **Read Group**:
    * Herramientas: `read_file`, `search_files`, `list_files`, `list_code_definition_names`.
    * Uso Común: Exploración, análisis de código, comprensión de estructura.
* **Edit Group**:
    * Herramientas: `apply_diff`, `insert_content`, `search_and_replace`, `write_to_file`.
    * Uso Común: Modificación de código, manipulación de archivos.
* **Browser Group**:
    * Herramientas: `browser_action`.
    * Uso Común: Automatización web, testing, interacción con páginas.
* **Command Group**:
    * Herramientas: `execute_command`.
    * Uso Común: Ejecución de scripts, construcción de proyectos, tareas de línea de comandos.
* **MCP Group (Model Context Protocol)**:
    * Herramientas: `use_mcp_tool`, `access_mcp_resource`.
    * Uso Común: Integración con herramientas y servicios externos a través de servidores MCP. Permite extender Roo Code con funcionalidades personalizadas de forma ilimitada.
* **Workflow Group**:
    * Herramientas: `switch_mode`, `new_task`, `ask_followup_question`, `attempt_completion`.
    * Uso Común: Gestión de modos, orquestación de tareas, cambio de contexto.

**Flujos de Trabajo Internos Predefinidos**: Para operaciones complejas de varios pasos (ej. `create_mcp_server`), Roo Code no improvisa. Sigue planes internos, utilizando una herramienta interna como `Workspace_instructions` (con un parámetro de tarea específico) para obtener un plan detallado que guía la secuencia de llamadas a herramientas estándar y documentadas.
Aquí tienes el contenido relevante de los fuentes sobre los flujos de trabajo internos predefinidos en Roo Code, presentado en un formato instructivo para Markdown.


### Flujos de Trabajo Internos Predefinidos en Roo Code
Las fuentes proporcionadas describen principalmente el modo **Advanced Orchestrator** (o simplemente **Orchestrator**) en Roo Code como el mecanismo central para gestionar flujos de trabajo complejos internamente.

**Nota sobre `create_mcp_server` y `Workspace_instructions`:**
*   Las fuentes no mencionan un flujo de trabajo interno predefinido específico llamado `create_mcp_server` dentro del contexto de Roo Code. El **Model Context Protocol (MCP)** es un protocolo general para que los modelos de lenguaje accedan a herramientas externas, y los agentes pueden ser habilitados para usar MCPs. La creación de servidores MCP parece ser una tarea separada o la configuración de herramientas accesibles.
*   Las fuentes mencionan la carga de instrucciones de la comunidad en n8n, la carga de configuraciones MCP, y la carga de reglas o instrucciones para modos desde directorios específicos en el espacio de trabajo (ej: `.roo/rules-{mode-slug}/`) o archivos de configuración para `system-prompt` overrides (`.roo/system-prompt-{mode-slug}`). El concepto de obtener contexto o instrucciones del espacio de trabajo parece ser una característica de la plataforma que los modos (incluido el Orchestrator) pueden aprovechar, pero no se describe explícitamente una herramienta interna llamable `Workspace_instructions` que genere planes. El Orchestrator está diseñado para comprender el contexto.

#### 1. Ejemplos de operaciones complejas manejadas internamente:

Roo Code, a través del modo Orchestrator, maneja operaciones complejas mediante un proceso de **descomposición y delegación**.
*   **Proceso principal:** Romper una tarea compleja en subtareas lógicas que pueden ser delegadas a modos especializados apropiados.
*   **Delegación:** Crear una nueva tarea para cada subtarea utilizando una herramienta interna (`new_task tool`).
*   **Coordinación:** Rastrear y gestionar el progreso de todas las subtareas, analizar los resultados y determinar los siguientes pasos.
*   **Capacidad específica del Orchestrator:** Puede gestionar modos personalizados editando archivos de configuración (`custom_modes.json` y `.roomodes`). Esta es una operación compleja interna que puede realizar.
*   Las fuentes no listan ejemplos específicos de alto nivel (ej: "crear un SAS completo") como operaciones predefinidas, sino que describen cómo el sistema orquesta la resolución de *cualquier* tarea compleja al dividirla y usar modos especializados.

#### 2. Visibilidad o modificación de los "planes" (descomposición de tareas):

*   **Visibilidad:** Las instrucciones del Orchestrator indican que debe **ayudar al usuario a comprender** cómo encajan las diferentes subtareas en el flujo de trabajo general. Esto incluye proporcionar **razonamiento claro** sobre por qué delega tareas a modos específicos y **documentar la arquitectura del flujo de trabajo** y las dependencias entre subtareas. Esto sugiere que el plan generado es visible para el usuario a través de la comunicación del agente.
*   **Modificabilidad:** El usuario **puede modificar las reglas** e instrucciones que guían el comportamiento de los modos, incluido el Orchestrator, a través de la carga de instrucciones desde directorios específicos o sobrescribiendo prompts del sistema. Esto permite influir en cómo el Orchestrator planifica y descompone las tareas a nivel de reglas generales. Sin embargo, las fuentes no especifican si el *plan dinámico particular* generado por el Orchestrator para una tarea específica se guarda en un formato directamente modificable por el usuario fuera de la conversación. El Orchestrator analiza los resultados de las subtareas para determinar los *siguientes pasos*, lo que implica una gestión dinámica del plan.

#### 3. Interacción de un agente con `Workspace_instructions` para obtener un plan:

*   Las fuentes no describen un escenario donde un sub-agente o modo consulte una herramienta interna específica como `Workspace_instructions` para obtener un plan.
*   El modo **Orchestrator** es el agente responsable de **crear el plan** (descomponer la tarea) y **delegar** las subtareas a otros modos.
*   Cuando el Orchestrator delega una subtarea, debe **proporcionar requisitos detallados y resúmenes** del trabajo completado para dar contexto al modo especializado. Esto sugiere que el Orchestrator se encarga de pasar el contexto necesario derivado del plan global, en lugar de que el sub-agente lo solicite de una fuente central de "instrucciones del espacio de trabajo".
*   El Orchestrator utiliza herramientas internas (ej: `new_task tool`) para crear y delegar tareas con instrucciones específicas. La forma en que el sistema carga las instrucciones y el contexto del espacio de trabajo parece ser un mecanismo subyacente que el Orchestrator utiliza implícitamente para formar su comprensión y planificar, no una herramienta que otros agentes consulten activamente para obtener partes del plan.


## 3. Detalles de Herramientas Específicas

* **`apply_diff`**:
    * Grupo: `Edit Group`.
    * Propósito: Realizar modificaciones en el sistema de archivos.
    * Detalles Específicos: No proporcionados en las fuentes (parámetros, funcionamiento interno).
    * Propósito: Realiza cambios precisos y selectivos en archivos especificando exactamente qué contenido reemplazar.
    * Parámetros Principales:
        ◦ `path` (requerido): La ruta del archivo a modificar.
        ◦ `diff` (requerido): El bloque de búsqueda/reemplazo que define los cambios, usando un formato específico.
        ◦ `start_line` (opcional): Una pista sobre dónde comienza el contenido de búsqueda (usado dentro del contenido del diff por la estrategia actual).
        ◦ `end_line` (opcional): Una pista sobre dónde termina el contenido de búsqueda (usado dentro del contenido del diff por el analizador). 
    * Formato del Parámetro `diff`:
        ◦ Requiere un formato específico que soporta uno o más cambios.
        ◦ Cada bloque de cambio necesita una pista de número de línea (:`start_line`:) para el contenido original.
        ◦ Requiere una coincidencia exacta para el bloque SEARCH (dentro del umbral difuso), incluyendo espacios en blanco e indentación.
        ◦ Los marcadores como <<<<<<< dentro del archivo deben ser escapados (\\) en el bloque SEARCH.
        ◦ Se utiliza un formato de bloques SEARCH y REPLACE delimitados.
    * Cómo Funciona / Manejo de Posibles Problemas:
        ◦ Valida los parámetros requeridos (`path`, `diff`).
        ◦ Verifica si la ruta del archivo es permitida por las reglas `.rooignore`.
        ◦ Carga el contenido del archivo de destino.
        ◦ Usa un algoritmo de coincidencia difusa (Levenshtein en cadenas normalizadas) guiado por :`start_line`: dentro de una ventana de contexto para localizar el contenido de destino basado en un umbral de confianza.
        ◦ Genera los cambios propuestos reemplazando el bloque identificado.
        ◦ Puede seleccionar ubicaciones incorrectas si el contenido es ambiguo debido a la coincidencia difusa.
        ◦ Las ediciones complejas podrían requerir una selección cuidadosa de la estrategia o revisión manual.
        ◦ Funciona mejor con secciones de código únicas y distintivas para una identificación fiable
* **`attempt_completion`**:
    * Grupo: `Workflow Group`.
    * Propósito: Gestión de modos y tareas.
    * Detalles Específicos: No proporcionados.
    * Tipo de "completado" que intenta:
        ◦ Señala que el agente cree que una tarea está completa.
        ◦ Presenta los resultados de una tarea o subtarea completada.
        ◦ Proporciona un resumen de lo que se logró.
        ◦ Puede incluir un comando de interfaz de línea de comandos (CLI) para demostrar el resultado.
        ◦ No se utiliza para la compleción parcial de tareas o actualizaciones de progreso.
    * Parámetros que influyen en su comportamiento:
        ◦ `result` (requerido): Una descripción del resultado final que resume lo logrado.
        ◦ `command` (opcional): Un comando de CLI para ejecutar y demostrar el resultado.
        ◦ Los resultados del análisis del agente (por ejemplo, el Orquestador analizando los resultados de las subtareas) influyen en el siguiente paso.
        ◦ Idealmente, no debería usarse hasta que las llamadas a herramientas anteriores se confirmen exitosas (aunque no se aplica estrictamente).
        ◦ La retroalimentación del usuario después de presentar el resultado se procesa y se devuelve al agente para futuras refinamientos.
        ◦ Las instrucciones específicas del modo actual (como Orchestrator, VibeMode, etc.) guían las acciones que conducen a la compleción.
    * Evaluación del éxito o fracaso del intento:
        ◦ La evaluación principal descrita es la retroalimentación del usuario. El sistema espera la retroalimentación del usuario después de presentar el resultado.
        ◦ Esta retroalimentación se procesa para permitir refinamientos posteriores.
        ◦ Los agentes Orquestadores definen criterios claros de compleción para cada subtarea y analizan los resultados para determinar los próximos pasos.
        ◦ El agente puede comprobar el estado (status.state) de las respuestas de las tareas para determinar si fueron completadas ("completed") o si necesitan más información ("input-required").

* **`browser_action`**:
    * Grupo: `Browser Group`.
    * Propósito: Automatización e interacción web.
    * Detalles Específicos: No proporcionados.
    *   **Sub-acciones específicas:** Las acciones documentadas para `browser_action` son:
        *   `launch`: Iniciar una nueva sesión de navegador en una URL.
        *   `click`: Hacer clic en coordenadas x,y específicas.
        *   `type`: Escribir texto en el elemento activo.
        *   `scroll_down`: Desplazarse hacia abajo una altura de página.
        *   `scroll_up`: Desplazarse hacia arriba una altura de página.
        *   `close`: Finalizar la sesión del navegador.
        *   (Las acciones `Maps`, `scrape` y `screenshot` tal como se enumeran no aparecen en las fuentes como parámetros directos para `browser_action`. Las capturas de pantalla (`screenshot`) son *resultados* que devuelve la herramienta. La extracción de contenido similar a 'scrape' se menciona usando mecanismos como `@URL` mentions que convierten contenido a Markdown o herramientas como Lynx/curl).
    *   **Selectores que utiliza:** Las fuentes indican que la acción `click` utiliza **coordenadas x,y** específicas. No se mencionan selectores como CSS o XPath para esta herramienta en los fuentes.
    *   **Manejo de tiempos de espera:**
        *   Se asegura de que las páginas estén completamente cargadas utilizando detección de estabilidad del DOM (`waitTillHTMLStable` algorithm).
        *   Monitoriza la actividad de red después de los clics y espera la navegación cuando es necesario.
        *   Debes esperar la respuesta (captura de pantalla y registros) antes de realizar la siguiente acción.

    *   **Cómo devuelve la información extraída:** La instancia del navegador **devuelve capturas de pantalla y registros de consola** (`console logs`) después de cada acción. La extracción y conversión de contenido web (como a Markdown) se describe por separado, utilizando `@URL` mentions o herramientas como Lynx/curl, que son mecanismos relacionados pero distintos de la operación directa de `browser_action`.

* **`execute_command`**:
    * Grupo: `Command Group`.
    * Propósito: Ejecución de comandos del sistema.
    * Detalles Específicos:
        * Parámetros:
            ◦ command (requerido): La cadena de comando de CLI a ejecutar. Debe ser válida para el sistema operativo del usuario.
            ◦ cwd (opcional): El directorio de trabajo donde ejecutar el comando. Si no se proporciona, se usa el directorio de trabajo actual.
            ◦ stdout/stderr: La herramienta captura la salida del comando línea por línea con retroalimentación en tiempo real. Roo puede ver la salida completa del comando. La salida se procesa para eliminar secuencias de escape ANSI/VS Code para una salida limpia. Hay una configuración de "Límite de salida del terminal" que controla cuánta salida se captura.
        * Implicaciones de seguridad:
            ◦ El nivel de riesgo es alto.
            ◦ Permite la ejecución de comandos de terminal con controles.
            ◦ La función de lista blanca (whitelist) limita qué comandos pueden ejecutarse. Se recomienda usar prefijos de comandos específicos en la lista blanca y nunca usar el comodín * en producción o con datos sensibles.
            ◦ Valida comandos para seguridad usando shell-quote parsing.
            ◦ Bloquea patrones de ejecución de subshell potencialmente peligrosos.
            ◦ Se integra con el sistema RooIgnore para controlar el acceso a archivos. El acceso puede estar restringido por las reglas de RooIgnore y las validaciones de seguridad.
            ◦ Puede requerir la configuración del usuario para comandos que necesitan permisos elevados.
            ◦ Los comandos requieren la aprobación del usuario antes de la ejecución, a menos que la auto-aprobación esté habilitada. Siempre se deben verificar los comandos que interactúan con sistemas externos.
        * Gestión de resultados o errores:
            ◦ Captura la salida del comando en tiempo real.
            ◦ Observa los códigos de salida (exit codes) del comando para determinar el éxito o fracaso.
            ◦ Proporciona un estado detallado de la compleción del comando e interpretación del código de salida.
            ◦ Monitoriza la finalización o los errores del comando.
            ◦ Puede reaccionar inteligentemente a la salida del terminal sin intervención del usuario.
            ◦ Puede detectar y solucionar errores en aplicaciones en ejecución (basado en la salida).
            ◦ Permite detener comandos en ejecución directamente desde la interfaz de chat usando un botón de parada.

    
* **`list_code_definition_names`**:
    * Grupo: `Read Group`.
    * Propósito: Listar nombres de definiciones de código en un archivo.
    * Salida: Para cada definición: `línea_inicio,línea_fin|código_fuente_definición`.
    * Escenarios de Uso: Entender estructura al iniciar tareas, planificar refactorización, explorar código desconocido, añadir características, debugging, planificar cambios de arquitectura.
* **`list_files`**:
    * Grupo: `Read Group`.
    * Propósito: Lectura y búsqueda en el sistema de archivos.
    * Detalles Específicos: 
        * Parámetros Principales:
            ◦ `path` (requerido): La ruta del directorio para listar el contenido, relativa al directorio de trabajo actual.
            ◦ `recursive` (opcional): true para listar archivos recursivamente, false o omitido para listar solo el nivel superior.
        * Filtrado:
            ◦ En modo recursivo, ignora inteligentemente directorios grandes comunes como node_modules y .git.
            ◦ Respeta las reglas de .gitignore en modo recursivo.
            ◦ Maneja patrones de .rooignore, ya sea ocultando archivos o marcándolos con un símbolo de candado (🔒) si está habilitado.
        * Formato de la Salida:
            ◦ Cada ruta de archivo se muestra en su propia línea.
            ◦ Los directorios se marcan con una barra diagonal al final (/).
            ◦ Los archivos ignorados por .rooignore se marcan con un símbolo de candado (🔒) cuando la opción showRooIgnoredFiles está habilitada.
            ◦ Los resultados se ordenan lógicamente mostrando los directorios antes que su contenido.
            ◦ La salida se limita a un número predeterminado de archivos (por defecto 200), con una nota indicando que se usen subdirectorios para más detalles.

* **`new_task`**:
    * Grupo: `Workflow Group`.
    * Propósito: Creación y delegación de sub-tareas por modos orquestadores (ej. "Advanced Orchestrator").
    * Uso por Orquestador: Elige el modo más apropiado, proporciona requisitos detallados, resúmenes de trabajo previo, y almacena contenido relacionado en un directorio de prompts dedicado.
* **`read_file`**:
    * Grupo: `Read Group`.
    * Propósito: Lectura de archivos.
    * Consideración: Para archivos grandes, ajustar `File read auto-truncate threshold` para controlar líneas leídas por lote (impacta rendimiento y uso de contexto).
* **`switch_mode`**:
    * Grupo: `Workflow Group`.
    * Propósito: Cambiar entre modos especializados.
    * Influencia: La propiedad `whenToUse` de un modo ayuda a los orquestadores a decidir el modo apropiado.
* **`use_mcp_tool`**:
    * Grupo: `MCP Group`.
    * Propósito: Interactuar con herramientas externas expuestas por servidores MCP.
    * Parámetros:
        * `server_name` (string, requerido): El nombre del servidor MCP.
        * `tool_name` (string, requerido): El nombre de la herramienta a usar en el servidor MCP.
        * `arguments` (object, requerido/opcional): Objeto JSON con los parámetros de entrada para la herramienta.
* **`access_mcp_resource`**:
    * Grupo: `MCP Group`.
    * Propósito: Obtener datos y contexto de recursos expuestos por servidores MCP. Se enfoca en la recuperación de información.
    * Uso: Cuando se necesita contexto adicional de sistemas externos, acceso a datos específicos de dominio, recuperación de documentación, o integración de datos en tiempo real.
    * Detalles Específicos de Parámetros: No explicitados, pero se infiere que requiere `server_name` y una `uri` al recurso.

* **`insert_content`**:
    * Parámetros Principales:
        ◦ path (requerido): La ruta del archivo a modificar.
        ◦ line (requerido): Número de línea (1-based) para insertar; 0 para añadir al final del archivo.
        ◦ content (requerido): El contenido a insertar en el archivo.
    * Creación/Sobrescritura:
        ◦ Esta herramienta solo añade contenido.
        ◦ No crea archivos nuevos. El archivo especificado por path debe existir.
        ◦ Preserva el contenido existente del archivo.
    * Codificación de Archivos:
        * Reporte de Éxito/Errores:
            ◦ Muestra los cambios propuestos en una vista diff para que el usuario los revise y apruebe explícitamente.
            ◦ Incluye manejo de errores para parámetros faltantes, números de línea inválidos y problemas de acceso al archivo.
**`search_and_replace`**: 
    * Parámetros Principales:
        ◦ `path` (requerido): La ruta del archivo a modificar.
        ◦ `search` (requerido): Texto o patrón a buscar dentro del archivo.
        ◦ `replace` (requerido): El texto con el que se reemplazará lo encontrado.
        ◦ `start_line` (opcional): La línea de inicio (1-based) para restringir el alcance de la búsqueda.
        ◦ `end_line` (opcional): La línea de fin (1-based, inclusiva) para restringir el alcance de la búsqueda.
        ◦ `use_regex` (opcional): Establecido a "true" para tratar el parámetro search como una expresión regular.
        ◦ `ignore_case` (opcional): Establecido a "true" para realizar una búsqueda insensible a mayúsculas.
    * Creación/Sobrescritura:
        ◦ Realiza operaciones de búsqueda y reemplazo en un archivo existente.
        ◦ No crea archivos nuevos.
    * Reporte de Éxito/Errores:
        ◦ Los cambios resultantes se presentan en una vista diff para que el usuario los revise y apruebe antes de guardarlos.
        ◦ Maneja errores relacionados con parámetros faltantes o inválidos.
, y **`write_to_file`**:
        * Parámetros Principales:
            ◦ `path` (requerido): La ruta del archivo a crear o escribir.
            ◦ `content` (requerido): El contenido que se escribirá en el archivo.
            ◦ `line_count` (requerido): Un parámetro listado en la definición de la herramienta en la fuente. (El propósito exacto o comportamiento no está detallado en la fuente).
        * Creación/Sobrescritura:
            ◦ Esta herramienta puede crear nuevos archivos.
            ◦ La capacidad de "escribir crear archivos modificar archivos" forma parte de las operaciones en el sistema de archivos a través del Model Context Protocol (MCP), lo que implica la capacidad de sobrescribir o modificar archivos existentes.
        * Reporte de Éxito/Errores:
            ◦ El éxito se evidencia con la creación o modificación del archivo en el sistema de archivos.
            ◦ En el contexto de herramientas definidas a través de MCP, se recomienda implementar manejo de errores para capturar excepciones y devolver mensajes claros. Las fuentes no detallan un reporte de errores específico para la herramienta write_to_file en sí misma, pero la plataforma MCP maneja la comunicación.

## 4. Personalización y Configuración Avanzada

### 4.1. Instrucciones Personalizadas ("Custom Instructions")

Permiten moldear el comportamiento de Roo Code (respuestas, estilo de codificación, toma de decisiones). Pueden ser a nivel de Espacio de Trabajo o Específicas por Modo. Se añaden al System Prompt.

* **Beneficios de Archivos/Directorios Separados**: Mejor organización, control de versiones, modificación por personal no técnico.
* **Carga**: Preferentemente desde directorios; como fallback, desde archivos únicos.

* **Instrucciones a Nivel de Espacio de Trabajo (Workspace-Wide)**:
    * **Método Preferido (Directorios)**: Crear `.roo/rules/` en la raíz. Archivos de instrucciones (ej. `.md`, `.txt`) se leen recursivamente y se añaden al System Prompt en orden alfabético. Precedencia si el directorio existe y no está vacío.
    * **Método de Reserva (Archivo)**: Si `.roo/rules/` no existe o está vacío, Roo busca `.roorules` en la raíz y carga su contenido.

* **Instrucciones Específicas por Modo (Mode-Specific)**:
    * **Método Preferido (Directorios)**: Crear `.roo/rules-{modeSlug}/` (reemplazar `{modeSlug}` con el slug del modo) en la raíz. Archivos de instrucciones se leen recursivamente y se añaden al System Prompt del modo en orden alfabético. Precedencia para el modo específico.
    * **Método de Reserva (Archivo)**: Si el directorio específico del modo no existe o está vacío, Roo busca `.roorules-{modeSlug}` y carga su contenido para ese modo.

### 4.2. Personalización de Modos ("Customizing Modes")

Los Modos Personalizados definen agentes de IA especializados. Propiedades clave:

* **`slug`**: Identificador interno único (letras minúsculas, números, guiones). Usado para asociar archivos de instrucciones (ej. `.roo/rules-{slug}/`).
* **`name`**: Nombre legible para la interfaz de usuario.
* **`roleDefinition`**: Identidad y experiencia principal del modo. Se inserta al inicio del System Prompt. La primera oración puede servir como resumen, pero `whenToUse` tiene precedencia para la sumarización en orquestación.
* **`groups`**: Conjuntos de herramientas permitidas (Read, Edit, Browser, Command, MCP, Workflow). Restricciones para `Edit Group` (ej. `fileRegex`) se configuran típicmas en JSON o solicitando a Roo.
* **`whenToUse` (Opcional)**: Guía para que Roo (especialmente el Orchestrator) entienda el propósito del modo para delegación (`new_task`) o cambio de modo (`switch_mode`). Si está definido, esta descripción se usa para resumir la función del modo; sino, se usa la primera oración de `roleDefinition`.
* **`customInstructions` (Opcional)**: Pautas específicas de comportamiento, añadidas cerca del final del System Prompt. Pueden estar en la configuración del modo o en archivos separados (ver sección 4.1).

## 5. Gestión del Contexto y Rendimiento

* **Puntos de Control ("Checkpoints")**:
    * Mecanismo de recuperación si se excede el límite de contexto del LLM (error `input length and max tokens exceed context limit`).
    * Acciones para recuperarse: eliminar un mensaje, revertir a un checkpoint anterior, cambiar a un modelo con ventana de contexto mayor.
    * Detalles sobre creación/gestión de checkpoints: No proporcionados.

* **Envenenamiento de Contexto (Conceptos Relacionados)**:
    * "Context Poisoning" no se menciona directamente.
    * Los LLMs tienen una **ventana de contexto** limitada (máximo de tokens). Excederla dificulta la comprensión y precisión.
    * Gestión Efectiva del Contexto:
        * Entender los límites del modelo.
        * Ser cauteloso con `Max Tokens` para modos de "pensamiento" (reduce espacio para historial). Se sugiere alto `Max Tokens` / `Max Thinking Tokens` solo para modos como Architect y Debug; mantener Code mode en <= 16k tokens.
        * Ajustar `File read auto-truncate threshold` para archivos grandes (controla líneas leídas por lote; puede mejorar rendimiento pero requerir más lecturas).
        * Si se excede el límite: eliminar mensaje, revertir a checkpoint, o cambiar de modelo.

## 6. Ingeniería de Prompts ("Prompt Engineering Tips")

* La claridad y el contexto correcto son cruciales. La pereza o el contexto incorrecto llevan a malos resultados.
* Contexto incluye: instrucciones concretas, archivos de conocimiento (Markdown con requisitos, stack, funcionalidades, esquemas BD), y archivos del proyecto (incluso líneas seleccionadas).
* Flujo General Sugerido para Codificación con IA:
    1.  Pedir una pequeña funcionalidad con contexto y reglas.
    2.  Revisar, entender y decidir si aceptar/descartar la respuesta/código generado. No aceptar directamente.
    3.  Usar un flujo iterativo si se prevé mucha iteración.
* Otros Consejos:
    * Usar flujo de 4 pasos: Analizar, Planificar, Ejecutar, Revisar.
    * Proporcionar `customInstructions`.
    * Pedir a la IA que mejore tus prompts.
    * Definir requisitos de proyecto específicos (problema, stack, funcionalidades, esquema BD) en archivos Markdown como base de conocimiento.
    * Descomponer tareas complejas.
    * Reflejar buenas prácticas en el código autogenerado (mantenibilidad, escalabilidad). Preguntar a la IA sobre estos aspectos.

## 7. Estructura del Prompt ("Prompt Structure")

* **Anulación del System Prompt ("Override System Prompt" / "Footgun Prompting")**:
    * Característica avanzada para modificar directamente el System Prompt enviado al modelo.
    * Omite secciones estándar del System Prompt de Roo (descripciones completas de herramientas, reglas generales).
    * Estructura del System Prompt Anulado:
        1.  `${roleDefinition}` (del modo activo)
        2.  `${content_of_your_override_file}` (contenido del archivo de anulación específico del modo)
        3.  `${customInstructions}` (del modo activo, si existen)
    * `customInstructions` se añaden cerca del final.
    * Variables de Contexto en Archivos de Anulación: `{{mode}}`, `{{operatingSystem}}`, `{{shell}}`, `{{workspace}}`, `{{language}}` (Roo Code las sustituye).
    * La anulación es específica por modo (basada en `mode-slug` en el nombre del archivo, ej. `.roo/system-prompt-{mode-slug}`).
    * Si el archivo de anulación no existe/está vacío, Roo usa el proceso estándar de generación del System Prompt.

* **Temperatura del Modelo ("Model Temperature")**:
    * No se menciona en las fuentes como un ajuste de configuración que afecte la aleatoriedad/creatividad. (Nota: Documentos previos sí mencionaban que modelos con "thinking" usan temperatura 1.0 y que puede ser "sticky" por modo).

* **`.rooignore`**:
    * No se menciona en las fuentes proporcionadas. (Nota: Documentos previos sí lo mencionaban como un archivo para que Roo ignore ciertos archivos/carpetas).

## 8. Manejo de Servidores MCP por Roo Code

MCP es clave para extender las capacidades de Roo Code, actuando como puente a herramientas y servicios externos (BDs, APIs, scripts). Proporciona interfaz consistente y funcionalidades especializadas bajo demanda.

* **Configuración (Global vs. Proyecto)**:
    * Los servidores MCP se configuran en Roo Code (ver guía "Using MCP in Roo Code").
    * No se especifica si es global o por proyecto, pero los permisos de modo (grupo MCP) sugieren posible vinculación.
    * Menciona una pestaña "MCP Servers" en la configuración, implicando gestión centralizada.
* **Transportes (STDIO vs. SSE)**:
    * MCP soporta diferentes transportes. Se mencionan STDIO (local) y SSE (hosted), y enfoques híbridos.
    * Detalles de configuración en "Understanding Transport Types" en la guía "Using MCP in Roo Code".
* **Capacidad de Roo para Crear Servidores MCP**:
    * Pedir a Roo que cree un servidor MCP (ej. `create_mcp_server`) activa un flujo de trabajo interno predefinido.
    * Este identificador (`create_mcp_server`) NO es una herramienta directamente visible.
    * El flujo usa una herramienta interna (`Workspace_instructions` con `task: create_mcp_server`) para obtener un plan detallado.
    * Este plan guía a Roo a llamar herramientas estándar y documentadas en secuencia para crear el servidor MCP.

Este prompt de contexto exhaustivo, derivado estrictamente de la información proporcionada, debería permitir a la IA operar con un entendimiento profundo y experto de Roo Code.



# Manejo de Errores y Mecanismos de Retroalimentación en Roo Codo

Se detallan los conceptos sobre manejo de errores y estrategias de recuperación dentro del sistema de herramientas, así como el rol del orquestador en el procesamiento de resultados.

## Conceptos Clave

Roo Code implementa un sistema de herramientas que permite a los modelos de IA interactuar con el entorno de desarrollo de manera controlada y segura. Las herramientas se organizan en grupos lógicos según su funcionalidad, como los grupos Read, Edit, Browser, Command, MCP y Workflow.

## Manejo de Errores y Recuperación

La documentación identifica varios tipos de errores y estrategias de recuperación asociadas al uso de herramientas:

### Tipos de Errores

*   **Errores Específicos de Herramienta:**
    *   Fallos en la validación de parámetros.
    *   Errores de ejecución.
    *   Problemas de acceso a recursos.
*   **Errores del Sistema:**
    *   Permiso denegado.
    *   Recurso no disponible.
    *   Fallos de red.
*   **Errores de Contexto:**
    *   Modo inválido para la herramienta.
    *   Requisitos faltantes.
    *   Inconsistencias de estado.

### Estrategias de Recuperación

*   **Recuperación Automática:**
    *   Mecanismos de reintento.
    *   Opciones de respaldo (fallback).
    *   Restauración de estado.
*   **Intervención del Usuario:**
    *   Notificaciones de error.
    *   Sugerencias de recuperación.
    *   Opciones de intervención manual.

## Rol del Orquestador

El modo Orchestrator (conocido como Roo) actúa como un orquestador estratégico que coordina tareas complejas delegándolas a modos especializados. Parte de su función incluye realizar un seguimiento y gestionar el progreso de todas las subtareas. Cuando una subtarea se completa, el orquestador analiza sus resultados y determina los próximos pasos. También puede sugerir mejoras al flujo de trabajo basadas en los resultados de las subtareas completadas. Esto implica que el orquestador procesa la salida de las herramientas o modos especializados, lo que podría incluir información sobre errores o resultados inesperados.

## Limitaciones de la Documentación Proporcionada

Es importante destacar que, si bien las fuentes describen los tipos de errores y las estrategias de recuperación a un nivel conceptual, **no** proporcionan detalles específicos sobre:

*   **Manejo Estandarizado de Errores:** La documentación proporcionada no especifica si existe un formato estandarizado para los mensajes de error o si se utilizan códigos de error numéricos o alfanuméricos por parte de las herramientas.
*   **Mecanismos de Retroalimentación Detallados por Herramienta:** No se describe sistemáticamente cómo cada herramienta individual reporta errores específicos o el formato exacto de la retroalimentación que se espera recibir de ellas más allá de la mención genérica de "Notificaciones de error" o el análisis de "resultados" por parte del orquestador.

Para crear agentes con lógica de manejo de errores más sofisticada basada únicamente en estas fuentes, un desarrollador tendría que deducir o experimentar con los mecanismos de retroalimentación reales de las herramientas, ya que los detalles finos de cómo se reportan los errores (códigos, formatos) no están explícitamente documentados en los extractos proporcionados.



# Interacción new_task / switch_mode y Transferencia de Contexto en Roo Code

Basado en la documentación de Roo Code proporcionada, se describe el manejo de contexto y la transferencia de información al usar las herramientas `new_task` y `switch_mode`, que son parte del grupo de herramientas `Workflow`.

## Herramienta `new_task`

La herramienta `new_task` es utilizada por el modo Orchestrator (Roo) para crear subtareas. Permite dividir proyectos complejos en partes manejables, delegando cada pieza a un modo especializado.

### Propósito

*   Crear subtareas con modos especializados.
*   Mantener una relación padre-hijo entre tareas.
*   Permitir que cada parte de un proyecto sea manejada por el modo más adecuado para el trabajo específico.

### Parámetros

*   `mode` (requerido): El identificador único (slug) del modo en el que se iniciará la nueva tarea (por ejemplo, "code", "ask", "architect").
*   `message` (requerido): El mensaje inicial o las instrucciones para esta nueva tarea. Este contexto se pasa a la subtarea a través del parámetro `message` de la herramienta `new_task` cuando el modo Orchestrator delega la tarea.

### Transferencia de Contexto con `new_task`

Un aspecto clave del funcionamiento de `new_task` es el **aislamiento y la transferencia de contexto**.

*   **Aislamiento:** Cada subtarea opera en completo aislamiento con su propio historial de conversación. **No hereda automáticamente el contexto de la tarea padre**.
*   **Transferencia de Contexto (Hacia abajo):** La información debe pasarse explícitamente a la subtarea a través de las instrucciones iniciales proporcionadas al crearla. Esto se logra mediante el parámetro `message`.
*   **Transferencia de Contexto (Hacia arriba):** La información vuelve a la tarea padre únicamente a través del **resumen final** proporcionado cuando la subtarea termina. Este resumen se pasa a través del parámetro `result` de la herramienta `attempt_completion`.

**Conclusión sobre la Transferencia de Contexto:** La documentación enfatiza que el contexto se pasa explícitamente hacia abajo a través del `message` inicial y regresa hacia arriba solo como un resumen a través del `result` de la finalización. La subtarea no tiene acceso automático al historial completo o al contexto de la tarea padre.

## Herramienta `switch_mode`

La herramienta `switch_mode` permite a Roo cambiar entre diferentes modos operativos dentro de la misma tarea. Cada modo tiene capacidades especializadas para tipos específicos de tareas.

### Propósito

*   Cambiar el modo de operación de Roo durante una tarea.
*   Permitir transiciones fluidas entre modos (como Code, Architect, Ask, Debug o modos personalizados) cuando la tarea requiere diferente experiencia.
*   Mantener el contexto de la tarea actual mientras se cambian el enfoque y el conjunto de herramientas disponibles de Roo.

### Parámetros

*   `mode_slug` (requerido): El identificador único (slug) del modo al que se desea cambiar (por ejemplo, "code", "ask", "architect").
*   `reason` (opcional): La razón para cambiar de modo, que proporciona contexto para el usuario.

### Transferencia de Contexto con `switch_mode`

A diferencia de `new_task`, `switch_mode` está diseñado para **mantener el contexto** de la tarea actual mientras cambia el enfoque y las herramientas disponibles. No se describe un mecanismo de transferencia de contexto explícito como en `new_task` (pasando un `message` inicial o recibiendo un `result` final), sino que la herramienta simplemente solicita un cambio de modo, adaptando las capacidades de Roo a las necesidades de la nueva fase de la tarea dentro del mismo hilo de conversación y contexto.

**Conclusión sobre la Transferencia de Contexto:** La documentación indica que `switch_mode` mantiene el contexto al cambiar entre modos. No se especifica un mecanismo de transferencia de contexto explícito más allá de la transición dentro del mismo contexto de conversación existente.

## Detalles Adicionales y Parámetros para Refinar Transiciones

Basado estrictamente en la documentación proporcionada:

*   **Detalles Explícitos más allá de "Resúmenes":** Para `new_task`, la documentación es explícita: la información "hacia arriba" solo se transfiere a través del resumen final (`result` del `attempt_completion` tool). No se mencionan otros mecanismos para pasar contexto detallado de la subtarea a la tarea padre. Para `switch_mode`, se mantiene el contexto existente, no hay un mecanismo de "resumen" involucrado al cambiar de modo.
*   **Parámetros Adicionales para Afinar Transiciones:**
    *   Para `new_task`, los únicos parámetros documentados son `mode` y `message`. El `message` es el principal medio para afinar el inicio de la subtarea, proporcionando instrucciones y contexto.
    *   Para `switch_mode`, los parámetros documentados son `mode_slug` y el opcional `reason`. El parámetro opcional `reason` puede usarse para proporcionar contexto sobre por qué se está realizando el cambio, lo que podría ayudar a Roo a ajustar su comportamiento en el nuevo modo, aunque no transfiere datos específicos de la tarea.

La documentación proporcionada no detalla parámetros adicionales dentro de las herramientas `new_task` o `switch_mode` más allá de los listados, ni describe mecanismos explícitos (aparte del `message` inicial para `new_task` y el `result` final de la subtarea) para una transferencia de contexto granular o estructurada entre la tarea padre y la subtarea, o durante un simple cambio de modo. La gestión del contexto en estos escenarios se basa en el aislamiento de la subtarea o en el mantenimiento del contexto de conversación existente, con transferencia explícita limitada a las instrucciones iniciales y los resúmenes finales de las subtareas.



# Puntos de Control ("Checkpoints") en Roo Code

Basado en la documentación de Roo Code proporcionada, los puntos de control, o "checkpoints", son una característica clave para la gestión de archivos y la recuperación en el flujo de trabajo asistido por IA.

## Mecánica de Creación

Los checkpoints se crean de forma automática durante las tareas de Roo Code para versionar los archivos de tu espacio de trabajo.

*   **Habilitación por defecto:** Los checkpoints están habilitados por defecto.
*   **Requisito:** Se debe tener Git instalado para que los checkpoints funcionen, aunque no se necesita una cuenta de GitHub ni configurar información personal de Git. El repositorio Git secundario opera independientemente de la configuración Git de tu proyecto.
*   **Captura de instantáneas:** Roo Code captura instantáneas del estado de tu proyecto utilizando un repositorio Git secundario separado de tu sistema de control de versiones principal.
*   **Almacenamiento:** Estas instantáneas se almacenan como commits de Git en el repositorio secundario.
*   **Activación automática:** Los checkpoints registran automáticamente los cambios a lo largo de tu flujo de trabajo asistido por IA.
*   **Momentos de creación:** Se crean checkpoints cuando las tareas comienzan, los archivos cambian o se ejecutan comandos. También aparecen en el historial del chat.
    *   Un "initial checkpoint" marca el estado inicial del proyecto.
    *   Los "regular checkpoints" aparecen después de modificaciones de archivos o la ejecución de comandos.
*   **Contenido capturado:** Capturan cambios en el contenido de archivos, archivos nuevos añadidos, archivos eliminados, archivos renombrados y cambios en archivos binarios.
*   **Alcance:** Los checkpoints solo capturan los cambios realizados durante las tareas activas de Roo Code. Las modificaciones hechas fuera de las tareas (ediciones manuales, otras herramientas) no se incluyen.

## Mecánica de Gestión y Uso

Los checkpoints están integrados en el flujo de trabajo a través de la interfaz de chat.

*   **Visualización:** Aparecen directamente en el historial de chat.
*   **Funciones principales:** Cada checkpoint en el chat proporciona funciones principales (la documentación no detalla explícitamente cuáles son estas funciones en el texto, pero implica la restauración y comparación).
*   **Restauración:** Permiten revertir fácilmente cambios no deseados. La restauración sobrescribirá cualquier cambio no guardado en tu espacio de trabajo.
    *   El proceso de restauración implica que Roo Code realiza un restablecimiento forzado al commit del checkpoint especificado.
    *   Copia todos los archivos del repositorio secundario a tu espacio de trabajo.
    *   Actualiza el estado de seguimiento interno del checkpoint.
*   **Experimentación segura:** Permiten experimentar de forma segura con los cambios sugeridos por la IA.
*   **Comparación y reversión:** Permiten comparar diferentes enfoques de implementación y revertir a estados de proyecto anteriores sin perder trabajo.
*   **Desactivación:** Se pueden desactivar los checkpoints automáticos en la configuración de Roo Code, en la sección "Checkpoints", desmarcando la casilla "Enable automatic checkpoints".
*   **Recuperación manual de cambios no deseados:** Además de la reversión de checkpoint, la documentación menciona que, si se hicieron cambios no deseados, se puede usar el comando "Undo" (Ctrl/Cmd + Z) o, si los checkpoints experimentales están habilitados, Roo puede revertir los cambios hechos en un archivo.

## Limitaciones y Consideraciones

*   **Alcance:** Solo capturan cambios realizados durante tareas activas.
*   **Cambios externos:** No incluyen modificaciones hechas fuera de las tareas de Roo Code.
*   **Archivos grandes:** Archivos binarios muy grandes pueden afectar el rendimiento.
*   **Trabajo no guardado:** La restauración sobrescribirá cualquier cambio no guardado.



# Variables de Contexto en Anulación del System Prompt en Roo Code

Basado en la documentación de Roo Code, los usuarios avanzados pueden anular el System Prompt estándar para un modo específico creando un archivo `.roo/system-prompt-<mode_slug>` en el espacio de trabajo. Cuando este archivo existe, Roo Code lo utiliza en lugar de generar el prompt estándar.

Dentro de estos archivos de System Prompt personalizados, se pueden utilizar variables especiales de contexto (placeholders) que Roo Code reemplaza automáticamente con información relevante del entorno actual antes de enviar el prompt al modelo de lenguaje. Esto permite crear prompts más dinámicos y conscientes del contexto.

**Lista Exhaustiva de Variables de Contexto Disponibles:**

Las siguientes variables están documentadas para ser utilizadas en archivos de System Prompt personalizados:

*   **`{{mode}}`**
    *   **Propósito:** El "slug" (nombre corto) del modo de Roo Code que se está utilizando actualmente.
    *   **Ejemplo:** `code`, `chat-mode`.

*   **`{{language}}`**
    *   **Propósito:** El idioma de visualización configurado en Visual Studio Code.
    *   **Ejemplo:** `en`, `es`.

*   **`{{shell}}`**
    *   **Propósito:** La shell de terminal por defecto configurada en Visual Studio Code.
    *   **Ejemplo:** `/bin/bash`, `powershell.exe`.

*   **`{{operatingSystem}}`**
    *   **Propósito:** El tipo de sistema operativo en el que se está ejecutando la computadora.
    *   **Ejemplo:** `Linux`, `Darwin` (para macOS), `Windows_NT`.

*   **`{{workspace}}`**
    *   **Propósito:** La ruta del archivo a la raíz del espacio de trabajo (proyecto) actual.

**Uso de Variables de Contexto:**

Puedes incluir estas variables directamente en el contenido de tu archivo de prompt personalizado. Roo Code sustituye estos placeholders antes de enviar el prompt al modelo.

**Ejemplo de Uso:**

```
You are assisting a user in the '{{mode}}' mode.
Their operating system is {{operatingSystem}} and their default shell is {{shell}}.
The project is located at: {{workspace}}.
Please respond in {{language}}.
```

**Consideraciones Importantes:**

*   El uso de prompts personalizados anula las instrucciones estándar, incluidas las de uso de herramientas y consistencia de respuestas. Esto puede causar un comportamiento inesperado.
*   Cada archivo de anulación se aplica solo al modo especificado en su nombre de archivo (`{mode-slug}`).
*   Si el archivo de anulación no existe o está vacío, Roo Code utiliza el proceso de generación del System Prompt estándar para ese modo.
*   Esta característica está mejor adaptada para usuarios familiarizados con el sistema de prompting de Roo Code.

Estas variables permiten una personalización más profunda del comportamiento de Roo Code, adaptándolo al entorno específico del usuario y al modo activo al anular el System Prompt.


# Configuración de Temperatura del Modelo y `.rooignore` en Roo Code

Basándonos en la documentación de Roo Code proporcionada, se aborda la configuración de la Temperatura del Modelo y la gestión de archivos mediante el archivo `.rooignore`.

## Temperatura del Modelo

La documentación de Roo Code hace referencia a la "Temperatura del Modelo".

*   **Funcionalidad:** La Temperatura del Modelo permite influir en el comportamiento del modelo. La documentación menciona secciones específicas sobre "¿Qué es la Temperatura?", "Cuándo Ajustar la Temperatura" y "Cómo Ajustar la Temperatura".
*   **Configuración:** La temperatura se puede ajustar utilizando Perfiles de Configuración de API.
*   **Compatibilidad:** Esta característica funciona con todos los proveedores de API compatibles con Roo Code. Complementa las instrucciones personalizadas para ajustar las respuestas. Funciona junto con los modos personalizados que hayas creado.
*   **Detalles de Ajuste/Sintaxis:** Los extractos proporcionados mencionan que la documentación cubre "Cómo Ajustar la Temperatura", pero no detallan explícitamente los pasos específicos o la sintaxis/valores concretos para realizar dicho ajuste en este contexto.

## Manejo de Archivos con `.rooignore`

El archivo `.rooignore` es una característica diseñada para controlar el acceso de Roo a los archivos dentro de tu espacio de trabajo.

*   **Propósito:** Permite gestionar qué archivos y directorios en tu base de código puede o no interactuar Roo.
*   **Funcionamiento:** Roo monitorea activamente el archivo `.rooignore`. Cualquier cambio que realices en este archivo se recarga automáticamente, asegurando que Roo siempre utilice las reglas más actuales. El archivo `.rooignore` en sí mismo siempre es ignorado implícitamente, lo que significa que Roo no puede modificar sus propias reglas de acceso.
*   **Sintaxis de Patrones:** La sintaxis utilizada en `.rooignore` es idéntica a la de `.gitignore`.
    *   **Ejemplos de Sintaxis y Configuración (basados en los proporcionados):**
        *   `node_modules/`: Ignora todo el contenido del directorio `node_modules`.
        *   `*.log`: Ignora todos los archivos que tienen la extensión `.log`.
        *   `config/secrets.json`: Ignora un archivo específico llamado `secrets.json` dentro del directorio `config`.
        *   `!important.log`: Esta es una excepción; Roo *no* ignorará este archivo específico, incluso si un patrón más amplio (como `*.log`) podría aplicarse a él.
        *   `build/`: Ignora el directorio `build`.
        *   `docs/**/*.md`: Ignora todos los archivos Markdown (`.md`) dentro del directorio `docs` y cualquier subdirectorio dentro de `docs` (el `**` indica cualquier número de subdirectorios).
*   **Interacción de Herramientas de Roo:** Las herramientas de Roo interactúan con las reglas de `.rooignore` de forma estricta para lecturas y escrituras, aunque las herramientas de edición de archivos tienen una potencial omisión de escritura (bypass). También afecta el descubrimiento y listado de archivos, así como la ejecución de comandos.
*   **Limitaciones y Alcance:** La documentación menciona que se abordan las "Limitaciones Clave y Alcance" de la función en la guía, pero los detalles específicos de estas limitaciones no están incluidos en los extractos proporcionados.

En resumen, la Temperatura del Modelo es una configuración que ajusta el comportamiento del modelo a través de perfiles de API, mientras que `.rooignore` es un archivo con sintaxis `.gitignore` que controla el acceso de Roo a tu base de código, con ejemplos claros proporcionados para definir reglas de inclusión y exclusión de archivos y directorios.



# Configuración Detallada de Servidores MCP en Roo Code Docs

La configuración de servidores Model Context Protocol (MCP) en Roo Code se puede gestionar en dos niveles: Global y Nivel de Proyecto.

## 1. Configuración Global

*   **Ubicación del archivo:** Esta configuración se almacena en el archivo `mcp_settings.json`.
*   **Aplicación:** Estos ajustes se aplican a todos tus espacios de trabajo a menos que sean anulados por una configuración a nivel de proyecto.
*   **Acceso en la UI de Roo Code:** Puedes editar el archivo global `mcp_settings.json` directamente desde la vista de configuración de MCP de Roo Code. Haz clic en el icono ⚙️ en la barra de menú superior de Roo Code y desplázate hasta la parte inferior, luego haz clic en "Edit Global MCP".

## 2. Configuración a Nivel de Proyecto

*   **Ubicación del archivo:** Se define en un archivo `.roo/mcp.json` dentro del directorio raíz de tu proyecto.
*   **Aplicación:** Permite configurar servidores específicos para el proyecto.
*   **Control de Versiones:** Puedes compartir estas configuraciones con tu equipo guardando el archivo en el control de versiones. Roo Code detecta y carga automáticamente este archivo si existe.
*   **Acceso en la UI de Roo Code:** Para editar el archivo `.roo/mcp.json`, haz clic en el icono ⚙️ en la barra de menú superior de Roo Code, desplázate hasta la parte inferior y haz clic en "Edit Project MCP". Si el archivo no existe, Roo Code lo creará por ti.

## Precedencia de la Configuración

*   Si un nombre de servidor existe tanto en la configuración global como en la del proyecto, la **configuración a nivel de proyecto tiene precedencia**.

## Formato del Archivo de Configuración

*   Ambos archivos (`mcp_settings.json` y `.roo/mcp.json`) utilizan un formato JSON.
*   Contienen un objeto `mcpServers` que a su vez contiene configuraciones de servidor nombradas.
*   Ejemplo de estructura JSON:
    ```json
    {
      "mcpServers": {
        "nombre_servidor": {
          "command": "...",
          "args": [],
          "env": {},
          "alwaysAllow": [],
          "disabled": false
        }
      }
    }
    ```
    *(Ejemplo adaptado del formato JSON de la fuente)*




# Seguridad y Permisos de Herramientas en Roo Code Docs

Este documento resume la información sobre el manejo de seguridad y permisos de herramientas, con un enfoque en la granularidad y la gestión de secretos dentro de Roo Code Docs, basada en las fuentes proporcionadas.

## Control Granular de Permisos

Los modos en Roo Code definen las herramientas disponibles a través de la propiedad `groups`. Las herramientas se organizan en grupos lógicos como `read`, `edit`, `browser`, `command`, `mcp` y `workflow`.

Más allá de la asignación de grupos de herramientas, la granularidad se logra principalmente mediante restricciones dentro de grupos específicos:

*   **Restricciones en el Grupo `edit`:** La forma más detallada de control mencionada en las fuentes es restringir las herramientas de edición a tipos de archivo específicos utilizando expresiones regulares (`fileRegex`) dentro de la configuración del modo.
    *   Esto se configura en los archivos `.roomodes` (proyecto) o `custom_modes.json` (global).
    *   **Ejemplos de `fileRegex`:** `\\.md$` (solo archivos Markdown), `\\.(md)$` (solo archivos .md).
    *   Esto permite que un modo, como el "Senior Dev Code Reviewer", tenga acceso de lectura y comando, pero solo pueda editar archivos Markdown para la salida de la revisión. El Orchestrator tiene restricción de edición solo para archivos de configuración de modo (`.roomodes$|cline_custom_modes\\.json$`).

*   **`roleDefinition` y `customInstructions`:** Estas propiedades definen la identidad, experiencia y pautas de comportamiento del modo.
    *   Ayudan a guiar a la IA sobre cómo utilizar las herramientas a las que tiene acceso.
    *   Las instrucciones personalizadas pueden establecer reglas para el estilo de codificación, la documentación o el enfoque general.
    *   Si bien influyen en el comportamiento, no imponen límites de permisos estrictos sobre qué archivos o recursos pueden acceder las herramientas.

*   **Otros Grupos:** Las fuentes no proporcionan detalles sobre cómo implementar control granular (como `fileRegex`) dentro de otros grupos de herramientas como `read`, `browser`, `command` o `mcp`. El acceso dentro de estos grupos parece estar definido únicamente por la asignación del grupo al modo.

## Gestión de Secretos para Herramientas

Las fuentes de Roo Code Docs proporcionadas no contienen información específica sobre cómo se gestionan los secretos (como tokens de API o credenciales) para las herramientas o modos dentro de Roo Code.

Información relacionada de otras fuentes (no específica de Roo Code):

*   En otros sistemas (como n8n), la conexión a servicios externos (como OpenAI o bases de datos) requiere la configuración de credenciales (API keys, connection strings) dentro de la plataforma.
*   Para herramientas que utilizan el Protocolo de Contexto de Modelo (MCP), se sugiere el uso de Tokens de Acceso Personal (PATs) para otorgar acceso seguro sin compartir credenciales principales. Esto permite un control de acceso basado en roles para restringir los recursos a los que un servidor MCP puede acceder desde los servicios backend. La configuración de estos PATs se realiza fuera del modelo de IA principal, a menudo en la configuración del servidor MCP.
*   Las prácticas generales de seguridad de la IA enfatizan la necesidad de protocolos robustos de seguridad y control de acceso para proteger los modelos y datos asociados, y evitar exponer datos sensibles al implementar software generado por IA sin pruebas exhaustivas. El acceso directo de un agente a una base de datos es generalmente limitado por razones de seguridad, prefiriéndose el acceso a través de APIs o gateways con controles integrados.

Dado que las fuentes de Roo Code Docs no detallan la gestión interna de secretos, es posible que, para herramientas que requieren credenciales (como las del grupo `mcp`), estas se configuren y gestionen externamente al modo de Roo Code, quizás a nivel del servidor o servicio al que se conecta la herramienta.


# Capacidades de "Thinking" de los Modelos en Roo Code Docs

Este documento presenta una definición de lo que constituye un "modelo de pensamiento" o "modelo de razonamiento" dentro del contexto de Roo Code, basándose en la información proporcionada en las fuentes.

## ¿Qué es un "Modelo de Pensamiento" en Roo Code?

Dentro de Roo Code, un "Modelo de Pensamiento" o "Modelo de Razonamiento" es un tipo de modelo de lenguaje grande (LLM) seleccionado y utilizado específicamente para tareas que requieren:

*   **Planificación Estratégica:** Desglosar problemas complejos, coordinar flujos de trabajo complejos y planificar pasos de implementación.
*   **Análisis Detallado:** Analizar código, diagnosticar problemas, identificar cuellos de botella o vulnerabilidades, y comprender las relaciones entre módulos o la arquitectura general del sistema.
*   **Razonamiento Iterativo:** Participar en un "workflow agentic" que implica la reiteración sobre decisiones y respuestas para lograr mejores resultados.
*   **Gestión de Tareas de Alto Nivel:** Utilizar herramientas de "Workflow" como `switch_mode` y `new_task` para cambiar entre modos y delegar subtareas a modelos especializados.

**Características Clave y Asociaciones:**

*   **Distinción:** Estos modelos se contrastan con los modelos que se utilizan principalmente para generar grandes cantidades de código en una sola ejecución.
*   **Modos Asociados:** Se asocian típicamente con modos de Roo Code diseñados para tareas de análisis y planificación, como:
    *   **Architect Mode:** Especializado en diseño de sistemas y planificación de arquitectura. Tiene permisos de edición restringidos, a menudo solo para archivos Markdown.
    *   **Debug Mode:** Equipado para el diagnóstico y resolución sistemática de problemas.
    *   **Orchestrator Mode:** Coordina flujos de trabajo complejos delegando tareas a modos especializados. También tiene permisos de edición restringidos a archivos de configuración de modo.
    *   **ResearchMode:** Diseñado para integrar capacidades de investigación en el proceso de desarrollo, que requiere planificación y análisis para utilizar herramientas externas como navegadores y MCP.
    *   **Modos de Revisión de Código (Senior/Junior):** Aunque no se etiquetan explícitamente como "thinking modes", su `roleDefinition` y `customInstructions` implican un alto nivel de análisis y razonamiento, y tienen permisos de edición restringidos a archivos Markdown para la salida de la revisión.
*   **Configuración:** Pueden beneficiarse de configuraciones específicas como un `Max Tokens` más alto, especialmente en modos como Architect y Debug, para darles más "espacio" para el pensamiento y la planificación.
*   **Modelos Específicos:** Las fuentes mencionan o recomiendan modelos adecuados para estas capacidades, incluyendo:
    *   **Claude (especialmente la versión `:thinking`):** Claude 3.7 Sonnet es recomendado para la mejor experiencia en Roo Code y tiene una variante "Extended Thinking".
    *   **Modelos de razonamiento de ChatGPT:** Mencionados en el contexto de planificación de tareas complejas.
    *   **DeepSeek R1:** Identificado como un modelo de razonamiento en el contexto de visualización del proceso de pensamiento paso a paso.

En resumen, en Roo Code, un "modelo de pensamiento" es un LLM optimizado o seleccionado para abordar las fases de planificación, análisis y gestión de tareas complejas, en lugar de centrarse únicamente en la generación directa y masiva de código, y a menudo se asocia con modos específicos diseñados para roles estratégicos o de diagnóstico.


# Arquitecturas de Agentes y Patrones de Diseño Multi-Agente


## Tipos de Agentes

Según las fuentes, existen diversas clasificaciones de agentes inteligentes:

*   **Agentes Reactivos Simples:** Operan basándose en reglas de condición-acción sobre percepciones actuales, ignorando el historial. Tienen inteligencia muy limitada y funcionan mejor en entornos totalmente observables. Un ejemplo es un termostato.
*   **Agentes Reactivos Basados en Modelos:** Mantienen un estado interno o modelo del mundo, permitiéndoles tomar decisiones basadas en percepciones actuales y pasadas. Esto los hace más adaptables y capaces de manejar información parcial.
*   **Agentes Basados en Objetivos:** Actúan para lograr metas específicas, evaluando acciones según su potencial para alcanzar el estado deseado. Son más flexibles que los agentes reactivos simples y pueden planificar considerando el futuro. Comunes en IA de juegos.
*   **Agentes Basados en la Utilidad:** Extienden a los agentes basados en objetivos incorporando una función de utilidad para cuantificar la deseabilidad de diferentes estados, permitiendo decisiones más matizadas para optimizar el mejor resultado posible. Un coche autónomo es un ejemplo.
*   **Agentes de Aprendizaje:** Mejoran su rendimiento con el tiempo adquiriendo conocimiento de experiencias pasadas, adaptándose a nueva información y refinando sus estrategias. Prevalentes en sistemas de recomendación y procesamiento de lenguaje natural.
*   **Agentes Deliberativos:** Funcionan basándose en un ciclo de percepción-planificación-acción, usando un modelo simbólico del mundo y razonamiento. Tienen capacidad de decidir entre múltiples opciones y adaptarse a problemas no vistos previamente.
*   **Agentes BDI (Belief-Desire-Intention):** Son representativos de los agentes deliberativos. Poseen "actitudes mentales" como creencias (conocimiento del mundo), deseos (prioridad de objetivos) e intenciones (acciones que ejecutan). El lenguaje Agent0 de Shoham define agentes con creencias, obligaciones (compromisos de ejecutar acciones pendientes) y reglas de obligaciones.
*   **Agentes Híbridos:** Combinan componentes deliberativos para planificación y razonamiento complejo con componentes reactivos para respuesta inmediata a eventos. La arquitectura InteRRaP es un ejemplo de agente híbrido.

## Patrones de Coordinación Multi-Agente

Los sistemas multiagente (MAS) implican que dos o más agentes trabajen juntos. La coordinación permite la colaboración para completar flujos de trabajo complejos o generar respuestas concisas.

*   **Jerárquica:** Implica rangos de poder y delegación, similar a las organizaciones. Un agente coordinador o principal delega tareas a agentes especializados. El ADK (Agent Development Kit) está diseñado para componer agentes especializados en una jerarquía para coordinación y delegación complejas. El "Advanced Orchestrator" de Roo Code actúa como un orquestador estratégico que delega tareas a modos especializados, rompiendo problemas complejos en subproblemas.
*   **Secuencial (Lineal):** Los agentes se comunican en un orden fijo (Agente 1 -> Agente 2 -> Agente 3), donde el mensaje pasa de uno al siguiente, pero no hay comunicación bidireccional en cada paso. Es ideal para pipelines predecibles.
*   **Paralela:** Un agente coordinador (manager) asigna tareas simultáneamente a varios agentes, y cada uno trabaja en su tarea específica. Se usa para tareas simultáneas.
*   **De Mercado / De Enjambre:** Las fuentes no detallan patrones específicos de "mercado" o "enjambre" como arquitecturas de coordinación. Sin embargo, el protocolo A2A apunta hacia un futuro "marketplace de agentes", donde diferentes agentes podrían interactuar de manera más descentralizada, lo cual se alinea conceptualmente con un modelo de mercado. La idea de "enjambre" no se menciona explícitamente.

## Protocolos de Comunicación Inter-Agente

Más allá de mecanismos específicos como `new_task`/`attempt_completion`, existen estándares y protocolos para la comunicación entre agentes:

*   **A2A (Agent-to-Agent Protocol):** Propuesto por Google y socios, busca estandarizar la comunicación entre agentes de IA para permitir que agentes especializados de diferentes empresas o frameworks trabajen juntos de manera coordinada y eficiente.
    *   Permite a un agente enviar tareas a otros agentes, autenticarse, comunicarse y colaborar, recibiendo respuestas inmediatas o asincrónicas.
    *   Está basado en JSON-RPC 2.0 sobre HTTP(S), con soporte para Server-Sent Events (SSE).
    *   Los agentes anuncian sus capacidades (skills) en una "Agent Card" (archivo JSON) que incluye URL, requisitos de autenticación, modelos usados, etc.. Esta tarjeta permite el descubrimiento de capacidades.
    *   La interacción se centra en la realización de **Tareas**, unidades fundamentales de trabajo con ID único y ciclo de vida. El resultado se devuelve como "artefactos".
    *   Los agentes pueden intercambiar **Mensajes Estructurados** durante la ejecución para coordinarse, aclarar o comunicar contexto. Los mensajes pueden contener "partes" con tipos de contenido específicos para negociar formatos e incluso capacidades de UI.
    *   Es complementario a MCP (Model Context Protocol), no compite con él. Mientras A2A se enfoca en la interacción entre agentes a un nivel alto y colaborativo (flujos de trabajo multiagente), MCP se enfoca en cómo un *solo* agente interactúa con herramientas o contexto externo a un nivel granular.
    *   Permite que agentes creados con diferentes frameworks (como ADK, LangGraph, CrewAI) se comuniquen entre sí.
*   Las fuentes proporcionadas no mencionan los estándares FIPA o KQML.

## Descomposición de Problemas

La descomposición de problemas es una estrategia clave en sistemas multiagente para abordar la complejidad.

*   Consiste en dividir problemas complejos en subtareas más manejables.
*   En arquitecturas multiagente, un agente principal o coordinador puede recibir un problema complejo, descomponerlo y delegar las subtareas a agentes especializados. Esto se alinea con la separación de responsabilidades para reducir el ruido y la confusión en agentes individuales.
*   Se pueden descomponer tareas grandes como "Desarrollar un plan estratégico" en subtareas como "Realizar análisis DAFO", "Definir objetivos", "Proponer iniciativas", etc., que pueden ser asignadas a diferentes agentes.
*   La cooperación entre agentes, donde uno resuelve localmente y recurre a otros para el resto, implica una forma de descomposición.

## Asignación de Tareas y Negociación

Los mecanismos para asignar tareas y la capacidad de negociación son fundamentales para la colaboración en MAS.

*   **Asignación de Tareas:** Típicamente se realiza mediante la delegación de un agente coordinador a agentes especializados.
    *   En el protocolo A2A, un agente "Cliente" inicia la asignación de una tarea enviando una solicitud a un agente "Remoto".
    *   El agente Cliente puede usar el "Descubrimiento de Capacidades" (mediante la Agent Card) para identificar qué agente Remoto es el más adecuado para una tarea antes de asignarla.
*   **Negociación:** Es un aspecto mencionado en los sistemas multiagente.
    *   En el modelo Agent0, existen "Obligaciones" (compromisos) que se refieren a la ejecución de acciones pendientes.
    *   En el contexto de A2A, la negociación ocurre al intercambiar mensajes, donde las "partes" de contenido permiten a los agentes acordar el formato del contenido y las capacidades de la interfaz de usuario necesarias para la interacción.
    *   Las fuentes no describen un mecanismo de asignación de tareas basado en "pujas" o subastas entre agentes para conseguir tareas.


# Gestión del Conocimiento y Contexto Compartido/Distribuido en Sistemas Multi-Agente

Basado en las fuentes proporcionadas, la gestión del conocimiento y el contexto compartido/distribuido en sistemas multi-agente se aborda a través de varios mecanismos:

## Ontologías y Bases de Conocimiento Compartidas

*   **Bases de Datos Vectoriales (RAG):** Las bases de datos vectoriales son clave para proporcionar a los agentes acceso a conocimiento preciso, actualizado y contextualizado, actuando como una "memoria" para los agentes. Permiten que un agente sepa lo que la empresa ya sabe. Los sistemas RAG (Retrieval Augmented Generation) permiten a los agentes leer documentos cargados en una base vectorial y usar la información recuperada como contexto para responder preguntas o realizar tareas. Este conocimiento puede ser específico de un dominio, como documentos legales. El agente accede a esta información mediante búsqueda semántica a través de los embeddings almacenados.
*   **Modelos del Mundo:** Los agentes, especialmente los reactivos basados en modelos o los basados en objetivos/utilidad, mantienen un estado interno o "modelo del mundo" que refleja aspectos no observables del entorno, basándose en la historia de percepciones. Este modelo contiene conocimiento sobre cómo evoluciona el mundo y cómo las acciones del agente lo afectan. En arquitecturas como InteRRaP, existen modelos del mundo, mental y social en una base de conocimiento jerárquica.
*   **Agent Cards (Protocolo A2A):** En el protocolo A2A, los agentes anuncian sus capacidades (skills) mediante una "Agent Card" en formato JSON. Esta tarjeta actúa como una forma de conocimiento compartido que permite a otros agentes (clientes) descubrir y identificar quién es el más adecuado para una tarea.
*   **Archivos de Proyecto y Documentos:** En proyectos colaborativos (como el desarrollo de software asistido por IA), los requisitos del proyecto, el stack tecnológico y la lista de tareas pueden almacenarse en documentos (como archivos Markdown) que sirven como base de conocimiento principal para los agentes. Las herramientas que permiten a los agentes interactuar con el sistema de archivos (`desktop Command` a través de MCP/Tools) facilitan el acceso y la modificación de estos documentos compartidos.

## Mantenimiento de la Coherencia del Contexto

*   **Protocolo A2A:** El protocolo Agent-to-Agent está diseñado para permitir que los agentes mantengan el contexto de la conversación a lo largo de diferentes sistemas. La comunicación en A2A se estructura en torno a "tareas" con un ID único y un ciclo de vida, lo que ayuda a los agentes a mantenerse sincronizados sobre el estado de finalización.
*   **Mensajes Estructurados en A2A:** Los agentes pueden intercambiar "mensajes estructurados" durante la ejecución de una tarea para coordinarse, aclarar información o comunicar contexto. Estos mensajes pueden incluir "partes" de contenido (texto, multimedia, formularios), permitiendo negociar el formato y las capacidades de la interfaz de usuario para una interacción coherente.
*   **Comunicación Bidireccional en A2A:** A2A soporta la comunicación bidireccional entre agentes, lo que permite que un agente informe a otro sobre el estado de su tarea, si ha finalizado o si hay dependencias. Esto es crucial para la coordinación, especialmente cuando las tareas tienen un orden o dependen del resultado de otras.
*   **Orquestación y Delegación:** En sistemas jerárquicos o coordinados, un agente principal (orquestador o manager) delega tareas a agentes especializados. Este enfoque centralizado puede ayudar a gestionar el acceso a recursos compartidos o a la información, asegurando que las subtareas se realicen en un orden lógico o se sincronicen adecuadamente.
*   **Herramientas de Interacción con el Entorno:** Dar a los agentes herramientas para interactuar directamente con el entorno compartido, como herramientas para leer y escribir archivos (ej. `desktop Command`), les permite acceder y modificar el estado compartido (ej. archivos del proyecto). La coherencia, en este caso, puede depender de la coordinación implícita o explícita entre los agentes que usan la misma herramienta en los mismos archivos.

## Propagación de Información Relevante

*   **Intercambio de Mensajes y Tareas (A2A):** La comunicación principal en A2A se basa en que un agente cliente envía una "tarea" a un agente remoto, y el resultado se devuelve como "artefactos". Esto asegura que la información relevante (el resultado de una subtarea) sea propagada de vuelta al agente que la necesita o a otros agentes colaboradores. Los mensajes adicionales durante la tarea también propagan contexto e instrucciones.
*   **Flujos de Trabajo Multi-Agente:** La estructura misma de los flujos de trabajo multi-agente, ya sean secuenciales, paralelos o jerárquicos, define cómo la información fluye entre los agentes. En un flujo secuencial, el resultado de un agente se pasa como entrada al siguiente. En un flujo paralelo, un coordinador distribuye información o tareas a varios agentes simultáneamente.
*   **Uso de Herramientas y MCP:** Cuando un agente utiliza una herramienta (a menudo vía MCP) para interactuar con una fuente de datos externa o un servicio, la información obtenida o la acción realizada a través de la herramienta es internalizada por ese agente. Si esta información es relevante para otros agentes, el agente que la obtuvo debe propagarla a través de los mecanismos de comunicación del sistema multi-agente (como A2A).
*   **Bases de Conocimiento Compartidas (Vectoriales/Documentos):** Si múltiples agentes acceden a la misma base de conocimiento (ej. base vectorial, archivos de proyecto), los cambios o adiciones a esta base (como cargar nuevos documentos o modificar archivos) pueden, teóricamente, hacer que nueva información esté disponible para todos los agentes que la consulten. Sin embargo, las fuentes no detallan mecanismos específicos para notificar activamente a los agentes sobre los cambios en tiempo real en estas bases, más allá de la capacidad de los agentes para consultarlas cuando sea necesario.




# Planificación y Razonamiento en Agentes

Basado en las fuentes proporcionadas, la planificación y el razonamiento en agentes inteligentes se abordan de la siguiente manera:

## Técnicas de Planificación

*   Los agentes basados en objetivos requieren información sobre su meta y pueden combinarla con información sobre los resultados de posibles acciones para elegir aquellas que ayuden a alcanzar el objetivo. A veces, esto implica considerar secuencias complejas de acciones. Tienen en cuenta consideraciones sobre el futuro, lo que los hace más flexibles.
*   Los agentes deliberativos funcionan siguiendo un paradigma basado en el ciclo percepción-planificación-acción.
*   El modelo de agente BDI (Creencias, Deseos, Intenciones), un tipo de agente deliberativo, incluye "deseos" (cómo se priorizan los objetivos del agente) e "intenciones" (la acción o secuencia de acciones escogida).
*   La arquitectura InteRRaP, un modelo híbrido, incluye capas de planificación local y cooperativa en su unidad de control del agente. Sus requisitos incluyen un comportamiento dirigido por objetivos, donde los agentes seleccionan acciones basándose en los fines que persiguen y los medios disponibles. Requieren conocimiento necesario para la planificación.
*   Los agentes a menudo incorporan módulos de planificación como parte de su estructura. Una arquitectura modular de agente puede incluir un módulo de planificación.
*   Los agentes de IA generativa modernos combinan un componente generativo (que produce posibles planes) con un núcleo de toma de decisiones entrenado mediante algoritmos de planificación para evaluar estas salidas y elegir acciones.
*   El aprendizaje por refuerzo jerárquico (HRL) descompone tareas complejas en subtareas más simples, permitiendo a los agentes desarrollar estrategias a alto nivel mientras gestionan el control a bajo nivel, lo que acelera el aprendizaje y mejora la interpretabilidad y escalabilidad. Esto puede considerarse una técnica de planificación jerárquica.
*   Las interacciones en sistemas multiagente pueden obedecer a la coexistencia de planes, generados preferentemente de forma distribuida, que implican la generación, selección y ejecución de operaciones candidatas.
*   El flujo típico de un agente implica planificar, actuar, observar y producir un resultado.
*   Las fuentes no mencionan específicamente las técnicas de planificación STRIPS (Stanford Research Institute Problem Solver) o HTN (Hierarchical Task Networks) por nombre, aunque el concepto de descomponer tareas o considerar secuencias de acciones y la planificación jerárquica está presente en la descripción de algunos agentes y arquitecturas.

## Razonamiento bajo Incertidumbre

*   Los agentes inteligentes son capaces de procesar sus percepciones y responder o actuar de manera racional, logrando objetivos y tendiendo a maximizar un resultado esperado. Para ello, utilizan varios algoritmos y técnicas de procesamiento de datos para analizar información y adaptar su comportamiento.
*   El conocimiento sobre el estado actual del mundo no siempre es suficiente para los agentes.
*   Los agentes reactivos basados en modelos mantienen un estado interno o "modelo del mundo" que les permite tomar decisiones basándose tanto en percepciones actuales como en experiencias pasadas. Esto les permite manejar información parcial.
*   Los agentes basados en utilidad evalúan la deseabilidad de diferentes estados del mundo utilizando una función de utilidad para elegir acciones que maximicen su satisfacción o utilidad general. Están diseñados para tomar decisiones que maximicen su utilidad esperada, especialmente en entornos inciertos donde los resultados no están garantizados.
*   Los agentes pueden acceder a conocimiento preciso y actualizado utilizando bases de datos vectoriales (RAG), lo que les permite consultar conocimiento organizacional propio en lugar de "inventar" respuestas o alucinar. Acceder a conocimiento relevante reduce la incertidumbre y mejora la precisión. LlamaIndex permite a los agentes recuperar datos contextualmente para ofrecer respuestas precisas y razonamientos dinámicos.
*   La transparencia y la explicabilidad (XAI) son preocupaciones éticas importantes debido a la naturaleza de "caja negra" de muchos modelos de IA, lo que dificulta entender cómo se toman las decisiones. Aunque no es una técnica de razonamiento bajo incertidumbre, aborda la incertidumbre del usuario sobre el proceso de decisión del agente.

## Aprendizaje en Sistemas Multi-Agente

*   Un agente inteligente adquiere conocimiento con su desempeño.
*   Los agentes de aprendizaje mejoran su rendimiento con el tiempo al aprender de sus experiencias. Pueden adaptarse a nuevas situaciones y optimizar sus acciones basándose en la retroalimentación.
*   Los agentes de aprendizaje utilizan técnicas como el aprendizaje por refuerzo y el aprendizaje supervisado. Procesan bucles de retroalimentación de su entorno para ajustar sus acciones.
*   Los agentes tienen la capacidad de decidir y adaptarse a problemas no vistos previamente. Tienen autorreflexión, lo que les permite reiterar sobre sus pensamientos o decisiones para autocorregirse y mejorar su desempeño.
*   Los sistemas multiagente consisten en dos o más agentes que trabajan juntos. La comunicación entre ellos permite que se pasen tareas y colaboren para generar respuestas más concisas o completar flujos de trabajo complejos.
*   Separar responsabilidades en agentes especializados con un contexto para responder tareas específicas reduce el "ruido", ahorra tiempo de respuesta y procesamiento, y reduce las alucinaciones. Esto mejora el rendimiento colectivo del sistema.
*   Se pueden utilizar diferentes modelos de lenguaje (LLMs) para agentes con distintos roles dentro de un sistema multiagente. La mezcla de LLMs con diferentes fortalezas puede conducir a mejores respuestas.
*   Existen frameworks para la construcción de sistemas multiagente (como CrewAI, Agency Swarm, Microsoft AutoGen) que facilitan la conexión y comunicación entre agentes. El Agent Development Kit (ADK) permite construir aplicaciones modulares y escalables componiendo múltiples agentes especializados en una jerarquía, facilitando la coordinación y delegación complejas.
*   Los benchmarks muestran que los sistemas multiagente pueden mejorar significativamente el porcentaje de acierto al generar código en comparación con los modelos de lenguaje grandes individuales.
*   El ADK proporciona herramientas de evaluación integradas para evaluar sistemáticamente el rendimiento del agente, tanto la calidad de la respuesta final como la trayectoria de ejecución paso a paso, frente a casos de prueba predefinidos. La depuración y optimización integradas en plataformas como Vertex AI permiten visualizar el procesamiento, la toma de decisiones y la interacción con herramientas, identificando cuellos de botella y errores para la optimización continua. Agent Engine admite herramientas de evaluación para mejorar el rendimiento de los agentes en función del uso en el mundo real.
*   La visión del desarrollo multiagente apunta hacia sistemas donde los agentes pueden trabajar en concierto como un verdadero equipo.


# Ética, Seguridad y Gobernanza en Sistemas Multi-Agente

Aquí se presenta una investigación sobre las consideraciones éticas, de seguridad y gobernanza en sistemas multi-agente, basada en las fuentes proporcionadas.

## Responsabilidad y Atribución

*   Una preocupación ética significativa es determinar quién es responsable cuando los agentes de IA toman decisiones que causan daño o errores, ya sean desarrolladores, organizaciones o la propia IA.
*   La falta de responsabilidad en los sistemas de IA crea desafíos en los marcos legales y éticos.
*   Se sugiere que los gobiernos y organizaciones establezcan leyes que definan la responsabilidad de la IA.
*   Es necesario un marco de responsabilidad y gobernanza para desarrolladores y usuarios de LLMs (componentes clave de los agentes) en caso de daños, incluyendo mecanismos de supervisión y control.
*   La ética de la IA se enfoca en los principios que guían el desarrollo y uso de sistemas de IA para asegurar que sean justos, transparentes y alineados con valores humanos.
*   El énfasis de Anthropic en la seguridad y la ética, la interpretabilidad y la transparencia contribuye a la fiabilidad y confianza, lo que puede ayudar a comprender cómo se toman las decisiones y, por extensión, a atribuir responsabilidades.
*   El desarrollo ético de agentes de IA, incluyendo la rendición de cuentas (accountability), es visto como una responsabilidad social.
*   La "Responsabilidad y Cumplimiento" es una capa considerada en arquitecturas de sistemas multi-agente, lo que sugiere su importancia en el diseño del sistema.

## Privacidad y Seguridad de Datos

*   La privacidad y la seguridad son preocupaciones éticas significativas y fundamentales en el desarrollo de agentes de IA.
*   Proteger la información personal de los usuarios es crucial para evitar riesgos de exposición indebida.
*   La seguridad de los datos es esencial para sistemas de IA confiables.
*   Plataformas como Vertex AI ofrecen seguridad de nivel empresarial para agentes, gestionando permisos con controles de identidad y restringiendo la actividad de agentes a perímetros seguros para proteger datos sensibles.
*   Se pueden definir límites en los agentes para controlar interacciones y validar parámetros antes de la ejecución de herramientas, lo que ayuda a proteger los datos.
*   Normativas de cumplimiento como GDPR, PCI DSS, HIPAA, SOX, FISMA e ISO 27001 recomiendan monitorear cambios en archivos confidenciales para identificar accesos no autorizados y garantizar la integridad de los datos.
*   El monitoreo y auditoría de actividades (quién, cuándo, desde dónde) son útiles para investigar violaciones de seguridad de datos.
*   El Modelo de Contexto de Protocolo (MCP) se considera beneficioso para casos sensibles a la seguridad (como sanidad, finanzas, legal) porque su diseño centralizado facilita la aplicación de políticas y control de acceso, permitiendo a los agentes acceder a datos y herramientas externas en tiempo real.
*   Las mejores prácticas al definir herramientas en MCP incluyen el diseño cuidadoso de los alcances de permiso para limitar el acceso y retorno de datos por parte del Servidor MCP solo a información autorizada, previniendo fugas.
*   Los datos almacenados en bases de datos vectoriales pueden configurarse con metadatos para asegurar controles de acceso basados en roles o políticas (RBAC/PBAC).
*   El protocolo Agent2Agent (A2A) fue diseñado con la seguridad por defecto como principio clave, soportando autenticación y autorización a nivel empresarial.
*   Se han identificado amenazas comunes específicas para sistemas multi-agente A2A.

## Control y Supervisión Humana

*   Se recomienda el uso de sistemas "Human-In-The-Loop" (HITL) para asegurar la supervisión humana en decisiones críticas tomadas por agentes de IA, como en ámbitos médicos o legales.
*   Es fundamental mantener una supervisión humana efectiva sobre los LLMs dentro de los agentes, permitiendo revisar, corregir o revertir decisiones importantes.
*   Las organizaciones pueden formar Comités de Ética en IA para revisar y aprobar modelos antes de su implementación.
*   La revisión humana es parte importante de la implementación y uso de sistemas de IA, incluyendo el monitoreo de usos indebidos.
*   La evaluación humana, realizada por expertos y usuarios, es una técnica para valorar el comportamiento y resultados de un sistema de IA, complementando métricas automáticas y ayudando a detectar errores o comportamientos no deseados.
*   Algunos frameworks para construir agentes, como Autogen o CrewAI, facilitan la implementación de Human-in-the-Loop, permitiendo la intervención manual en puntos clave para mejorar precisión y control.
*   Las plataformas pueden ofrecer funciones de seguimiento exhaustivas para monitorear automáticamente el comportamiento de los agentes, visualizando su razonamiento, selección de herramientas y rutas de ejecución, lo que proporciona la información necesaria para la supervisión humana.


```markdown
# Herramientas y Frameworks para el Desarrollo Multi-Agente (Generales)

Esta sección detalla las herramientas y frameworks mencionados en las fuentes para el desarrollo de sistemas multi-agente.

## Frameworks de Desarrollo Multi-Agente

Los frameworks proporcionan estructuras y abstracciones para simplificar la creación y gestión de sistemas donde múltiples agentes interactúan y colaboran.

*   **Agent Development Kit (ADK)**
    *   Es un framework de código abierto de Google, diseñado para simplificar el desarrollo completo de la pila de sistemas de agentes y multi-agente.
    *   Está optimizado para sistemas de agente y multi-agente complejos.
    *   Permite compilar aplicaciones modulares y escalables componiendo múltiples agentes especializados en una jerarquía, facilitando la coordinación y delegación complejas.
    *   Funciona con una amplia gama de modelos de IA, incluyendo Gemini, modelos accesibles a través de Vertex AI Model Garden, y modelos de otros proveedores (Anthropic, Meta, Mistral AI, AI21 Labs) mediante integración con LiteLLM.
    *   Permite equipar a los agentes con diversas capacidades utilizando herramientas precompiladas, herramientas de Model Context Protocol (MCP), bibliotecas de terceros como LangChain y LlamaIndex, e incluso usando otros agentes como herramientas (por ejemplo, LangGraph, CrewAI).
    *   Ofrece control preciso sobre el comportamiento y la orquestación de los agentes.
    *   Incluye una experiencia de desarrollador integrada con CLI y UI web para desarrollar, probar y depurar localmente, permitiendo inspeccionar eventos, estado y ejecución paso a paso.
    *   Cuenta con evaluación incorporada para probar sistemáticamente las rutas de ejecución y la calidad de la respuesta.
    *   Ofrece una ruta clara hacia la implementación, incluyendo opciones administradas.
    *   Es compatible con frameworks de código abierto populares como LangChain, LangGraph, AG2 o Crew.ai.

*   **CrewAI**
    *   Es un framework en Python.
    *   Está diseñado específicamente para construir sistemas multi-agente, donde múltiples agentes especializados trabajan juntos para realizar una tarea.
    *   Se considera un framework intuitivo para empezar en el desarrollo de sistemas multi-agente, adecuado para principiantes.
    *   ADK se integra con CrewAI, permitiendo usar agentes CrewAI como herramientas o construir flujos de trabajo multi-agente que involucren agentes CrewAI.
    *   Algunos desarrolladores han experimentado con CrewAI y lo recomiendan.

*   **LangChain**
    *   Es un framework que se menciona en el contexto de la construcción de agentes y la integración de herramientas.
    *   Se utiliza para conectar bases de datos vectoriales a agentes funcionales.
    *   ADK ofrece integración con LangChain.
    *   Es un framework de código abierto popular para crear flujos de trabajo multi-agente.
    *   Se menciona junto con LangGraph como una herramienta para crear sistemas multi-agente.

*   **AutoGen (Microsoft)**
    *   Es uno de los principales frameworks mencionados para la construcción de sistemas multi-agente.

*   **agency swarm**
    *   Es otro de los principales frameworks mencionados para la construcción de sistemas multi-agente.
    *   Se describe como más enfocado en la personalización en comparación con CrewAI.

*   **n8n**
    *   Es una plataforma de código abierto popular para la automatización de flujos de trabajo, que se puede auto-alojar.
    *   Se recomienda para construir automatizaciones o agentes que necesitan llamar a herramientas externas.
    *   Es potente y versátil.
    *   Permite crear flujos completos conectando servicios a través de prompts.
    *   Se ha utilizado para crear agentes con sistemas RAG (Generación Aumentada por Recuperación) para leer documentos.
    *   Permite empezar a tener sistemas multi-agente mediante la copia y pegado de plantillas de agentes.
    *   Se sugiere que podría convertirse en la herramienta estándar por defecto para crear agentes debido a su facilidad de uso.
    *   Se considera adecuada para principiantes que buscan ofrecer servicios basados en agentes.

*   **LangGraph**
    *   Se menciona como una herramienta para construir sistemas multi-agente y se integra con ADK.
    *   Se utiliza en la creación de flujos de trabajo multi-agente con frameworks de código abierto, a menudo asociado con LangChain.

## Protocolos Fundamentales para la Colaboración de Agentes

Aunque no son frameworks de desarrollo en sí mismos, estos protocolos son esenciales para permitir la comunicación y colaboración entre agentes desarrollados potencialmente con diferentes frameworks.

*   **Agent2Agent (A2A) Protocol**
    *   Es un protocolo abierto propuesto por Google para estandarizar la comunicación entre agentes de inteligencia artificial.
    *   Busca facilitar que diferentes agentes especializados, incluso creados por distintas empresas o con distintos frameworks, puedan trabajar juntos de manera coordinada y eficiente.
    *   Permite a los agentes comunicarse entre sí independientemente de cómo han sido construidos.
    *   Está diseñado para soportar autenticación y autorización de nivel empresarial.
    *   Se basa en estándares existentes como HTTP, SSE y JSON-RPC.
    *   Permite que los agentes publiquen sus funciones y negocien cómo interactuarán.
    *   El objetivo principal es permitir que los agentes colaboren en sus modalidades naturales y no estructuradas, incluso cuando no comparten memoria, herramientas o contexto.
    *   Complementa el protocolo MCP de Anthropic.
    *   Permite que los agentes descubran dinámicamente las capacidades de los demás, negocien formatos de interacción y mantengan el contexto de la conversación a través de los sistemas.
    *   Es fundamental para la interoperabilidad universal de los agentes de IA colaborativa.
    *   El protocolo se encuentra en un estado temprano ("muy verde" / beta).

*   **Model Context Protocol (MCP)**
    *   Es un protocolo desarrollado por Anthropic que se enfoca en cómo las aplicaciones externas proporcionan contexto estructurado y herramientas a un agente basado en un modelo de lenguaje en tiempo de ejecución.
    *   Su objetivo principal es mejorar las capacidades de *un solo agente* durante la inferencia.
    *   Funciona mediante un Servidor MCP que expone herramientas y un Cliente MCP que actúa como puente para que el LLM/Agente interactúe con esas herramientas.
    *   Permite al LLM, durante su razonamiento, listar y ejecutar herramientas proporcionadas por el Servidor MCP.
    *   Trabaja a un nivel bajo y granular, enfocado en el uso dinámico de herramientas y el aumento de contexto.
    *   Es un estándar abierto y de facto con una comunidad creciente.
    *   Permite cambiar flexiblemente entre proveedores de LLM sin reescribir la lógica de integración.
    *   Se utiliza para integrar sistemas RAG, permitiendo a los agentes acceder a conocimiento específico.
    *   Open AI ha aceptado este protocolo para su SDK de agentes oficial.
    *   Complementa a A2A.

## Otras Herramientas Relevantes

*   **LangGraph:** Aunque ya mencionado bajo frameworks y como parte de ADK, se destaca como una herramienta específica para definir la orquestación y los flujos en sistemas multi-agente, a menudo utilizada en conjunto con LangChain.
*   **LlamaIndex:** Se menciona como una biblioteca de terceros que se puede integrar con ADK para equipar a los agentes con capacidades. Aunque no es un framework multi-agente per se, es relevante para la gestión y recuperación de datos utilizada por agentes, incluyendo en contextos multi-agente (como RAG).
*   **Zapier / Make / N8N:** Mencionado como plataformas de automatización. Si bien no se describen como frameworks para *construir* la lógica multi-agente interna, pueden ser útiles para conectar agentes o sistemas de agentes con otros servicios o flujos de trabajo externos.