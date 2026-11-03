# 02 - REST CRUD with JPA

**Concepts:** Controller → Service → Repository layers, constructor injection, `JpaRepository`,
`@Entity`, H2 in-memory database, `data.sql` seeding, `@Transactional`, `@ResponseStatus` exceptions

```bash
./gradlew :02-rest-crud-jpa:bootRun
curl http://localhost:8080/api/employees
```

- Swagger UI: http://localhost:8080/swagger-ui.html
- H2 console: http://localhost:8080/h2-console (JDBC URL `jdbc:h2:mem:company`, user `sa`)
