## 8. How do Flask Blueprints improve the structure of your API?
### Question

Why did you use Flask Blueprints in your project, and how do they improve API structure?

### Short Explanation

This evaluates your ability to write modular, scalable Flask applications.

### Answer

I used Blueprints to separate API routes (student_bp) from the core app creation logic. It keeps routes modular, improves readability, and makes the application scalable as the number of modules grows.

### Detailed Explanation
#### 🔹 Step 1 — Defining the blueprint
```py
student_bp = Blueprint('student', __name__)
```

#### 🔹 Step 2 — Adding CRUD routes inside
```py
@student_bp.route('', methods=['POST'])
def add_student():
```

#### 🔹 Step 3 — Registering blueprint
```py
app.register_blueprint(student_bp, url_prefix='/students')
```

#### Advantages
- Clean separation of concerns
- Makes API versioning easier:
- Example: /api/v1/students
- Enables multi-module or microservice design
- Improves testability and maintainability

### Real-world Insight

While scaling the API to include courses, enrollment, and faculty, each feature can have its own blueprint without impacting the main app.

### Key Takeaway

Blueprints make your Flask API modular, clean, and production-ready.
