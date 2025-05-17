# README: 🤖 Sistema Avanzado de Agentes de IA para Gestión y Desarrollo de Proyectos Basado en Markdown (Optimizado para Roo Code) 📄

**Fecha de Creación (Sistema de Agentes):** 2025-05-17 10:14:12 AM CST 🗓️
**Versión del Sistema de Agentes:** 1.4 (Alineado con Roo Code: Modos, Herramientas, MCP, Checkpoints)
**Autor del Sistema y Prompts:** Ranndy Salas Umaña
**Repositorio:** [https://github.com/Ranndy-90/Prompt_Agentes_IA](https://github.com/Ranndy-90/Prompt_Agentes_IA) (Este mismo repositorio)

## 1. Propósito de Este Repositorio y Sistema 🎯

Este `README.md` y el contenido de este repositorio describen la arquitectura de prompts e instrucciones para un sistema avanzado de Agentes de Inteligencia Artificial (IA). Estos agentes están diseñados para gestionar y ejecutar proyectos de desarrollo de software de manera autónoma y profesional, operando dentro del entorno **Roo Code en VS Code**.

El sistema se fundamenta en:
* El uso exclusivo de archivos Markdown (`.md`) para la comunicación, la gestión de artefactos y toda la documentación del proyecto que los agentes crearán y utilizarán.
* La configuración de cada agente de IA como un **Modo Personalizado** dentro de Roo Code, con su propia personalidad, conjunto de herramientas permitidas e instrucciones de comportamiento detalladas.
* La utilización de las **herramientas internas de Roo Code** (para lectura/escritura de archivos, ejecución de comandos, interacción con el navegador, gestión de flujo de trabajo) y su capacidad para interactuar con **servidores MCP (Model Context Protocol)** externos.

El objetivo es proporcionar una guía clara y los recursos necesarios (prompts detallados, definiciones de roles, estructura documental) para configurar, utilizar, replicar o contribuir a este sistema de agentes dentro del ecosistema Roo Code.

## 2. Visión General del Sistema de Agentes y su Arquitectura de Prompts 💡

El sistema se basa en un conjunto de agentes de IA, cada uno actuando como un "Modo Personalizado" en Roo Code con objetivos específicos. Cada agente utiliza un Modelo de Lenguaje de Gran Tamaño (LLM) como su "cerebro", y sus capacidades se extienden mediante las herramientas proporcionadas por Roo Code y los servidores MCP configurados.

### 2.1. Estructura de Carpetas de Este Repositorio (`Prompt_Agentes_IA`):

La organización de este repositorio es crucial para entender y gestionar las definiciones de los agentes:

```text
/Prompt_Agentes_IA/
├── /AGENTES_DEFINICIONES_Y_PROMPTS/  # Contiene los prompts completos para cada agente especializado
│   ├── /ACP_Asistente_Configuracion_Proyectos/
│   │   └── v1.0_ACP_Asistente_Configuracion_Proyectos_Prompt.md # Archivo único por versión
│   │   └── ... (futuras versiones v1.1, v1.2, etc.)
│   │
│   ├── /Pepe_Product_Owner/
│   │   └── v1.0_Pepe_PO_Agente_IA_Prompt.md
│   │   └── ...
│   │
│   ├── /Isma_Scrum_Master/
│   │   └── v1.0_Isma_SM_Agente_IA_Prompt.md
│   │   └── ...
│   │
│   ├── /Dory_Diseñadora_UXUI_DevTeam/
│   │   └── v1.0_Dory_DUX_Agente_IA_Prompt.md
│   │   └── ...
│   │
│   ├── /Melody_Frontend_DevTeam/
│   │   └── v1.0_Melody_EDF_Agente_IA_Prompt.md
│   │   └── ...
│   │
│   ├── /Topa_Backend_DevTeam/
│   │   └── v1.0_Topa_EDB_Agente_IA_Prompt.md
│   │   └── ...
│   │
│   └── /Voro_QA_DevTeam/
│       └── v1.0_Voro_EPS_Agente_IA_Prompt.md
│       └── ...
│
├── /INSTRUCCIONES_BASE_ROOCODE/ # Contiene las directrices fundamentales para todos los agentes
│   └── v1.0_RooCode_Directrices_Globales_Agente_Maestro.md
│   └── ... (futuras versiones)
│
├── agent_version_history.md  # Log de cambios y versiones de los prompts de los agentes
├── README.md                 # El meta-README que describe este sistema de agentes
└── ... (otros archivos de soporte como LICENSE.md, .gitignore, CONTRIBUTING.md)
```

---

### 2.2. Componentes de la Definición de Cada Agente 🧩

Cada archivo `vX.X_NombreAgente_Prompt_Completo.md` (ubicado dentro de la carpeta del agente correspondiente en `/AGENTES_DEFINICIONES_Y_PROMPTS/`) está estructurado en tres secciones vitales. Estas secciones, en conjunto, definen completamente cómo un agente de IA debe ser configurado como un **Modo Personalizado** en Roo Code y cómo debe operar autónomamente:

1.  **`Definición del Rol, Experiencia y Personalidad`**:
    * **Función para Roo Code:** Este contenido completo se utiliza como el valor para la propiedad `roleDefinition` en la configuración JSON del Modo Personalizado. Es la instrucción fundamental que define la identidad, el expertise, el tono y la perspectiva general del agente cuando ese modo está activo. La primera frase es particularmente importante si el campo `whenToUse` no se detalla extensamente, ya que Roo Code podría usarla como un resumen conciso del propósito del modo.
    * **Estrategia:** Proporciona a Roo Code una base sólida para que la IA emule un rol profesional específico y altamente competente. Guía no solo *qué* hace el agente, sino *cómo* se presenta, se comporta y razona.

2.  **`Cuándo Usar este Modo (para el IDE)`**:
    * **Función para Roo Code:** El contenido de esta sección se utiliza para el valor de la propiedad `whenToUse` en la configuración JSON del Modo Personalizado. Esta descripción es crucial para que Roo Code (especialmente el modo `🪃 Orchestrator` o al usar la herramienta `switch_mode`) determine cuándo es apropiado activar o delegar tareas a este modo específico.
    * **Estrategia:** Ayuda a Roo Code a tomar decisiones de orquestación más inteligentes y a los usuarios humanos a seleccionar el modo correcto para la tarea en cuestión.

3.  **`Instrucciones Personalizadas para el Modo (Prompt de Rol y Operación Autónoma)`**:
    * **Función para Roo Code:** Este es el **núcleo de la "programación" del agente para su modo específico**. El contenido de esta sección se implementa en Roo Code utilizando la **estructura de archivos y directorios `.roo/rules-{mode-slug}/`**. El ACP (Asistente de Configuración de Proyectos) será instruido para tomar esta Sección 3 y desglosarla en múltiples archivos `.md` más pequeños y lógicos (ej. `01_objetivo_principal.md`, `02_documentos_clave.md`, `03_ciclo_operativo.md`, `04_uso_herramientas_roocode.md`, `05_interaccion_mcp.md`, `06_heuristicas.md`, `07_rutina_diaria.md`). Roo Code carga estas reglas en orden alfabético desde el directorio `.roo/rules-{slug_del_modo}/` y las añade al system prompt del modo, después de la `roleDefinition` y cualquier `customInstructions` definida en el JSON.
    * **Estrategia:** Estas instrucciones detalladas guían a la IA para operar autónomamente, especificando su ciclo operativo, cómo usar las **herramientas internas de Roo Code** (como `read_file`, `apply_diff`, `execute_command`, `list_files`, `browser_action`, y las herramientas de flujo de trabajo como `ask_followup_question`, `new_task`, `switch_mode`, `attempt_completion`), cómo interactuar con **servidores MCP externos** (diferenciando el uso de `access_mcp_resource` para obtener datos/contexto y `use_mcp_tool` para ejecutar acciones/funciones específicas del servidor MCP), cómo gestionar sus entregables en Markdown (incluyendo el versionamiento de documentos del proyecto con encabezados YAML y el registro en `document_version_history.md`), y las heurísticas fundamentales para su toma de decisiones autónoma. Se enfatiza la adherencia a las directrices avanzadas (seguridad, estándares, "Vibe Coding" asistido por herramientas, etc.).

### 2.3. Directrices Globales para Roo Code (Anteriormente Instrucciones Base del Agente Maestro) 🧬

* El archivo (versionado) en la carpeta `/INSTRUCCIONES_BASE_ROOCODE/` (ej. `v1.0_RooCode_Directrices_Globales.md`) contiene las **directrices fundamentales y universales** que se aplican a CUALQUIER modo operando en Roo Code para este sistema.
* **Función para Roo Code:** Este contenido se configura como las **"Custom Instructions for All Modes"** en la Prompts Tab del IDE de Roo Code, o potencialmente como parte de un archivo `.roo/rules/00-global-directives.md` (o `.roorules`) a nivel de workspace en el proyecto generado por el ACP. Define el "ADN" o comportamiento común a todos los agentes.
* **Estrategia:** Estas instrucciones base son el primer nivel de configuración que Roo Code considera. Las "Instrucciones Personalizadas" de cada modo (cargadas desde `.roo/rules-{slug_del_modo}/`) las complementan y especializan, sin contradecirlas, gracias al orden de combinación de prompts de Roo Code.

### 2.4. Herramientas y Capacidades del Entorno Roo Code Asumidas:

Este sistema de prompts está diseñado para aprovechar al máximo las siguientes capacidades de Roo Code:

* **Gestión de Modos Personalizados:** Capacidad de definir modos con `slug`, `name`, `roleDefinition`, `whenToUse`, `groups` (toolsets permitidos con `fileRegex` para edición), y `customInstructions` (vía JSON o, preferentemente, archivos en `.roo/rules-{slug}/`).
* **Herramientas Internas de Roo Code:** Se instruye a los agentes para usar el conjunto completo de herramientas de Roo Code según su rol:
    * `Read Group`: `read_file`, `search_files`, `list_files`, `list_code_definition_names`.
    * `Edit Group`: `apply_diff`, `insert_content`, `search_and_replace`, `write_to_file`.
    * `Browser Group`: `browser_action`.
    * `Command Group`: `execute_command` (respetando la whitelist).
    * `MCP Group`: `use_mcp_tool`, `access_mcp_resource`.
    * `Workflow Group`: `ask_followup_question`, `attempt_completion`, `switch_mode`, `new_task`.
* **Interacción con Servidores MCP:** Uso claro y diferenciado de `use_mcp_tool` (para ejecutar acciones/funciones específicas de un servidor MCP, como `get-library-docs` de Context7 o `create_branch` de GitHub MCP) y `access_mcp_resource` (para obtener datos/contexto de un servidor MCP a través de un URI, como `repo://owner/repo/contents/file` para GitHub MCP o `context7://library_docs?id=...` si Context7 expusiera recursos vía URI). Los agentes consultarán el "Catálogo de Servidores MCP" (generado por ACP en `ESTANDARES_Y_GOBERNANZA_TI.md`) para los detalles de invocación.
* **Checkpoints:** Sistema automático de versionamiento de Roo Code, del cual los agentes son conscientes como una red de seguridad para la experimentación y la recuperación de errores durante la edición de archivos.
* **`.rooignore`:** Respeto por este archivo para controlar el acceso de la IA a los archivos del workspace.
* **Configuración de Modelo y Temperatura ("Sticky Models"):** Posibilidad de asignar LLMs y temperaturas específicas por modo, lo cual se considerará al definir cada modo personalizado.
* **Concurrencia:** Se confía en que Roo Code, a través de la serialización en la ejecución de sus herramientas (especialmente las de edición de archivos y comandos), maneja adecuadamente la concurrencia para prevenir la corrupción de archivos cuando múltiples agentes (operando en diferentes instancias/modos de Roo Code en el mismo workspace) intentan modificar recursos compartidos.

## 3. Cómo Utilizar los Archivos `.md` de Este Repositorio para Configurar Agentes en Roo Code ⚙️

La implementación de estos agentes en Roo Code sigue un proceso de configuración claro:

1.  **Configurar las Directrices Globales de Roo Code (Una Sola Vez por Workspace/Globalmente):**
    * Copie el contenido completo del archivo de la versión más reciente en `/INSTRUCCIONES_BASE_ROOCODE/` (ej. `v1.0_RooCode_Directrices_Globales.md`).
    * Péguelo en la sección "Custom Instructions for All Modes" en la Prompts Tab de Roo Code, o defínalo a nivel de workspace mediante un archivo en `.roo/rules/` (ej. `.roo/rules/00-global-directives.md`) en el proyecto que el ACP generará.

2.  **Configurar cada Agente Especializado como un Modo Personalizado en Roo Code:**
    * Para cada agente (`ACP`, `@Pepe`, `@Isma`, etc.):
        * Navegue a la carpeta del agente en `/AGENTES_DEFINICIONES_Y_PROMPTS/` (ej. `/Pepe_Product_Owner/`).
        * Seleccione el archivo de la versión más reciente del prompt completo (ej. `v1.0_Pepe_PO_Agente_IA_Prompt.md`).
        * **En Roo Code (preferentemente editando el archivo `.roomodes` del proyecto que el ACP generará, o el `custom_modes.json` global):**
            * **`slug`:** Defina un slug único (ej. "acp-configurator", "po-pepe").
            * **`name`:** Use el nombre descriptivo del agente (ej. "⚙️ ACP Configurator", "🦉 @Pepe (P. Owner)").
            * **`roleDefinition`:** Copie el contenido de la **Sección 1 ("Definición del Rol, Experiencia y Personalidad")** del archivo de prompt del agente.
            * **`whenToUse`:** Copie el contenido de la **Sección 2 ("Cuándo Usar este Modo")** del archivo de prompt del agente.
            * **`groups`:** Configure los toolsets y `fileRegex` apropiados para el rol del agente, basándose en las herramientas que se espera que utilice (detalladas en su Sección 3 de "Instrucciones Personalizadas").
            * **`customInstructions` (JSON):** Puede dejarse vacío, ya que las instrucciones detalladas se cargarán desde archivos.
        * **Archivos de Instrucciones Detalladas (`.roo/rules-{slug}/`):**
            * El **ACP será instruido para tomar el contenido de la Sección 3 ("Instrucciones Personalizadas para el Modo")** del archivo de prompt del agente, desglosarlo en múltiples archivos `.md` lógicos y con nombres secuenciales (ej. `01_objetivo.md`, `02_documentos_clave.md`, etc.), y colocarlos dentro de la carpeta `.roo/rules-{slug_del_agente}/` en el proyecto que el ACP genere. Roo Code cargará estas instrucciones en orden alfabético.

## 4. Versionamiento y Mantenimiento de Prompts (`agent_version_history.md`) 📊

* El archivo `agent_version_history.md` en la raíz de este repositorio es crucial para la gobernanza y evolución del sistema de agentes. Registra:
    * Cada agente (incluyendo el Agente Maestro del IDE y el ACP).
    * La versión de su prompt/definición.
    * La fecha de la versión.
    * El autor del cambio (Ranndy Salas Umaña).
    * La ruta al archivo Markdown que contiene esa versión específica del prompt completo.
    * Un resumen de los cambios significativos introducidos en esa versión del prompt.
* **Estrategia:** Cuando se mejora o modifica significativamente el prompt de un agente, se crea un **nuevo archivo versionado** en su carpeta respectiva (ej. `v1.1_Pepe_PO_Agente_IA_Prompt.md`) y se añade una nueva entrada detallada al `agent_version_history.md`.

## 5. Flujo de Trabajo Conceptual del Proyecto Generado por ACP 🚀

1.  El **ACP** (configurado como un Modo Personalizado en Roo Code) se ejecuta primero. Genera la infraestructura documental del proyecto `[NOMBRE DEL PROYECTO]`, incluyendo el archivo `.roomodes` con las definiciones de todos los agentes especializados, y las carpetas `.roo/rules-{slug}/` con sus instrucciones detalladas.
2.  Los agentes especializados (`@Pepe (P. Owner)`, `@Isma (Scrum Ms)`, etc.) se "activan" (o son activados por el usuario o un orquestador) cada uno en su propio Modo Personalizado configurado en Roo Code.
3.  Cada agente opera siguiendo las Directrices Globales de Roo Code, su `roleDefinition` (de la configuración del modo), y las instrucciones detalladas cargadas desde sus archivos en `.roo/rules-{slug_del_agente}/`.
4.  La colaboración y ejecución del proyecto ocurren dentro de la estructura de archivos `.md` del proyecto.

## 6. Propósito de Este Repositorio (Reiteración) 🎯

Este repositorio **NO CONTIENE EL PROYECTO DE SOFTWARE EN SÍ**, sino que contiene el **"CÓDIGO FUENTE" (en forma de prompts, definiciones de modo y directrices) PARA LOS AGENTES DE IA** que construirán y gestionarán dichos proyectos utilizando Roo Code. Es una infraestructura para la creación y gestión de equipos de IA autónomos y profesionales en el entorno Roo Code.

## 7. Derechos de Uso y Propiedad Intelectual ©️

Este "Sistema Avanzado de Agentes de IA para Gestión y Desarrollo de Proyectos Basado en Markdown (Optimizado para Roo Code)", incluyendo todos los prompts, definiciones de agentes y documentación contenida en este repositorio, es una obra original y propiedad intelectual de **Ranndy Salas Umaña**.

**Copyright (c) 2025 Ranndy Salas Umaña. Todos los derechos reservados.**

El contenido de este repositorio es para uso exclusivo del autor con el fin de desarrollar e implementar sistemas y soluciones comerciales. No se permite la reproducción, distribución, modificación o uso comercial de este material sin el consentimiento explícito y por escrito del autor.

Para cualquier consulta relacionada con el uso o licencia de este sistema, por favor contactar directamente a Ranndy Salas Umaña.

---