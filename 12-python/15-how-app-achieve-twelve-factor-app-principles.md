## 15. How does the application achieve the Twelve-Factor App principles?
### Question

Explain how your API aligns with the 12-factor methodology.

### Short Explanation

Tests your understanding of cloud-native and production-ready app practices.

### Answer

My application follows several 12-factor principles including config via environment variables, dependency isolation, strict versioning, logs as event streams, and portability across environments.

### Detailed Explanation
**✔ Config via env vars**
- Config class loads values using load_dotenv().

**✔ Dependencies isolated**
- requirements.txt explicitly lists all dependencies.

**✔ Logs as event streams**
- Custom logger with both console and file handlers.

**✔ Dev/prod parity**
- Uses SQLite for tests and PostgreSQL for production.

**✔ Stateless & portable**
- No local file dependencies inside the code.

### Key Takeaway

Your Flask project is designed with cloud-native, production-grade practices.
