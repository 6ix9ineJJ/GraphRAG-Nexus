GraphRAG Nexus
Conversational Q&A and RAG over Tabular Data using Knowledge Graphs

GraphRAG Nexus is an AI-powered system that enables users to interact with tabular datasets (CSV, Excel) using natural language. The project converts structured data into a Neo4j knowledge graph and combines graph-based reasoning with Retrieval-Augmented Generation (RAG) to answer complex questions without requiring users to write database queries.

The system uses LangChain graph agents and GPT-3.5 to translate user questions into Cypher queries, execute them on the graph database, and generate human-readable responses through a Gradio-based chatbot interface.

🎯 Project Goals

Make tabular data conversational and accessible

Enable non-technical users to query structured datasets

Combine knowledge graphs with LLM-based reasoning

Support multiple query strategies for different use cases

🏗️ System Overview

User Question (Natural Language)
↓
Strategy Selection (Simple / Improved / RAG)
↓
Graph Agent & Retrieval Logic
↓
Neo4j Knowledge Graph (Cypher Queries)
↓
LLM Response Generation (GPT-3.5)
↓
Natural Language Answer

✨ Features

Natural language Q&A over CSV and XLSX files

Automatic knowledge graph construction

Neo4j graph database with Cypher querying

Graph-based and vector-based RAG support

Multiple interaction strategies

Web-based chatbot interface built with Gradio

🧠 Interaction Modes
1️⃣ Simple Graph Agent

Direct natural language → Cypher translation

Fast and lightweight querying

Best suited for well-structured questions

2️⃣ Improved Graph Agent

Enhanced query understanding

Better error handling and reasoning

More reliable answer generation

3️⃣ RAG Mode

Uses vector embeddings for semantic similarity

Handles ambiguous or loosely phrased queries

Combines retrieval with graph traversal

🗂️ Knowledge Graph Backend

Database: Neo4j

Data Model: Nodes and relationships

Query Language: Cypher

Example Domain: Movie dataset (actors, movies, genres)

Example Cypher Query

MATCH (a:Actor)-[:ACTED_IN]->(m:Movie)
WHERE a.name = "Tom Hanks"
RETURN m.title

🧰 Tech Stack

Frontend: Gradio

Backend: Python, LangChain

Database: Neo4j

LLM: OpenAI GPT-3.5

Embeddings: OpenAI Embeddings

Query Language: Cypher

⚠️ Security & Design Notes

Use READ-ONLY Neo4j credentials

Avoid WRITE permissions to prevent data manipulation

LLM outputs are non-deterministic; graph construction may vary slightly between runs

Knowledge graphs can be built manually or via LLM-based graph transformers

📦 Requirements

Linux or Windows (tested on WSL)

Python 3.9+

OpenAI or Azure OpenAI API credentials

Neo4j v5.17.0 or higher

🔧 Installation
sudo apt update && sudo apt upgrade
python3 -m venv graphrag-env
git clone <repository-url>
cd GraphRAG-Nexus
source graphrag-env/bin/activate
pip install -r requirements.txt

▶️ Running the Project

Create a Neo4j database (Desktop or Remote)

Upgrade Neo4j to v5.17.0+

Install required plugins:

APOC

Graph Data Science Library

Update neo4j.conf to allow CSV imports and procedures

Prepare the knowledge graph using notebooks in:

explore/Movie_sample_csv_data

Test Cypher queries in:

explore/3_query_movieDB_with_cypher.ipynb

Run the application:

python src/app.py
