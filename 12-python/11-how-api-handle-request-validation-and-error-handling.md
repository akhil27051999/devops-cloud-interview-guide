## 11. How does your API handle request validation and error handling?
### Question

How do you validate incoming JSON data in your CRUD endpoints, and how do you return meaningful error responses?

### Short Explanation

This checks whether you understand input validation, defensive coding, and consistent error handling.

### Answer

I validate every incoming JSON payload by checking required fields and returning structured error messages with appropriate HTTP status codes.

### Detailed Explanation
#### 🔹 Step 1 — Required fields validation
```sh
In add_student():

if not data or not all(key in data for key in ('name', 'domain', 'gpa', 'email')):
    return jsonify({"error": "Missing data"}), 400
```

#### 🔹 Step 2 — Validation for update
```sh
if not data or not any(key in data for key in ('name', 'domain', 'gpa', 'email')):
    return jsonify({"error": "No valid fields provided"}), 400
```

#### 🔹 Step 3 — Automatic 404 handling
```sh
student = Student.query.get_or_404(student_id)
```
- Flask automatically returns:
```sh
{"message": "404 Not Found"}
```

### 🔹 Benefits
- Avoids inserting invalid or partial data
- Reduces API errors and undefined behavior
- Keeps API responses consistent and predictable

### Real-world Insight

When incorrect payloads were sent in testing, the validation prevented DB issues like null values and duplicate emails.

### Key Takeaway

Validate every request and return clear error messages with appropriate HTTP status codes.
