## 7. How does SQLAlchemy ORM work in your project? Why did you choose ORM over raw SQL?
### Question

Explain how SQLAlchemy ORM is used in your application and the benefits of using ORM instead of direct SQL queries.

### Short Explanation

This checks your understanding of database abstraction, ORM modeling, and clean data handling.

### Answer

I use SQLAlchemy ORM to represent the Student table as a Python class. ORM helps interact with the database using Python objects instead of raw SQL queries, improving readability, maintainability, and reducing SQL injection risk.

### Detailed Explanation
#### 🔹 Step 1 — ORM Model Definition
```py
class Student(db.Model):
    __tablename__ = "students"
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), nullable=False)
```

#### 🔹 Step 2 — Python object becomes a database row
```py
new_student = Student(name="Amit", domain="ECE", gpa=8.7, email="a@gmail.com")
db.session.add(new_student)
db.session.commit()
```

#### 🔹 Step 3 — ORM Querying
```py
students = Student.query.all()
student = Student.query.get_or_404(id)
```

#### Benefits
- Cleaner code
- Prevents SQL injection
- Easier migrations
- Database-agnostic (PostgreSQL → MySQL → SQLite)
- Works seamlessly with Flask-Migrate / Alembic

### Real-world Insight

During unit testing, I switched the DB from PostgreSQL to SQLite in-memory with zero code changes — a major ORM advantage.

### Key Takeaway

ORM provides abstraction, safety, and portability, making your CRUD API cleaner and more maintainable.
