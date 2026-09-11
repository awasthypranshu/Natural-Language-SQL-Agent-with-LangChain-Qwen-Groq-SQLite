# 🤖 SQL Agent — Natural Language to SQL

A **read-only AI SQL Agent** built with **LangChain**, **Groq**, **Qwen**, **SQLite**, and **SQLAlchemy**.

The agent allows users to ask questions about a SQL database in **natural language**. LangChain's SQL Agent analyzes the question, understands the database schema, generates the required SQL query, executes it, and returns the result.

For example:

> **"Which student has the highest marks?"**

The agent can determine the appropriate SQL query and retrieve the answer directly from the database.

---

## 🚀 Features

* 🧠 **Natural Language SQL Queries**
* 🤖 **LangChain SQL Agent**
* ⚡ **Qwen LLM through Groq**
* 🗄️ **SQLite Database**
* 🔒 **Read-Only Database Access**
* 🔍 Automatic database schema understanding
* 🧩 SQLAlchemy database integration
* 💬 Ask questions without writing SQL manually

---

## 🏗️ Tech Stack

| Technology     | Purpose                     |
| -------------- | --------------------------- |
| **Python**     | Core programming language   |
| **LangChain**  | Agent framework             |
| **Qwen**       | Large Language Model        |
| **Groq**       | LLM inference               |
| **SQLite**     | Database                    |
| **SQLAlchemy** | Database connection         |
| **Streamlit**  | Application interface setup |

---

## 🔄 How It Works

```text
User Question
      ↓
   Qwen LLM
      ↓
LangChain SQL Agent
      ↓
Database Schema Inspection
      ↓
Generate SQL Query
      ↓
Execute Query on SQLite
      ↓
Return Answer
```

The agent uses LangChain's `create_sql_agent()` and `SQLDatabaseToolkit` to interact with the database.

---

## 🧠 Example

Instead of writing:

```sql
SELECT name
FROM students
ORDER BY marks DESC
LIMIT 1;
```

You can simply ask:

```text
Which student has the highest marks?
```

The SQL Agent handles the SQL generation and database interaction automatically.

---

## 🔐 Read-Only Database

The project connects to the SQLite database using a **read-only connection**:

```python
creator = lambda: sqlite3.connect(
    f"file:{file_path}?mode=ro",
    uri=True
)
```

This prevents the agent from modifying the database.

The agent is therefore intended primarily for:

* Reading data
* Filtering data
* Sorting data
* Aggregating data
* Answering analytical questions

---

## 📂 Project Structure

```text
SQL-Agent/
│
├── SQLAgent.ipynb
├── student.db
└── README.md
```

---

## ⚙️ Installation

Clone the repository:

```bash
git clone https://github.com/your-username/SQL-Agent.git
cd SQL-Agent
```

Install the required packages:

```bash
pip install langchain
pip install langchain-community
pip install langchain-groq
pip install sqlalchemy
pip install streamlit
```

---

## 🔑 Environment Setup

The project uses **Groq** to access the Qwen model.

Create a Groq API key and configure it as an environment variable.

### Windows

```bash
set GROQ_API_KEY=your_api_key
```

### Linux / macOS

```bash
export GROQ_API_KEY=your_api_key
```

You can also use a `.env` file when configuring the project.

---

## 🗄️ Database

The project uses:

```text
student.db
```

The database should be placed in the project directory.

The current implementation connects to it as a **read-only SQLite database**.

---

## 🧪 Implementation

### Initialize the LLM

```python
from langchain_groq import ChatGroq

llm = ChatGroq(
    model="qwen/qwen3.6-27b",
    max_tokens=800
)
```

### Connect to SQLite

```python
def configure_sql():
    file_path = Path("student.db").absolute()

    creator = lambda: sqlite3.connect(
        f"file:{file_path}?mode=ro",
        uri=True
    )

    engine = create_engine(
        "sqlite:///",
        creator=creator
    )

    return SQLDatabase(engine)
```

### Create the SQL Toolkit

```python
toolkit = SQLDatabaseToolkit(
    llm=llm,
    db=configure_sql()
)
```

### Create the SQL Agent

```python
agent = create_sql_agent(
    llm=llm,
    toolkit=toolkit
)
```

### Ask a Question

```python
response = agent.invoke(
    "Which student has the highest marks?"
)

print(response)
```

---

## 💡 Example Questions

Once connected to the database, questions can be asked in natural language, such as:

```text
Which student has the highest marks?
```

```text
How many students are in the database?
```

```text
Show the students who scored more than 80 marks.
```

```text
What is the average marks of all students?
```

The exact questions depend on the tables and columns available in `student.db`.

---

## 🧩 Key LangChain Components

### `SQLDatabase`

Provides LangChain with an interface to the SQL database.

### `SQLDatabaseToolkit`

Provides database-related tools that the agent can use.

### `create_sql_agent`

Creates an agent capable of reasoning about the user's question and interacting with the SQL database.

### `ChatGroq`

Connects LangChain to the Qwen model hosted through Groq.

---

## 📌 What I Learned

Through this project, I explored:

* Building AI agents with LangChain
* Connecting LLMs to SQL databases
* Natural language → SQL generation
* SQL database schema inspection
* LangChain tool calling
* SQLAlchemy database connectivity
* SQLite read-only connections
* Using Groq for LLM inference
* Agent-based database querying

---

## 🔮 Future Improvements

Some possible improvements include:

* 💬 Build a complete Streamlit chat interface
* 🧠 Add conversation memory
* 📊 Generate charts from query results
* 🔐 Add stronger SQL query validation
* 🗂️ Support multiple databases
* ⚡ Add query caching
* 📄 Add RAG for database documentation and business knowledge
* 🛡️ Improve protection against unsafe or unintended queries

---

## 👨‍💻 Author

**Pranshu Awasthy**

Computer Science Engineering Student

Interested in:

**Generative AI • Agentic AI • Machine Learning • Data Analytics • DSA**

---

⭐ If you found this project useful, consider giving the repository a star!
