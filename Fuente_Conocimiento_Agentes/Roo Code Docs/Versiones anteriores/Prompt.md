# Contexto Experto para la Creación de Agentes de IA en Roo Code (VSCode)

## 1. Entendimiento Fundamental de Roo Code

Roo Code es un **entorno de ejecución avanzado para Agentes de IA (denominados "Modos")** integrado en VSCode. No es simplemente un editor de código, sino una plataforma que gestiona:

* La interacción con Modelos de Lenguaje Grandes (LLM).
* La disponibilidad y el uso de **herramientas específicas de Roo Code**.
* El **contexto** del proyecto (archivos, código base, instrucciones).
* La **seguridad** (auto-aprobación de herramientas, `.rooignore`).
* La comunicación con servicios externos a través del **Model Context Protocol (MCP)**.

## 2. Estrategia Central: "Modos Personalizados" como Agentes de IA

La estrategia clave es definir a cada miembro de un equipo de IA (ej., ACP, @Pepe, @Isma) como un **"Modo Personalizado"** dentro de Roo Code. Cada Modo se configurará con los siguientes atributos esenciales:

* **`slug`**: Un identificador único y conciso para el modo (ej., "po-pepe", "acp-configurator"). Es crucial para asociar archivos de reglas.
* **`name`**: Un nombre descriptivo y legible para mostrar en la interfaz de Roo Code (ej., "🦉 @Pepe (PO)").
* **`roleDefinition`**: Corresponde a la "Definición del Rol, Experiencia y Personalidad" del agente. Esta es la instrucción fundamental que define la identidad, el expertise y el comportamiento general del agente. La primera frase es especialmente importante para el resumen del modo.
* **`whenToUse`**: Describe cuándo es apropiado activar o delegar tareas a este modo. Utilizado por el modo `🪃 Orchestrator` y para la selección manual.
* **`groups`**: Define los **Tool Groups** de Roo Code a los que el Modo/Agente tendrá acceso. Estos grupos son:
    * `Read`: Para leer archivos e información.
    * `Edit`: Para modificar archivos, con la posibilidad de restringir por `fileRegex` a tipos de archivo o rutas específicas.
    * `Browser`: Para interactuar con un navegador web.
    * `Command`: Para ejecutar comandos de terminal (respetando la whitelist de Roo Code).
    * `MCP`: Para interactuar con servidores MCP.
    * `Workflow`: Herramientas siempre disponibles para la gestión de tareas y flujo de diálogo (ej., `ask_followup_question`, `new_task`, `switch_mode`, `attempt_completion`).
    La asignación cuidadosa de `groups` y `fileRegex` es vital para la seguridad y el enfoque del agente.
* **`customInstructions`**: Estas son las instrucciones detalladas y específicas que guían la operación autónoma del agente. La **estrategia preferida** es cargar estas instrucciones desde archivos Markdown ubicados en el directorio `.roo/rules-{slug_del_modo}/` dentro de cada proyecto. El Agente de Configuración de Proyectos (ACP) será responsable de generar esta estructura y desglosar la "Sección 3: Instrucciones Personalizadas para el Modo (Prompt de Rol y Operación Autónoma)" de cada agente en múltiples archivos lógicos dentro de esta carpeta (ej., `01_identidad_objetivo.md`, `04_uso_herramientas_roocode.md`). Roo Code carga estas reglas en orden alfabético.

## 3. Uso Explícito y Detallado de Herramientas de Roo Code

Los prompts y las `customInstructions` de los agentes deben instruirlos para utilizar las **herramientas específicas de Roo Code** de manera explícita. Esto significa:

* En lugar de "leer el archivo X", se debe instruir: "**usa la herramienta `read_file` de Roo Code con el parámetro `path: X`**".
* En lugar de "ejecuta este script", se debe instruir: "**usa la herramienta `execute_command` de Roo Code con los parámetros `command: [script]` y `cwd: [directorio_de_trabajo]`**".
* Para la interacción con el navegador: "**usa la herramienta `browser_action` de Roo Code con la sub-acción `launch`, `click`, `type`, etc.**"
* Para la escritura de archivos: Se prefiere `apply_diff` para modificaciones precisas. `write_to_file` debe usarse con precaución.
* Para listar archivos: `list_files`.
* Para listar definiciones de código: `list_code_definition_names`.
* Para crear nuevas tareas (orquestación): `new_task`.
* Para cambiar de modo: `switch_mode`.

