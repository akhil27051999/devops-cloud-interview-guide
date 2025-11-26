## 20. What are the common errors you encountered, and how did you troubleshoot them?
### Question

Mention real issues you faced while building this API and how you solved them.

### Short Explanation

Tests practical experience.

### Answer

Common issues included database connection errors, missing env variables, migration mismatches, and JSON payload validation errors. Logs greatly helped in diagnosing these issues.

### Detailed Explanation

#### Issue 1 — Flask couldn't connect to PostgreSQL
- Cause: Wrong POSTGRES_HOST
- Fix: Pass correct Docker container name + port in .env.

#### Issue 2 — Migration mismatch
- Cause: Modified model without creating a migration
- Fix:
```sh
flask db migrate
flask db upgrade
```

#### Issue 3 — JSON missing required fields
- Fix: Added validation logic.

#### Issue 4 — Tests failing because DB wasn’t created

- Fix: db.create_all() in test fixture.

### Key Takeaway

Troubleshooting depends on good logging, proper configuration, and understanding ORM behavior.
