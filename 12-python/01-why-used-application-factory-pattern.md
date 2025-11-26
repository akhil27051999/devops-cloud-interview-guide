## 1. Why did you use the Application Factory pattern (create_app()), and what problem does it solve?
### Short Explanation

This question checks if you understand scalable Flask architecture and why Flask apps should not be created globally.

### Answer

The Application Factory pattern allows dynamic creation of Flask instances with different configurations (dev, prod, test). It improves modularity, avoids circular imports, and is essential for unit testing.

### Detailed Explanation
#### 🧾 Step 1 — Avoiding a Global Flask App

- If you define:
```py
app = Flask(__name__)
```
- at the top level, it creates issues:
  - Cannot change configs dynamically
  - Cannot create multiple independent test apps
  - Causes circular imports with blueprints and extensions

- Your design instead uses:
```sh
def create_app(config_class=None):
    app = Flask(__name__)
```

#### 🔧 Step 2 — Dynamic Configuration
- You allow switching configs:
```sh
if config_class:
    app.config.from_object(config_class)
else:
    app.config.from_object('app.config.Config')
```
- This is how tests load:
```sh
app = create_app(config_class=TestConfig)
```

#### 🧠 Step 3 — Extension Initialization
- Instead of binding SQLAlchemy globally, you initialize it per app:
```sh
db.init_app(app)
migrate.init_app(app, db)
```
#### 🔌 Step 4 — Blueprint Registration After DB
- Blueprints depend on models → so you register them only after db is ready:
```sh
from .routes import student_bp
app.register_blueprint(student_bp, url_prefix='/students')
```
### 💡 Real-world Insight

- Without the factory pattern, your unit tests using SQLite in-memory DB would not work because the global app would still connect to PostgreSQL.

### Key Takeaway

Application Factory = essential for scalability, testing, and clean architecture in Flask.
