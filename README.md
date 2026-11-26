# 🎬 CineGraph AI
### A Hybrid GraphRAG-Powered Movie Intelligence Assistant

CineGraph AI is an intelligent movie assistant built using **Hybrid GraphRAG (Graph Retrieval-Augmented Generation)** architecture. It combines **Neo4j Knowledge Graphs** and **Pinecone Vector Search** to answer both structured and semantic questions about movies through a conversational AI interface.

Powered by **Google Gemini** and **LangChain**, the application understands user queries, retrieves relevant information from multiple knowledge sources, and generates contextual responses through an interactive Streamlit interface.

---

## 🚀 Live Demo

**Try CineGraph AI here:**

🔗 [CineGraph AI – Movie Intelligence Assistant](https://movie-graphrag-assistant-sfmdprqrfqocvevxbqbnrg.streamlit.app/)

---

## 🚀 Key Features

- **Hybrid GraphRAG Architecture:** Combines graph-based retrieval and vector-based semantic search.
- **Knowledge Graph Retrieval:** Retrieves structured information about movies, directors, actors, genres, and release years using Neo4j.
- **Semantic Movie Search:** Searches movie plots, storylines, themes, and narrative details using Pinecone vector embeddings.
- **AI-Powered Query Understanding:** Uses Google Gemini to understand natural language queries and select the appropriate retrieval tools.
- **Intelligent Tool Selection:** Automatically determines whether a question requires graph retrieval, vector search, or both.
- **Conversational Interface:** Maintains conversation history for contextual multi-turn interactions.
- **Cypher Query Guardrails:** Validates generated Cypher queries to restrict database operations to read-only queries.
- **Self-Healing Query Execution:** Attempts to correct failed Cypher queries using an LLM-powered correction chain.
- **Interactive Streamlit UI:** Provides a user-friendly chat interface with example questions and database query inspection.

---

## 🏗️ System Architecture

CineGraph AI follows a Hybrid GraphRAG architecture with two complementary retrieval mechanisms.

### 1. Data Ingestion Pipeline

The movie dataset is loaded from a CSV file using LangChain's CSVLoader.

The ingestion pipeline:
1. Loads movie information and plot descriptions.
2. Extracts metadata such as title, release year, director, cast, and genre.
3. Creates movie relationships in Neo4j.
4. Splits movie plots into smaller chunks.
5. Generates vector embeddings using Google Gemini Embeddings.
6. Stores the embeddings in Pinecone for semantic retrieval.

### 2. Hybrid Retrieval System

The application uses two specialized tools:

**Graph Retrieval – Neo4j**
- Retrieves structured movie information.
- Finds directors and actors associated with movies.
- Searches movies by genre and release year.
- Explores relationships between movies, people, and genres.

**Vector Retrieval – Pinecone**
- Performs semantic similarity search.
- Retrieves relevant movie plot descriptions.
- Identifies storylines based on themes and narrative context.
- Finds movies based on descriptions of specific scenes or events.

### 3. LLM-Powered Response Generation

Google Gemini acts as the reasoning engine.

- Understands user questions.
- Selects the appropriate retrieval tool.
- Processes retrieved information.
- Generates clear, contextual responses.

### Architecture Flow

```text
             User Query
                  |
                  v
          Streamlit Interface
                  |
                  v
          LangChain + Gemini
                  |
                  v
          Intelligent Tool Selection
                  |
           +------+------+
           |             |
           v             v
       Graph Tool     Vector Tool
           |             |
           v             v
         Neo4j        Pinecone
      Knowledge       Vector
        Graph          Store
           |             |
           +------+------+
                  |
                  v
          Retrieved Results
                  |
                  v
          Gemini LLM
                  |
                  v
          Contextual Answer
                  |
                  v
          Streamlit Chat UI
```

---

## 🛠️ Tech Stack

| Category | Technologies |
|---|---|
| Programming Language | Python |
| Frontend | Streamlit |
| LLM | Google Gemini 2.5 Flash |
| Embeddings | Google Gemini Embedding 001 |
| AI Framework | LangChain |
| Knowledge Graph Database | Neo4j AuraDB |
| Vector Database | Pinecone |
| Data Processing | Pandas, CSVLoader |
| Text Splitting | RecursiveCharacterTextSplitter |
| Environment Management | Python-dotenv |

---

## 🗄️ Knowledge Graph Schema

The Neo4j database represents movie information through nodes and relationships.

### Nodes

- `Movie`: Title, release year, origin, Wikipedia page.
- `Person`: Director and actor names.
- `Genre`: Movie genre.

### Relationships

```text
(Person)-[:DIRECTED]->(Movie)

(Person)-[:ACTED_IN]->(Movie)

(Movie)-[:HAS_GENRE]->(Genre)
```

Unique constraints are created for movie titles, person names, and genre names to prevent duplicate nodes.

---

## 📂 Project Structure

```text
CineGraph-AI/
│
├── app.py
│
├── main.py
├── main_new.py
│
├── wiki_movie_plots_deduped.csv
│
├── requirements.txt
├── .env.example
├── .gitignore
│
└── README.md
```

---

## ⚙️ Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/CineGraph-AI.git

cd CineGraph-AI
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
```

Activate the environment:

**Windows:**
```bash
venv\Scripts\activate
```

**Linux / macOS:**
```bash
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the project root directory.

Add your credentials:

```env
# Neo4j AuraDB
NEO4J_URI=your_neo4j_uri
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=your_neo4j_password
NEO4J_DATABASE=neo4j

# Pinecone
PINECONE_API_KEY=your_pinecone_api_key
PINECONE_INDEX_NAME=your_pinecone_index_name

# Google Gemini
GOOGLE_API_KEY=your_google_api_key
```

**Important:** Configure your Pinecone index with an embedding dimension of `768` to match the embedding model configuration.

### 5. Configure Databases

Before running the application:

- Create a Neo4j AuraDB instance.
- Create a Pinecone index with the required embedding dimension.
- Add the corresponding credentials to your `.env` file.

### 6. Add the Dataset

Place `wiki_movie_plots_deduped.csv` in the project root directory.

The application uses the following dataset columns:

- Release Year
- Title
- Origin/Ethnicity
- Director
- Cast
- Genre
- Wiki Page
- Plot

### 7. Run the Application

```bash
streamlit run app.py
```

The application will open in your browser.

---

## 💡 Example Queries

Try asking CineGraph AI questions such as:

**Structured Queries**
- Who directed The Great Train Robbery and in what year?
- List romance movies from 1929 or 1930.
- Which movies belong to a particular genre?

**Semantic Queries**
- Find a movie plot where bandits rob a train on horseback.
- Search for movies involving revenge and betrayal.
- Find a movie based on a particular storyline.

**Combined Queries**
- Who directed Show Boat (1929), and what is its storyline?
- Find movies of a particular genre and describe their plots.

---

## 🔐 Security & Reliability

- Environment variables are used to manage API credentials.
- `.env` is excluded through `.gitignore`.
- Cypher query guardrails restrict generated database queries.
- Failed queries can trigger an LLM-based correction mechanism.
- Missing retrieval results are handled without intentionally inventing movie information.

---

## 📊 Dataset

The project uses `wiki_movie_plots_deduped.csv`, containing movie metadata and plot descriptions.

The current Streamlit ingestion implementation processes up to **1,000 movie records** for the initial database population.

---

## 🎯 Learning Outcomes

Through this project, I explored and implemented:

- Hybrid GraphRAG architecture.
- Knowledge graph construction using Neo4j.
- Vector embeddings and semantic similarity search.
- Integration of multiple retrieval tools using LangChain.
- LLM-based tool calling and query generation.
- Cypher query validation and error correction.
- Building conversational AI applications with Streamlit.
- Integrating multiple external AI services and databases.

---

## 🔮 Future Enhancements

- Expand the knowledge graph to include the complete movie dataset.
- Implement advanced movie recommendation capabilities.
- Add filtering based on actors, directors, genres, and release years.
- Improve retrieval accuracy through hybrid ranking and reranking.
- Introduce persistent conversational memory.
- Deploy the application to a cloud hosting platform.

---

## 👩‍💻 Author

**Mousumi Badyakar**

---

⭐ If you find this project interesting, consider giving the repository a star!
