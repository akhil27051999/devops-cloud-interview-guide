## 13. How did you implement unit testing in your REST API?
### Question

Explain your testing setup, test client usage, and why the in-memory SQLite DB is helpful.

### Short Explanation

Tests your understanding of automated testing, pytest, fixtures, and test isolation.

### Answer

I use pytest with a custom client fixture that creates the app with TestConfig, uses an in-memory SQLite DB, and tears down the tables after each test.

### Detailed Explanation
#### 🔹 Step 1 — Test app setup
- conftest.py:
```py
app = create_app(config_class=TestConfig)
```

#### 🔹 Step 2 — Create a temporary DB
```py
db.create_all()
```

#### 🔹 Step 3 — In-memory DB
```sql
SQLALCHEMY_DATABASE_URI = "sqlite:///:memory:"
```
#### Benefits:
- Fast
- No external DB dependency
- Perfect for CI pipelines

#### 🔹 Step 4 — Test client
- with app.test_client() as client:
    yield client

#### 🔹 Step 5 — Test CRUD actions

**Example:**
```py
res = client.post("/students", json=student_data)
assert res.status_code == 201
```

### Real-world Insight

When running tests in GitHub Actions, in-memory DB ensures high-speed, isolated test execution.

### Key Takeaway

Unit testing ensures API reliability and prevents regressions during future updates.
