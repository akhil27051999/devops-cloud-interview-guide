## 3. Why did you use SQLAlchemy ORM instead of writing raw SQL queries?
### Short Explanation

Evaluates understanding of ORM advantages.

### Answer

SQLAlchemy provides ORM-level abstractions, input validation, safer handling of queries, and avoids SQL injection while simplifying CRUD operations.

### Detailed Explanation
#### 🔧 Your Model
```py
class Student(db.Model):
    id = db.Column(db.Integer, primary_key=True)
```

- SQLAlchemy automatically:
  - Converts Python objects ↔ SQL rows
  - Performs type checks
  - Handles relationships
  - Generates SQL under the hood

#### ⚙️ Ease of Queries
```sql
Student.query.all()
Student.query.get_or_404(id)
```
- No SQL needed.

#### 🔐 Built-In Safety
- ORM prevents SQL injection because it uses parameterized queries.

#### 📦 Migration Friendliness
- Flask-Migrate uses SQLAlchemy models for automated schema migrations.

### 💡 Real-world Insight
- ORM drastically reduces code required for CRUD APIs and avoids hand-written SQL mistakes.

### Key Takeaway
SQLAlchemy provides safety, maintainability, and productivity for backend APIs.
