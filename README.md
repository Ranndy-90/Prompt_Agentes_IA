#  README: Sistema de Agentes de IA para Gestión de Proyectos Basado en Markdown

**Fecha de Creación (Sistema de Agentes):** 2025-05-16 CST
**Desarrollado por: Ranndy Salas Umaña**
**GitHub:** https://github.com/Ranndy-90/Prompt_Agentes_IA
**Versión del Sistema de Agentes:** 1.0

## 1. Propósito de Este Documento

Este `README.md` describe la estructura, configuración y operación del sistema de Agentes de Inteligencia Artificial (IA) diseñado para gestionar y ejecutar proyectos de desarrollo de software de manera autónoma, utilizando exclusivamente archivos Markdown (`.md`) para la comunicación, la gestión de artefactos y la documentación.

El objetivo es proporcionar una guía clara para cualquier persona que necesite entender, configurar, utilizar o mantener este sistema de agentes.

## 2. Visión General del Sistema de Agentes

Este sistema se compone de dos tipos principales de agentes de IA:

1.  **ACP (Asistente de Configuración de Proyectos):** Un agente de IA de tarea única, responsable de la generación inicial de toda la estructura de carpetas y la documentación base del proyecto.
2.  **Agentes Especializados de Rol Scrum:** Un equipo de agentes de IA que asumen los roles Scrum (Product Owner, Scrum Master, Development Team con sus especialidades) y operan de forma continua y autónoma para ejecutar el proyecto, basándose en la infraestructura documental creada por el ACP.

Todos los agentes interactúan y progresan en el proyecto a través de la creación, modificación y lectura de un conjunto estructurado de archivos `.md` dentro de un directorio de proyecto dedicado.

## 3. Configuración y Uso de los Agentes en el IDE

Cada agente de IA (tanto el ACP como los agentes especializados) se configura en el IDE proporcionando tres componentes clave para su "Modo" de operación:

1.  **Definición del Rol, Experiencia y Personalidad:**
    * Describe la identidad, el tono, el enfoque principal, el nivel de expertise simulado y las habilidades clave del agente. Este componente define la "persona" del agente de IA.

2.  **Cuándo Usar este Modo (para el IDE):**
    * Proporciona una descripción clara de cuándo el modo específico de este agente es más efectivo y para qué tipos de tareas es más adecuado. Ayuda al usuario del IDE a seleccionar el agente correcto para la acción deseada.

3.  **Instrucciones Personalizadas para el Modo (Prompt de Rol y Operación Autónoma):**
    * Este es el **prompt principal y más detallado** que se le da al agente. Contiene su objetivo específico, los documentos `.md` clave que debe analizar y/o generar, su ciclo operativo autónomo, las heurísticas para la toma de decisiones y su rutina de trabajo. Es, en esencia, el "programa" o "sistema operativo" del agente para ese rol.

### 3.1. Agente ACP (Asistente de Configuración de Proyectos)

* **Fin del ACP:**
    * El ACP es un agente de **tarea única y ejecución inicial**. Su único propósito es ejecutar el "Prompt para el Asistente de Configuración de Proyectos (ACP) Estratégico" (que se le proporciona como `${userInput}` en el modo "Iniciar nueva tarea" del IDE).
    * Este prompt le instruye para crear toda la estructura de carpetas y el conjunto inicial de archivos `.md` (plantillas, guías, políticas, roles, etc.) que el proyecto necesita para comenzar.
    * Una vez que el ACP ha completado esta generación, **su función en el proyecto ha terminado.** No participa en los Sprints ni en la operación continua del proyecto.
* **Configuración en el IDE:**
    * **Solicitud de Soporte (Tipo de Tarea):** "Iniciar nueva tarea".
    * **Definición del Rol, Experiencia y Personalidad:** (Referirse al documento Markdown específico del ACP).
    * **Cuándo Usar este Modo:** (Referirse al documento Markdown específico del ACP - usar solo una vez al inicio del proyecto).
    * **Instrucciones Personalizadas para el Modo (`${userInput}`):** El "Prompt para el Asistente de Configuración de Proyectos (ACP) Estratégico" completo.
* **Orden de Uso:** El ACP es el **PRIMER** agente que se ejecuta para un nuevo proyecto.

### 3.2. Agentes Especializados de Rol Scrum

Estos agentes (`@AN (PO)`, `@CD (SM)`, `@DUX (Dev Team)`, `@EDF (Dev Team)`, `@EDB (Dev Team)`, `@EPS (Dev Team)`) están diseñados para operar de forma continua y autónoma después de que el ACP haya configurado la infraestructura documental.

* **Fin de los Agentes Especializados:**
    * Cada agente especializado ejecuta las responsabilidades de su rol Scrum (Product Owner, Scrum Master, o miembro del Development Team con una especialidad) interactuando con los archivos `.md` según su "Prompt de Rol y Operación Autónoma".
    * Colaboran asíncronamente a través de `chats_entre_agentes.md` y modificando los artefactos `.md` relevantes.
    * Su objetivo es desarrollar el producto `[NOMBRE DEL PROYECTO]` siguiendo el proceso Scrum adaptado.
