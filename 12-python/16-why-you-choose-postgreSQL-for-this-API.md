## 16. Why did you choose PostgreSQL for this API, and how is it connected?
### Question

Why PostgreSQL, and how does your Flask app connect to it?

### Short Explanation

Checks DB design choices and SQLAlchemy connection understanding.

### Answer

PostgreSQL is reliable, ACID-compliant, supports JSON fields, and works well with SQLAlchemy. Connection happens via a dynamically-built URI using environment variables.

### Detailed Explanation
- Connection URI
```py
SQLALCHEMY_DATABASE_URI = f"postgresql://{POSTGRES_USER}:{POSTGRES_PASSWORD}@{POSTGRES_HOST}:{POSTGRES_PORT}/{POSTGRES_DB}"
```

### Advantages
- Strong consistency
- Better concurrency
- Supports migrations very well
- Works in containerized environments

### Key Takeaway

PostgreSQL is ideal for production CRUD APIs requiring consistency and scalability.
