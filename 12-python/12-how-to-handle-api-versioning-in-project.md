## 12. How do you handle API versioning in your project? Why is it important?
### Question

What approach would you use to add API versioning to this project?

### Short Explanation

Checks if you understand backward compatibility and API evolution.

### Answer

Flask Blueprints allow me to version the API cleanly by using prefixes such as /api/v1/students. This keeps new and old versions independent.

### Detailed Explanation

### Blueprint versioning structure
```sh
app.register_blueprint(student_bp, url_prefix='/api/v1/students')
```
#### Why it matters
- Old mobile/clients continue working
- New features can evolve without breaking existing users
- Improves release strategy for production APIs

### Real-world Insight

If the Student model changes (e.g., add phone number), I can release /api/v2/students without disrupting older clients.

### Key Takeaway

Versioning protects clients from breaking changes and allows APIs to evolve safely.
