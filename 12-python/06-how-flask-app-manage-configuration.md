## 6. How does your Flask application manage configuration, and why is this important?
### Question

How are configurations handled in your Flask REST API (such as DB URL, debug mode, credentials) and why is this considered a best practice?

### Short Explanation

This question checks your understanding of environment-based configuration, 12-factor app principles, and avoiding hard-coding secrets.

### Answer

My Flask API uses a dedicated Config class inside app/config.py, and all sensitive values such as database credentials, debug flag, and host/port are loaded from environment variables using python-dotenv. This follows the 12-Factor App principle of separating config from code.

### Detailed Explanation
#### 🔹 Step 1 — Loading environment variables
Your code uses:
```py
load_dotenv()
```
- This pulls values from .env or system environment variables.

#### 🔹 Step 2 — Config class stores all application settings
```py
class Config:
    POSTGRES_USER = os.getenv('POSTGRES_USER')
    POSTGRES_PASSWORD = os.getenv('POSTGRES_PASSWORD')
    SQLALCHEMY_DATABASE_URI = ...
```

#### 🔹 Step 3 — App loads config on startup
```py
app.config.from_object('app.config.Config')
```

#### 🔹 Step 4 — Security and flexibility
- Secrets are not hardcoded.
- Config can change between dev/stage/prod without modifying code.
- Supports Docker, Kubernetes, CI/CD, and GitHub Actions workflows cleanly.

### Real-world Insight

When deploying the API to Docker, I passed DB credentials via docker-compose.yml without touching code, ensuring security and environment-specific flexibility.

### Key Takeaway

Configuration belongs in environment variables — not in the source code — making the application portable, secure, and environment-agnostic.