---

# Instructivo Detallado para el Uso de Herramientas de Roo Code (basado en fuentes)

Este instructivo se basa *únicamente* en la información presente en los documentos de Roo Code proporcionados en las fuentes. Describe las herramientas y características de personalización mencionadas y cómo utilizarlas según lo documentado.

Las herramientas de Roo Code están diseñadas para hacer la codificación más rápida, fácil y eficiente. Utilizan modelos de lenguaje grandes (LLMs) para interactuar, con tres tipos principales de mensajes: el **System Prompt** (instrucciones iniciales que definen las capacidades, persona y reglas operativas de Roo), **User Messages** (contenido enviado por ti a Roo), y **Assistant Messages** (respuestas generadas por el LLM).

## 1. Instrucciones Personalizadas (Custom Instructions)

Las Instrucciones Personalizadas te permiten personalizar el comportamiento de Roo Code, proporcionando guía específica que moldea sus respuestas, estilo de codificación y procesos de toma de decisiones. Esto es una de las características mencionadas en la documentación general de Roo Code.

Estas instrucciones pueden aplicarse a todo el espacio de trabajo o ser específicas para un modo (una tarea especializada o un contexto de trabajo). Organizar instrucciones largas o complejas en múltiples archivos manejables y gestionar las instrucciones con control de versiones son beneficios de usar archivos/directorios para instrucciones. Permitir que miembros del equipo no técnicos modifiquen instrucciones sin editar JSON también es una ventaja.

Roo Code carga estas instrucciones preferiblemente desde directorios y, como método de reserva (fallback), desde archivos únicos.

### 1.1. Instrucciones a Nivel de Espacio de Trabajo (Workspace-Wide)

Estas instrucciones se aplican a todos los modos dentro del proyecto actual.

**Método Preferido: Basado en Directorios (`.roo/rules/`)**

1.  Crea un directorio llamado `.roo/rules/` en la raíz de tu espacio de trabajo.
2.  Coloca archivos de instrucciones (por ejemplo, `.md`, `.txt`) dentro de este directorio.
3.  Puedes organizar las instrucciones en subdirectorios; Roo Code leerá archivos recursivamente.
4.  El contenido de los archivos se añadirá al System Prompt en **orden alfabético** según el nombre del archivo.
5.  Este método tiene precedencia si el directorio existe y contiene archivos.

**Método de Reserva: Basado en Archivos (`.roorules`)**

1.  Si el directorio `.roo/rules/` no existe o está vacío, Roo Code buscará un único archivo `.roorules` en la raíz del espacio de trabajo.
2.  Si se encuentra, su contenido se cargará y aplicará.

### 1.2. Instrucciones Específicas por Modo (Mode-Specific)

Estas instrucciones se aplican solo cuando estás utilizando un modo específico (que son tareas especializadas).

**Método Preferido: Basado en Directorios (`.roo/rules-{modeSlug}/`)**

1.  Crea un directorio en la raíz de tu espacio de trabajo con un nombre que siga el formato `.roo/rules-{modeSlug}/`, donde `{modeSlug}` es el "slug" (identificador) de tu modo. Por ejemplo, para un modo escritor de documentación, podrías usar `.roo/rules-docs-writer/`.
2.  Coloca archivos de instrucciones dentro de este directorio.
3.  Los archivos se leen recursivamente dentro de la estructura del directorio.
4.  El contenido de los archivos se añade al System Prompt en **orden alfabético** por nombre de archivo.
5.  Este método tiene precedencia sobre el método de archivo de reserva para el modo específico si el directorio existe y contiene archivos.

**Método de Reserva: Basado en Archivos (`.roorules-{modeSlug}`)**

1.  Si el directorio `.roo/rules-{modeSlug}/` no existe o está vacío, Roo Code buscará un único archivo con el nombre `.roorules-{modeSlug}` (por ejemplo, `.roorules-code`) en la raíz del espacio de trabajo.
2.  Si se encuentra, su contenido se cargará para ese modo específico.

## 2. Anulación del System Prompt (Override System Prompt) - "Footgun Prompting"

