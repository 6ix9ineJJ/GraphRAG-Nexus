# GraphRAG-Nexus: Knowledge Graph Q&A and RAG Framework

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://python.org)
[![Neo4j](https://img.shields.io/badge/Neo4j-5.17%2B-green.svg)](https://neo4j.com)
[![LangChain](https://img.shields.io/badge/LangChain-Latest-orange.svg)](https://python.langchain.com)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A production-ready **Knowledge Graph + Retrieval-Augmented Generation (RAG)** framework that enables natural language interaction with tabular data through advanced graph database technology.

## 🚀 Overview

GraphRAG-Nexus transforms traditional tabular data (CSV, Excel) into intelligent knowledge graphs, allowing users to perform sophisticated Q&A and RAG operations using natural language. The system leverages **Neo4j graph databases**, **OpenAI GPT models**, and **Langchain agents** to create a conversational interface for complex data analysis.

## ✨ Key Features

- 🧠 **Knowledge Graph Construction**: Automated conversion of tabular data to graph structures
- 💬 **Natural Language Q&A**: Ask questions about your data in everyday language
- 🔍 **Advanced RAG**: Semantic search with vector embeddings
- 🎯 **Multiple Interaction Modes**: Simple Agent, Improved Agent, and RAG modes
- 🛡️ **Production Security**: Read-only database access with scoped permissions
- 📊 **Real-time Analytics**: Instant responses powered by graph queries

## 🏗️ Architecture

```
User Query → Strategy Selection → Graph Database → AI Processing → Natural Language Response
     ↓              ↓                    ↓              ↓              ↓
  Gradio UI    LangChain Agent      Neo4j Graph    OpenAI GPT     Answer Output
```

**Key NOTE:** Remember to NOT use a Neo4j with WRITE privileges. Use only READ and limit the scope. Otherwise your user can manupulate the data (e.g ask your chain to delete data). So, make sure that your database connection permissions are always scoped as narrowly as possible for your chain/agent’s needs (This warning applies to both designing the chatbot and constructing the knowledge graph using LLMs).

**Key NOTE:** Knowledge graphs, which form the backbone of the chatbot's data structure, can be built with input from domain experts or through advanced language models like the Langchain 'LLM Graph Transformer'. 

**Key NOTE:** Familiarity with database query languages such as Pandas for Python, SQL, and Cypher can enhance the user's ability to ask more better questions and have a richer interaction with the graph agent.

**Key NOTE:** Keep that in mind tha LLMs are non-diterministic. Therefore, if you use LLMs for constructing the knowledge graph, you might get slightly different results on each execution.



## Main underlying techniques used in this chatbot:
- Knowledge graph construction
- LLM chains and agents
- Cypher query

## Requirements:
- Operating System: Linux OS or Windows. (I am running the project on Linux WSL for windows)
- OpenAI or Azure OpenAI Credentials: Required for GPT functionality.

## Installation:
- Ensure you have Python installed along with required dependencies.
```
sudo apt update && sudo apt upgrade
python3 -m venv tabular-kg-env
git clone <the repository>
cd TabularData-KnowledgeGraph-Q&A-With-GPT
source ...Path to the environment/tabular-kg-env/bin/activate
pip install -r requirements.txt
```

## Execution:
1. Create and start a graphDB in Neo4j remotely or using the desktop app. (I used desktop app)
2. Upgrade your graph to be at least `Version 5.17.0`.
3. Install `APOC` and `Graph Data Science Library` plugins.
4. Modify the neo4j.conf:
  - Comment out `server.directories.import=import` ==> (`# server.directories.import=import`)
  - Uncomment `# dbms.security.auth_enabled=true` ==> (`dbms.security.auth_enabled=true`)
  - make sure this line is set as: `dbms.security.allow_csv_import_from_file_urls=true`
  - make sure this line is set as: `dbms.security.procedures.unrestricted=jwt.security.*,apoc.*,genai.*`
  - make sure this line is set as: `dbms.security.procedures.allowlist=apoc.*,gds.*,genai.*`
  - copy `neo4j-genai-plugin-5.17.0.jar` from `products` folder and paste it into `plugins`.

4. Load your data, prepare the knowledge graph and inject the data into the Graph database. These steps are performed in `explore/Movie_sample_csv_data`, `1_load_and_save_movide_data.ipynb` and `2_AzureOpenAI_GraphDB_RAG_data_preparation.ipynb`.
5. Test your Graph database using direct cypher queries. Check `explore/3_query_movieDB_with_cypher.ipynb`
6. Run the app:
```
python src/app.py
```
7. Start chatting!

## 🛠️ Technology Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Graph Database** | Neo4j 5.17+ | Knowledge graph storage and querying |
| **AI Models** | OpenAI GPT-3.5/4 | Natural language processing |
| **Framework** | LangChain | Agent orchestration and chains |
| **UI Framework** | Gradio | Web-based chat interface |
| **Query Language** | Cypher | Graph database queries |
| **Embeddings** | OpenAI Text Embeddings | Semantic similarity search |

## 🚀 Quick Start

### Prerequisites
- **Python 3.8+**
- **Neo4j 5.17+** (Desktop or Server)
- **OpenAI API Key** or **Azure OpenAI Credentials**

### Installation

```bash
# Clone the repository
git clone https://github.com/6ix9ineJJ/GraphRAG-Nexus.git
cd GraphRAG-Nexus

# Create virtual environment
python -m venv venv

# Activate environment
# Windows:
venv\Scripts\activate
# Linux/Mac:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### Database Setup

1. **Start Neo4j Database**
   ```bash
   # Using Neo4j Desktop or start server
   ```

2. **Install Required Plugins**
   - APOC (Awesome Procedures on Cypher)
   - Graph Data Science Library

3. **Configure Neo4j** (`neo4j.conf`)
   ```conf
   # Security settings
   dbms.security.auth_enabled=true
   dbms.security.allow_csv_import_from_file_urls=true
   dbms.security.procedures.unrestricted=jwt.security.*,apoc.*,genai.*
   dbms.security.procedures.allowlist=apoc.*,gds.*,genai.*
   ```

4. **Environment Variables**
   ```bash
   export OPENAI_API_KEY=your_api_key_here
   export OPENAI_API_BASE=https://api.openai.com/v1
   export OPENAI_API_VERSION=2023-12-01-preview
   ```

### Data Preparation

```bash
# Run the knowledge graph construction notebooks
# 1. Load and prepare data
jupyter explore/Movie_RAG_GraphDB/1_load_and_save_movie_data.ipynb

# 2. Build knowledge graph
jupyter explore/Movie_RAG_GraphDB/2_AzureOpenAI_GraphDB_RAG_data_preparation.ipynb

# 3. Test graph queries
jupyter explore/Movie_RAG_GraphDB/3_query_movieDB_with_cypher.ipynb
```

### Launch Application

```bash
python src/app.py
```

The application will be available at `http://localhost:7860`

## 💡 Usage Examples

### Sample Questions

**Q&A Mode:**
- "What was the cast of the movie Jumanji?"
- "Which movies did Tom Hanks act in?"
- "What are the most common genres for movies released in 1995?"

**RAG Mode:**
- "What movies are about love and adventure?"
- "Find movies similar to The Matrix"
- "Show me action movies from the 1990s"

## 📊 Project Structure

```
GraphRAG-Nexus/
├── src/
│   ├── app.py                    # Main application entry point
│   └── utils/
│       ├── chatbot.py            # RAG engine and chat logic
│       ├── improved_chain.py      # Enhanced graph agent
│       ├── load_config.py       # Configuration management
│       └── ui_settings.py       # UI preferences
├── explore/
│   ├── Movie_RAG_GraphDB/       # Movie pipeline notebooks
│   ├── empty_graphdb.ipynb      # Database initialization
│   ├── explore_movie_data.ipynb   # Data exploration
│   ├── test_connection_*.ipynb   # Connectivity tests
│   └── test_gpt_and_*.ipynb     # Model validation
├── data/
│   └── movie_csv/               # Source datasets
├── configs/
│   └── app_config.yml           # Application configuration
├── images/                     # UI assets and diagrams
├── requirements.txt              # Python dependencies
└── sample_questions.md         # Test queries
```

## 🔧 Configuration

The application uses `configs/app_config.yml` for settings:

```yaml
llm_config:
  model_name: "gpt-4o-mini"
  temperature: 0.0
  embedding_model_name: "text-embedding-3-small"
  system_message: "You are a helpful AI assistant..."

neo4j_config:
  uri: "bolt://localhost:7687"
  username: "neo4j"
  password: "your_password"
  database: "neo4j"
```

## 🎯 Interaction Modes

### 1. Simple Agent
- Direct Cypher query generation
- Fast, straightforward responses
- Best for simple factual queries

### 2. Improved Agent  
- Enhanced query understanding
- Better error handling
- Optimized for complex questions

### 3. RAG Mode
- Vector similarity search
- Semantic understanding
- Handles ambiguous queries better

## 📈 Performance & Security

- **🔒 Security**: Read-only database access by default
- **⚡ Speed**: Sub-second response times for most queries
- **🎯 Accuracy**: Enhanced by graph relationships
- **📊 Analytics**: Built-in query logging and feedback

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Additional Resources

### Neo4j Documentation
- **Fuzzy Search**: [apoc.text.fuzzyMatch](https://neo4j.com/labs/apoc/4.3/overview/apoc.text/apoc.text.fuzzyMatch/)
- **Soundex Search**: [Neo4j Soundex Guide](https://neo4j.com/developer/kb/how-to-perform-a-soundex-search/)
- **Vector Indexes**: [Neo4j Vector Documentation](https://neo4j.com/docs/cypher-manual/current/indexes/semantic-indexes/vector-indexes/)

### Sample Datasets
- **Movie Dataset**: [Movies CSV](https://raw.githubusercontent.com/tomasonjo/blog-datasets/main/movies/movies_small.csv)
- **Medical Reports**: [Medical Data](https://github.com/neo4j-partners/neo4j-generative-ai-azure/tree/main/ingestion/data)

### Framework Documentation
- **LangChain Agents**: [Official Documentation](https://python.langchain.com/v0.1/docs/use_cases/graph/quickstart/)
- **Gradio Interface**: [Gradio Docs](https://www.gradio.app/docs/interface)
- **OpenAI API**: [Developer Guide](https://platform.openai.com/docs/quickstart?context=python)
- **Neo4j Python**: [Neo4j Documentation](https://neo4j.com/docs/getting-started/)

## 🙏 Acknowledgments

- **Neo4j** for the powerful graph database
- **OpenAI** for advanced language models
- **LangChain** for agent orchestration
- **Gradio** for the intuitive web interface

## 📞 Support

For questions and support:
- 📧 Create an [Issue](https://github.com/6ix9ineJJ/GraphRAG-Nexus/issues)
- 📖 Check the [Wiki](https://github.com/6ix9ineJJ/GraphRAG-Nexus/wiki)
- 💬 Join the [Discussions](https://github.com/6ix9ineJJ/GraphRAG-Nexus/discussions)

---

**⭐ If this project helped you, please give it a star on GitHub!**
