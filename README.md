# DeepPaladar
Una mirada de la cultura catalana a través de la comida. 


![image](https://github.com/user-attachments/assets/cac36753-45cd-4797-951e-ea120a0340e3)

## 1. Objeto del proyecto. 
El fin de este proyecto es crear un chatbot interactivo basado en Streamlit que, basandose en los libros de recetas y de historia de cocina catalana, sea capaz de especificarte la historia y contexto de un plato, darte la receta de ese plato, o hacerte sugerencias de alimentación para cada momentos del día. 


![image](https://github.com/user-attachments/assets/5cac8163-432f-4f71-8094-bd666cf85253)


## 2. Librerías

| Librería | Descripción |
|----------|------------|
| **uuid** | Genera identificadores únicos para cada sesión de usuario. |
| **streamlit** | Crea una aplicación web interactiva. |
| **langchain_core.messages.HumanMessage** | Gestiona los mensajes enviados por los usuarios. |
| **langchain_core.prompts.PromptTemplate** | Estructura preguntas y respuestas del modelo. |
| **langchain.chat_models.ChatOpenAI** | Usa un modelo de OpenAI (GPT) para generar respuestas. |
| **langchain.vectorstores.FAISS** | Gestiona bases de datos de vectores para búsquedas semánticas. |
| **langgraph.graph.START, MessagesState, StateGraph** | Define flujos de trabajo y transiciones en la aplicación. |
| **langgraph.checkpoint.memory.MemorySaver** | Guarda el estado de la conversación. |
| **langchain_openai.OpenAIEmbeddings** | Convierte texto en vectores numéricos. |
| **dotenv** | Carga variables de entorno desde un archivo `.env`. |


## 3. Background del proyecto.



## 4. Clases y funciones relevantes.
### 4.1. Clase Chatbot.
Esta clase se encarga de gestionar toda la  lógica del chatbot dede la interacción con el modelo del lenguaje asií como con el RAC. 

- **Inicialización( __init__):** con self.thread_id se genera un identificador único para cada sesión. 
- **Inicialización del modelo OpenAI:** Inicializamos el modelo ChatOpenIA, la matriz semántica del mismmo (OpenAIEmbbedings) y la base de datos FAISS donde hemos almacenado los documentos de búsqueda semántica. Estos documentos actúan como los criterios de respuesta sobre los que actuará el modelo. 
- **Promptemplate:**  define la estructura de las preguntas y respuestas. Se incluyen variables como el historial de la conversación, el contexto y la nueva pregunta.
- **StateGraph:** Grafo de flujo para gestionar la palicación.
### 4.2. Función _build_graph: 
Con ella configuramos el flujo de trabajo. Específicamente, añadiendo una conexión entre el estado inicial (START) y el modelo de lenguaje para generar respuestas.
### 4.3. Función call_model: 
Este método recoge el último mensaje de la conversación como pregunta, utilizará la búsqueda vectorial con "retriver.get_relevant_documents()" para encontrar los documentos relevantes dentre del db_vectorial. Tambíen se encarga de formatear la pregunta junto con el historial y el contexto, para crear el prompt que se la pasará al modelo. Tras esto, recogemos la respuesta del modelo. 
### 4.4. Función send_message: 
Prepara y envía el mensaje del usuario através del flujo de trabajo. 
### 4.5 Función Chain: 
Esta función es un "wrapper" para interactuar con el chatbot. Toma la entrada del usuario (prompt), envía el mensaje al chatbot, y devuelve la respuesta y los documentos relevantes.
- --
## 5. Streamlit. 


