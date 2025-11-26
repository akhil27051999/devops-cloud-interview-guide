## 10. How do database migrations work in your Flask app using Flask-Migrate?
### Question

Explain how Alembic/Flask-Migrate handles database migrations in your project.

### Short Explanation

This checks your knowledge of schema versioning and DB lifecycle management.

### Answer

The project uses Flask-Migrate (built on top of Alembic) to generate and apply database schema changes. It keeps DB schema in sync with the ORM models without manual SQL.

### Detailed Explanation
#### 🔹 Step 1 — Initialize migration tool
```sh
flask db init
```
#### 🔹 Step 2 — Auto-generate migration from model changes
```sh
flask db migrate -m "Created students table"
```
#### 🔹 Step 3 — Apply migration
```sh
flask db upgrade
```

#### 🔹 Step 4 — Why it matters
- Prevents schema drift
- Allows rollbacks when needed
- Works well with CI/CD pipelines
- Makes schema version-controlled

### Real-world Insight

When modifying the Student model to add new columns, I generated a migration instead of manually altering PostgreSQL tables.

### Key Takeaway

Flask-Migrate ensures your database schema evolves safely and consistently across development and production.
