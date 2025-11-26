## 2. Explain how your API loads configuration through environment variables and why it's important.

### Short Explanation

This checks knowledge of the Twelve-Factor App methodology and secure config handling.

### Answer

Configuration values like DB credentials are loaded from environment variables through dotenv and the Config class, ensuring nothing is hard-coded.

### Detailed Explanation
#### 🧾 Step 1 — Loading Environment Variables

- In app/config.py:
```sh
load_dotenv()
```
- This loads .env or external env variables.

#### 🔐 Step 2 — Separating Config From Code
```sh
POSTGRES_USER = os.getenv('POSTGRES_USER', 'user')
POSTGRES_PASSWORD = os.getenv('POSTGRES_PASSWORD', 'password')
```
- This follows 12-factor principles:
  - Config is externalized
  - Code works the same in dev/staging/prod
  - No sensitive info committed to GitHub

#### ⚙️ Step 3 — Building DB URI Dynamically
- Your DB URL is constructed using environment variables:
```sh
SQLALCHEMY_DATABASE_URI = f"postgresql://{POSTGRES_USER}:{POSTGRES_PASSWORD}@..."
```
#### 🧪 Step 4 — Test Configuration Overrides It
- In testing:
```sh
SQLALCHEMY_DATABASE_URI = "sqlite:///:memory:"
```
- No need to modify production config.

### 💡 Real-world Insight

During deployments, teams simply update environment variables in Docker/ECS/Kubernetes rather than modifying code.

### Key Takeaway

Environment variables provide security, flexibility, and easy environment-based configuration.