Esta es una característica avanzada para modificar directamente el System Prompt que se envía al modelo. Se describe como "Footgun Prompting" debido a su potencial para alterar significativamente el comportamiento del agente.

**Acceso a la Característica:**

1.  Haz clic en el icono ⚙️ en la barra de menú superior de Roo Code.
2.  Expande la sección "Advanced: Override System Prompt".
3.  Hacer clic en el enlace de la ruta de archivo dentro de la explicación abrirá o creará el archivo de anulación correcto para el modo actualmente seleccionado en VS Code.

**Efecto de la Anulación:**

*   Al usar esta anulación, las secciones estándar del System Prompt de Roo Code, como las descripciones de herramientas, reglas y capacidades, son omitidas o "puenteadas" (bypassed).
*   Solo se conservan la `roleDefinition` central del modo y cualquier `customInstructions` que hayas configurado para ese modo.
*   El System Prompt final enviado al modelo se construye de la siguiente manera:
    1.  `${roleDefinition}`
    2.  `${content_of_your_override_file}`
    3.  `${customInstructions}`
   

## 3. Uso de Herramientas Específicas: `list_code_definition_names`

Roo Code puede utilizar herramientas para interactuar con el entorno o acceder a información específica. Una de estas herramientas mencionadas es `list_code_definition_names`.

**Funcionalidad:**

*   Esta herramienta lista los nombres de las definiciones de código dentro de un archivo.
*   El resultado incluye los números de línea de inicio y fin de cada definición, el símbolo de barra vertical (`|`) como separador, y el código fuente real de la definición.
*   El formato de salida ayuda a ver rápidamente tanto la ubicación como los detalles de implementación de las definiciones.

**Ejemplos de Uso (Cuándo se utiliza):**

Aunque las fuentes no detallan los *pasos específicos para ejecutar* esta herramienta a través de un comando directo del usuario, sí describen los escenarios en los que Roo la utiliza:

*   **Al iniciar una nueva tarea:** Roo lista definiciones de código clave para comprender la estructura general del proyecto.
*   **Al planificar refactorización:** Roo la usa para identificar clases y funciones que podrían verse afectadas.
*   **Al explorar bases de código desconocidas:** Roo mapea las construcciones de código importantes antes de sumergirse en los detalles de implementación.
*   **Al añadir nuevas características:** Roo identifica patrones existentes y definiciones de código relevantes para mantener la consistencia.
*   **Al solucionar errores (debugging):** Roo mapea la estructura de la base de código para localizar posibles fuentes del problema.
*   **Al planificar cambios de arquitectura:** Roo identifica todos los componentes afectados en los archivos.

## Otras Características Mencionadas

Además de las herramientas y personalizaciones detalladas anteriormente, la documentación general de Roo Code menciona otras características clave:

*   **Custom Modes:** Permiten definir tareas especializadas. (Las fuentes no proporcionan un instructivo paso a paso sobre *cómo* crear un Custom Mode, solo cómo añadirle instrucciones personalizadas).
*   **Local Models:** Soporte para el uso de modelos de lenguaje que se ejecutan localmente. (Las fuentes mencionan la posibilidad de instalar LLMs en tu propio ordenador, pero no explican cómo Roo Code interactúa específicamente con ellos una vez instalados localmente).
*   **Auto-Approval Settings:** Ajustes para flujos de trabajo más rápidos. (Las fuentes no detallan cómo configurar o utilizar estos ajustes).

Es importante recordar que la efectividad de las herramientas de IA, incluyendo las de Roo Code, a menudo depende de la claridad y el contexto proporcionado. La ingeniería de prompts requiere habilidad. Para proyectos más complejos o bases de código antiguas, las IAs pueden no funcionar tan bien y pueden requerir más "arreglo" manual. Asegurarse de que el código autogenerado sea mantenible y escalable es crucial. El testing también debe sostener la generación de código con IA.

---



### Interacción con Servidores MCP:

Los agentes deben ser instruidos para usar las herramientas MCP de Roo Code de forma diferenciada:

