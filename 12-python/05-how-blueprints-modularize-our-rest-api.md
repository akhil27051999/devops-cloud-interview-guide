## 5. How do Blueprints help modularize your REST API? Why did you use them?
### Short Explanation

Evaluates scalable routing and code organization.

### Answer

Blueprints allow splitting routes into modular components so the app stays clean, reusable, and scalable.

### Detailed Explanation
#### 🧾 Your Blueprint
```py
student_bp = Blueprint('student', __name__)
```
- All CRUD routes live here.

#### ⚙️ Registered in Factory
```py
app.register_blueprint(student_bp, url_prefix='/students')
```
- So:
  - /students
  - /students/<id>
  are grouped logically.

### 🧠 Benefits
- Avoids a single huge app.py
- Allows multiple modules like /admins, /courses, /payments
- Makes testing easier per blueprint

### 💡 Real-world Insight
- Large production APIs may have 20–30 blueprints.

### Key Takeaway
- Blueprints organize REST APIs into clean, scalable modules.
