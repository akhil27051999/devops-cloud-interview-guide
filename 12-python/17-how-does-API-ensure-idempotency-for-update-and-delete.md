## 17. How does your API ensure idempotency for update and delete operations?
### Question

How does your API behave when updating/deleting a resource that may or may not exist?

### Short Explanation

Checks understanding of REST semantics and safe API behavior.

### Answer

get_or_404 ensures that update and delete operate only on valid resources. If the student doesn't exist, a 404 error is immediately returned.

### Detailed Explanation
**Example**
```py
student = Student.query.get_or_404(student_id)
```

- This prevents:
  - Updating non-existent objects
  - Deleting non-existent rows
  - Silent failures

### Key Takeaway

Idempotency ensures APIs behave safely and predictably even under repeated requests.
