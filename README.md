# README: 🤖 Sistema de Agentes de IA para Gestión de Proyectos Basado en Markdown 📄

**Fecha de Creación (Sistema de Agentes):** 2025-05-15 22:55:30 CST 🗓️
**Versión del Sistema de Agentes:** 1.2 (Incluye referencia al Agente Maestro del IDE)


**Autor del Sistema y Prompts:** Ranndy Salas Umaña 
**Repositorio:** [https://github.com/Ranndy-90/Prompt_Agentes_IA]

## 1. Propósito de Este Documento y Repositorio 🎯

Este `README.md` y el contenido de este repositorio describen la estructura, configuración y operación de un sistema de Agentes de Inteligencia Artificial (IA) diseñado para gestionar y ejecutar proyectos de desarrollo de software de manera autónoma. El sistema se fundamenta en el uso exclusivo de archivos Markdown (`.md`) para la comunicación, la gestión de artefactos y toda la documentación del proyecto.

El objetivo es proporcionar una guía clara y los recursos necesarios (prompts, definiciones de roles, estructura documental) para cualquier persona que necesite entender, configurar, utilizar, replicar o contribuir a este sistema de agentes.

## 2. Visión General del Sistema de Agentes 💡

Este sistema se compone de varios niveles de configuración para los agentes de IA:

1.  **Agente Maestro del IDE (Configuración Base Universal):**
    * Representa la configuración fundamental y las directrices de comportamiento que se aplican a **todos** los agentes de IA que operan a través del IDE para este sistema. Estas son las "Instrucciones Personalizadas para Todos los Modos" y definen el "ADN" común, como el idioma, el profesionalismo, la adherencia a la documentación del proyecto y los principios generales de operación.

2.  **ACP (Asistente de Configuración de Proyectos) Estratégico:**
    * Un agente de IA de **tarea única**, que opera bajo las directrices del Agente Maestro del IDE, y es responsable de la generación inicial de toda la estructura de carpetas y la documentación base (plantillas, guías, políticas, roles) para un nuevo proyecto de software. Prepara el terreno para los agentes especializados.

3.  **Agentes Especializados de Rol Scrum:**
    * Un equipo de agentes de IA que asumen los roles Scrum tradicionales (Product Owner, Scrum Master, y miembros del Development Team con especialidades como UX/UI, Frontend, Backend y QA).
    * Cada uno opera bajo las directrices del Agente Maestro del IDE, y adicionalmente, con su propio conjunto de "Instrucciones Personalizadas para el Modo (Prompt de Rol y Operación Autónoma)" que define sus responsabilidades y ciclo operativo específico.
    * Estos agentes operan de forma **continua y autónoma** después de la configuración inicial del ACP, ejecutando el proyecto y colaborando a través de los archivos Markdown.

Todos los agentes interactúan y progresan en el proyecto mediante la creación, modificación y lectura de un conjunto estructurado de archivos `.md` dentro de un directorio de proyecto dedicado, el cual es generado por el ACP.

## 3. Componentes Clave del Sistema de Agentes (Contenido del Repositorio) 🧩

Este repositorio contiene:

* **Instrucciones para el Agente Maestro del IDE:** El archivo `INSTRUCCIONES_BASE_AGENTE_MAESTRO_IDE.md` (o como lo hayas nombrado) que contiene las "Instrucciones Personalizadas para Todos los Modos". Este es el primer nivel de configuración.
* **Definiciones Detalladas de Agentes Especializados:** Para el ACP y cada rol Scrum, se proporcionan:
    * `Definición del Rol, Experiencia y Personalidad`: Describe la "persona" del agente.
    * `Cuándo Usar este Modo (para el IDE)`: Guía para la selección de la tarea del agente.
    * `Instrucciones Personalizadas para el Modo (Prompt de Rol y Operación Autónoma)`: El prompt detallado y fundamental que define el comportamiento, las responsabilidades y el ciclo operativo del agente, complementando las instrucciones del Agente Maestro del IDE.
* **Estructura Documental del Proyecto:** Las plantillas y el contenido inicial que el ACP generaría para un nuevo proyecto, incluyendo artefactos Scrum, protocolos, guías, políticas, etc.
* **Metodología Subyacente:** Un marco Scrum adaptado para la colaboración asíncrona de agentes de IA, con un fuerte énfasis en seguridad, tecnologías modernas, estándares internacionales, y gestión de datos.

## 4. Configuración y Uso de los Agentes en un IDE 🛠️

1.  **Configuración del Agente Maestro del IDE (Una Sola Vez):**
    * En la sección del IDE para "Instrucciones personalizadas para todos los modos", ingresar el contenido del archivo `INSTRUCCIONES_BASE_AGENTE_MAESTRO_IDE.md`. Esto establece el comportamiento fundamental para cualquier agente invocado.

2.  **Agente ACP (Asistente de Configuración de Proyectos):**
    * **Función:** Ejecutar una **única vez** al inicio de un nuevo proyecto.
    * **Configuración en IDE:**
        * **Solicitud de Soporte (Tipo de Tarea):** "Iniciar nueva tarea".
        * **Instrucciones Personalizadas para el Modo (`${userInput}`):** Utilizar el "Prompt para el Asistente de Configuración de Proyectos (ACP) Estratégico" completo.
    * **Resultado:** Creación de la estructura de carpetas y todos los archivos `.md` iniciales del proyecto.

3.  **Agentes Especializados de Rol Scrum (`@AN (PO)`, `@CD (SM)`, `@DUX`, `@EDF`, `@EDB`, `@EPS`):**
    * **Función:** Operar de forma continua y autónoma durante todo el ciclo de vida del proyecto.
    * **Configuración en IDE (para cada agente):**
        * **Solicitud de Soporte (Tipo de Tarea):** Generalmente "Iniciar nueva tarea" para activar un ciclo operativo, o un modo de operación continua si el IDE lo soporta.
        * **Instrucciones Personalizadas para el Modo:** Utilizar el "Prompt de Rol y Operación Autónoma" específico y completo para ese rol. Estas instrucciones son la **configuración específica del rol** que se suma a las instrucciones del Agente Maestro del IDE.
        * **Activación (`${userInput}` para "Iniciar nueva tarea"):** Un comando breve para que el agente (ya configurado con sus instrucciones de rol y las generales del Agente Maestro) ejecute su ciclo de trabajo o se enfoque en una tarea.

## 5. Flujo de Interacción del Sistema 🔄

1.  Se configuran las **Instrucciones del Agente Maestro del IDE**.
2.  El **ACP** se ejecuta primero (heredando el comportamiento base del Agente Maestro y siguiendo su prompt específico) y crea la infraestructura documental.
3.  Los **Agentes Especializados** se "activan". Cada uno hereda el comportamiento base del Agente Maestro y sigue adicionalmente su propio "Prompt de Rol y Operación Autónoma".
4.  Cada agente consulta su archivo de rol en `/documentacion_general/roles_y_responsabilidades/` y la `guia_operativa_del_equipo.md` para entender su contexto y cómo proceder.
5.  El `@CD (SM)` comienza a facilitar el proceso Scrum.
6.  La colaboración y el progreso se registran en `chats_entre_agentes.md` y en la modificación de los artefactos `.md`.

## 6. Propósito de Este Repositorio 🎯

Este repositorio sirve como:
* Un **"blueprint"** para construir y configurar un equipo de agentes de IA capaces de gestionar proyectos de software.
* Un **conjunto de prompts avanzados** y detallados para guiar el comportamiento de dichos agentes.
* Un **ejemplo de un sistema de gestión de proyectos** completamente basado en Markdown y operado por IA.
* Una base para la **investigación y experimentación** en colaboración de IA, desarrollo de software autónomo y flujos de trabajo ágiles en entornos distribuidos y asíncronos.

## 7. Contribuciones y Licencia 📝

(Placeholder: "[Considera añadir información sobre cómo otros podrían contribuir a este sistema de agentes, si es un proyecto abierto, y bajo qué licencia se distribuye (ej. MIT, Apache 2.0).]")

---