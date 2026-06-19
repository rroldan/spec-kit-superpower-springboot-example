# Quickstart: Validate Task Create Endpoint

Prerequisites:
- Java 21
- Maven
- Docker (for Testcontainers in integration tests)

Steps:
1. Build tests and run integration tests:

```bash
mvn -DskipTests=false test
```

2. Run the application locally:

```bash
mvn spring-boot:run
```

3. Create a task with `curl` (replace host/port if different):

```bash
curl -i -X POST http://localhost:8080/api/v1/tasks \
  -H 'Content-Type: application/json' \
  -d '{"title":"Buy groceries","description":"Milk, bread, eggs"}'
```

Expected result:
- `201 Created` with `Location: /api/v1/tasks/{id}` and JSON body matching `Task` schema.

