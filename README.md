# 🧠 Chat2DB + Ollama: Local AI SQL Copilot

This project sets up a private, local AI SQL assistant using [Chat2DB](https://github.com/chat2db/chat2db) and [Ollama](https://ollama.com), with the model `fit2cloud/chat2db-sql:7b-q8_0`.

---

## 📦 Requirements

- Docker (recommended: [Colima](https://github.com/abiosoft/colima) for macOS)
- [Ollama](https://ollama.com) (installed locally)

---

## 🚀 Setup Instructions

### 1. Install Docker (Colima on macOS)

```bash
brew install colima
colima start
```

---

### 2. Install Chat2DB via Docker Compose

Create a `docker-compose.yml` file:

```yaml
version: '3'
services:
  chat2db:
    image: chat2db/chat2db:latest
    container_name: chat2db
    ports:
      - "10824:10824"
    volumes:
      - ~/.chat2db-docker:/root/.chat2db
```

Start the container:

```bash
docker-compose up -d
```

---

### 3. Install and Start Ollama

```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama serve &
```

---

### 4. Pull the Supported AI Model

```bash
ollama pull fit2cloud/chat2db-sql:7b-q8_0
```

---

### 5. Configure DB Connection in Chat2DB

1. Open: [http://localhost:10824/workspace](http://localhost:10824/workspace)
2. Click the **+** on the left to add a new MySQL connection
3. If your MySQL is inside a Docker container, use:
   - **Host**: `host.docker.internal`
4. Choose the **MySQL** driver
5. Test the connection to ensure it's working

---

### 6. Add AI Model for SQL Copilot

1. In Chat2DB, open **Custom AI** from the left sidebar
2. Select **Custom** as the AI model type
3. Leave **API Key** empty
4. Set **API Host** to:  
   ```
   http://host.docker.internal:11434/v1/chat/completions/
   ```
5. Set **Model Name** to:  
   ```
   fit2cloud/chat2db-sql:7b-q8_0
   ```

Now you can use natural language prompts in Chat2DB to generate SQL!

---

## 🧪 Example Prompt

> how many records in table customers

---

## 🖼️ Screenshot

![AI Copilot Prompt Example](./resources/imgs/SCR-20250622-ujor.png)

---

## 📝 Notes

- If your MySQL runs outside Docker, use `127.0.0.1` as the host.
- You can try other Ollama-compatible SQL models from their [model library](https://ollama.com/library).
- For advanced control, consider adding schema context in the prompt like:

  ```
  ### Database Schema:
  Table customers(id int, name varchar, age int)

  ### Question:
  Find all customers older than 30.

  ### SQL:
  ```

---

Happy hacking! 🧑‍💻🔥
