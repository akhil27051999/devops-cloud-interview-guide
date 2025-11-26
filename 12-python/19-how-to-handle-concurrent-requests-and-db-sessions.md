## 19. How do you handle concurrent requests and database sessions?
### Question

How does your app maintain thread safety when multiple requests hit the server?

### Short Explanation

Tests backend fundamentals.

### Answer

Flask uses a thread-local context, and SQLAlchemy manages DB sessions per request. Each HTTP request gets an isolated database session that is committed or rolled back independently.

### Detailed Explanation
- SQLAlchemy handles session management
- Every request gets its own DB session
- No shared state between requests
- Prevents race conditions

### Key Takeaway

Database session isolation ensures consistent and safe CRUD operations.
