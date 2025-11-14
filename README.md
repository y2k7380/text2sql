# Datrics Text2SQL: Open-Source, High-Accuracy Natural Language to SQL Conversion

Datrics Text-to-SQL engine designed to understand databases effortlessly, turning plain English into accurate SQL queries. Our solution emphasizes advanced Retrieval-Augmented Generation (RAG) techniques rather than simply providing frameworks for developers to fine-tune models themselves and can work out-of-the-box.

# Whitepaper
[ResearchGate](https://www.researchgate.net/publication/389944067_Datrics_Text2SQL_A_Framework_for_Natural_Language_to_SQL_Query_Generation)

## What makes us stand out?
1. Semantic Layer: 
We leverage your database documentation and examples, extracting meaningful concepts to enhance precision.

2. Smart Example Matching: 
While other solutions struggle with unseen tables, our advanced search and reranking capabilities intelligently generalize from similar examples, ensuring reliable query generation.

3. Instant Documentation: 
Connect your database and instantly generate detailed documentation—no manual effort required.

4. Flexible AI Integration: 
Easily integrate with existing LLMs for enhanced, customized performance.

# Dependencies

Text2SQL agent uses:
- **ChromaDB** for storing vector embeddings
- **OpenAI Embeddings** for semantic search
- **LiteLLM** for flexible LLM integration - supports OpenAI, Anthropic, Ollama, and 100+ other providers

You can use local models via Ollama or cloud-based LLMs from various providers.

# Prerequsites

python >= 3.11
docker for running local test database

# Getting started

### Create virtual environment

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install .
```

### Set-up LLM

1. open `descriptors/default/t2sql_descriptor.json` with any text editor
2. set litellm settings to [router_model_list](https://docs.litellm.ai/docs/routing#quick-start)

#### Using Ollama (Local LLMs)

To use Ollama with local models like Llama, Mistral, or Qwen:

1. Install and start Ollama: https://ollama.ai
2. Pull your desired model (e.g., `ollama pull llama3.1`)
3. Update `t2sql_descriptor.json`:

```json
{
  "router_model_list": [
    {
      "model_name": "ollama_llama",
      "litellm_params": {
        "model": "ollama/llama3.1",
        "api_base": "http://localhost:11434"
      }
    }
  ],
  "model_sql": "ollama_llama",
  "model_table_selection": "ollama_llama"
}
```

**Popular Ollama models for SQL generation:**
- `llama3.1:8b` - Fast and efficient
- `qwen2.5-coder:7b` - Good for code/SQL tasks
- `deepseek-coder:6.7b` - Optimized for coding

#### Using Cloud LLMs

**OpenAI:**
```json
{
  "router_model_list": [
    {
      "model_name": "gpt4",
      "litellm_params": {
        "model": "gpt-4-turbo",
        "api_key": "sk-..."
      }
    }
  ],
  "model_sql": "gpt4",
  "model_table_selection": "gpt4"
}
```

**Anthropic Claude:**
```json
{
  "router_model_list": [
    {
      "model_name": "claude",
      "litellm_params": {
        "model": "claude-3-5-sonnet-20241022",
        "api_key": "sk-ant-..."
      }
    }
  ],
  "model_sql": "claude",
  "model_table_selection": "claude"
}
```


# Run streamlit application

```bash
docker-compose up -d
streamlit run main.py
```

In the app: open "Documentation" tab and click on "Run schema indexing" - this will create the semantic layer of the database
You can start asking question right after it's finished.

# Connecting to your database

open descriptors/default/t2sql_descriptor.json with any text editor
set access to your database "db" object

## Supported Databases

- PostgreSQL
- MySQL
- MSSQL
- Redshift
- Snowflake

## Database Configuration Examples

### PostgreSQL
```json
"db": {
  "source": "postgres",
  "connection_config": {
    "schema": "public",
    "password": "postgres",
    "host": "localhost",
    "database": "dvdrental",
    "user": "postgres",
    "port": 5433
  }
}
```

### MySQL
```json
"db": {
  "source": "mysql",
  "connection_config": {
    "schema": null,
    "password": "mysql",
    "host": "localhost",
    "database": "testdb",
    "user": "mysql",
    "port": 3307
  }
}
```

## SSH Tunnel Support

In case if you need to use ssh tunnel, add ssh_tunnel to your descriptor:

```json
"ssh_tunnel": {
    "username": "",
    "private_key_path": "",
}
```

You can provide the documentation in the "documentation" tab or click on "Run schema indexing" - this will create the semantic layer of the database.

You can start using app just right after that.

# Contributors

- Tetyana Hladkykh: FireFlyTy ([@FireFlyTy](https://github.com/FireFlyTy))
- Kirill Kirikov: kkirikov ([@kkirikov](https://github.com/kkirikov))

### Need AI Agent? 
Contact sales@datrics.ai

### Want to support project? 
Contact kk@datrics.ai 
