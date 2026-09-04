# GameZone

A single-page gaming-themed website built with Spring Boot, used to demonstrate a Jenkins CI/CD pipeline.

## Run locally

```bash
mvn clean package
java -jar target/gamezone-1.0.0.jar
```

Then open: http://localhost:8081

## Run tests

```bash
mvn test
```

## API

`GET /api/status` → `{ "status": "online", "message": "GameZone server is running" }`

## CI/CD

See `Jenkinsfile` for the Declarative Pipeline: Checkout → Build → Test → Package → Deploy.
