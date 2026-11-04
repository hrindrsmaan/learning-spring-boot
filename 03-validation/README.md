# 03 - Validation (starter)

**Builds on:** [02-rest-crud-jpa](../02-rest-crud-jpa). It's the same Employee CRUD API, with three new
fields: `email`, `age`, `department`.

**Your job:** add input validation so bad requests are rejected with `400 Bad Request` instead of being saved.
This module has **no validation yet**. You add all of it.

## Run it

```bash
./gradlew :03-validation:bootRun
# or: cd 03-validation && ../gradlew bootRun
```

> Both 02 and 03 use port 8080. Stop lesson 02 first, or run this one on another port:
> `SERVER_PORT=8083 ./gradlew :03-validation:bootRun`

- Swagger UI: http://localhost:8080/swagger-ui.html
- H2 console: http://localhost:8080/h2-console (JDBC URL `jdbc:h2:mem:company`, user `sa`)

## Step 0 - See the problem

Send obviously bad data:

```bash
curl -i -H 'Content-Type: application/json' \
  -d '{"name":"","email":"nope","age":15,"salary":500,"department":"MARKETING"}' \
  http://localhost:8080/api/employees
```

Right now this returns **`201 Created`**, so the garbage is saved. When you're done, the same request
should return **`400 Bad Request`**.

## Tasks

Work through these in order. Rerun the app and the curl above after each step.

### 1. Add the dependency
Validation isn't included in the web starter. Add the validation starter to `build.gradle`.
<details><summary>Hint</summary>

`implementation 'org.springframework.boot:spring-boot-starter-validation'`
</details>

### 2. Add rules to `Employee`
Use annotations from `jakarta.validation.constraints`. Suggested rules:

| Field        | Rule |
|--------------|------|
| `name`       | required, not just spaces, 2-50 characters |
| `email`      | required, valid email format |
| `age`        | required, between 18 and 65 |
| `salary`     | required, at least 10000 |
| `department` | (do this one in task 5) |

Give each rule a friendly `message = "..."`.
<details><summary>Hint</summary>

Look at `@NotBlank`, `@Size`, `@Email`, `@NotNull`, `@Min`, `@Max`, `@DecimalMin`.
</details>

### 3. Turn validation on in the controller
Run the bad curl again. It's **still `201`**. Annotations alone do nothing; something has to *trigger* them.
Fix `create()` and `update()` in `EmployeeController`.
<details><summary>Hint</summary>

Put `@Valid` (from `jakarta.validation`) in front of `@RequestBody`.
</details>

**Tip:** the default 400 response hides your messages. To see them, add this to `application.properties`:
```properties
server.error.include-message=always
server.error.include-binding-errors=always
```
(Lesson 04 replaces this with a clean custom error format.)

### 4. Validate the path variable
`GET /api/employees/0` and `/api/employees/-5` make no sense. Reject IDs that aren't positive with `400`.
<details><summary>Hint</summary>

Annotations work on method parameters too: `@PathVariable @Positive Long id`.
</details>

### 5. Write a custom constraint: `@ValidDepartment`
Only `ENGINEERING`, `SALES`, `HR`, `FINANCE` are allowed. There's no built-in annotation for that, so write your own.
Create a `validation` package with two classes:
- `ValidDepartment`: the annotation
- `DepartmentValidator`: the logic

<details><summary>Hint 1: what the annotation needs</summary>

`@Constraint(validatedBy = DepartmentValidator.class)`, `@Target(ElementType.FIELD)`,
`@Retention(RetentionPolicy.RUNTIME)`, and three members that are required by the spec:
`String message()`, `Class<?>[] groups()`, `Class<? extends Payload>[] payload()`, each with a `default`.
</details>
<details><summary>Hint 2: the validator</summary>

`implements ConstraintValidator<ValidDepartment, String>` and override `isValid(String value, ConstraintValidatorContext ctx)`.
Convention: return `true` for `null` and let `@NotNull` decide whether the field is required.
</details>

### 6. Unit-test your rules (no Spring needed)
Create `src/test/java/com/learn/validation/EmployeeValidationTest.java`. Test one valid employee and
one broken field per rule.
<details><summary>Hint</summary>

```java
Validator validator = Validation.buildDefaultValidatorFactory().getValidator();
Set<ConstraintViolation<Employee>> violations = validator.validate(employee);
```
Each violation's `getPropertyPath()` tells you which field failed.
</details>

```bash
./gradlew :03-validation:test
```

## Done when

- [ ] The bad curl from step 0 returns `400` and lists all problems
- [ ] A valid employee still returns `201`
- [ ] `/api/employees/0` returns `400`
- [ ] `"department":"MARKETING"` is rejected by your custom validator
- [ ] Your tests pass

## Bonus

1. `phone` field: exactly 10 digits (`@Pattern`)
2. `joiningDate` (`LocalDate`): not in the future (`@PastOrPresent`)
3. Make `@ValidDepartment` case-insensitive
4. **Think about it:** what's the difference between `@NotNull`, `@NotEmpty` and `@NotBlank` for a `String`?
5. **Think about it:** we put validation annotations on the JPA entity. What problems could that cause?
   (Lesson 05 introduces DTOs.)
