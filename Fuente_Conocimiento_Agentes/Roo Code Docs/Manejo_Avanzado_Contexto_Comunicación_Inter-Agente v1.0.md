# Prompt Complementario: Manejo Avanzado de Contexto y Comunicación Inter-Agente en Roo Code v1.0

**Propósito:** Este documento proporciona estrategias detalladas, ejemplos prácticos y consideraciones avanzadas para la gestión efectiva del contexto y la comunicación entre agentes (Modos) dentro de un Sistema Multi-Agente (SMA) implementado en Roo Code. Se enfoca en el uso táctico de las herramientas `new_task`, `attempt_completion`, y `ask_followup_question`, así como en mecanismos indirectos de compartición de contexto.

**Audiencia:** Una IA que ya posee el conocimiento del prompt "Contexto Experto para el Diseño de Sistemas Multi-Agente con Énfasis en Roo Code v1.0" y que busca profundizar en la implementación práctica de flujos de comunicación y manejo de contexto en SMA con Roo Code.

**Referencia Principal Asumida:** La IA debe referirse al prompt principal para los detalles de las herramientas (`new_task`, `attempt_completion`, etc.) y las definiciones de los Modos.

---

## 1. Estrategias Detalladas para la Formulación del Contexto en `message` (al usar `new_task`)

La herramienta `new_task` es el principal mecanismo para que un agente Orquestador delegue una subtarea a un agente especializado. La calidad y completitud del parámetro `message` son cruciales para el éxito de la subtarea, dado el aislamiento del historial de conversación de la subtarea.

### 1.1. Principios Generales para Estructurar el `message`:
* **Claridad y Concisión:** El `message` debe ser inequívoco y directo.
* **Autocontención:** Debe incluir *toda* la información necesaria para que el agente especializado comience y complete la tarea, o sepa cómo obtenerla (ej., referencias a archivos a leer).
* **Especificidad del Rol:** El `message` debe estar alineado con el `roleDefinition` y las `customInstructions` del modo receptor.
* **Formato:** Considerar el uso de Markdown para estructurar el `message` si es complejo, facilitando su procesamiento por el LLM del agente receptor.

### 1.2. Ejemplos Concretos de Estructuración del `message`:

#### Ejemplo 1: Agente de Código (`@CoderAgent`)
* **Tarea del Orchestrator:** "Implementar la función `calculateTotalPrice` en `src/utils/cart.js` que tome un array de items y devuelva el precio total. Considerar descuentos por item y un descuento general si el total supera los $100. Seguir los estándares de codificación del proyecto."

* **Estructura del `message` para `@CoderAgent` (pasado vía `new_task`):**
    ```markdown
    **Tarea Asignada: Implementar Función de Cálculo de Precio Total**

    **Descripción General:**
    Debes implementar una función llamada `calculateTotalPrice` en el archivo `src/utils/cart.js`.
    Esta función recibirá un array de objetos `item`, donde cada item tiene las propiedades `price` (number) y `discount` (number, opcional, porcentaje ej. 0.10 para 10%).
    La función debe calcular el precio total de los items, aplicando descuentos individuales.
    Adicionalmente, si el subtotal (después de descuentos individuales) supera los $100, se debe aplicar un descuento general del 5% al subtotal.
    La función debe retornar el precio final como un número.

    **Archivo a Modificar:**
    * `path`: `src/utils/cart.js`
        * (Orchestrator podría incluir un fragmento del archivo si es relevante usando `read_file` previamente:
            ```javascript
            // Contenido actual de src/utils/cart.js (o relevante)
            export const existingFunction = () => { ... };
            // Inserta la nueva función aquí o donde corresponda
            ```
            )

    **Requisitos Específicos:**
    1.  Manejar casos donde el array de items esté vacío (debe retornar 0).
    2.  Asegurar que los cálculos de descuento sean correctos.
    3.  La función debe ser exportada.

    **Estándares de Codificación a Seguir:**
    * Referencia: `project_docs/coding_standards.md` (El Orchestrator podría instruir al `@CoderAgent` a usar `read_file` si no se asume que ya lo conoce).
    * Usar JSDoc para documentar la función, sus parámetros y lo que retorna.

    **Resultado Esperado (para `attempt_completion`):**
    * Confirmación de la implementación.
    * Ruta del archivo modificado.
    * El bloque de código de la función implementada.
    * (Opcional) Un diff de los cambios realizados.
    ```

