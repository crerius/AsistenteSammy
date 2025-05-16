# 🇪🇸 README.md en Español

# 🤖 Asistente Virtual "Sammy" para el Hotel Paraíso Caribeño 🌴 (Notebook de Colab)

**Creado por: Grupo #3** 🧑‍💻
**Curso: Talento Tech de MinTIC en alianza con la CUN** 🎓🇨🇴

## 1. Introducción 🌟

Este proyecto consiste en el desarrollo de "Sammy", un asistente virtual inteligente diseñado para el Hotel Paraíso Caribeño, ubicado en la hermosa Santa Marta 🏖️. Sammy tiene como objetivo principal mejorar la experiencia del cliente al proporcionar información, gestionar consultas y ayudar con tareas como la verificación de usuarios, el registro y la creación de solicitudes de reserva 💬.

El asistente ha sido desarrollado en un entorno de Google Colab, utilizando Python como lenguaje de programación principal y aprovechando las capacidades avanzadas del modelo de lenguaje grande (LLM) Gemini de Google 🧠 para la comprensión del lenguaje natural y la generación de respuestas.

## 2. Objetivos del Proyecto 🎯

* Crear un asistente conversacional capaz de entender y responder a las solicitudes de los usuarios de manera natural ✔️.
* Integrar el asistente con una base de datos (MongoDB Atlas 💾) para gestionar información de usuarios, paquetes, servicios y reservas.
* Implementar funcionalidades clave como:
    * Bienvenida y recolección de datos del usuario 👋.
    * Verificación de usuarios existentes y registro de nuevos usuarios 📝.
    * Consulta de información sobre paquetes de alojamiento, habitaciones y servicios/tours adicionales 🏨🗺️.
    * Creación de solicitudes de reserva, incluyendo la selección de paquetes y servicios 📅.
    * Consulta del estado de reservas existentes 🔍.
    * Actualización del estado de las reservas 🔄.
* Proporcionar una interfaz de usuario interactiva dentro del notebook de Google Colab utilizando `ipywidgets` para facilitar la demostración y la interacción 💻.
* Demostrar el uso de la técnica de "function calling" (uso de herramientas 🛠️) con el LLM Gemini para realizar acciones específicas y obtener datos de fuentes externas (la base de datos).

## 3. Tecnologías Utilizadas ⚙️

* **Lenguaje de Programación:** Python 🐍
* **Entorno de Desarrollo:** Google Colaboratory (Colab) ☁️
* **Modelo de Lenguaje (IA):** API de Gemini (específicamente, el modelo `gemini-1.5-flash-latest` o similar) a través de la librería `google-generativeai` 🧠.
* **Base de Datos:** MongoDB Atlas (NoSQL, servicio en la nube), interactuando mediante la librería `pymongo` 📊.
* **Interfaz de Usuario en Colab:** `ipywidgets` para crear una experiencia de chat interactiva dentro del notebook 🗣️.
* **Manejo de Credenciales:** `google.colab.userdata` para acceder a API keys de forma segura 🔑.
* **Otras Librerías Python:**
    * `os`
    * `datetime`
    * `uuid`
    * `html`

## 4. Estructura del Notebook de Colab 🏗️

El proyecto en Google Colab está organizado en varias celdas, cada una con una responsabilidad específica:

1.  **Celda 1: Instalación de Librerías** 📥:
    * Instala las dependencias necesarias (`google-generativeai`, `pymongo`, `ipywidgets`).
2.  **Celda 2: Importaciones Esenciales y Configuración Inicial** ⚙️:
    * Importa módulos requeridos y define variables globales (URIs de conexión, nombres de colecciones, API keys). Inicializa objetos de base de datos y el modelo de IA. Define `DEBUG_MODE`.
3.  **Celda 3: Función Auxiliar de Serialización** 🔄:
    * Contiene `_serializar_documento_para_llm` para convertir datos de MongoDB (especialmente `ObjectId` y `datetime`) a un formato procesable por el LLM.
4.  **Celda 4: Definición de Funciones Herramienta para Sammy** 🛠️:
    * Define las funciones Python que Sammy (el LLM) puede "llamar" para interactuar con la base de datos o realizar cálculos (ej. `Bienvenida_cliente`, `verificar_o_registrar_usuario`, `crear_reserva`). Agrupadas en `herramientas_disponibles_colab`.
5.  **Celda 5: Función de Inicialización Principal (`inicializar_app_colab`)** ▶️:
    * Establece la conexión con MongoDB Atlas.
    * Configura el cliente de Gemini con la API Key.
    * Crea la instancia del `GenerativeModel` de Gemini, pasándole las herramientas y la `system_instruction`.
    * Inicia la sesión de chat (`chat_session_sammy`).
6.  **Celda 6: Función para Procesar Mensajes (`procesar_mensaje_colab`)** 💬:
    * Maneja cada turno de la conversación. Envía mensajes al `chat_session_sammy`, gestiona el bucle de "function calling" y extrae la respuesta final del LLM.
