# 🗄️ Natural Language SQL Agent with LangChain & IBM watsonx.ai

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)
![IBM watsonx.ai](https://img.shields.io/badge/IBM_watsonx.ai-052FAD?style=for-the-badge&logo=ibm&logoColor=white)
![IBM Granite](https://img.shields.io/badge/IBM_Granite-0F62FE?style=for-the-badge&logo=ibm&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-D71F00?style=for-the-badge&logo=sqlalchemy&logoColor=white)

A command-line SQL agent that turns plain-English questions into SQL queries, runs them against a MySQL database, and returns a readable answer. It is built with **LangChain**, **IBM Granite** (via watsonx.ai), and the **Chinook** sample database.

---

## 📌 Overview

Writing SQL by hand is a barrier for non-technical users. This project shows how an LLM-powered agent can bridge that gap. Given a question such as *"Which country has the most customers?"*, the agent:

1. Inspects the database schema
2. Writes a SQL query
3. Executes it and checks the result
4. Self-corrects if the query fails
5. Responds in natural language

The agent uses the **ReAct** (Reason + Act) pattern, and `verbose=True` prints each Thought → Action → Observation step so the reasoning is fully visible.

---

## 🧰 Tech Stack

| Component | Technology |
|---|---|
| LLM | `ibm/granite-4-h-small` on IBM watsonx.ai |
| Agent framework | LangChain (`create_sql_agent`, ZERO_SHOT_REACT_DESCRIPTION) |
| Database | MySQL with the Chinook sample dataset |
| DB connector | `mysql-connector-python` + SQLAlchemy |
| Interface | Python CLI (`argparse`) |

---

## ⚙️ How It Works(Architecture)

```
User prompt ──► LangChain SQL Agent ──► Granite LLM (watsonx.ai)
                       │                        │
                       │◄──── generates SQL ────┘
                       ▼
                 MySQL (Chinook)
                       │
                       ▼
              Natural-language answer
```

**Model settings:** `max_new_tokens=1024`, `temperature=0.2` (focused, deterministic output), `top_p=0.95`, `repetition_penalty=1.2`.

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- A running MySQL server with the [Chinook database](https://github.com/lerocha/chinook-database) loaded
- Access to IBM watsonx.ai (project ID and credentials)

### Installation

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
pip install ibm-watsonx-ai ibm-watson-machine-learning langchain langchain-community mysql-connector-python
```

### Configuration

Set your database details as environment variables instead of hard-coding them:

```bash
export MYSQL_USER="your_username"
export MYSQL_PASSWORD="your_password"
export MYSQL_HOST="your_host"
export MYSQL_PORT="3306"
```

Then read them in `sql_agent.py`:

```python
import os
mysql_username = os.getenv("MYSQL_USER")
mysql_password = os.getenv("MYSQL_PASSWORD")
mysql_host = os.getenv("MYSQL_HOST")
mysql_port = os.getenv("MYSQL_PORT", "3306")
```


### Usage

```bash
python sql_agent.py --prompt "How many customers are there in each country?"
```

**Example questions**

- `"List the top 5 best-selling artists."`
- `"Which genre has the most tracks?"`
- `"What is the total revenue per year?"`

---

## 📁 Project Structure

```
├── sql_agent.py      # Agent setup, DB connection, and CLI entry point
└── README.md
```

---

## 🔍 Key Learnings

- Connecting an LLM to a live SQL database through LangChain's agent toolkit
- Using the ReAct pattern for multi-step reasoning with tools
- Tuning generation parameters for reliable query generation
- Handling malformed LLM output with `handle_parsing_errors=True`
- Keeping secrets out of source code

---

## ⚠️ Limitations & Next Steps

- Generated SQL can occasionally be incorrect, so review results for critical use cases
- Use a **read-only** database user to prevent accidental data changes
- Planned improvements: a Streamlit/Gradio front end, query logging, and support for additional databases

---

## 📄 License

This project is licensed under the MIT License.

---

## 🙋 Author

**Samuel** · Data Scientist & AI Engineer
[LinkedIn](https://linkedin.com/in/samuel-olagbenro) · [GitHub](https://github.com/SamuelOlagbenro)