#### Ejemplo 2: Agente de Investigación (`@ResearchAgent`)
* **Tarea del Orchestrator:** "Investigar las mejores prácticas actuales para la optimización de rendimiento en aplicaciones React con Server-Side Rendering (SSR) y generar un resumen."

* **Estructura del `message` para `@ResearchAgent`:**
    ```markdown
    **Tarea Asignada: Investigación sobre Optimización de Rendimiento React SSR**

    **Pregunta de Investigación Principal:**
    ¿Cuáles son las mejores prácticas y técnicas más efectivas en [Año Actual] para optimizar el rendimiento de aplicaciones React que utilizan Server-Side Rendering (SSR)?

    **Palabras Clave Sugeridas:**
    * React SSR performance
    * Next.js optimization
    * SSR caching strategies React
    * Code splitting React SSR
    * Lazy loading components SSR

    **Fuentes Ya Exploradas (para evitar redundancia):**
    * `https://react.dev/reference/react-dom/server` (Revisado superficialmente)
    * `https://nextjs.org/docs/pages/building-your-application/rendering/server-side-rendering` (Revisado)

    **Alcance y Enfoque:**
    * Centrarse en técnicas aplicables a aplicaciones de tamaño mediano a grande.
    * Considerar aspectos de Time to First Byte (TTFB), Largest Contentful Paint (LCP), e interactividad.
    * Identificar herramientas o librerías comunes que asistan en estas optimizaciones.

    **Formato Esperado del Resultado (para `attempt_completion`):**
    * Un resumen en Markdown (300-500 palabras).
    * Una lista de 5-7 mejores prácticas clave, cada una con una breve explicación.
    * Enlaces a 3-5 artículos o recursos primarios de alta calidad que respalden los hallazgos.
    * Secciones: Introducción, Prácticas de Optimización (detalladas), Herramientas, Conclusión, Referencias.
    ```

#### Ejemplo 3: Agente de QA (`@QAAgent`)
* **Tarea del Orchestrator:** "Realizar pruebas funcionales para la nueva funcionalidad de login (PBI-123) según los criterios de aceptación y los casos de prueba definidos."

* **Estructura del `message` para `@QAAgent`:**
    ```markdown
    **Tarea Asignada: Pruebas Funcionales - Funcionalidad de Login (PBI-123)**

    **Contexto del PBI-123:**
    * Título: Implementar Autenticación de Usuarios
    * Descripción: "Como usuario, quiero poder iniciar sesión con mi correo y contraseña para acceder a mi panel de control."
    * (Orchestrator podría incluir la descripción completa del PBI si es extensa, o un enlace al archivo del PBI).

    **Criterios de Aceptación (CA) a Validar (extraídos de `PBI-123.md`):**
    1.  CA-1: El usuario puede iniciar sesión con credenciales válidas.
    2.  CA-2: Se muestra un mensaje de error específico para credenciales incorrectas.
    3.  CA-3: Se muestra un mensaje de error si el campo de correo o contraseña están vacíos.
    4.  CA-4: Tras un login exitoso, el usuario es redirigido al `/dashboard`.
    5.  CA-5: La contraseña se envía de forma segura (verificación conceptual, no de implementación).

    **Artefactos a Probar:**
    * URL de la página de login: `https://[entorno_de_pruebas]/login`
    * (Si hay una build específica, proporcionar detalles o acceso)

    **Casos de Prueba a Ejecutar:**
    * Referencia al documento de casos de prueba: `testing_docs/PBI-123_test_cases.md`
        * (Instruir al `@QAAgent` a leer este archivo usando `read_file`).
        * (Opcional: El Orchestrator podría listar los IDs de los casos de prueba más críticos directamente).

    **Acciones Esperadas:**
    1.  Leer y comprender los CAs y los casos de prueba.
    2.  Ejecutar los casos de prueba en el entorno especificado.
    3.  Documentar los resultados de cada caso de prueba (éxito/fallo, observaciones, evidencia si es necesario).
    4.  Reportar cualquier bug encontrado según la plantilla `project_docs/bug_report_template.md`.

    **Formato Esperado del Resultado (para `attempt_completion`):**
    * Resumen general del estado de las pruebas para PBI-123.
    * Enlace al documento de casos de prueba actualizado con los resultados (`testing_docs/PBI-123_test_cases_results.md`).
    * Lista de IDs de bugs reportados (si alguno).
    * Confirmación de que todos los CAs fueron cubiertos.
    ```

### 1.3. Técnicas de Resumen de Contexto Previo (para el Orchestrator):
Si la tarea padre (gestionada por el Orchestrator) tiene un historial de conversación muy largo antes de delegar con `new_task`, es ineficiente y propenso a exceder límites de tokens pasar todo el historial. El Orchestrator debe ser instruido (en sus propias `customInstructions`) para resumir la información crítica.

* **Prompt de Resumen para el Orchestrator (ejemplo para sus `customInstructions`):**
    ```
    "Antes de delegar una nueva tarea con `new_task`, revisa el historial de conversación actual de ESTA tarea. Identifica y resume concisamente los siguientes puntos clave que sean RELEVANTES para la subtarea a delegar:
    1.  Objetivo principal original de la tarea padre.
    2.  Decisiones clave ya tomadas.
    3.  Resultados o artefactos de subtareas previas que son prerrequisito o entrada para la nueva subtarea.
    4.  Requisitos o restricciones específicas mencionadas por el usuario que apliquen a la nueva subtarea.
    5.  Cualquier impedimento o problema no resuelto que la nueva subtarea deba considerar.
    Este resumen DEBE incluirse como parte del 'message' para la herramienta `new_task`."
    ```

### 1.4. Manejo de Límites de Tokens al Pasar Contexto:
Cuando el contexto esencial es demasiado grande incluso después del resumen, o si se refiere a documentos extensos:

* **Estrategia Principal: Referencias a Archivos + `read_file`:**
    * El Orchestrator, en el `message` de `new_task`, debe instruir explícitamente al agente especializado a leer archivos específicos usando la herramienta `read_file`.
    * **Ejemplo en `message`:**
        ```markdown
        **Contexto Adicional Requerido:**
        Para comprender completamente los requisitos de la API, por favor, lee los siguientes documentos usando la herramienta `read_file`:
        1.  `path`: `project_docs/api_specification_v2.md`
        2.  `path`: `project_docs/data_models.json`

        Analiza el contenido de estos archivos antes de proceder con la implementación.
        ```
* **Priorización del Contexto:** El Orchestrator debe priorizar qué información es absolutamente crítica para el `message` y qué puede ser referenciada.
* **Fragmentación de Tareas (si es necesario):** Si una subtarea aún requiere demasiado contexto que no puede ser manejado eficientemente, el Orchestrator podría necesitar descomponerla aún más.

---

## 2. Estrategias para la Formulación del `result` (al usar `attempt_completion`)

Cuando un agente especializado completa su tarea, usa `attempt_completion` para devolver el trabajo al Orchestrator (o a la tarea padre). La estructura del parámetro `result` es vital para que el Orchestrator pueda procesar la información eficientemente.

### 2.1. Principios Generales para Estructurar el `result`:
* **Claridad y Completitud:** El `result` debe ser claro sobre lo que se logró y si la tarea se considera completamente finalizada por el agente.
* **Accionabilidad para el Orchestrator:** El Orchestrator debe poder extraer la información necesaria para decidir los siguientes pasos o para consolidar resultados.
* **Consistencia:** Definir formatos consistentes para tipos de tareas similares ayuda al Orchestrator.

### 2.2. Ejemplos de Formatos de `result`:

La elección entre JSON, Markdown u otros formatos depende de la complejidad de la información y de cómo el Orchestrator está instruido para procesarla. **JSON estructurado es a menudo preferible para datos que el Orchestrator necesita analizar programáticamente.** Markdown es bueno para resúmenes legibles por humanos.

#### Ejemplo 1: Agente de Código (`@CoderAgent`)
* **Contexto:** Ha implementado la función `calculateTotalPrice`.
* **Estructura del `result` para `@CoderAgent` (JSON podría ser útil aquí):**
    ```json
    {
      "status": "éxito", // o "éxito_con_advertencias", "fallo_parcial"
      "summary": "Función 'calculateTotalPrice' implementada en src/utils/cart.js según los requisitos. Maneja descuentos individuales y un descuento general sobre $100. Documentada con JSDoc.",
      "artifact_path": "src/utils/cart.js",
      "code_block": "export const calculateTotalPrice = (items) => {\n  // ... código implementado ...\n};",
      "diff_output": "--- a/src/utils/cart.js\n+++ b/src/utils/cart.js\n@@ -5,3 +5,15 @@\n export const existingFunction = () => { ... };\n+// Inserta la nueva función aquí o donde corresponda\n+\n+/**\n+ * @param {Array<Object>} items - Array de items con price y discount.\n+ * @returns {number} - Precio total final.\n+ */\n+export const calculateTotalPrice = (items) => {\n+  // ... código implementado ...\n+};\n",
      "notes": "Se siguieron todos los estándares de codificación. Pruebas unitarias locales pasan (simulado para este ejemplo)."
    }
    ```
    * **Alternativa en Markdown (más simple, menos parseable):**
        ```markdown
        **Resultado de Tarea: Implementar Función `calculateTotalPrice`**

        **Estado:** Éxito.
        **Resumen:** Función `calculateTotalPrice` implementada en `src/utils/cart.js` según los requisitos. Maneja descuentos individuales y un descuento general sobre $100. Documentada con JSDoc.
        **Archivo Modificado:** `src/utils/cart.js`
        **Código Implementado:**
        ```javascript
        export const calculateTotalPrice = (items) => {
          // ... código implementado ...
        };
        ```
        **Notas:** Se siguieron todos los estándares de codificación.
        ```

#### Ejemplo 2: Agente de Investigación (`@ResearchAgent`)
* **Contexto:** Ha investigado sobre optimización React SSR.
* **Estructura del `result` para `@ResearchAgent` (Markdown es adecuado):**
    ```markdown
    **Resultado de Tarea: Investigación sobre Optimización de Rendimiento React SSR**

    **Estado:** Éxito.

    **Informe de Investigación:**

    ### Introducción
    Este informe resume las mejores prácticas actuales para optimizar el rendimiento en aplicaciones React con SSR...

    ### Prácticas de Optimización Clave
    1.  **Caching Estratégico (Nivel CDN, Servidor, Aplicación):** Reduce la carga del servidor...
    2.  **Code Splitting y Lazy Loading:** Envía solo el JavaScript necesario...
        * Técnica: `React.lazy()` con `Suspense`.
    3.  ... (más prácticas)

    ### Herramientas y Librerías
    * `next-bundle-analyzer`: Para visualizar tamaño de bundles.
    * ...

    ### Conclusión
    La optimización de React SSR es un proceso multifacético...

    ### Referencias Principales
    1.  [Título Artículo 1](URL_ARTICULO_1)
    2.  [Título Documentación Oficial](URL_DOCS_2)
    3.  ...
    ```

### 2.3. Inclusión de Estado y Metadatos Explícitos:
Sí, es altamente recomendable que el `result` incluya:

* **Un campo de `estado` explícito:**
    * Valores posibles: `"success"`, `"success_with_warnings"`, `"partial_failure"`, `"total_failure"`, `"requires_clarification"`.
    * Esto permite al Orchestrator tomar decisiones de flujo más fácilmente.
* **Metadatos relevantes:**
    * `timestamp_completion`: Cuándo se completó.
    * `agent_slug_completed_by`: Qué agente lo completó.
    * `task_id_completed`: El ID de la tarea que se está completando (si el sistema los usa).
    * `files_modified`: [`path1`, `path2`]
    * `files_created`: [`path3`]
    * `files_read_for_context`: [`path4`, `path5`] (útil para trazabilidad)

    **Ejemplo con Estado y Metadatos (JSON):**
    ```json
    {
      "metadata": {
        "task_id_completed": "subtask_001",
        "agent_slug_completed_by": "coder_agent_v2",
        "timestamp_completion": "2025-05-18T16:00:00Z",
        "status": "success_with_warnings"
      },
      "payload": {
        // ... contenido específico del resultado de la tarea, como en ejemplos anteriores ...
      },
      "warnings": [
        "No se pudo verificar el estándar de codificación XYZ-123 debido a la falta de un linter configurado."
      ],
      "errors": [] // Lista de errores si status es "partial_failure" o "total_failure"
    }
    ```

---

## 3. Casos de Uso de Múltiples Intercambios (`ask_followup_question` y Retroalimentación)

La comunicación no siempre es unidireccional. Los agentes especializados pueden necesitar clarificaciones.

### 3.1. Ejemplo de Diálogo (Orchestrator - Sub-Agente):

1.  **Orchestrator (`new_task` -> `@CoderAgent`):**
    * `message`: "Implementa una API endpoint `/users/{id}` que devuelva los datos del usuario. Usa el `UserModel` existente."

2.  **`@CoderAgent` (detecta ambigüedad):**
    * **Piensa:** "No sé qué campos específicos del `UserModel` debo devolver. ¿Todos? ¿Solo un subconjunto? ¿Debo manejar el caso de `id` no encontrado con un 404 o de otra manera?"
    * **Acción (`ask_followup_question` -> Orchestrator):**
        * `question`: "¿Qué campos específicos del `UserModel` deben incluirse en la respuesta del endpoint `/users/{id}`? Además, ¿cómo debo manejar el caso en que un `id` de usuario no se encuentre (ej. retornar 404, un objeto vacío, o un error específico)?"

3.  **Orchestrator (recibe la pregunta y procesa):**
    * **Piensa:** "Buena pregunta. Necesito especificar los campos y el manejo de errores."
    * (El Orchestrator podría tener esta información, consultarla de un documento de requisitos, o incluso preguntar al usuario humano si la tarea original vino de él).
    * **Respuesta al `@CoderAgent` (como parte del flujo de conversación de la subtarea):**
        * `assistant_message`: "Para el endpoint `/users/{id}`, por favor incluye los campos `id`, `username`, `email`, y `date_joined` del `UserModel`. Si el `id` del usuario no se encuentra, la API debe devolver un código de estado HTTP 404 con un cuerpo JSON: `{\"error\": \"User not found\"}`."

4.  **`@CoderAgent` (recibe la clarificación y procede):**
    * Implementa la funcionalidad según las especificaciones aclaradas.

5.  **`@CoderAgent` (`attempt_completion` -> Orchestrator):**
    * `result`: (JSON o Markdown estructurado como se describió antes, con el código, rutas, etc.)

### 3.2. Procesamiento de Retroalimentación del Usuario por el Orchestrator:

1.  **Orchestrator (presenta el `result` del `@CoderAgent` al usuario humano o a otra tarea padre).**
2.  **Usuario Humano (o Agente Padre) provee retroalimentación:** "El endpoint funciona, pero olvidaste añadir el campo `last_login` que también es necesario."
3.  **Orchestrator (procesa la retroalimentación):**
    * **Piensa:** "Necesito que `@CoderAgent` modifique la implementación para incluir `last_login`."
    * **Acción (`new_task` o continuando la conversación con `@CoderAgent` si la subtarea no se cerró completamente, aunque `new_task` para una modificación clara es más limpio):**
        * `mode`: (slug del `@CoderAgent`)
        * `message`:
            ```markdown
            **Tarea Asignada: Modificación de Endpoint `/users/{id}`**

            **Contexto:**
            Basado en la implementación previa de `/users/{id}` (tu tarea anterior `subtask_001`), se requiere una modificación.

            **Requisito Adicional:**
            Además de los campos `id`, `username`, `email`, y `date_joined`, por favor, añade el campo `last_login` a la respuesta del endpoint.
            Asegúrate de que el manejo de errores (404 para usuario no encontrado) se mantenga.

            **Archivo a Modificar:** `src/api/user_endpoint.js` (o la ruta correcta)

            **Resultado Esperado:**
            * Confirmación de la modificación.
            * Ruta del archivo modificado.
            * El bloque de código actualizado.
            ```
    * (El Orchestrator podría también pasar el código anterior del `@CoderAgent` como contexto en este nuevo `message` para facilitar la modificación).

---

## 4. Manejo de Contexto Compartido a Través de Archivos (Simulando "Memoria Compartida")

Aunque `new_task` aísla historiales, los agentes operan en el mismo espacio de trabajo y pueden usar archivos para comunicación indirecta o para pasar artefactos grandes.

### 4.1. Escenario: Agente de Análisis de Datos y Agente de Reportes

* **Agente A (`@DataAnalyzerAgent`):** Procesa un archivo CSV grande (`raw_data.csv`), realiza cálculos y genera un conjunto de datos procesados.
* **Agente B (`@ReportGeneratorAgent`):** Toma los datos procesados y genera un informe en PDF o Markdown.

### 4.2. Flujo Orquestado:

1.  **Orchestrator (`new_task` -> `@DataAnalyzerAgent`):**
    * `message`:
        ```markdown
        **Tarea: Procesar Datos de Ventas**
        1. Lee el archivo `data/raw_data.csv` usando `read_file`.
        2. Calcula las ventas totales por producto y por región.
        3. Guarda los resultados agregados en un nuevo archivo JSON llamado `processed_data/aggregated_sales.json` usando `write_to_file`. Asegúrate de que el formato sea un array de objetos, cada uno con `product_id`, `region`, `total_sales`.
        4. En tu `result` para `attempt_completion`, confirma la ruta del archivo generado.
        ```

2.  **`@DataAnalyzerAgent` ejecuta la tarea y usa `attempt_completion`:**
    * `result`:
        ```json
        {
          "metadata": { "status": "success", ... },
          "payload": {
            "summary": "Datos de ventas procesados y agregados. Resultados guardados.",
            "output_file_path": "processed_data/aggregated_sales.json"
          }
        }
        ```

3.  **Orchestrator (procesa el `result` de `@DataAnalyzerAgent`):**
    * Extrae `output_file_path`: `processed_data/aggregated_sales.json`.

4.  **Orchestrator (`new_task` -> `@ReportGeneratorAgent`):**
    * `message`:
        ```markdown
        **Tarea: Generar Informe de Ventas Agregadas**
        1. Lee los datos procesados del archivo `processed_data/aggregated_sales.json` usando `read_file`.
        2. Con base en estos datos, genera un informe en Markdown (`reports/sales_report_2025_Q1.md`) que incluya:
            * Un resumen general.
            * Una tabla de ventas por producto.
            * Una tabla de ventas por región.
            * (Opcional) Un gráfico simple en Mermaid si es posible.
        3. Usa `write_to_file` para guardar el informe.
        4. En tu `result` para `attempt_completion`, confirma la ruta del informe generado.
        ```

### 4.3. Convenciones y Formatos para Comunicación Indirecta:

* **Nomenclatura de Archivos:** El Orchestrator (o las `customInstructions` de los agentes) debe definir convenciones claras para los nombres de archivos intermedios (ej. `[tipo_dato]_[etapa_proceso]_[timestamp_o_id].json`).
* **Formatos de Datos:** Especificar formatos de datos estructurados (JSON, CSV con encabezados definidos, XML) para los archivos intermedios para asegurar la interoperabilidad. Esquemas JSON pueden ser útiles.
* **Ubicaciones de Archivos:** Usar directorios dedicados (ej. `/temp_artifacts`, `/processed_data`, `/reports_drafts`) para organizar estos archivos.
* **Limpieza (Opcional):** El Orchestrator podría tener una fase final para limpiar archivos intermedios si ya no son necesarios.

---

## 5. Gestión de Dependencias entre Tareas Asignadas a Diferentes Agentes

Esto se relaciona estrechamente con el punto anterior, pero se enfoca en el flujo lógico que el Orchestrator debe gestionar.

### 5.1. Escenario: Implementación Frontend depende de API Backend

* **Tarea A (Agente `@BackendDev`):** Desarrollar y desplegar (simulado) un endpoint de API `/api/products`.
* **Tarea B (Agente `@FrontendDev`):** Crear un componente React que consuma el endpoint `/api/products` y muestre los datos.

### 5.2. Flujo Orquestado por el Orchestrator:

1.  **Orchestrator (`new_task` -> `@BackendDev`):**
    * `message`: "Desarrolla el endpoint `/api/products`. Debe devolver una lista de productos con `id`, `name`, `price`. Documenta el formato exacto de la respuesta JSON en tu `result`."

2.  **`@BackendDev` completa y usa `attempt_completion`:**
    * `result`:
        ```json
        {
          "metadata": { "status": "success", ... },
          "payload": {
            "summary": "Endpoint /api/products desarrollado y desplegado (simulado).",
            "endpoint_url": "https://[sim_api_url]/api/products",
            "response_format_example": [
              { "id": 1, "name": "Product A", "price": 10.99 },
              { "id": 2, "name": "Product B", "price": 5.49 }
            ],
            "notes": "El endpoint soporta método GET."
          }
        }
        ```

3.  **Orchestrator (espera y procesa el `result` de `@BackendDev`):**
    * Identifica que la Tarea A está completa y es exitosa.
    * Extrae `endpoint_url` y `response_format_example` del `payload`.

4.  **Orchestrator (`new_task` -> `@FrontendDev`):**
    * `message`:
        ```markdown
        **Tarea: Implementar Componente React para Listar Productos**

        **Contexto de API (Resultado de Tarea Backend Previa):**
        * URL del Endpoint: `https://[sim_api_url]/api/products`
        * Método: GET
        * Formato de Respuesta Esperado (ejemplo):
            ```json
            [
              { "id": 1, "name": "Product A", "price": 10.99 },
              { "id": 2, "name": "Product B", "price": 5.49 }
            ]
            ```

        **Requisitos del Componente:**
        1.  Realizar una petición GET al endpoint proporcionado.
        2.  Mostrar los productos en una lista o tabla (mostrar `name` y `price`).
        3.  Manejar estados de carga y error.
        4.  Escribir el componente en `src/components/ProductList.jsx`.

        **Resultado Esperado:**
        * Confirmación de implementación.
        * Ruta del archivo del componente.
        * Bloque de código del componente.
        ```

### 5.3. Consideraciones para el Orchestrator:

* **Gestión de Estado de Tareas:** El Orchestrator debe mantener un seguimiento del estado de todas las tareas delegadas.
* **Manejo de Fallos en Dependencias:** Si la Tarea A falla, el Orchestrator no debe iniciar la Tarea B o debe manejar el fallo adecuadamente (ej. notificar, intentar re-ejecutar Tarea A, etc.).
* **Paralelización:** Si hay múltiples tareas independientes, el Orchestrator puede delegarlas en paralelo. Si hay dependencias, debe secuenciarlas.

---

Este prompt complementario, enfocado en ejemplos prácticos y estrategias detalladas, debería potenciar significativamente la capacidad de una IA para diseñar y razonar sobre Sistemas Multi-Agente efectivos y eficientes dentro del entorno Roo Code.