7.  **Celda 7: Interfaz Gráfica con `ipywidgets` y Lógica de Chat** 🖥️:
    * Define estilos HTML y crea widgets para la interfaz de chat. Implementa la lógica de la UI para mostrar la conversación y manejar interacciones. Llama a `inicializar_app_colab()` e inicia la interacción.

## 5. Funcionamiento del Asistente (Interacción con Gemini) 💡

El núcleo de la inteligencia de Sammy reside en el modelo Gemini y su capacidad de "function calling":

1.  **Prompt de Sistema (`system_instruction`)** 📜: Define la personalidad de Sammy, su rol, objetivos y cómo/cuándo usar las funciones herramienta.
2.  **Mensaje del Usuario** 🗣️: El mensaje se pasa al modelo Gemini.
3.  **Decisión del LLM** 🤔: El LLM decide si responder directamente o "llamar" a una función herramienta.
4.  **Ejecución de la Herramienta (Python)** ⚙️: Si se llama a una herramienta, el código Python correspondiente se ejecuta (ej. consulta a MongoDB).
5.  **Resultado de la Herramienta al LLM** 📤: El resultado de la función Python se envía de vuelta al LLM.
6.  **Respuesta Final del LLM** ✅: El LLM formula una respuesta en lenguaje natural para el usuario, utilizando la información de la herramienta si es necesario, pero sin mostrar el JSON crudo.

Este ciclo se repite para cada turno de la conversación 🔁.

## 6. Cómo Usar el Notebook de Colab 🚀

1.  **Configurar Secrets** 🔑: Antes de ejecutar, configura las siguientes variables en los "Secrets" de Google Colab (icono de llave 🗝️):
    * `MONGODB_ATLAS_URI`: Tu cadena de conexión a MongoDB Atlas.
    * `GEMINI_API_KEY`: Tu clave API para los modelos Gemini de Google AI Studio.
2.  **Ejecutar Celdas en Orden** 🔢: Es crucial ejecutar todas las celdas del notebook en orden (Celda 1 a Celda 7).
3.  **Interactuar con Sammy** 💬: Una vez que la Celda 7 se ejecute y muestre "¡Todo listo!...", podrás usar la interfaz de chat para hablar con Sammy.
4.  **Modo de Depuración (`DEBUG_MODE`)** 🐞: En la Celda 2, puedes cambiar `DEBUG_MODE = True` para ver logs detallados. Para una presentación limpia, mantenlo en `False`.
5.  **Finalizar** 🛑: Escribe "salir" en el chat para terminar la conversación.

## 7. Posibles Mejoras Futuras ✨

* Implementación de una lógica de disponibilidad de habitaciones más robusta 🛌.
* Cálculo de precios finales más preciso (considerando temporadas, ofertas, etc.) 💰.
* Gestión de sesiones de chat más avanzada para un entorno multiusuario 👥.
* Integración con un frontend web externo (React, Vue) y un backend Flask/FastAPI desplegado en la nube 🌐.
* Añadir más herramientas para Sammy (ej. modificar reservas, obtener información turística de Santa Marta) 🛠️➕.

---

Este proyecto demuestra una aplicación práctica de los modelos de lenguaje grandes con capacidad de uso de herramientas para crear asistentes virtuales funcionales y útiles. 🎉

---
---

# 🇬🇧 README.md in English

# 🤖 "Sammy" Virtual Assistant for Paraíso Caribeño Hotel 🌴 (Colab Notebook)

**Created by: Group #3** 🧑‍💻
**Course: Talento Tech by MinTIC in partnership with CUN** 🎓🇨🇴

## 1. Introduction 🌟

This project involves the development of "Sammy," an intelligent virtual assistant designed for the Paraíso Caribeño Hotel, located in the beautiful Santa Marta 🏖️. Sammy's main goal is to enhance the customer experience by providing information, managing inquiries, and assisting with tasks such as user verification, registration, and creating booking requests 💬.

The assistant has been developed in a Google Colab environment, using Python as the primary programming language and leveraging the advanced capabilities of Google's Gemini Large Language Model (LLM) 🧠 for natural language understanding and response generation.

## 2. Project Objectives 🎯

* Create a conversational assistant capable of understanding and responding to user requests naturally ✔️.
* Integrate the assistant with a database (MongoDB Atlas 💾) to manage user information, packages, services, and reservations.
* Implement key functionalities such as:
    * Welcome and user data collection 👋.
    * Verification of existing users and registration of new users 📝.
    * Querying information about accommodation packages, rooms, and additional services/tours 🏨🗺️.
    * Creating booking requests, including package and service selection 📅.
    * Querying the status of existing reservations 🔍.
    * Updating the status of reservations 🔄.
* Provide an interactive user interface within the Google Colab notebook using `ipywidgets` for easy demonstration and interaction 💻.
* Demonstrate the use of the "function calling" technique (tool usage 🛠️) with the Gemini LLM to perform specific actions and retrieve data from external sources (the database).

