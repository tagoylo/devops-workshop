# Docker Compose
## Python Web App + Redis Cache

Python web app that stores and retrieves a counter from **Redis**.

## Step 1: Create Project Structure

```bash
mkdir compose-interaction && cd compose-interaction
touch app.py requirements.txt docker-compose.yml
```
---

## Step 2: Add Flask App (`app.py`)

```python
from flask import Flask
import redis

app = Flask(__name__)
r = redis.Redis(host='redis', port=6379)

@app.route('/')
def hello():
    count = r.incr('hits')
    return f"Hello! This page has been visited {count} times."

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## Step 3: Add Requirements (`requirements.txt`)

```
flask
redis
```

---

## Step 4: Create `docker-compose.yml`

```yaml
version: "3.8"
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: redis:alpine
```

---

## Step 5: Create `Dockerfile`

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY app.py .
CMD ["python", "app.py"]
```

---

## Step 6: Run Everything

```bash
docker-compose up --build
```

---

## Step 7: Test Interaction

- Open [http://localhost:5000](http://localhost:5000)
- Refresh the page → The counter increases because **Flask talks to Redis**.