* **Configuración en el IDE (para cada agente especializado):**
    * **Solicitud de Soporte (Tipo de Tarea):** Generalmente "Iniciar nueva tarea" para activar un ciclo operativo, o un modo de operación continua si el IDE lo soporta.
    * **Definición del Rol, Experiencia y Personalidad:** (Referirse al documento Markdown específico de cada rol, ej., `product_owner_AN.md` en la sección de descripción del rol).
    * **Cuándo Usar este Modo:** (Referirse al documento Markdown específico de cada rol - generalmente indica operación continua durante todo el proyecto).
    * **Instrucciones Personalizadas para el Modo:** El "Prompt de Rol y Operación Autónoma" completo y específico para ese rol. Este prompt es la configuración base o "sistema operativo" persistente del agente para su rol. El `${userInput}` al activar el modo "Iniciar nueva tarea" sería un comando para ejecutar su ciclo (ej., "Ejecutar ciclo operativo como @AN (PO)").
* **Orden de Uso:** Se activan después de que el ACP ha completado su trabajo y el Sprint 000 de revisión y adaptación de la documentación ha comenzado. Operan en paralelo y de forma continua.

## 4. Estructura de la Documentación Generada y Relación entre Agentes

El ACP genera una estructura de carpetas (detallada en `folder_structure_guide.md`) que contiene:

* **Documentos Fundamentales (Raíz):** `README.md` (este archivo, si es sobre el sistema de agentes, o el del proyecto si es para el proyecto en sí), `communication_protocol.md`, `scrum_process_overview.md`, etc. Estos establecen las reglas del juego.
* **Artefactos Scrum:** `product_backlog.md`, `definition_of_done.md`, y plantillas para Sprints en `/sprints/`.
* **Roles y Responsabilidades:** En `/documentacion_general/roles_y_responsabilidades/`, cada agente especializado encuentra su descripción de rol detallada y cómo debe operar. Estas descripciones se basan en los "Prompts de Rol y Operación Autónoma".
* **Documentación Técnica, de Diseño y Pruebas:** Plantillas y guías en `/documentacion_general/` para que los agentes especializados las completen.
* **Chat Central:** `chats_entre_agentes.md` es el nexo de comunicación donde los agentes interactúan, notifican progreso, hacen preguntas y reportan impedimentos.

### Flujo de Interacción (Simplificado):

1.  **ACP** crea toda la estructura y documentación inicial. Su última acción es escribir el mensaje de bienvenida en `chats_entre_agentes.md`.
2.  Los agentes especializados (`@AN (PO)`, `@CD (SM)`, `@DevelopmentTeam`) se "activan".
3.  Cada agente consulta su archivo de rol en `/documentacion_general/roles_y_responsabilidades/` y la `guia_operativa_del_equipo.md` para entender su contexto y cómo proceder.
4.  El `@CD (SM)` comienza a facilitar el proceso Scrum, iniciando el Sprint 000 (cuya planificación está en `sprint_000_backlog.md`).
5.  El `@AN (PO)` comienza a gestionar el `product_backlog.md`.
6.  El `@DevelopmentTeam` (DUX, EDF, EDB, EPS) comienza a trabajar en las tareas del `sprint_000_backlog.md` y luego en los PBIs de los sprints subsiguientes, cada uno enfocándose en sus especialidades pero colaborando a través de `chats_entre_agentes.md` y los archivos `.md` compartidos.
7.  Todos los agentes deben **analizar primero `chats_entre_agentes.md`** para obtener las últimas actualizaciones y luego los archivos específicos de su rol o tareas actuales (`sprint_X_backlog.md`, `product_backlog.md`, etc.).

## 5. Importancia de los Prompts de Rol ("Instrucciones Personalizadas")

Los documentos de "Instrucciones Personalizadas para el Modo (Prompt de Rol y Operación Autónoma)" para cada agente especializado son **CRUCIALES**. Definen:

* Su identidad y objetivo.
* Los archivos `.md` clave que deben analizar y con los que deben interactuar.
* Su ciclo operativo autónomo (qué hacen, en qué orden).
* Cómo se comunican y colaboran con otros agentes.
* Las heurísticas y directrices para su toma de decisiones (seguridad, estándares, herramientas, etc.).

Estos prompts deben ser la **configuración base y persistente** para cada agente especializado en el IDE.

## 6. Mantenimiento y Evolución del Sistema

Este sistema de agentes y su documentación base están diseñados para ser el punto de partida. El equipo (los agentes de IA, bajo la facilitación del `@CD (SM)`) puede y debe proponer mejoras a los procesos, plantillas y guías durante las Sprint Retrospectives. Los cambios acordados se reflejarán en la actualización de los archivos `.md` correspondientes.

Este `README.md` debe ser consultado por cualquier persona que necesite entender cómo está diseñado y cómo opera este sistema de desarrollo de software autónomo basado en agentes de IA y Markdown.