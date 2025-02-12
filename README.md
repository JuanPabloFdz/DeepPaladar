# DeepPaladar
Una mirada de la cultura catalana a través de la comida. 


![image](https://github.com/user-attachments/assets/5cf300ce-249f-4f27-a9a8-daa42d681511)


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
Si se observa el código se verá que la llamada de los transformes se hace de forma directa, esto es, sin hacer la previa transformación enla matriz vectorial semantica necesaria para el funcionamiento del RAC. Sin embargo, esta aun no ha sido construida tambien en el desarrollo pero no ha sido incluida por motivos de limpieda y ampliación de las fuentes. DISPONIBLE EN PRÓXIMAS ACTUALIZACIONES. 

Sin embargo, es importante imformar de algunos pasos necesarios de cara a la posible replicación de este proyecto. El primero de estos pasos estriva en hacer una seleción y carga de documentos que contiene la información que queremos usar como referencia (Veáse información RAC). Dicha carga la hacemos por medio de PyPDFLoader, tras lo cual haceos la división del texto en diferentes chunks. Un avez reaslizada esta fase pasamos a hacer la matriz semántica con dichos chunks usando el OpeAIEmbbedings() y la función FAISS. Por último se crea un vector store el cual es el que se presenta en este repositorio bajo el nombre de "faiss_index". 

A continuación un ejemplo de como hacer este proceso: 
```python
# Define paths for your PDF books
pdf_book1 = r"C:\Users\jpfb\OneDrive\1_WBS\9. Generative AI\Cocina catalana -- Cocina Regional (Susaeta), Madrid, 1990_ -- Susaeta Ediciones -- 9788430590742 -- 8441600ef692117053233f0871104c97 -- Anna’s Archive (1).pdf"
pdf_book2 = r"C:\Users\jpfb\OneDrive\1_WBS\9. Generative AI\Un viaje por la cocina catalana -- Equipo Susaeta -- Madrid, Spain, 2013 -- Tikal -- 9788499282466 -- 0bab4334fdd518bb5c8c69f484480616 -- Anna’s Archive.pdf"

# Initialize loaders for each PDF
loader_book1 = PyPDFLoader(pdf_book1)
loader_book2 = PyPDFLoader(pdf_book2)

# Load and optionally split each PDF into documents (e.g., one document per page)
documents_book1 = loader_book1.load_and_split(RecursiveCharacterTextSplitter(chunk_size=5000, chunk_overlap=150))
documents_book2 = loader_book2.load_and_split(RecursiveCharacterTextSplitter(chunk_size=5000, chunk_overlap=150))
all_documents = documents_book1 + documents_book2
from langchain.vectorstores import FAISS  # Example vector store for retrieval tasks

# Initialize OpenAIEmbeddings
embeddings = OpenAIEmbeddings()
# Build a vector store using these embeddings
vector_store = FAISS.from_documents(all_documents, embeddings)
vector_store.save_local("./content/faiss_index")




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
## 5. Fuentes del RAC.
- Equipo Susaeta. (2013). Un viaje por la cocina catalana. Madrid, España: Tikal.
- Susaeta Ediciones. (1990). Cocina catalana. Madrid, España: Susaeta Ediciones.

## 6. 