* **`use_mcp_tool`**: Para ejecutar una **acción o función específica** expuesta por un servidor MCP. Requiere `server_name` (alias del servidor en Roo Code), `tool_name` (nombre de la herramienta expuesta por el servidor MCP, ej., `get-library-docs` para un servidor como Context7), y `arguments` (los parámetros que espera la herramienta del servidor MCP).
* **`access_mcp_resource`**: Para **obtener datos, información o contexto** de un servidor MCP a través de un URI. Requiere `server_name` y la `uri` específica del recurso (ej., `github-tools.repo://owner/repo/contents/file` o `context7://library_docs?id=some_id`).

Los agentes deben consultar el "Catálogo de Servidores MCP" (que el ACP generará en `ESTANDARES_Y_GOBERNANZA_TI.md` del proyecto) para los detalles de invocación correctos (`server_name`, `tool_name`, `uri`, `arguments`).

## 4. Conceptos Adicionales de Roo Code Relevantes

* **Tool Use Overview**: Documentación de Roo Code que detalla las herramientas disponibles y cómo usarlas. Es una referencia fundamental.
* **Customizing Modes**: Describe cómo definir y personalizar los modos, incluyendo la estructura de archivos `.roo/rules-{slug}/`.
* **Checkpoints**: Sistema de versionado automático de Roo Code que actúa como red de seguridad durante la edición de archivos. Los agentes deben ser conscientes de su existencia.
* **Context Poisoning**: Posibilidad de que el contexto de la sesión de chat se contamine. Los agentes deben ser instruidos para reconocer esto y, si es necesario, sugerir un reinicio de la sesión.
* **Prompt Engineering Tips & Prompt Structure**: Guías sobre cómo estructurar prompts efectivos para los LLM, cruciales para la `roleDefinition` y las `customInstructions`.
* **Model Temperature**: La temperatura del modelo puede ser configurada por modo ("Sticky Models"). Los agentes deben adaptar su creatividad vs. determinismo según la tarea y esta configuración (modelos con "thinking" requieren temperatura de 1.0).
* **`.rooignore`**: Archivo que especifica qué archivos y carpetas deben ser ignorados por Roo Code y sus herramientas. Los agentes deben respetar estas reglas.
* **Servidores MCP (Global vs. Proyecto, STDIO vs. SSE, Creación por Roo)**: Entender cómo Roo Code gestiona la configuración y comunicación con servidores MCP es importante para la depuración y configuración avanzada. Roo Code puede incluso crear servidores MCP.
* **Directrices Globales para Roo Code**: Instrucciones generales (configuradas en la "Prompts Tab" del IDE o en `.roo/rules/00-global-directives.md`) que se aplican a todos los modos. Las instrucciones específicas del modo las complementan y tienen precedencia.
* **System Prompt Override / "Footgun Prompting"**: Si un archivo `.roo/system-prompt-{mode-slug}` existe, reemplaza gran parte de las instrucciones estándar de Roo Code, y el modo opera bajo ese nuevo conjunto de directivas.

## 5. Implicaciones para la Estrategia de Creación de Agentes

* La **definición de cada agente (Modo Personalizado)** es la piedra angular. La `roleDefinition` establece la identidad, y las `customInstructions` (desglosadas en archivos en `.roo/rules-{slug}/`) dictan la operación autónoma detallada, incluyendo el uso explícito de herramientas de Roo Code y MCP.
* El **ACP (Asistente de Configuración de Proyectos)** juega un rol crucial al generar no solo la estructura documental del proyecto, sino también el archivo `.roomodes` (con las definiciones de todos los demás agentes/modos) y la estructura completa de carpetas y archivos `.roo/rules-{slug_del_agente}/` con las instrucciones desglosadas para cada agente.
* Los **prompts deben ser meticulosamente diseñados** para guiar a los LLM a pensar paso a paso y a utilizar las herramientas de Roo Code de la manera prevista.
* La **seguridad y el control de acceso** se manejan principalmente a través de la configuración de `groups` y `fileRegex` en la definición de cada Modo, y mediante el archivo `.rooignore`.
* La **flexibilidad y modularidad** del sistema de agentes se logra definiendo roles especializados y permitiendo que Roo Code (o un modo orquestador) delegue tareas al modo más apropiado.

Este contexto consolidado, extraído de tu información, debería permitir a una IA comprender profundamente cómo diseñar y estructurar Agentes de IA que operen eficazmente dentro del ecosistema de Roo Code en VSCode.