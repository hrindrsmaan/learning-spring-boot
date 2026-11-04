# Learning Spring Boot

A step-by-step path through Spring Boot, from the basics to advanced topics.
Each numbered folder is a **standalone, runnable Spring Boot app** that focuses on one concept.

## How to run a lesson

```bash
./gradlew :01-hello-spring-boot:bootRun     # run one lesson
./gradlew :02-rest-crud-jpa:test            # test one lesson
./gradlew build                             # build everything
```

## Lessons

| #  | Lesson                                         | Concept                                             | Status  |
|----|------------------------------------------------|-----------------------------------------------------|---------|
| 01 | [hello-spring-boot](01-hello-spring-boot)      | First app, `@SpringBootApplication`, `@RestController` | ✅ Done |
| 02 | [rest-crud-jpa](02-rest-crud-jpa)              | Layered CRUD API, Spring Data JPA, H2, Swagger UI   | ✅ Done |
| 03 | [validation](03-validation)                    | Bean Validation (`@Valid`, `@NotBlank`, ...)        | 🚧 In progress |
| 04 | exception-handling                             | `@RestControllerAdvice`, `ProblemDetail`            | ⏳ Todo |
| 05 | dto-and-mapping                                | DTOs vs entities, records, mapping                  | ⏳ Todo |
| 06 | configuration-and-profiles                     | `application.yml`, `@ConfigurationProperties`, profiles | ⏳ Todo |
| 07 | jpa-relationships                              | `@OneToMany`, `@ManyToOne`, fetch types             | ⏳ Todo |
| 08 | pagination-and-sorting                         | `Pageable`, custom queries, `@Query`                | ⏳ Todo |
| 09 | testing                                        | `@WebMvcTest`, `@DataJpaTest`, MockMvc              | ⏳ Todo |
| 10 | real-database                                  | PostgreSQL, Flyway migrations, Testcontainers       | ⏳ Todo |
| 11 | spring-security-basics                         | Security filter chain, users, roles                 | ⏳ Todo |
| 12 | jwt-authentication                             | Stateless auth with JWT                             | ⏳ Todo |
| 13 | actuator-and-observability                     | Actuator, metrics, health checks                    | ⏳ Todo |
| 14 | caching                                        | `@Cacheable`, cache providers                       | ⏳ Todo |
| 15 | async-and-scheduling                           | `@Async`, `@Scheduled`, virtual threads             | ⏳ Todo |
| 16 | rest-client                                    | Calling other APIs with `RestClient`                | ⏳ Todo |
| 17 | messaging                                      | Kafka / RabbitMQ                                    | ⏳ Todo |
| 18 | docker-and-deployment                          | Containerizing and deploying                        | ⏳ Todo |

## Adding a new lesson

1. Create a folder `NN-topic-name/` with `build.gradle`, `src/main/java/com/learn/<topic>/...`
2. Add `include 'NN-topic-name'` to `settings.gradle`
3. Put only that lesson's dependencies in its `build.gradle` (shared setup is in the root `build.gradle`)
4. Add a short `README.md` in the lesson folder and tick it off in the table above
# learning-spring-boot
