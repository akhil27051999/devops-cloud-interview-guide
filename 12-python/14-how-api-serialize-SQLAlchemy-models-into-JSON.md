## 14. How does your API serialize SQLAlchemy models into JSON?
### Question

How does your API convert ORM model objects into JSON responses?

### Short Explanation

Tests understanding of serialization and API response formatting.

### Answer

I manually serialize ORM objects into JSON dictionaries inside the route functions using list comprehensions and JSON formatting.

### Detailed Explanation
#### Example serialization
```py
student_list = [
    {
        "id": student.id,
        "name": student.name,
        "domain": student.domain,
        "gpa": student.gpa,
        "email": student.email
    } for student in students
]
```
- Why manual serialization?
  - No extra libraries required
  - Full control over output formatting
  - Avoids leaking sensitive fields

### Key Takeaway

Manual serialization ensures clean, predictable JSON output while maintaining security and control.
