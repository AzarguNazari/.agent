---
name: java-code-style
description: >-
  Java and Spring style rules—dependency injection, Lombok, Optionals, formatting,
  Checkstyle/PMD, and quality gates.
---

## Trigger
Use when writing, reviewing, or refactoring Java code.

## Rules

### General
- Never modify method parameters (no side effects)
- No reflection (`java.lang.reflect.*`)
- No `var` keyword
- No `TODO`/`FIXME` comments unless linked to a Jira ticket
- Remove all unused code (variables, methods, classes)
- Use `records` for plain data holders; never add constructors to records with >2 fields — use `@Builder` instead
- No `@With` on records in mappers — use `@Builder`
- No `java.util.Calendar`, `java.util.Date`, `java.util.File`
- `final` keyword only on classes/class fields, not inside methods

### Dependency Injection
- Constructor injection only; annotate with `@Autowired` on package-private constructor
- Max 3 dependencies preferred, hard max = 5
- No `@AllArgsConstructor`, `@NoArgsConstructor`, `@RequiredArgsConstructor` for DI
- A bean with 0 dependencies → not a bean, remove from DI container
- Beans must not be extended (no `extends` on Spring beans)
- No interfaces unless multiple implementations exist

### Methods & Parameters
- Max 5 parameters per method/constructor (>3 = warning)
- Either all params on one line OR each param on its own line — no mixed layout
- Closing `)` always on same line as last parameter, never on a new line
- Single-purpose methods; name must reflect purpose (`fetch`, `compute`, `determine` > `get`)
- Use guard clauses (early return) instead of nested ifs

### Optionals
- Only use `Optional` as a method return type
- Never use `Optional` as a record/constructor parameter
- Never create `Optional` just to avoid null checks

### Spring
- Prefer `@ConfigurationProperties` record over `@Value`
- Prefer explicit `@Query` SQL over method-name conventions
- No `ResponseEntity<>` return type unless setting headers; use `@ResponseStatus` or `ResponseStatusException`

### Enums
- Never use `null` as an enum value; use `UNKNOWN` or `NONE`

### Lombok (allowed subset)
```
lombok.toString.onlyExplicitlyIncluded = true
```
- Allowed: `@Log4j2`, `@Builder`, `@Getter`, `@Setter`, `@ToString` (explicit only)
- Forbidden: `@Data`, `@Value`, `@val`, `@var`, `@SneakyThrows`, `@NonNull`, `@AllArgsConstructor`, `@NoArgsConstructor`, `@RequiredArgsConstructor`
- Use `@ToString(onlyExplicitlyIncluded = true)` + `@ToString.Include` on safe fields only

### Annotations
- No `@NotNull`, `@NonNull`, `@Nullable` from Jetbrains/Lombok/Findbugs/JSR-305
- `jakarta.validation` annotations ARE allowed for runtime validation with `@Validated`

### Exception Handling
- No exceptions for control flow
- No empty catch blocks
- No catch-log-rethrow pattern
- Use multi-catch `catch (IOException | OtherException ex)` where possible
- Only catch specific exceptions; catching `Exception`/`RuntimeException`/`Throwable` requires inline comment justification

### Formatting
- Indentation: 4 spaces; line continuation: 8 spaces
- Max line length: 170 characters
- No trailing whitespace
- Line breaks: `\n` only
- Switch: `case` indented inside `switch`; prefer new arrow syntax

### Imports order
1. Static imports
2. `de.c24.*`
3. `de.check24.*`
4. 3rd-party (including jakarta)
5. `java.*`
6. `javax.*`

### Naming
- Singular for package and class names: `CountryService` not `CountriesService`

### JavaDoc (public API only)
- All public classes, constructors, methods must have JavaDoc
- Description mandatory and meaningful
- `@throws` for all known exceptions
- `@param` and `@return` when not obvious from signature
- No `@author`, `@since`, `@created`

### Constants
- Private if used only within the class
- Create a constant only when same value serves same business purpose in multiple places

### Concurrency
- Avoid where possible
- If needed: prefer `ExecutorService` or Spring `@Async`; use `parallel()` / `parallelStream()` with extreme caution

### AutoCloseable
- Always use try-with-resources for `AutoCloseable` resources

### Messaging
- Dead Letter Queue only for technical failures (deserialization error, no handler)
- Business errors must NOT go to DLQ; make them visible through proper business mechanisms

### Utilities
- Avoid central utility classes
- Avoid `StringUtils` from Spring/Apache/Micrometer — use Java core SDK equivalents
- Do not add a new dependency just for a small helper function

## Code Quality
- Checkstyle and PMD: zero tolerance — any violation breaks the build
- No new suppression comments allowed
- JaCoCo coverage target: 80% (line, method, class, branch)
- Use AssertJ for assertions (not JUnit assertions, not Hamcrest)
- Use Mockito; avoid `any()`, `anyList()`, `anyString()` matchers — use real objects

## Output format
When generating Java code, always comply with all rules above. Flag any rule violation with an inline comment `// VIOLATION: <rule>` if demonstrating bad code.
