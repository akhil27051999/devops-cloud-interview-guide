## 9. How do you implement logging in your Flask REST API?
### Question

Explain how logging is implemented in your project and why centralized logging is important.

### Short Explanation

This validates your understanding of observability and API troubleshooting.

### Answer

I created a custom setup_logger function that configures both console and file logging. Each API request logs meaningful events such as health checks, errors, and CRUD operations.

### Detailed Explanation
#### 🔹 Step 1 — Logger configuration
Your logger.py includes:
```sh
logger = logging.getLogger(name)
logger.setLevel(level)
```

#### 🔹 Step 2 — Console handler
```py
console_handler = logging.StreamHandler()
```

#### 🔹 Step 3 — File handler
```py
file_handler = logging.FileHandler(log_file)
```

####🔹 Step 4 — Logging usage example
```py
app.logger.info("Student Management API started successfully.")
```

### Benefits
- Helps troubleshoot production issues
- Ensures traceability
- Aids debugging during CI/CD deployments
- Helps in security auditing

### Real-world Insight

When testing, missing payload errors and invalid JSON inputs were easily identified via log entries.

### Key Takeaway

Structured logging is essential for debugging and maintaining a production-grade API.
