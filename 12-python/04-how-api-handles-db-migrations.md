## 4. Explain how your API handles database migrations using Flask-Migrate & Alembic.
### Short Explanation
Checks understanding of schema evolution.

### Answer

Flask-Migrate integrates Alembic with SQLAlchemy models to auto-generate migration scripts and update the database schema safely.

### Detailed Explanation
#### 🧾 Step 1 — Migrate Instance Initialized
- In app/__init__.py:
```py
migrate = Migrate()
migrate.init_app(app, db)
```

#### 🔧 Step 2 — Creating Migrations
- You would run:
```sh
flask db init
flask db migrate -m "create student table"
flask db upgrade
```
- This generates migration scripts:
```py
def upgrade():
    op.create_table('students', ...)
```
#### 🧠 Step 3 — Based on Models
- Migrations track changes in:
```py
class Student(db.Model):
```
- Modify model → run migrate → DB updates.

### 💡 Real-world Insight
Migrations prevent production outages due to manual schema changes.

### Key Takeaway
Flask-Migrate safely evolves your database schema without dropping data.