## 3. Technologies Used ⚙️

* **Programming Language:** Python 🐍
* **Development Environment:** Google Colaboratory (Colab) ☁️
* **Language Model (AI):** Gemini API (specifically, the `gemini-1.5-flash-latest` model or similar) via the `google-generativeai` library 🧠.
* **Database:** MongoDB Atlas (NoSQL, cloud service), interacting via the `pymongo` library 📊.
* **UI in Colab:** `ipywidgets` to create an interactive chat experience within the notebook 🗣️.
* **Credentials Management:** `google.colab.userdata` to securely access API keys 🔑.
* **Other Python Libraries:**
    * `os`
    * `datetime`
    * `uuid`
    * `html`

## 4. Colab Notebook Structure 🏗️

The project in Google Colab is organized into several cells, each with a specific responsibility:

1.  **Cell 1: Install Libraries** 📥:
    * Installs necessary dependencies (`google-generativeai`, `pymongo`, `ipywidgets`).
2.  **Cell 2: Essential Imports and Initial Configuration** ⚙️:
    * Imports required Python modules and defines global variables (connection URIs, collection names, API keys). Initializes database objects and the AI model. Defines `DEBUG_MODE`.
3.  **Cell 3: Auxiliary Serialization Function** 🔄:
    * Contains `_serializar_documento_para_llm` to convert MongoDB data (especially `ObjectId` and `datetime`) into a format processable by the LLM.
4.  **Cell 4: Definition of Tool Functions for Sammy** 🛠️:
    * Defines the Python functions that Sammy (the LLM) can "call" to interact with the database or perform calculations (e.g., `Bienvenida_cliente`, `verificar_o_registrar_usuario`, `crear_reserva`). Grouped in `herramientas_disponibles_colab`.
5.  **Cell 5: Main Initialization Function (`inicializar_app_colab`)** ▶️:
    * Establishes the connection with MongoDB Atlas.
    * Configures the Gemini client with the API Key.
    * Creates the Gemini `GenerativeModel` instance, passing it the tools and the `system_instruction`.
    * Starts the chat session (`chat_session_sammy`).
6.  **Cell 6: Message Processing Function (`procesar_mensaje_colab`)** 💬:
    * Handles each turn of the conversation. Sends messages to `chat_session_sammy`, manages the "function calling" loop, and extracts the LLM's final response.
7.  **Cell 7: GUI with `ipywidgets` and Chat Logic** 🖥️:
    * Defines HTML styles and creates widgets for the chat interface. Implements UI logic to display the conversation and handle user interactions. Calls `inicializar_app_colab()` and starts the interaction.

## 5. Assistant's Functionality (Interaction with Gemini) 💡

The core of Sammy's intelligence lies in the Gemini model and its "function calling" capability:

1.  **System Prompt (`system_instruction`)** 📜: Defines Sammy's personality, role, objectives, and how/when to use the tool functions.
2.  **User Message** 🗣️: The user's message is passed to the Gemini model.
3.  **LLM Decision** 🤔: The LLM decides whether to respond directly or "call" a tool function.
4.  **Tool Execution (Python)** ⚙️: If a tool is called, the corresponding Python code is executed (e.g., a MongoDB query).
5.  **Tool Result to LLM** 📤: The result of the Python function is sent back to the LLM.
6.  **LLM Final Response** ✅: The LLM formulates a natural language response for the user, using information from the tool if necessary, but without displaying raw JSON.

This cycle repeats for each turn in the conversation 🔁.

## 6. How to Use the Colab Notebook 🚀

1.  **Configure Secrets** 🔑: Before running, configure the following variables in Google Colab's "Secrets" (key icon 🗝️):
    * `MONGODB_ATLAS_URI`: Your MongoDB Atlas connection string.
    * `GEMINI_API_KEY`: Your API key for Google AI Studio's Gemini models.
2.  **Run Cells in Order** 🔢: It is crucial to run all notebook cells in order (Cell 1 to Cell 7).
3.  **Interact with Sammy** 💬: Once Cell 7 executes and displays "¡Todo listo!..." (All set!), you can use the chat interface to talk to Sammy.
4.  **Debug Mode (`DEBUG_MODE`)** 🐞: In Cell 2, you can change `DEBUG_MODE = True` to see detailed logs. For a clean presentation, keep it `False`.
5.  **End** 🛑: Type "salir" in the chat to end the conversation.

## 7. Possible Future Improvements ✨

* Implementation of more robust room availability logic 🛌.
* More accurate final price calculation (considering seasons, offers, etc.) 💰.
* More advanced chat session management for a multi-user environment 👥.
* Integration with an external web frontend (React, Vue) and a cloud-deployed Flask/FastAPI backend 🌐.
* Adding more tools for Sammy (e.g., modifying reservations, obtaining tourist information for Santa Marta) 🛠️➕.

---

This project demonstrates a practical application of large language models with tool-use capabilities to create functional and useful virtual assistants. 🎉
