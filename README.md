# DeepPaladar
Una mirada de la cultura catalana a través de la comida. 


![image](https://github.com/user-attachments/assets/cac36753-45cd-4797-951e-ea120a0340e3)

## 1. Objeto del proyecto. 
El fin de este proyecto es crear un chatbot interactivo basado en Streamlit que, basandose en los libros de recetas y de historia de cocina catalana, sea capaz de especificarte la historia y contexto de un plato, darte la receta de ese plato, o hacerte sugerencias de alimentación para cada momentos del día. 


![image](https://github.com/user-attachments/assets/5cac8163-432f-4f71-8094-bd666cf85253)



## 2. Librerias. 

uuid: Para generar un identificador único para cada sesión de usuario.
streamlit: Para crear una aplicación web interactiva.
langchain_core.messages.HumanMessage: Utilizado para crear mensajes enviados por los usuarios al modelo.
langchain_core.prompts.PromptTemplate: Para estructurar las preguntas y respuestas del modelo.
langchain.chat_models.ChatOpenAI: Para usar un modelo de OpenAI como GPT para generar respuestas.
langchain.vectorstores.FAISS: Usado para gestionar una base de datos de vectores, que ayuda a buscar documentos relevantes basados en similitudes semánticas.
langgraph.graph.START, MessagesState, StateGraph: Se utilizan para definir un flujo de trabajo o "workflow" para gestionar el estado y las transiciones dentro de la aplicación.
langgraph.checkpoint.memory.MemorySaver: Para guardar el estado de la conversación y otros eventos.
langchain_openai.OpenAIEmbeddings: Para convertir texto en vectores numéricos que el modelo pueda procesar y comparar.
dotenv: Para cargar variables de entorno, como la clave API de OpenAI desde un archivo .env.

## 3. Background del proyecto.

## 4. Clases y funciones relevantes.


## 5. Streamlit. 


