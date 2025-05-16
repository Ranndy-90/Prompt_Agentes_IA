# README: 🤖 Sistema Avanzado de Agentes de IA para Gestión y Desarrollo de Proyectos Basado en Markdown 📄

**Fecha de Creación (Sistema de Agentes):** 2025-05-16 14:22:00 CST 🗓️
**Versión del Sistema de Agentes:** 1.3 (Detalla estructura de prompts y estrategia de uso)


**Autor del Sistema y Prompts:** Ranndy Salas Umaña - 
**Repositorio:** [https://github.com/Ranndy-90/Prompt_Agentes_IA](https://github.com/Ranndy-90/Prompt_Agentes_IA) (Este mismo repositorio)

## 1. Propósito de Este Repositorio y Sistema 🎯

Este repositorio alberga la **arquitectura completa de prompts e instrucciones** para un sistema avanzado de Agentes de Inteligencia Artificial (IA) diseñado para gestionar y ejecutar proyectos de desarrollo de software de manera autónoma y profesional. El sistema se fundamenta en el uso exclusivo de archivos Markdown (`.md`) para la comunicación, la gestión de artefactos y toda la documentación del proyecto que los agentes crearán y utilizarán.

El objetivo es proporcionar:
* Una **base de conocimiento centralizada** para la configuración y operación de cada agente de IA.
* **Prompts detallados y versionados** que actúan como el "software" o "sistema operativo" de cada agente.
* Una **metodología clara** sobre cómo estos agentes colaboran dentro de un marco Scrum adaptado.
* Un **recurso para desarrolladores, investigadores y entusiastas de la IA** interesados en sistemas de agentes autónomos para el desarrollo de software.

## 2. Visión General del Sistema de Agentes y su Arquitectura de Prompts 💡

El sistema se basa en un conjunto de agentes de IA, cada uno con un rol y responsabilidades específicas, que operan siguiendo directrices fundamentales y prompts de rol detallados.

### 2.1. Estructura de Carpetas de Este Repositorio (`Prompt_Agentes_IA`):

La organización de este repositorio es crucial para entender y gestionar las definiciones de los agentes:

```text
/Prompt_Agentes_IA/
├── /AGENTES_DEFINICIONES_Y_PROMPTS/  # Contiene los prompts completos para cada agente especializado
│   ├── /ACP_Asistente_Configuracion_Proyectos/
│   │   ├── v1.0_ACP_Prompt_Completo.md
│   │   └── ... (futuras versiones)
│   ├── /AN_PO_Product_Owner/
│   │   ├── v1.0_AN_PO_Prompt_Completo.md
│   │   └── ...
│   ├── /CD_SM_Scrum_Master/
│   │   ├── v1.0_CD_SM_Prompt_Completo.md
│   │   └── ...
│   ├── /DUX_DevTeam_Diseñadora_UXUI/
│   │   ├── v1.0_DUX_Prompt_Completo.md
│   │   └── ...
│   ├── /EDF_DevTeam_Desarrollador_Frontend/
│   │   ├── v1.0_EDF_Prompt_Completo.md
│   │   └── ...
│   ├── /EDB_DevTeam_Desarrollador_Backend/
│   │   ├── v1.0_EDB_Prompt_Completo.md
│   │   └── ...
│   └── /EPS_DevTeam_QA/
│       ├── v1.0_EPS_Prompt_Completo.md
│       └── ...
│
├── /INSTRUCCIONES_BASE_AGENTE_MAESTRO_IDE/ # Contiene las directrices fundamentales para todos los agentes
│   ├── v1.0_Instrucciones_Base.md
│   └── ... (futuras versiones)
│
├── agent_version_history.md  # Log de cambios y versiones de los prompts de los agentes
├── README.md                 # Este archivo
└── ... (otros archivos de soporte como LICENSE, .gitignore)
```

---
### 2.2. Componentes de la Definición de Cada Agente 🧩

Cada archivo `vX.X_NombreAgente_Prompt_Completo.md` (ubicado dentro de la carpeta del agente correspondiente en `/AGENTES_DEFINICIONES_Y_PROMPTS/`) está estructurado en tres secciones vitales. Estas secciones, en conjunto, definen completamente cómo un agente de IA debe ser configurado en un IDE o plataforma de IA, y cómo debe operar autónomamente:

1.  **`Definición del Rol, Experiencia y Personalidad`**:
    * **Función:** Esta sección establece la "persona" del agente. Describe su título de rol, cómo se percibe a sí mismo dentro del equipo, su tono y estilo de comunicación (incluyendo el uso sugerido de emojis para añadir un toque de personalidad sin perder profesionalismo), su enfoque principal, el nivel de expertise que debe simular y las habilidades clave que posee. También se incluye una directriz sobre su "malicia constructiva" para roles donde la anticipación de problemas (especialmente de seguridad) es crucial.
    * **Estrategia de Uso:** Proporciona a la IA una base sólida para la emulación de un rol profesional específico y altamente competente. Guía no solo *qué* hace el agente, sino *cómo* se presenta y se comporta, contribuyendo a una dinámica de equipo más realista y efectiva, incluso entre agentes de IA.

2.  **`Cuándo Usar este Modo (para el IDE)`**:
    * **Función:** Es una guía descriptiva para el usuario humano que configura el agente en un Entorno de Desarrollo Integrado (IDE) o plataforma de IA. Detalla las situaciones y los tipos de tareas para los cuales el "modo" operativo de ese agente (definido por su prompt completo) es más efectivo y adecuado.
    * **Estrategia de Uso:** Ayuda a asegurar que el agente correcto sea invocado o configurado para la tarea correcta dentro del sistema general de agentes. Optimiza el uso de recursos y la relevancia de la acción del agente, contextualizando su propósito dentro del ciclo de vida del proyecto. Para el ACP, indica una ejecución única inicial (y un modo de mantenimiento opcional); para los roles Scrum, indica una operación continua.

3.  **`Instrucciones Personalizadas para el Modo (Prompt de Rol y Operación Autónoma)`**:
    * **Función:** Este es el **núcleo programático del agente**. Es el prompt detallado y extenso que se le proporciona al motor de IA (idealmente como un "system prompt" o un prompt de tarea principal persistente y versionado) para definir su comportamiento específico, su ciclo operativo autónomo, los documentos `.md` clave que debe analizar y/o generar, cómo interactúa con otros agentes a través de `chats_entre_agentes.md`, cómo gestiona sus entregables en Markdown (incluyendo el versionamiento de documentos del proyecto con encabezados YAML y el registro en `document_version_history.md`), y las heurísticas fundamentales para su toma de decisiones autónoma. Incluye directrices explícitas sobre el uso de herramientas (priorizando la terminal/consola), la consulta de documentación oficial (vía MCP Context7 si está disponible), la adherencia a estándares internacionales (ISO, ITIL, COBIT, Six Sigma conceptualmente), la gestión de datos y la seguridad.
    * **Estrategia de Uso:** Estas instrucciones buscan guiar a la IA para que opere de manera autónoma y continua (para los roles Scrum) o para una tarea específica y compleja (para el ACP), adhiriéndose a las directrices avanzadas de realismo, profesionalismo y excelencia técnica. Es, en esencia, el "software" o el "sistema operativo" detallado del agente para ese rol específico.

### 2.3. Instrucciones Base del Agente Maestro del IDE 🧬

* El archivo (versionado) en la carpeta `/INSTRUCCIONES_BASE_AGENTE_MAESTRO_IDE/` (ej. `v1.0_Instrucciones_Base.md`) contiene las **directrices fundamentales y universales** que se aplican a CUALQUIER agente operando bajo este sistema a través del IDE o plataforma de IA.
* **Función:** Define el "ADN" o comportamiento común a todos los agentes: idioma de operación, profesionalismo, adherencia general a la documentación del proyecto, principios de seguridad, uso de herramientas, consulta de documentación oficial, formato de comunicación base, etc.
* **Estrategia de Uso:** Estas instrucciones base son el **primer nivel de configuración** en el IDE (generalmente como "instrucciones personalizadas para todos los modos" o un system prompt global). Las "Instrucciones Personalizadas para el Modo" de cada agente especializado luego se suman, complementan y especializan este comportamiento base, sin contradecirlo.

## 3. Cómo Utilizar los Archivos `.md` como Prompts para los Agentes de IA ⚙️

La implementación de estos agentes en un IDE o plataforma de IA sigue un proceso de configuración claro:

1.  **Configuración del Agente Maestro del IDE (Una Sola Vez por Entorno de IDE):**
    * Copie el contenido completo del archivo de la versión más reciente en `/INSTRUCCIONES_BASE_AGENTE_MAESTRO_IDE/` (ej. `v1.1_Instrucciones_Base.md`).
    * Péguelo en la sección del IDE designada para "Instrucciones personalizadas para todos los modos" o su equivalente funcional. Esto establece el comportamiento fundamental para cualquier agente invocado dentro de este sistema.

2.  **Configuración de un Agente Específico (ACP o Rol Scrum):**
    * Navegue a la carpeta del agente deseado en `/AGENTES_DEFINICIONES_Y_PROMPTS/` (ej. `/AN_PO_Product_Owner/`).
    * Seleccione el archivo de la versión más reciente del prompt completo (ej. `v1.1_AN_PO_Prompt_Completo.md`).
    * **Para la configuración persistente del rol/modo en el IDE (especialmente para los roles Scrum de operación continua):**
        * La **Sección 1 (`Definición del Rol, Experiencia y Personalidad`)** y la **Sección 2 (`Cuándo Usar este Modo`)** de este archivo sirven como metadatos y descripción para el humano que configura el agente en el IDE. Ayudan a entender el propósito y las características del agente.
        * El contenido completo de la **Sección 3 (`Instrucciones Personalizadas para el Modo (Prompt de Rol y Operación Autónoma)`)** es el que se debe copiar y pegar en el campo del IDE destinado a las instrucciones específicas de ese modo o rol del agente. Este es el prompt principal que la IA ejecutará para desempeñar su rol.
    * **Para el Agente ACP (Tarea Única de Generación Inicial):**
        * Seleccione el modo "Iniciar nueva tarea" (o equivalente) en el IDE.
        * Copie el contenido de la Sección 3 del archivo `vX.X_ACP_Prompt_Completo.md` y péguelo como el `${userInput}` o el prompt principal para esa tarea de generación inicial.
    * **Para los Agentes Scrum (Activación de Ciclos Operativos):**
        * Asumiendo que la Sección 3 de su prompt de rol ya está configurada como su instrucción base persistente en el IDE:
        * Para "activar" un ciclo de trabajo o una tarea específica dentro de su rol, puede usar el modo "Iniciar nueva tarea" con un `${userInput}` breve que actúe como un comando o un foco contextual (ej., `"@CD (SM), ejecutar ciclo diario de Scrum Master, con especial atención al impedimento #IMP-005."`),

## 4. Versionamiento y Mantenimiento de Prompts (`agent_version_history.md`) 📊

* El archivo `agent_version_history.md` en la raíz de este repositorio es crucial para la gobernanza y evolución del sistema de agentes. Registra:
    * Cada agente (incluyendo el Agente Maestro del IDE y el ACP).
    * La versión de su prompt/definición.
    * La fecha de la versión.
    * El autor del cambio (Ranndy Salas Umaña).
    * La ruta al archivo Markdown que contiene esa versión específica del prompt completo.
    * Un resumen de los cambios significativos introducidos en esa versión del prompt.
* **Estrategia:** Cuando se mejora o modifica significativamente el prompt de un agente, se crea un **nuevo archivo versionado** en su carpeta respectiva (ej. `v1.2_AN_PO_Prompt_Completo.md`) y se añade una nueva entrada detallada al `agent_version_history.md`. Esto asegura la trazabilidad, permite la reproducibilidad y facilita la reversión a versiones anteriores si una nueva versión del prompt no produce los resultados esperados.

## 5. Flujo de Trabajo Conceptual del Proyecto Generado por ACP 🚀

Una vez que el ACP ha ejecutado su prompt y generado la estructura documental de un nuevo proyecto `[NOMBRE DEL PROYECTO]`:

1.  Los agentes especializados (`@AN (PO)`, `@CD (SM)`, `@DevelopmentTeam`) se "activan" (o son activados).
2.  Cada agente "conoce" su rol y cómo operar consultando su archivo de rol específico en `/[NOMBRE DEL PROYECTO]/documentacion_general/roles_y_responsabilidades/` (que es una instancia generada por el ACP basada en los prompts de este repositorio) y la `/[NOMBRE DEL PROYECTO]/guia_operativa_del_equipo.md`.
3.  El `@CD (SM)` comienza a facilitar el proceso Scrum, iniciando el Sprint 000 (cuya planificación está en `/[NOMBRE DEL PROYECTO]/sprints/sprint_000_setup/sprint_000_backlog.md`).
4.  El `@AN (PO)` comienza a gestionar el `/[NOMBRE DEL PROYECTO]/product_backlog.md`.
5.  El `@DevelopmentTeam` (DUX, EDF, EDB, EPS) comienza a trabajar en las tareas del `sprint_000_backlog.md` y luego en los PBIs de los sprints subsiguientes, cada uno enfocándose en sus especialidades pero colaborando intensamente a través de `/[NOMBRE DEL PROYECTO]/chats_entre_agentes.md` y los archivos `.md` compartidos.
6.  Todos los agentes deben **analizar primero `/[NOMBRE DEL PROYECTO]/chats_entre_agentes.md`** (y sus índices/resúmenes) para obtener las últimas actualizaciones y luego los archivos específicos de su rol o tareas actuales.

## 6. Propósito de Este Repositorio (Reiteración) 🎯

Este repositorio **NO CONTIENE EL PROYECTO DE SOFTWARE EN SÍ**, sino que contiene el **"CÓDIGO FUENTE" (en forma de prompts y definiciones detalladas) PARA LOS AGENTES DE IA** que construirán y gestionarán dichos proyectos. Es una infraestructura para la creación y gestión de equipos de IA autónomos y profesionales.

## 7. Derechos de Uso y Propiedad Intelectual ©️

Este "Sistema de Agentes de IA para Gestión y Desarrollo de Proyectos Basado en Markdown", incluyendo todos los prompts, definiciones de agentes y documentación contenida en este repositorio, es una obra original y propiedad intelectual de **Ranndy Salas Umaña**.

**Copyright (c) 2025 Ranndy Salas Umaña. Todos los derechos reservados.**

El contenido de este repositorio es para uso exclusivo del autor con el fin de desarrollar e implementar sistemas y soluciones comerciales. No se permite la reproducción, distribución, modificación o uso comercial de este material sin el consentimiento explícito y por escrito del autor.

Para cualquier consulta relacionada con el uso o licencia de este sistema, por favor contactar directamente a Ranndy Salas Umaña.
---