# Java Scenario-Based Interview Questions and Answers
## Beginner to Advanced | Real-World Applications | Core Java + Backend + System Design

Java interviews often test more than definitions. Interviewers want to know how you solve real problems, investigate failures, understand Java internals, and make decisions under production constraints.

This comprehensive question bank covers **Core Java, OOP, collections, exception handling, multithreading, JVM internals, databases, Spring Boot, REST APIs, security, payments, microservices, Docker, Kubernetes, and system design.**

---

### The 7-Step Scenario Framework
Every scenario in this guide can be approached with rigorous engineering discipline:
1. **Clarify the requirement:** Identify the expected behavior and constraints.
2. **Identify the failure or bottleneck:** Determine whether it concerns logic, memory, concurrency, I/O, security, or architecture.
3. **Explain the underlying mechanism:** Describe what Java, the JVM, the database, or the network is doing.
4. **Present a solution:** Provide clean, compilable Java code, SQL, or an investigation plan.
5. **Discuss alternatives:** Compare approaches and their trade-offs.
6. **Consider production risks:** Think about concurrency, retries, security, observability, and failure recovery.
7. **Explain verification:** Describe the tests, metrics, logs, or experiments that establish whether the solution works.

---

# Master Question Bank Overview

| Part | Engineering Domain | Scenarios Covered |
|:---:|---|---|
| **Part 1** | [Core Java Scenarios](#part-1-core-java-scenarios) | Scenarios 1–5 (`==` vs `equals()`, Pass-by-Value, `NullPointerException`, String Pool & Enums, `ArrayList` vs `HashSet` Performance) |
| **Part 2** | [Object-Oriented Programming Scenarios](#part-2-object-oriented-programming-scenarios) | Scenarios 6–8 (Polymorphic Payment Providers, Preventing Subclass Bypass with Template Method, Single Responsibility Principle & Outbox Pattern) |
| **Part 3** | [Exception Handling and Reliability](#part-3-exception-handling-and-reliability) | Scenarios 9–10 (`FileNotFoundException` in Docker Containers, Indiscriminate Catching vs Ambiguous Payment Timeouts) |
| **Part 4** | [Collections and Data Structures](#part-4-collections-and-data-structures) | Scenarios 11–12 (Mutable `HashMap` Keys, `ConcurrentModificationException` & Fail-Fast Iterators) |
| **Part 5** | [Multithreading and Concurrency](#part-5-multithreading-and-concurrency) | Scenarios 13–15 (Lost Counter Updates & Atomics, Thread Deadlocks & Lock Ordering, High Latency with Low CPU Usage) |
| **Part 6** | [Databases and Transactions](#part-6-databases-and-transactions) | Scenarios 16–17 (Preventing Overselling on the Last Item, Query Degradation at 10 Million Rows & Indexing Trade-offs) |
| **Part 7** | [Spring Boot and REST APIs](#part-7-spring-boot-and-rest-apis) | Scenarios 18–19 (Handling HTTP 500 on Invalid User Input with Jakarta Validation, Debugging Container Database Connectivity) |
| **Part 8** | [Payments and Distributed Systems](#part-8-payments-and-distributed-systems) | Scenarios 20–21 (Payment Provider Timeout & Reconciliation, API Idempotency Keys for Duplicate Mobile Retries) |
| **Part 9** | [Docker, Kubernetes, and Production](#part-9-docker-kubernetes-and-production) | Scenarios 22–23 (Container Immediate Exit Diagnostics, Debugging `Running` Kubernetes Pods with Unreachable APIs) |
| **Part 10** | [Advanced Java Design Scenarios](#part-10-advanced-java-design-scenarios) | Scenarios 24–26 (Refactoring 12-Parameter Methods with Records/Builders, JVM Memory Leaks in Garbage-Collected Apps, Thread-Per-Request Exhaustion & Thread Pools) |
| **Part 11** | [Coding Interview Scenarios](#part-11-coding-interview-scenarios) | Scenarios 27–30 (Finding Duplicates at Scale, First Non-Repeating Character & UTF-16 Code Points, Merging Sorted Arrays & External Merge Sort, Designing a Distributed Rate Limiter) |
| **Part 12** | [Deep-Dive Continuation: Advanced OOP & Collections](#part-12-deep-dive-continuation-advanced-oop--collections) | Scenarios 31–36 (`Integer` Caching Pitfalls, `BigDecimal` Financial Precision, `HashMap` Red-Black Tree Collisions, `ConcurrentHashMap` Traps, `TreeMap` Comparator Contracts) |
| **Part 13** | [Deep-Dive Continuation: JVM Internals & Concurrency](#part-13-deep-dive-continuation-jvm-internals--concurrency) | Scenarios 37–43 (ClassLoader Conflicts, G1GC / ZGC Tuning & OOM, `volatile` & Happens-Before, `ThreadLocal` Leaks, `CompletableFuture` Deadlocks, Java 21 Virtual Threads Pinning) |
| **Part 14** | [Deep-Dive Continuation: Databases, Spring Boot & Microservices](#part-14-deep-dive-continuation-databases-spring-boot--microservices) | Scenarios 44–55 (HikariCP Pool Starvation, Hibernate N+1 Queries, `@Transactional` Self-Invocation Pitfall, Spring Bean Thread-Safety, BOLA/IDOR Security, Transactional Outbox, Kafka Rebalance Storms, Redis Cache Stampede, Circuit Breakers, Graceful K8s Shutdown) |

---

# Part 1: Core Java Scenarios

## Scenario 1: Two objects have the same data, but `==` returns `false`

**Interview question:** You create two `Employee` objects with the same ID and name. Why does comparing them using `==` return `false`?

```java
class Employee {
    int id;
    String name;

    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }
}

public class Main {
    public static void main(String[] args) {
        Employee e1 = new Employee(101, "Binil");
        Employee e2 = new Employee(101, "Binil");

        System.out.println(e1 == e2);
    }
}
```

**Output:**
```text
false
```

### Answer

The `==` operator compares object identity (memory reference addresses on the JVM heap) when used with reference types. Although `e1` and `e2` contain identical field values, each `new Employee(...)` allocation creates a distinct object instance on the heap.

To compare their logical contents, override `equals()` and `hashCode()` (or use a Java `record` for immutable value types):

```java
import java.util.Objects;

class Employee {
    private final int id;
    private final String name;

    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;

        if (!(obj instanceof Employee other)) {
            return false;
        }

        return id == other.id
                && Objects.equals(name, other.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }
}
```

Now:

```java
System.out.println(e1.equals(e2));
```

returns `true`.

**Real-world application:** Comparing employee records, detecting duplicate products, checking whether a user exists in a collection, and implementing value-based domain objects.

**Follow-up:** What happens if you override `equals()` but not `hashCode()`?

Hash-based collections such as `HashSet` and `HashMap` will fail to behave as expected. By the `Object.hashCode()` contract, if two objects are equal according to `equals()`, they **must** return the same `hashCode()`. Without overriding `hashCode()`, `Object.hashCode()` returns an identity-based hash code, placing logically equal objects into different hash buckets so `set.contains(e2)` or `map.get(e2)` returns `false` or `null`.

---

## Scenario 2: A method modifies an object, but reassignment does not affect the caller

**Interview question:** Why does the following method change the object's field but fail to replace the caller's object?

```java
class Box {
    int value;
}

public class Main {
    static void modify(Box box) {
        box.value = 100;

        box = new Box();
        box.value = 200;
    }

    public static void main(String[] args) {
        Box original = new Box();
        original.value = 10;

        modify(original);

        System.out.println(original.value);
    }
}
```

**Output:**
```text
100
```

### Answer

Java always uses **pass-by-value**. For object references, the value being passed is the reference (pointer address) to the heap object.

```text
Step 1: modify(original) called
Stack (main):   original ──┐
                           ├──> Heap: Box@1 (value = 10 -> 100)
Stack (modify): box ───────┘

Step 2: box = new Box() inside modify()
Stack (main):   original ─────> Heap: Box@1 (value = 100)
Stack (modify): box ──────────> Heap: Box@2 (value = 200)
```

When `original` is passed to `modify()`, Java copies the reference value into the local stack parameter `box`.

Initially, both references point to the same heap object (`Box@1`). Therefore, changing `box.value = 100` mutates the object observed through `original`.

However, this statement:

```java
box = new Box();
```

only updates the local parameter `box` on `modify`'s stack frame to point to a new heap object (`Box@2`). It does not change the caller's `original` variable.

**Real-world application:** Updating customer DTOs, passing configuration objects between services, and understanding why method calls cannot reassign caller variables directly.

---

## Scenario 3: A program throws `NullPointerException`

**Interview question:** Your application crashes when accessing a customer's email address. How would you investigate and fix it?

```java
String email = customer.getEmail().toLowerCase();
```

### Answer

A `NullPointerException` can occur if:

- `customer` is `null`.
- `customer.getEmail()` returns `null`.

Since Java 14+ (JEP 358: Helpful NullPointerExceptions), the JVM prints the exact null expression in the stack trace (e.g., `Cannot invoke "String.toLowerCase()" because the return value of "Customer.getEmail()" is null`).

A defensive implementation validates inputs at the boundary:

```java
if (customer == null) {
    throw new IllegalArgumentException(
            "Customer is required"
    );
}

String email = customer.getEmail();

if (email == null || email.isBlank()) {
    throw new IllegalArgumentException(
            "Customer email is required"
    );
}

email = email.toLowerCase(java.util.Locale.ROOT);
```

Using `Locale.ROOT` avoids locale-dependent case conversion bugs (such as the Turkish dotless `ı` where `"TITLE".toLowerCase()` produces `"tıtle"` instead of `"title"`) for identifiers that require consistent, locale-independent normalization. Real email-address normalization should follow the application's identity policy rather than blindly lowercasing every possible address.

### Production debugging process

1. Read the complete stack trace (including Helpful NPE messages in Java 14+).
2. Identify the exact line and dereference chain that failed.
3. Determine which reference was `null`.
4. Trace where that value originated (missing DB column, unvalidated JSON payload, partial initialization).
5. Validate inputs at the appropriate system boundary (`Objects.requireNonNull`, Bean Validation, or `Optional` return types for nullable queries).
6. Add a regression test covering `null` and blank inputs.
7. Fix the underlying data or lifecycle problem instead of merely hiding the exception.

**Important:** Do not automatically wrap code in `catch (NullPointerException e)`. Catching `NullPointerException` conceals programming defects and corrupts downstream state.

---

## Scenario 4: A string comparison behaves unexpectedly

**Interview question:** Why might this code produce an unexpected result?

```java
String status = new String("SUCCESS");

if (status == "SUCCESS") {
    System.out.println("Completed");
}
```

### Answer

`==` compares reference identity, not string contents. While string literals like `"SUCCESS"` are interned in the JVM String Pool, calling `new String("SUCCESS")` explicitly allocates a new `String` object on the heap outside the String Pool, so `status == "SUCCESS"` evaluates to `false`.

Use `.equals()` with the constant first (Yoda notation) or after validation:

```java
if ("SUCCESS".equals(status)) {
    System.out.println("Completed");
}
```

Calling `"SUCCESS".equals(status)` also avoids a `NullPointerException` if `status` is `null`.

For a fixed set of statuses, an `enum` is strongly preferred:

```java
enum PaymentStatus {
    PENDING,
    SUCCESS,
    FAILED
}
```

```java
PaymentStatus status = PaymentStatus.SUCCESS;

if (status == PaymentStatus.SUCCESS) {
    System.out.println("Completed");
}
```

Comparing enum constants using `==` is safe, fast, and compile-time checked because the JVM guarantees that each enum constant is a singleton instance per class loader.

**Real-world application:** Payment statuses, order statuses, user roles, configuration modes, and workflow states.

---

## Scenario 5: An application processes thousands of records slowly

**Interview question:** A program uses an `ArrayList` to store 100,000 user IDs and checks whether each incoming ID exists. The operation is slow. What would you change?

### Answer

Suppose the code looks like this:

```java
List<Integer> userIds = new ArrayList<>();

// Assume the list contains 100,000 IDs.

if (userIds.contains(50000)) {
    System.out.println("Found");
}
```

An `ArrayList.contains()` operation performs a sequential linear scan from index `0` to `n - 1`, giving `O(n)` time complexity per lookup. If you check 50,000 incoming requests against 100,000 stored IDs, the JVM performs up to $5 \times 10^9$ comparisons (`O(m * n)`).

If membership checks are frequent, use a `HashSet`:

```java
Set<Integer> userIds = new HashSet<>(existingUserIds);

if (userIds.contains(50000)) {
    System.out.println("Found");
}
```

Hash-based lookup is `O(1)` average time complexity (`O(log n)` worst-case in Java 8+ when bucket collisions exceed the treeification threshold of 8 for `Comparable` keys).

| Operation | `ArrayList` | `HashSet` | `LinkedHashSet` |
|---|---|---|---|
| Membership check (`contains`) | `O(n)` | `O(1)` average | `O(1)` average |
| Preserves insertion order | Yes | No | Yes |
| Allows duplicates | Yes | No | No |
| Index-based access (`get(i)`) | `O(1)` | No | No |

**Important:** A `HashSet` is not always the right choice. If duplicates matter or index access is required, retain the list. If you need both insertion ordering and `O(1)` membership checks, use `LinkedHashSet` and measure the additional memory footprint.

---

# Part 2: Object-Oriented Programming Scenarios

## Scenario 6: A payment application supports multiple payment providers

**Interview question:** Your application initially supports card payments. Later, the business requires PayPal and bank transfers. How would you design the code to avoid modifying the entire application whenever a new provider is added?

### Poor design

```java
class PaymentService {
    void pay(String type, double amount) {
        if (type.equals("CARD")) {
            // Card payment logic
        } else if (type.equals("PAYPAL")) {
            // PayPal payment logic
        } else if (type.equals("BANK")) {
            // Bank transfer logic
        }
    }
}
```

As more providers are added, the conditional logic violates the **Open/Closed Principle (OCP)** and becomes difficult to maintain, test, and isolate. Also note that `double` should never be used for monetary amounts due to binary floating-point rounding inaccuracies (`0.1 + 0.2 != 0.3`); use `long` (minor units such as cents/paise) or `BigDecimal`.

### Better design: Interface and polymorphism (Strategy Pattern)

```java
interface PaymentProvider {
    PaymentResult pay(PaymentRequest request);
}

class CardPaymentProvider implements PaymentProvider {
    @Override
    public PaymentResult pay(PaymentRequest request) {
        // Integrate with the card payment provider.
        return PaymentResult.success();
    }
}

class PayPalPaymentProvider implements PaymentProvider {
    @Override
    public PaymentResult pay(PaymentRequest request) {
        // Integrate with the PayPal API.
        return PaymentResult.success();
    }
}

record PaymentRequest(
        String transactionId,
        long amountInMinorUnits,
        String currency
) {}

record PaymentResult(boolean accepted) {
    static PaymentResult success() {
        return new PaymentResult(true);
    }
}
```

The interface defines the common contract, while each implementation handles provider-specific behavior.

> [!NOTE]
> In Java records, a static factory method cannot be named `accepted()` with zero arguments if the record component is also named `accepted`, because the compiler automatically generates an instance accessor `public boolean accepted()`. Naming the static factory `PaymentResult.success()` avoids the method signature collision.

### Why is this design useful?

- **Open for extension, closed for modification:** New providers can be added as new classes implementing `PaymentProvider` without modifying existing provider logic.
- **Independent testability:** Each provider can be unit-tested and mocked independently.
- **Dependency Inversion:** The core orchestration service depends on the `PaymentProvider` abstraction, not concrete gateways.
- **Isolation:** Provider-specific SDKs, headers, and error mappings stay encapsulated.

**Advanced follow-up:** How would you select the correct provider at runtime in Spring Boot?

Inject a `List<PaymentProvider>` into a registry bean, expose a `PaymentType supportedType()` method on `PaymentProvider`, and build an immutable `Map<PaymentType, PaymentProvider>` (`EnumMap`) at startup. Also consider dynamic routing based on provider health, retries, timeouts, idempotency, and reconciliation for ambiguous outcomes.

**Important:** A production payment interface needs more than a `boolean` result. It should model `PENDING`, `SUCCEEDED`, `DECLINED`, and `UNKNOWN` (timeout) states, along with provider reference IDs and failure codes.

---

## Scenario 7: A subclass accidentally bypasses important business rules

**Interview question:** A base class defines a method that validates a transaction, but a subclass overrides it and omits the validation. How would you prevent this?

### Answer

This problem occurs when inheritance is used without strict behavioral contracts, allowing subclasses to violate the **Liskov Substitution Principle (LSP)**.

Possible solutions include:

1. Mark the orchestration method `final` so subclasses cannot override it (**Template Method Pattern**).
2. Keep invariant-enforcing validation methods `private`.
3. Use **composition over inheritance** (injecting a `PaymentGatewayAdapter` into a final `PaymentValidationService`) when the relationship is not truly an "is-a" hierarchy.
4. Use Java 17+ `sealed` classes (`public sealed abstract class PaymentProcessor permits CardProcessor, BankProcessor`) to control which classes may extend the base class.
5. Add contract tests that run against every implementation.

Example using the Template Method pattern:

```java
abstract class PaymentProcessor {

    public final PaymentResult process(
            PaymentRequest request) {

        validate(request);
        return execute(request);
    }

    private void validate(PaymentRequest request) {
        if (request == null) {
            throw new IllegalArgumentException(
                    "Request is required"
            );
        }
        if (request.amountInMinorUnits() <= 0) {
            throw new IllegalArgumentException(
                    "Amount must be positive"
            );
        }
    }

    protected abstract PaymentResult execute(
            PaymentRequest request
    );
}
```

Because `process()` is `final` and `validate()` is `private`, no subclass can bypass validation when callers invoke `process()`. Subclasses only implement the protected `execute()` hook.

In a real payment workflow, validation is accompanied by authorization checks, idempotency verification, audit logging, and transaction-state management.

---

## Scenario 8: A class has too many responsibilities

**Interview question:** A single Java class handles user registration, password hashing, database queries, email notifications, and report generation. What is wrong with this design?

### Answer

The class is a "God Object" that violates the **Single Responsibility Principle (SRP)**—it has multiple unrelated reasons to change (database schema changes, password hashing algorithm upgrades, email provider switches, or reporting format updates).

A maintainable design separates responsibilities into focused collaborators:

```text
UserController
     |
     v
UserService
     |
     +---- UserRepository
     |
     +---- PasswordHasher
     |
     +---- EmailService
```

Each component has a single, clear responsibility:

```java
class UserService {
    private final UserRepository repository;
    private final PasswordHasher passwordHasher;
    private final EmailService emailService;

    UserService(
            UserRepository repository,
            PasswordHasher passwordHasher,
            EmailService emailService) {
        this.repository = repository;
        this.passwordHasher = passwordHasher;
        this.emailService = emailService;
    }

    void register(String email, String password) {
        String hashedPassword =
                passwordHasher.hash(password);

        User user = new User(email, hashedPassword);

        repository.save(user);
        emailService.sendWelcomeEmail(email);
    }
}
```

**Benefits:**

- **Easier unit testing:** You can test `UserService` using mock or fake implementations of `UserRepository` and `EmailService`.
- **Better separation of concerns:** Hashing policies (e.g., migrating from BCrypt to Argon2id) change only `PasswordHasher`.
- **Reduced coupling:** Database access is isolated from SMTP/HTTP email delivery.
- **Clearer ownership of failures:** Each component has well-defined error semantics.

**Production consideration:** Database persistence (`repository.save(user)`) and external network calls (`emailService.sendWelcomeEmail(email)`) should not be naively coupled inside a single synchronous database transaction:
1. Holding a database transaction open while waiting on an external SMTP/HTTP server exhausts the database connection pool.
2. If saving the user commits but the email service fails, the user gets an error even though their account exists (or if the transaction rolls back after sending the email, the user receives a welcome email for a non-existent account).
3. **Solution:** Persist the `User` and an `OutboxEvent` (`USER_REGISTERED`) in the same local database transaction (**Transactional Outbox Pattern**), and let an asynchronous worker publish the welcome email with retries.

---

# Part 3: Exception Handling and Reliability

## Scenario 9: A file exists locally but cannot be found in Docker

**Interview question:** Your Java application works on your laptop but throws `FileNotFoundException` (or `NoSuchFileException`) after deployment in a Docker container. Why?

### Possible causes

- The file was never copied into the Docker image (missing `COPY` instruction or excluded by `.dockerignore`).
- The application uses a relative path (e.g., `new File("config/settings.json")`) that resolves against a different working directory (`WORKDIR`).
- Case-sensitivity differences: Windows and macOS (default APFS) filesystems are case-insensitive (`Settings.json` matches `settings.json`), whereas Linux container filesystems are strictly case-sensitive.
- A file packaged inside a Spring Boot fat JAR is accessed via `java.io.File` or `Path.of(...)` instead of `ClassLoader.getResourceAsStream()` (files inside a nested JAR are not regular filesystem paths).
- The Kubernetes `ConfigMap` or Docker volume is mounted at a different path, or overwrote the target directory.
- The container runs as a non-root user (e.g., `USER 1001`) that lacks read permissions (`chmod`) on the file.

### How to investigate

Log the resolved absolute path, working directory, existence, and permissions at startup:

```java
import java.nio.file.Files;
import java.nio.file.Path;

public class Main {
    public static void main(String[] args) {
        Path path = Path.of("/app/config/settings.json");

        System.out.println(
                "Working dir: " + Path.of("").toAbsolutePath()
        );

        System.out.println(
                "Absolute path: " + path.toAbsolutePath()
        );

        System.out.println(
                "Exists: " + Files.exists(path)
        );

        System.out.println(
                "Readable: " + Files.isReadable(path)
        );
    }
}
```

**Best practice:** External configuration should be supplied through environment variables, mounted Kubernetes `ConfigMap`/`Secret` volumes with explicit absolute paths, or classpath resources read via `InputStream`, never assumed to exist in a developer's relative local directory.

---

## Scenario 10: Should you catch every exception?

**Interview question:** A developer writes this code to avoid application crashes:

```java
try {
    processPayment();
} catch (Exception e) {
    System.out.println("Something went wrong");
}
```

Is this good practice?

### Answer

No. Catching `Exception` indiscriminately and swallowing it (`System.out.println`) is a severe production anti-pattern:
- It loses the stack trace and root cause.
- It leaves the application in an inconsistent state while pretending the method succeeded.
- If `InterruptedException` is caught as part of `Exception`, it clears the thread's interrupt status and breaks cooperative thread cancellation.

A better approach distinguishes specific failure modes that the application can handle deterministically:

```java
try {
    processPayment();
} catch (PaymentDeclinedException e) {
    // Business failure: mark payment DECLINED and notify caller.
} catch (PaymentTimeoutException e) {
    // Ambiguous outcome: mark payment UNKNOWN/PENDING_RECONCILIATION.
    // Do NOT blindly issue a new charge without idempotency!
}
```

### Why does a payment timeout need special treatment?

A network or read timeout does **not** prove that a payment failed.

The payment provider might have received the request and charged the customer's card, but the network connection dropped before the HTTP response reached your server.

Blindly retrying with a new transaction identifier will charge the customer twice.

A robust system must:

1. Generate and persist a stable **idempotency key** before calling the external gateway.
2. Record the payment attempt in the database with initial status `INITIATED`.
3. Retry safely using the **exact same idempotency key** where the provider supports it.
4. Query the provider's transaction status API when the outcome is ambiguous (`PaymentTimeoutException`).
5. Run a background reconciliation worker for unresolved transactions.

**Interview lesson:** Exception handling must preserve failure semantics so the system can recover safely. Swallowing an exception and continuing is worse than failing fast.

---

# Part 4: Collections and Data Structures

## Scenario 11: A `HashMap` does not find an object that was previously inserted

**Interview question:** You insert a custom object into a `HashMap`, modify one of its fields, and then cannot retrieve the entry using that object. Why?

### Answer

If fields used by `equals()` and `hashCode()` mutate after an object is inserted as a key in a `HashMap` or `HashSet`, the key's recalculated hash code no longer points to the bucket where the entry was originally stored.

Example:

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Objects;

class EmployeeKey {
    int id;

    EmployeeKey(int id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof EmployeeKey other)) {
            return false;
        }

        return id == other.id;
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
}

public class Main {
    public static void main(String[] args) {
        Map<EmployeeKey, String> map = new HashMap<>();

        EmployeeKey key = new EmployeeKey(101);
        map.put(key, "Employee");

        key.id = 202;

        System.out.println(map.get(key));
    }
}
```

**Output:**
```text
null
```

### What happens internally?

1. On `map.put(key, "Employee")`, `key.hashCode()` is calculated using `id = 101`, and the entry is placed in bucket $B_{101}$ with cached hash $H_{101}$.
2. When `key.id = 202` is executed, the object in bucket $B_{101}$ mutates in place, but `HashMap` is not notified and does not rehash the entry.
3. On `map.get(key)`, `HashMap` computes the new hash $H_{202}$ and searches bucket $B_{202}$, which is empty—returning `null`.
4. Even worse, looking up `new EmployeeKey(101)` also returns `null` because although it lands in bucket $B_{101}$, the stored key now has `id = 202`, so `equals()` returns `false`. The entry is effectively orphaned inside the map!

**Best practice:** Always use **immutable** objects (`String`, `Integer`, `UUID`, or Java `record`s with immutable components) as keys in hash-based collections:

```java
record EmployeeKey(int id) {}
```

---

## Scenario 12: A `ConcurrentModificationException` occurs while removing elements

**Interview question:** Why can the following code fail?

```java
List<String> names = new ArrayList<>(
        List.of("A", "B", "C")
);

for (String name : names) {
    if (name.equals("B")) {
        names.remove(name);
    }
}
```

### Answer

The enhanced `for` loop (`for-each`) is compiled into an `Iterator` loop (`names.iterator()`). `ArrayList` maintains an internal structural modification counter called `modCount`. When the iterator is created, it snapshots `expectedModCount = modCount`.

Calling `names.remove(name)` directly on the list increments `modCount`, but does **not** update the iterator's `expectedModCount`. On the next iteration, `iterator.next()` calls `checkForComodification()`, sees `modCount != expectedModCount`, and throws `ConcurrentModificationException`.

### Solution 1: Use explicit `Iterator.remove()`

`Iterator.remove()` removes the current element and synchronizes `expectedModCount = modCount`:

```java
Iterator<String> iterator = names.iterator();

while (iterator.hasNext()) {
    String name = iterator.next();

    if (name.equals("B")) {
        iterator.remove();
    }
}
```

### Solution 2: Use `Collection.removeIf()` (Preferred in Java 8+)

```java
names.removeIf(name -> name.equals("B"));
```

`ArrayList.removeIf()` traverses the backing array in a single `O(n)` pass using a bitset/shift without `O(n^2)` repeated array shifting.

**Important:**
1. `ConcurrentModificationException` does **not** necessarily mean multiple threads are involved; it most commonly happens in a single thread when modifying a collection directly while iterating.
2. Fail-fast iterators operate on a best-effort basis and must never be relied upon for thread safety. For true concurrent access across threads, use `CopyOnWriteArrayList`, `ConcurrentHashMap`, or explicit synchronization.

---

# Part 5: Multithreading and Concurrency

## Scenario 13: A counter produces the wrong result when multiple threads run

**Interview question:** Ten threads each increment a shared counter 10,000 times. Why might the result be less than 100,000?

```java
class Counter {
    int count = 0;

    void increment() {
        count++;
    }
}
```

### Answer

The operation `count++` (compiled to `getfield`, `iconst_1`, `iadd`, `putfield` in JVM bytecode) is **not atomic**.

Conceptually, it involves three separate steps:

1. Read the current value of `count` from memory into a register.
2. Add `1`.
3. Write the updated value back to `count`.

If Thread A and Thread B both read `count = 42` at the same time, both compute `43` and both write `43`, resulting in a **lost update** (two increments occurred, but the value only increased by `1`). Furthermore, without synchronization or `volatile`, changes made by one thread may not even be immediately visible to other threads due to CPU caching and compiler optimizations.

### Solution 1: Intrinsic Lock (`synchronized`)

```java
class Counter {
    private int count = 0;

    synchronized void increment() {
        count++;
    }

    synchronized int getCount() {
        return count;
    }
}
```

Synchronizing both `increment()` and `getCount()` guarantees mutual exclusion and memory visibility (`happens-before` relationship).

### Solution 2: Lock-Free CAS (`AtomicInteger`)

```java
import java.util.concurrent.atomic.AtomicInteger;

class Counter {
    private final AtomicInteger count =
            new AtomicInteger();

    void increment() {
        count.incrementAndGet();
    }

    int getCount() {
        return count.get();
    }
}
```

`AtomicInteger` uses hardware-level Compare-And-Swap (CAS) instructions (`Unsafe.getAndAddInt`) without blocking threads.

### Solution 3: High-Contention Metrics (`LongAdder`)

```java
import java.util.concurrent.atomic.LongAdder;

class MetricsCounter {
    private final LongAdder count = new LongAdder();

    void increment() {
        count.increment();
    }

    long sum() {
        return count.sum();
    }
}
```

**Which should you choose?**

- `synchronized`: Best when protecting multiple related variables or compound business invariants together.
- `AtomicInteger` / `AtomicLong`: Best for a single counter when callers need an exact return value on each update (e.g., sequence generators, CAS thresholds).
- `LongAdder`: Best for high-throughput metrics/telemetry under heavy thread contention (stripes updates across an internal `Cell[]` array to eliminate CAS retry spinning), though `sum()` is not an atomic snapshot if concurrent updates are still occurring.

---

## Scenario 14: Two threads deadlock

**Interview question:** Two threads each hold a lock that the other thread needs. The application stops making progress. How do you investigate and prevent the problem?

### Example

```java
Object lockA = new Object();
Object lockB = new Object();
```

Imagine:

- **Thread 1** acquires `lockA` and attempts to acquire `lockB`.
- **Thread 2** acquires `lockB` and attempts to acquire `lockA`.

Neither thread can ever proceed.

### How to investigate in production

Capture a JVM thread dump using:
```bash
jcmd <pid> Thread.print
# or
jstack <pid>
```
The JVM automatically detects `synchronized` and `ReentrantLock` cycles and prints a section at the bottom of the thread dump:
```text
Found one Java-level deadlock:
=============================
"Thread-1":
  waiting to lock monitor 0x00007f8a... (object 0x000000076ab0, a java.lang.Object),
  which is held by "Thread-2"
"Thread-2":
  waiting to lock monitor 0x00007f8b... (object 0x000000076ab8, a java.lang.Object),
  which is held by "Thread-1"
```

### Prevention strategies

1. **Acquire locks in a strict, deterministic global order:** For example, when transferring money between two accounts, always lock the account with the smaller `accountId` first, then the larger `accountId`.
2. **Minimize lock scope:** Never hold an in-memory lock while making external RPC, HTTP, or database calls.
3. **Use timed lock acquisition:** `ReentrantLock.tryLock(timeout, TimeUnit)` allows a thread to back off, release its held lock, and retry if it cannot acquire the second lock within a deadline.

```java
void transfer(Account from, Account to, long amount) {
    Account firstLock = from.id() < to.id() ? from : to;
    Account secondLock = from.id() < to.id() ? to : from;

    synchronized (firstLock) {
        synchronized (secondLock) {
            from.debit(amount);
            to.credit(amount);
        }
    }
}
```

**Interview follow-up:** What are the four classic Coffman conditions required for deadlock?

1. **Mutual exclusion:** Resources cannot be shared simultaneously.
2. **Hold and wait:** A thread holding one resource waits to acquire additional resources.
3. **No preemption:** Resources cannot be forcibly taken away from the holding thread.
4. **Circular wait:** A closed chain of threads exists where each thread waits for a resource held by the next thread in the chain.

Enforcing a global lock acquisition order breaks **Circular wait**, making deadlock mathematically impossible.

---

## Scenario 15: A Java API is slow even though CPU usage is low

**Interview question:** A Java REST API takes three seconds to respond, but CPU usage remains around 10%. What could be wrong?

### Answer

Low CPU usage combined with high response latency means threads are **blocked or waiting on I/O or locks**, not executing CPU-intensive instructions.

Possible causes include:

- Slow, unindexed database queries or database row lock contention.
- High latency from a downstream REST/gRPC API.
- Database connection pool exhaustion (threads waiting on `HikariPool.getConnection()`).
- HTTP client connection pool exhaustion.
- Synchronized lock contention (threads parked in `BLOCKED` or `WAITING` state).
- DNS resolution delays or TCP connection timeouts.
- Web server thread pool saturation (e.g., all 200 Tomcat platform threads blocked on synchronous I/O).

### How to investigate

1. **Distributed Tracing (OpenTelemetry / Jaeger / Zipkin):** Measure the duration of each span in the request lifecycle:

```text
Request received
      |
      v
Authentication
      |
      v
Database query
      |
      v
External API
      |
      v
Response serialization
      |
      v
Response sent
```

For example, a trace waterfall might reveal:

```text
Total request:       3000 ms
Authentication:        10 ms
Database:             100 ms
External API:        2800 ms
Serialization:         20 ms
Other overhead:        70 ms
```

2. **Thread Dumps & JFR (Java Flight Recorder):** Inspect where request threads are parked (`RUNNABLE` in socket read vs `WAITING` on connection pool vs `BLOCKED` on monitor).
3. **Connection Pool Metrics:** Check `hikaricp_connections_pending` and `http_client_pool_pending`.

**Correct response:** Investigate the slow downstream dependency or pool bottleneck rather than scaling up CPU cores. Possible fixes include setting strict connect/read timeouts, connection pooling, caching, asynchronous processing, bulk/batch queries, or circuit breakers.

---

# Part 6: Databases and Transactions

## Scenario 16: Two customers purchase the last available item

**Interview question:** Your inventory database contains one item. Two customers attempt to buy it simultaneously. How do you prevent both orders from succeeding?

### Unsafe approach (Read-Modify-Write Race Condition)

```text
Thread A reads stock = 1
Thread B reads stock = 1
Thread A decides stock is sufficient (1 >= 1)
Thread B decides stock is sufficient (1 >= 1)
Thread A writes stock = 0
Thread B writes stock = 0 (or -1)
```

If implemented as a standard `SELECT quantity FROM inventory WHERE product_id = ?` in Java followed by an `UPDATE` under the default `READ COMMITTED` isolation level, both transactions read `1` and oversell the item.

### Approach 1: Atomic Conditional Update (Best for Single-Statement Decrement)

Push the invariant check into the `WHERE` clause of a single atomic `UPDATE` statement:

```sql
UPDATE inventory
SET quantity = quantity - 1
WHERE product_id = ?
  AND quantity > 0;
```

Check the number of affected rows returned by JDBC `PreparedStatement.executeUpdate()`:

- **`1` affected row:** The database acquired an exclusive row lock, verified `quantity > 0`, decremented the stock, and succeeded.
- **`0` affected rows:** Another transaction already decremented `quantity` to `0` (or the product does not exist). Reject the order immediately.

### Approach 2: Pessimistic Locking (`SELECT ... FOR UPDATE`)

When complex validation across multiple tables is required before decrementing:

```sql
BEGIN;
SELECT quantity FROM inventory WHERE product_id = ? FOR UPDATE;
-- Second concurrent transaction blocks here until the first commits/rolls back
UPDATE inventory SET quantity = quantity - 1 WHERE product_id = ?;
COMMIT;
```

### Approach 3: Optimistic Locking (`@Version`)

Add a `version` column to the `inventory` table:

```sql
UPDATE inventory
SET quantity = quantity - 1,
    version = version + 1
WHERE product_id = ?
  AND version = ?
  AND quantity > 0;
```

If affected rows is `0` (in JPA, `OptimisticLockException`), another transaction modified the row concurrently.

| Strategy | Contention Level | Pros | Cons |
|---|---|---|---|
| Atomic `UPDATE ... WHERE quantity > 0` | Low to High | Simple, single round-trip, database handles row lock | Limited to conditions expressible in SQL `WHERE` |
| Pessimistic (`SELECT ... FOR UPDATE`) | High contention | Prevents wasted work; serializes access cleanly | Holds DB locks longer; risk of deadlocks if locking multiple rows out of order |
| Optimistic (`@Version`) | Low contention | No read locks held during business logic | High retry/failure rate during flash sales (thundering herd) |

---

## Scenario 17: A database query becomes slower as the table grows

**Interview question:** A query runs quickly with 1,000 rows but becomes slow when the table contains 10 million rows.

```sql
SELECT *
FROM orders
WHERE customer_id = 500;
```

What would you investigate?

### Answer

With 1,000 rows, the entire table fits in a few 8 KB memory pages in the database buffer pool, so a **Full Table Scan (`Seq Scan`)** takes less than a millisecond. With 10 million rows, a sequential scan reads gigabytes of data from disk/memory for every single query (`O(n)`).

Possible causes include:

- **Missing index** on `customer_id`.
- **Non-SARGable predicate** preventing index usage (e.g., `WHERE CAST(customer_id AS VARCHAR) = '500'` or `WHERE YEAR(created_at) = 2026`, or implicit type mismatch between the Java parameter and column type).
- **`SELECT *` fetching wide columns** (e.g., `TEXT`/`JSONB` payloads) causing heavy heap table random I/O instead of an Index-Only Scan.
- **Unbounded result set** (a single power-user or B2B customer with 500,000 orders returning all rows without pagination).
- **Outdated table statistics** causing the query planner to choose a suboptimal execution plan.
- **Lock contention or I/O saturation.**

If the query frequently filters by `customer_id`, create a B-Tree index:

```sql
CREATE INDEX idx_orders_customer_id
ON orders(customer_id);
```

*(In production PostgreSQL on a live 10M-row table, use `CREATE INDEX CONCURRENTLY` so writes are not blocked during index creation.)*

### How to diagnose it

1. Run `EXPLAIN (ANALYZE, BUFFERS)` on the query.
2. Check whether the execution plan uses a `Seq Scan` (`O(n)`) or an `Index Scan` / `Bitmap Index Scan` (`O(log n)`).
3. Inspect buffer hits vs disk reads and the number of rows filtered.
4. Replace `SELECT *` with only the required columns and add pagination (`LIMIT` / keyset pagination).

**Interview lesson:** Do not add indexes blindly to every column. Every secondary index consumes RAM/disk and adds write amplification (`O(log n)` maintenance overhead) to every `INSERT`, `UPDATE`, and `DELETE`.

---

# Part 7: Spring Boot and REST APIs

## Scenario 18: A REST API returns HTTP 500 for invalid user input

**Interview question:** Your Spring Boot endpoint receives a negative product quantity and throws an exception. How would you return a meaningful HTTP response?

### Answer

Returning `500 Internal Server Error` for invalid client input violates HTTP semantics and triggers false-positive server alerts in monitoring systems. Client validation errors should return `400 Bad Request` (or `422 Unprocessable Entity`) with a structured error payload (such as RFC 7807 / RFC 9457 `ProblemDetail` in Spring Boot 3).

Use Jakarta Bean Validation (`@Valid`) and centralized exception handling (`@RestControllerAdvice`):

```java
import jakarta.validation.constraints.Min;
import jakarta.validation.constraints.NotBlank;

public record CreateProductRequest(
        @NotBlank(message = "Product name must not be blank")
        String name,
        @Min(value = 1, message = "Quantity must be at least 1")
        int quantity
) {}
```

Example controller:

```java
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.server.ResponseStatusException;
import jakarta.validation.Valid;

@RestController
@RequestMapping("/products")
public class ProductController {

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public String createProduct(
            @Valid @RequestBody CreateProductRequest request) {

        if (request.quantity() < 1) {
            throw new ResponseStatusException(
                    HttpStatus.BAD_REQUEST,
                    "Quantity must be at least 1"
            );
        }

        return "Product accepted";
    }
}
```

And a centralized `@RestControllerAdvice` for consistent error formatting:

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ProblemDetail;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import java.util.Map;
import java.util.stream.Collectors;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ProblemDetail handleValidationErrors(
            MethodArgumentNotValidException ex) {

        ProblemDetail problem = ProblemDetail.forStatusAndDetail(
                HttpStatus.BAD_REQUEST,
                "Request validation failed"
        );

        Map<String, String> fieldErrors = ex.getBindingResult()
                .getFieldErrors()
                .stream()
                .collect(Collectors.toMap(
                        err -> err.getField(),
                        err -> err.getDefaultMessage(),
                        (a, b) -> a
                ));

        problem.setProperty("errors", fieldErrors);
        return problem;
    }
}
```

### Common HTTP status codes

| Status | Meaning | Example |
|---|---|---|
| `200` | OK | Successful read or synchronous update |
| `201` | Created | Resource created (`POST /products`) |
| `204` | No Content | Successful deletion or update with no response body |
| `400` | Bad Request | Malformed JSON or invalid request field values |
| `401` | Unauthorized | Authentication credentials missing or invalid |
| `403` | Forbidden | Authenticated caller lacks permission for the resource |
| `404` | Not Found | Resource does not exist or is intentionally concealed |
| `409` | Conflict | Optimistic lock conflict or duplicate unique key |
| `429` | Too Many Requests | Rate limit exceeded |
| `500` | Internal Server Error | Unhandled server bug |
| `503` | Service Unavailable | Downstream outage or overload shedding |

**Important:** Never expose raw Java stack traces, SQL table names, or internal credentials in client-facing HTTP error responses. Log the full stack trace internally with a correlation/trace ID and return only the safe `traceId` to the client.

---

## Scenario 19: A Spring Boot application works locally but fails after deployment

**Interview question:** The application connects to a local database successfully, but in Docker or Kubernetes it cannot connect. What would you check?

### Answer

Investigate the runtime environment and networking topology rather than assuming the Java code is broken.

Check:

1. **Whether the application uses `localhost` or `127.0.0.1`:** Inside a Docker container or Kubernetes Pod, `localhost` refers to the **container's own network namespace**, not the host laptop or an external database container.
2. **Database hostname, DNS resolution, and port:** Verify the Kubernetes Service DNS name (e.g., `postgres-svc.prod.svc.cluster.local:5432`) or cloud RDS endpoint.
3. **Network connectivity, Security Groups, and `NetworkPolicy`:** Ensure Kubernetes `NetworkPolicy` or AWS Security Groups allow egress from the application pod and ingress on the database port.
4. **Credentials and Secrets:** Verify Kubernetes `Secret` keys match the environment variable names injected into the pod, and ensure there are no trailing newline characters (`\n`) in base64-encoded secrets.
5. **Spring Profiles and configuration precedence:** Check `SPRING_PROFILES_ACTIVE` so a local `application-dev.yml` does not override production environment variables.
6. **Database readiness and startup ordering:** In Docker Compose or Kubernetes, the app container may start before the database finishes initializing.
7. **TLS/SSL mode and certificates:** Cloud databases often enforce `sslmode=require` or `sslmode=verify-full`.

Externalize configuration cleanly in `application.properties` or `application.yml`:

```properties
spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
```

**Production lesson:** Configuration, networking, and secret management must be decoupled from the build artifact. Never hardcode database URLs or passwords in source code or Docker images.

---

# Part 8: Payments and Distributed Systems

## Scenario 20: The payment provider succeeds, but your application times out

**Interview question:** A customer clicks Pay. The payment provider processes the transaction, but your application times out before receiving the response. What should happen next?

### Answer

This is one of the most critical real-world distributed systems scenarios (the **Two Generals' Problem** over an unreliable network).

A client-side HTTP timeout does **not** mean the remote operation failed. Either:
1. The request was lost on the way to the payment provider (customer was **not** charged), OR
2. The provider processed the payment and charged the card, but the response was delayed or lost on the way back (customer **was** charged).

A safe architecture uses a **persistent state machine** before network calls and a **deterministic idempotency key**:

```text
Customer submits payment
          |
          v
Create local payment record (status = INITIATED, idempotency_key = UUID)
          |
          v
Send request using stable idempotency key
          |
          v
Provider processes payment
          |
          +-------------------------+
          |                         |
          v                         v
Response received             Response times out
          |                         |
          v                         v
Update local state             Mark status = UNKNOWN / PENDING_VERIFICATION
(SUCCEEDED / DECLINED)              |
                                    v
                             Query provider status API or retry
                             safely with SAME idempotency key
                                    |
                                    v
                             Reconcile final outcome
                             (via Polling + Signed Webhook + Daily Settlement)
```

### Essential design principles

- **Persist before calling out:** Insert the `payment_attempts` row with a unique `idempotency_key` and `status = INITIATED` in a committed local transaction *before* making the external HTTP call.
- **Pass the idempotency key to the provider:** Send `Idempotency-Key: <key>` in the provider's HTTP header so retries never double-charge.
- **Do not keep a DB transaction open during the HTTP call:** Commit the `INITIATED` state first, execute the external HTTP call outside any DB transaction, and then open a short transaction to record the final state.
- **Handle webhooks idempotently:** Verify the provider's cryptographic HMAC signature, and use atomic state transitions (`WHERE status IN ('INITIATED', 'UNKNOWN')`) so duplicate or out-of-order webhooks are safely ignored.
- **Background reconciliation:** Run a scheduled job that queries the provider for any payment stuck in `INITIATED` or `UNKNOWN` for more than $N$ minutes.

**Critical distinction:** A local database `@Transactional` annotation cannot roll back an external HTTP call to Stripe, Razorpay, or PayPal. You must design explicit state transitions and reconciliation.

---

## Scenario 21: An API receives duplicate requests

**Interview question:** A mobile client retries a request because the network is unreliable. The server receives the same order-creation request twice. How do you avoid creating two orders?

### Answer

Implement an **API Idempotency Layer** backed by a unique database constraint or atomic distributed lock.

1. **Client generates an `Idempotency-Key`:** When the user taps "Place Order", the mobile app generates a UUID v4 (or deterministic hash of the checkout session ID) and sends it in the `Idempotency-Key` HTTP header on every retry of that logical action.
2. **Atomic claim on the server:** The server inserts the key into an `idempotency_records` table with a `UNIQUE (user_id, idempotency_key)` constraint and a request payload hash.
3. **State handling:**

```text
Request with Idempotency-Key
  |
  v
Attempt atomic INSERT of (user_id, idempotency_key, payload_hash, status=IN_PROGRESS)
  |
  +---- Insert succeeds (New request) --------> Execute order creation in same DB tx,
  |                                             save response JSON, set status=COMPLETED
  |
  +---- Unique constraint violation (Duplicate)
          |
          +---- Payload hash mismatch --------> Return 422 Unprocessable Entity
          |                                     (Key reused with different parameters!)
          |
          +---- Status == COMPLETED ----------> Return cached HTTP status & response body
          |
          +---- Status == IN_PROGRESS --------> Return 409 Conflict (or wait/poll)
```

Example schema and Java logic:

```sql
CREATE TABLE idempotency_keys (
    user_id BIGINT NOT NULL,
    idempotency_key VARCHAR(64) NOT NULL,
    request_hash VARCHAR(64) NOT NULL,
    status VARCHAR(20) NOT NULL,
    response_code INT,
    response_body JSONB,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    PRIMARY KEY (user_id, idempotency_key)
);
```

By inserting the idempotency record and creating the order inside the **same database transaction** (or claiming the key atomically first), two concurrent retries arriving at the exact same millisecond cannot both pass an application-level `if (!exists)` check.

**Real-world applications:** Payments, order creation, ticket booking, inventory reservations, and financial ledger transfers.

---

# Part 9: Docker, Kubernetes, and Production

## Scenario 22: A Docker container starts and immediately exits

**Interview question:** You deploy a Java application in Docker. The container starts but exits after a few seconds. What would you investigate?

### Answer

A Docker container remains alive only as long as its `PID 1` foreground process is running. If `PID 1` exits or is killed by the Linux kernel, the container stops immediately.

Run these diagnostic commands first:

```bash
docker ps -a
docker logs <container_name>
docker inspect <container_name> --format '{{.State.ExitCode}} {{.State.OOMKilled}} {{.State.Error}}'
```

### How to interpret the Exit Code

| Exit Code | Meaning | Common Java Cause |
|---|---|---|
| `0` | Clean exit | The Java `main()` method finished normally (e.g., `spring-boot-starter-web` dependency was omitted so no embedded Tomcat server kept non-daemon threads running). |
| `1` | Application error | Unhandled exception during startup (missing `DB_URL` env var, port conflict, invalid YAML, `ClassNotFoundException`, or corrupt JAR). |
| `137` | `SIGKILL` (`128 + 9`) | The Linux **OOM Killer** terminated the JVM because total container memory (Heap + Metaspace + Direct Buffers + Thread Stacks) exceeded the Docker/Kubernetes memory limit (`OOMKilled: true`). |
| `143` | `SIGTERM` (`128 + 15`) | Container orchestrator (or failed Kubernetes liveness probe) sent a graceful stop signal. |

Example production-ready Dockerfile:

```dockerfile
FROM eclipse-temurin:21-jre

WORKDIR /app

# Create a non-root user for security
RUN useradd -r -u 1001 appuser
USER appuser

COPY target/app.jar app.jar

# Use exec form (JSON array) so java runs as PID 1 and receives SIGTERM directly
ENTRYPOINT ["java", "-XX:MaxRAMPercentage=75.0", "-jar", "app.jar"]
```

**Important:** Never set `-Xmx` to 100% of the container's memory limit. The JVM requires non-heap memory for Metaspace, thread stacks (`~1 MB` per platform thread), off-heap NIO direct buffers, GC structures, and native C libraries. Setting `-XX:MaxRAMPercentage=75.0` leaves 25% headroom for non-heap memory and prevents `Exit Code 137` (OOMKilled).

---

## Scenario 23: A Kubernetes pod is running, but the API is unavailable

**Interview question:** Kubernetes reports that your Java pod is `Running`, but requests to the application fail. How would you debug it?

### Answer

A `Running` pod status only means the container was created and its main process has not exited yet—it does **not** mean the Java web server is ready, listening on the right interface, or wired properly to the Kubernetes network path.

Investigate each layer systematically:

```text
Client
  |
  v
Ingress / Load Balancer (TLS, Host/Path routing)
  |
  v
Kubernetes Service (Selector labels, port -> targetPort)
  |
  v
Endpoints / EndpointSlices (Only populated when Pod ReadinessProbe passes!)
  |
  v
Pod Network Namespace (NetworkPolicy, containerPort)
  |
  v
Java Application (Bind address 0.0.0.0 vs 127.0.0.1, actual server.port)
```

Useful diagnostic commands:

```bash
kubectl get pods -o wide
kubectl describe pod <pod_name>
kubectl logs <pod_name>
kubectl get svc <service_name>
kubectl get endpoints <service_name>
kubectl get endpointslices -l kubernetes.io/service-name=<service_name>
kubectl port-forward pod/<pod_name> 8080:8080
```

### Checklist of common root causes

1. **Empty `Endpoints` due to failing Readiness Probe:** Run `kubectl get endpoints <service_name>`. If `ENDPOINTS` is `<none>`, either the Service's `spec.selector` labels do not match the Pod's `metadata.labels`, or the Pod's `readinessProbe` is failing.
2. **Port mismatch (`targetPort` vs `server.port`):** The Kubernetes Service defines `targetPort: 8080`, but the Spring Boot app started on port `8081` or `9090`.
3. **Binding to `127.0.0.1` instead of `0.0.0.0`:** If `server.address=127.0.0.1` is configured, the app only accepts loopback connections from inside the pod itself and rejects traffic coming through the pod's `eth0` interface.
4. **Slow Spring Boot startup killed by Liveness Probe:** If `livenessProbe` starts too early without a `startupProbe`, Kubernetes repeatedly restarts the pod before Spring finishes initializing.
5. **`NetworkPolicy` blocking ingress:** A restrictive `NetworkPolicy` drops traffic from the Ingress Controller namespace to the application pod.

---

# Part 10: Advanced Java Design Scenarios

## Scenario 24: A method has too many parameters

**Interview question:** Your service method takes 12 parameters, and developers frequently pass values in the wrong order. How would you improve the design?

### Poor design (Telescoping / Positional Parameter Trap)

```java
createOrder(
    101L,
    202L,
    5000L,
    "INR",
    true,
    "ONLINE",
    "123 MG Road",
    "BLR",
    "560001",
    null,
    false,
    "SUMMER10"
);
```

When multiple adjacent parameters share the same type (`long customerId, long productId, long amount` or `String currency, String paymentMethod`), accidentally swapping two arguments compiles without any warning and causes silent data corruption in production.

### Better design 1: Parameter Object (`record`) + Value Grouping

Group cohesive domain concepts into immutable records and validate invariants inside the compact constructor:

```java
public record Money(long amountInMinorUnits, String currency) {
    public Money {
        if (amountInMinorUnits <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
        if (currency == null || currency.length() != 3) {
            throw new IllegalArgumentException("3-letter ISO currency required");
        }
    }
}

public record CreateOrderRequest(
        long customerId,
        long productId,
        Money totalAmount,
        boolean expedited,
        String paymentMethod
) {}
```

### Better design 2: Builder Pattern for Complex Optional Parameters

When a request has many parameters or optional fields, add a fluent Builder (or staged builder) so every field is explicitly named at the call site:

```java
CreateOrderRequest request = CreateOrderRequest.builder()
        .customerId(101L)
        .productId(202L)
        .totalAmount(new Money(5000L, "INR"))
        .expedited(true)
        .paymentMethod("ONLINE")
        .build();

orderService.createOrder(request);
```

**Real-world benefit:** Self-documenting call sites, impossible to accidentally swap two `String` or `long` parameters, centralized invariant validation in the record constructor, and backward-compatible extensibility when new fields are added.

---

## Scenario 25: An application has a memory leak despite using garbage collection

**Interview question:** Java uses garbage collection. Why can an application still consume more and more memory until it throws `OutOfMemoryError`?

### Answer

A JVM Garbage Collector (G1GC, ZGC, Parallel GC) only reclaims objects that are **unreachable** from **GC Roots** (active thread stack variables, `static` fields, JNI references, active monitors). If an application unintentionally keeps references to objects it no longer logically needs (**unintentional object retention**), the GC is forbidden from collecting them, and old-generation heap usage climbs in a "sawtooth" pattern until `java.lang.OutOfMemoryError: Java heap space` is thrown.

### Common root causes of Java memory leaks

1. **Unbounded `static` collections or in-memory caches:** Adding entries to a `static Map` or `List` without eviction or TTL (use **Caffeine** with `maximumSize` instead of a raw `ConcurrentHashMap`).
2. **Unclosed resources:** Failing to close `InputStream`, JDBC `ResultSet`/`Connection`, or HTTP response bodies (`try-with-resources` prevents this).
3. **`ThreadLocal` variables on pooled threads:** Storing request data in a `ThreadLocal` and forgetting to call `threadLocal.remove()` in a `finally` block. Because Tomcat or executor threads live for the entire lifetime of the application, the `ThreadLocalMap` retains the object forever.
4. **Listeners and callbacks:** Registering an event listener on a long-lived singleton without unregistering it when the short-lived subscriber is destroyed.
5. **Custom map keys without `equals()`/`hashCode()`:** Repeatedly inserting new instances of a class that does not override `equals()` and `hashCode()` into a `HashMap` adds duplicate keys indefinitely instead of overwriting existing keys.

### Example

```java
import java.util.ArrayList;
import java.util.List;

public class Cache {
    private static final List<byte[]> DATA =
            new ArrayList<>();

    public static void addData() {
        DATA.add(new byte[1024 * 1024]); // Retains 1 MiB forever!
    }
}
```

### How to investigate in production

1. **Observe GC logs and metrics:** If post-GC Old Generation occupancy steadily rises over hours/days, you have a memory leak.
2. **Configure automatic heap dump on OOM:**
   ```text
   -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/var/log/app/heapdump.hprof
   ```
3. **Or capture a live heap histogram / dump via `jcmd`:**
   ```bash
   jcmd <pid> GC.class_histogram
   jcmd <pid> GC.heap_dump /tmp/live_dump.hprof
   ```
4. **Analyze in Eclipse MAT (Memory Analyzer Tool) or JProfiler:** Inspect the **Dominator Tree** (objects retaining the largest Shallow vs Retained Heap) and trace the **Path to GC Roots** to pinpoint the exact field holding the leaked references.

---

## Scenario 26: A Java application uses too many threads

**Interview question:** Your server creates a new thread for every incoming request (`new Thread(...).start()`). Under heavy load, performance collapses. Why?

### Answer

Classic Java platform threads (`java.lang.Thread`) are thin wrappers around **1:1 OS kernel threads**. Creating unbounded platform threads on every request causes severe degradation:

- **Native Memory Exhaustion:** Each platform thread allocates its own native stack (default `1 MB` on 64-bit Linux/Windows JVMs). Creating 5,000 threads consumes ~5 GB of off-heap native RAM, triggering `java.lang.OutOfMemoryError: unable to create native thread`.
- **OS Kernel Context-Switching Thrashing:** When thousands of runnable threads compete for 8 or 16 CPU cores, the OS scheduler spends more CPU cycles saving/restoring registers and invalidating L1/L2 CPU caches than executing application code.
- **Downstream Resource Exhaustion:** 5,000 concurrent threads opening 5,000 simultaneous database queries overwhelm the database and exhaust connection pools.

### Solution 1: Bounded `ThreadPoolExecutor` (Platform Threads)

In production, avoid `Executors.newFixedThreadPool()` if the queue can grow without bound (it uses an unbounded `LinkedBlockingQueue(Integer.MAX_VALUE)`, which can cause an OOM under sustained backpressure). Instead, construct a `ThreadPoolExecutor` with an **explicitly bounded queue** and rejection policy:

```java
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) {
        ExecutorService executor = new ThreadPoolExecutor(
                8,                                    // Core threads
                16,                                   // Max threads
                60L, TimeUnit.SECONDS,                // Idle keep-alive
                new ArrayBlockingQueue<>(500),        // Bounded queue!
                new ThreadPoolExecutor.AbortPolicy()  // Fail fast when overloaded
        );

        try {
            for (int i = 0; i < 20; i++) {
                int taskId = i;
                executor.submit(() ->
                        System.out.println("Processing task " + taskId)
                );
            }
        } finally {
            executor.shutdown();
        }
    }
}
```

### Solution 2: Java 21+ Virtual Threads (`Executors.newVirtualThreadPerTaskExecutor()`) + Semaphore Rate Limiting

In Java 21+ (Project Loom), **Virtual Threads** are lightweight user-mode threads managed by the JVM (~few hundred bytes of heap instead of a 1 MB OS stack). When a virtual thread blocks on network or database I/O, the JVM unmounts it from the carrier OS thread automatically:

```java
try (ExecutorService executor =
             Executors.newVirtualThreadPerTaskExecutor()) {
    // Can safely handle tens of thousands of concurrent I/O-bound tasks!
    // Still use a Semaphore or connection pool to cap concurrent calls to downstream DBs.
}
```

---

# Part 11: Coding Interview Scenarios

## Scenario 27: Find duplicate numbers in an array

**Question:** Given an integer array, identify duplicate values in the order they are first detected as duplicates.

Example:

```text
Input:  [1, 2, 3, 2, 4, 1, 5]
Output: [2, 1]
```

### Solution

```java
import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.Set;

public class Main {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 2, 4, 1, 5};

        Set<Integer> seen = new HashSet<>();
        Set<Integer> duplicates = new LinkedHashSet<>();

        for (int number : numbers) {
            if (!seen.add(number)) {
                duplicates.add(number);
            }
        }

        System.out.println(duplicates);
    }
}
```

Output:

```text
[2, 1]
```

### Complexity

- **Expected Time Complexity:** `O(n)` single pass through the array.
- **Auxiliary Space Complexity:** `O(n)` to store seen and duplicate elements.

`HashSet.add(element)` returns `false` if the element is already present in the set, allowing membership check and insertion in a single hash table operation rather than calling `seen.contains(number)` followed by `seen.add(number)`. `LinkedHashSet` deduplicates numbers that appear 3+ times while preserving detection order.

**Follow-up:** How would you find duplicates in a 1-billion-record dataset when you only have 2 GB of RAM?

Storing 1 billion `Integer` objects in a Java `HashSet` requires ~32–36 bytes per entry (>32 GB RAM), causing an immediate `OutOfMemoryError`. Depending on the data constraints:
1. **If values are bounded 32-bit integers (e.g., `0` to `2^31 - 1`):** Use two **BitSets** (`java.util.BitSet` or a raw `long[]` bitmap) tracking `seenOnce` and `seenMultiple`. $2^{31}$ bits requires only **256 MB** of RAM (`256 MB x 2 = 512 MB`), solving the entire problem in memory in `O(n)` time!
2. **If values are arbitrary 64-bit IDs or strings:** Use **Hash Partitioning** on disk: stream the file, compute `bucket = Math.floorMod(val.hashCode(), 256)`, and append each record to one of 256 temporary partition files on disk. Identical values are guaranteed to land in the exact same partition file. Then process each small partition file in memory one at a time using a `HashSet`.

---

## Scenario 28: Find the first non-repeating character

**Question:** Given a string, return the first character that appears exactly once.

Example:

```text
Input:  "swiss"
Output: "w"
```

### Solution 1: `LinkedHashMap` (General Purpose)

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        String input = "swiss";

        Map<Character, Integer> frequency =
                new LinkedHashMap<>();

        for (char c : input.toCharArray()) {
            frequency.put(
                    c,
                    frequency.getOrDefault(c, 0) + 1
            );
        }

        for (Map.Entry<Character, Integer> entry
                : frequency.entrySet()) {

            if (entry.getValue() == 1) {
                System.out.println(entry.getKey());
                return;
            }
        }

        System.out.println("No unique character");
    }
}
```

Output:

```text
w
```

### Solution 2: Full Unicode Code-Point Safe Implementation (Emoji / Supplementary Character Safe)

Java's primitive `char` is a 16-bit UTF-16 code unit. Characters outside the Basic Multilingual Plane (such as emojis like `🚀` or certain CJK ideographs) are represented as a **surrogate pair** of two `char` values. Iterating with `String.codePoints()` handles all Unicode characters accurately:

```java
import java.util.LinkedHashMap;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class UnicodeSafeFirstUnique {
    public static String firstUniqueCharacter(String input) {
        Map<Integer, Long> counts = input.codePoints()
                .boxed()
                .collect(Collectors.groupingBy(
                        Function.identity(),
                        LinkedHashMap::new,
                        Collectors.counting()
                ));

        return counts.entrySet().stream()
                .filter(e -> e.getValue() == 1L)
                .map(e -> Character.toString(e.getKey()))
                .findFirst()
                .orElse(null);
    }
}
```

### Complexity

- **Time Complexity:** `O(n)` where `n` is the length of the string.
- **Space Complexity:** `O(k)` where `k` is the number of distinct characters/code points in the alphabet.

---

## Scenario 29: Merge two sorted arrays

**Question:** Given two sorted arrays, merge them into one sorted array.

```text
Input:
[1, 3, 5]
[2, 4, 6]

Output:
[1, 2, 3, 4, 5, 6]
```

### Solution

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] a = {1, 3, 5};
        int[] b = {2, 4, 6};

        int[] result = new int[a.length + b.length];

        int i = 0;
        int j = 0;
        int k = 0;

        while (i < a.length && j < b.length) {
            if (a[i] <= b[j]) {
                result[k++] = a[i++];
            } else {
                result[k++] = b[j++];
            }
        }

        while (i < a.length) {
            result[k++] = a[i++];
        }

        while (j < b.length) {
            result[k++] = b[j++];
        }

        System.out.println(Arrays.toString(result));
    }
}
```

### Complexity

- **Time Complexity:** `O(n + m)` single linear traversal across both arrays.
- **Additional Space Complexity:** `O(n + m)` for the merged output array.

*(Note: Using `a[i] <= b[j]` instead of `<` preserves relative ordering of equal elements from `a` before `b`, which makes the merge step **stable** in Merge Sort.)*

**Follow-up:** How would you merge $K$ sorted files that are each 50 GB on disk?

Use **$K$-Way External Merge with a Min-Heap (`PriorityQueue`)**:
1. Open a buffered stream (`BufferedReader`) to each of the $K$ sorted files.
2. Create a `PriorityQueue<FileEntry>` of size $K$, where `FileEntry` holds `(currentValue, fileIndex)` ordered by `currentValue`.
3. Read the first line from each of the $K$ files and insert it into the min-heap.
4. Repeatedly `poll()` the smallest `FileEntry` from the min-heap, write its value to the output stream, and read the next line from that same `fileIndex` into the min-heap until all files reach `EOF`.
5. Memory usage is `O(K)` (just $K$ items in the heap at any time), and time complexity is `O(N log K)` across $N$ total records!

---

## Scenario 30: Design a rate limiter for a REST API

**Question:** Your application receives excessive requests from a single client. How would you limit requests to protect the service?

### Answer

A rate limiter controls how many requests a caller can make within a defined window to prevent abuse, resource starvation, and noisy-neighbor cascading failures.

### Comparison of Rate Limiting Algorithms

| Algorithm | How it works | Pros | Cons |
|---|---|---|---|
| **Fixed Window Counter** | Counts requests in fixed discrete windows (e.g., `12:00:00–12:00:59`). | Simple (`INCR` + `EXPIRE`), tiny memory footprint. | Boundary burst problem: a client sending 100 requests at `12:00:59` and 100 at `12:01:00` gets 200 requests in 2 seconds. |
| **Sliding Window Log** | Stores every request timestamp in a sorted set (`ZSET`) and removes entries older than `now - window`. | 100% accurate across any rolling interval. | High memory usage for high-volume limits. |
| **Sliding Window Counter** | Weighted average of previous fixed window count and current window count. | Smooths boundary bursts with `O(1)` memory. | Approximation (assumes uniform distribution in prior window). |
| **Token Bucket** | Bucket holds up to $B$ tokens and refills at $R$ tokens/second; each request consumes 1 token. | Allows controlled short bursts up to $B$ while enforcing long-term average rate $R$. | Requires atomic read-refill-decrement state. |
| **Leaky Bucket** | Requests enter a bounded FIFO queue and are processed at a constant rate. | Smooths traffic spikes into a flat outflow rate. | Adds queueing latency for bursts. |

### Thread-Safe In-Memory Token Bucket in Java (Single Node)

```java
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.atomic.AtomicReference;

public class TokenBucketRateLimiter {

    private record BucketState(double tokens, long lastRefillNanos) {}

    private final double capacity;
    private final double refillTokensPerNano;
    private final ConcurrentHashMap<String, AtomicReference<BucketState>> buckets =
            new ConcurrentHashMap<>();

    public TokenBucketRateLimiter(long capacity, long refillTokensPerSecond) {
        this.capacity = capacity;
        this.refillTokensPerNano = refillTokensPerSecond / 1_000_000_000.0;
    }

    public boolean tryAcquire(String clientId) {
        AtomicReference<BucketState> ref = buckets.computeIfAbsent(
                clientId,
                k -> new AtomicReference<>(new BucketState(capacity, System.nanoTime()))
        );

        while (true) {
            BucketState current = ref.get();
            long now = System.nanoTime();
            long elapsedNanos = Math.max(0, now - current.lastRefillNanos());
            double available = Math.min(
                    capacity,
                    current.tokens() + elapsedNanos * refillTokensPerNano
            );

            if (available < 1.0) {
                return false; // Rate limit exceeded (HTTP 429)
            }

            BucketState next = new BucketState(available - 1.0, now);
            if (ref.compareAndSet(current, next)) {
                return true;
            }
        }
    }
}
```

### Distributed Rate Limiting Across Multiple Instances

In a horizontally scaled Kubernetes deployment with 20 replicas, a local in-memory limiter allows $20 \times$ the intended limit unless state is shared.
- Use **Redis with a Lua script** (or libraries like **Bucket4j** with Redis/Redisson) so reading the bucket timestamp, calculating refilled tokens, and decrementing the token count execute **atomically** in a single Redis round-trip without race conditions.
- Return standard HTTP headers with `429 Too Many Requests`:
  - `X-RateLimit-Limit`
  - `X-RateLimit-Remaining`
  - `Retry-After`

**Follow-up:** What happens if Redis becomes unavailable?

Define an explicit **Fail-Open vs Fail-Closed** strategy accompanied by a circuit breaker:
- **Fail-Open with Local In-Memory Fallback (Recommended for most APIs):** If Redis times out (>15 ms), trip a circuit breaker and fall back to a local in-memory rate limiter (e.g., using Caffeine/Bucket4j local buckets divided by estimated cluster size) so a Redis outage does not take down all production traffic.
- **Fail-Closed:** Reject requests only for high-risk security endpoints (e.g., `/login` or `/otp/verify` where brute-force protection is mandatory).

---

# Part 12: Deep-Dive Continuation: Advanced OOP & Collections

## Scenario 31: Comparing two `Integer` wrapper objects with `==` works for `100` but fails for `200`

**Interview question:** In a staging test with small order quantities, `orderA.getQuantity() == orderB.getQuantity()` returned `true`. In production with quantity `200`, it returned `false`. Both return `Integer`. Why?

```java
Integer a = 100;
Integer b = 100;
System.out.println(a == b); // true

Integer x = 200;
Integer y = 200;
System.out.println(x == y); // false
```

### Answer

Autoboxing (`Integer a = 100;`) compiles to `Integer.valueOf(100)`.

Per the Java Language Specification (JLS §5.1.7), `Integer.valueOf(int)` caches `Integer` instances in an internal `IntegerCache` array for values in the range **`-128` to `127`** by default:
- For `100`, both `a` and `b` receive references to the exact same cached `Integer` object from `IntegerCache.cache`, so `a == b` (reference identity) accidentally returns `true`.
- For `200`, `Integer.valueOf(200)` allocates a new `Integer` object on the heap each time, so `x == y` returns `false`.

**Fix:** Always compare wrapper objects (`Integer`, `Long`, `Boolean`, `Double`) using `Objects.equals(x, y)` or `.intValue()`, or use primitive types (`int`, `long`) in domain objects when `null` is not needed.

---

## Scenario 32: Financial calculations lose cents when using `double` or `new BigDecimal(double)`

**Interview question:** An invoicing service calculates `0.1 + 0.2` or initializes `new BigDecimal(0.1)` and produces unexpected extra decimal places (`0.30000000000000004`). How do you fix financial calculations in Java?

### Answer

IEEE 754 binary floating-point types (`float` and `double`) cannot represent base-10 fractions like `0.1` exactly in binary (just as `1/3` cannot be represented finitely in base-10).

Even worse, calling `new BigDecimal(0.1)` passes the already-imprecise binary `double` (`0.1000000000000000055511151231257827021181583404541015625`) directly into `BigDecimal`!

### Correct Solutions

1. **Always construct `BigDecimal` from a `String` or `BigDecimal.valueOf(double)`:**
   ```java
   import java.math.BigDecimal;
   import java.math.RoundingMode;

   BigDecimal price = new BigDecimal("0.10");
   BigDecimal tax   = new BigDecimal("0.20");
   BigDecimal total = price.add(tax); // Exactly 0.30
   ```
2. **Always specify a scale and `RoundingMode` when dividing:** Calling `a.divide(b)` without a `RoundingMode` throws `ArithmeticException: Non-terminating decimal expansion` when the result is a repeating decimal (e.g., `1 / 3`):
   ```java
   BigDecimal split = new BigDecimal("100.00")
           .divide(new BigDecimal("3"), 2, RoundingMode.HALF_EVEN);
   ```
3. **Never use `BigDecimal.equals()` to compare numerical values without normalizing scale:**
   `new BigDecimal("10.0").equals(new BigDecimal("10.00"))` returns **`false`** because `equals()` checks both value and `scale` (`1 != 2`). Use `a.compareTo(b) == 0` for numeric equality, or store monetary values as `long` minor units (cents/paise).

---

## Scenario 33: `HashMap` latency spikes under hash collision attacks

**Interview question:** How does `HashMap` handle hash collisions internally in modern Java, and what happens if 10,000 keys return the exact same `hashCode()`?

### Answer

1. **Index calculation & hash perturbation:** `HashMap` computes `hash = (h = key.hashCode()) ^ (h >>> 16)` to mix the upper 16 bits into the lower bits (preventing collisions when the table size is small), and finds the bucket index via `(n - 1) & hash` (fast bitwise modulo since capacity `n` is always a power of 2).
2. **Chaining to Red-Black Tree (`TREEIFY_THRESHOLD = 8`):**
   - Initially, colliding entries in a bucket form a singly linked list (`O(n)` lookup).
   - When a bucket's list length reaches **8** AND the total table capacity is at least **`MIN_TREEIFY_CAPACITY = 64`** (otherwise the table simply resizes/doubles), `HashMap` converts that bucket from a linked list into a balanced **Red-Black Tree (`TreeNode`)**, improving worst-case lookup from `O(n)` to **`O(log n)`**.
   - If entries are removed and the bucket size drops to **`UNTREEIFY_THRESHOLD = 6`**, it converts back to a linked list.
3. **Why custom keys should implement `Comparable`:** When two distinct keys have the exact same `hashCode()`, the Red-Black Tree needs a deterministic ordering to branch left or right. If the key class implements `Comparable`, `TreeNode` uses `compareTo()` for `O(log n)` ordering.

---

## Scenario 34: Race conditions when using `HashMap` or `ConcurrentHashMap` across threads

**Interview question:** A developer replaces `HashMap` with `ConcurrentHashMap`, but the following code still suffers from a race condition. Why?

```java
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();

// Executed concurrently by multiple threads:
Integer current = map.get("views");
if (current == null) {
    map.put("views", 1);
} else {
    map.put("views", current + 1);
}
```

### Answer

Although `ConcurrentHashMap.get()` and `ConcurrentHashMap.put()` are individually thread-safe, combining them in an external **check-then-act** (`get` followed by `put`) sequence is **not atomic**. Two threads can both call `map.get("views")`, read the same value, and overwrite each other's update.

### Correct Solution: Use Atomic Compound Operations

Use `compute()`, `merge()`, or `computeIfAbsent()` which execute atomically inside the `ConcurrentHashMap` bucket lock:

```java
// Atomic increment using merge():
map.merge("views", 1, Integer::sum);

// Or using compute():
map.compute("views", (key, val) -> (val == null) ? 1 : val + 1);
```

**Important `ConcurrentHashMap` Pitfall in Spring Boot:** Never call `concurrentMap.computeIfAbsent(key, k -> loadFromSameMap(k))` recursively for a key that hashes to the same bucket, as `ConcurrentHashMap` throws `IllegalStateException: Recursive update` (or deadlocks if blocking external threads that try to mutate the same bucket).

---

## Scenario 35: `TreeMap` or `TreeSet` silently drops elements

**Interview question:** You insert two distinct `Product` objects with different IDs but the same price into a `TreeSet` configured with `Comparator.comparing(Product::price)`. Why does the set contain only one product?

```java
record Product(long id, String name, BigDecimal price) {}

Set<Product> products = new TreeSet<>(
        Comparator.comparing(Product::price)
);

products.add(new Product(1L, "Keyboard", new BigDecimal("50.00")));
products.add(new Product(2L, "Mouse",    new BigDecimal("50.00")));

System.out.println(products.size()); // Prints 1!
```

### Answer

Unlike `HashSet` (which uses `hashCode()` and `equals()`), **`TreeSet` and `TreeMap` determine uniqueness exclusively using `compareTo()` or `Comparator.compare()`**—they never call `equals()`!

Because both products have a price of `50.00`, `comparator.compare(p1, p2)` returns `0`. `TreeSet` treats any two elements where `compare() == 0` as duplicate keys and ignores the second insertion.

**Fix:** Always include a unique tie-breaker field (such as `Product::id`) in your `Comparator` so `compare()` returns `0` if and only if `equals()` is `true`:

```java
Set<Product> products = new TreeSet<>(
        Comparator.comparing(Product::price)
                  .thenComparingLong(Product::id)
);
```

---

## Scenario 36: Constructor calls an overridable method and observes `null` in a `final` field

**Interview question:** Why does the following program print `null` even though `name` is initialized in the subclass?

```java
abstract class BaseService {
    BaseService() {
        init(); // Calling an overridable method inside a constructor!
    }

    abstract void init();
}

class OrderService extends BaseService {
    private final String serviceName = "ORDER_SERVICE";

    @Override
    void init() {
        System.out.println("Starting: " + serviceName.toLowerCase());
    }
}
```

### Answer

This code throws a `NullPointerException` when `new OrderService()` is invoked!

In Java's object initialization order:
1. The superclass constructor (`BaseService()`) runs **before** subclass field initializers (`serviceName = "ORDER_SERVICE"`) and the subclass constructor body.
2. When `BaseService()` calls the overridden `init()` method, dynamic dispatch invokes `OrderService.init()` while `OrderService.serviceName` is still at its default uninitialized value (`null`).

**Rule (Effective Java Item 19):** Constructors must **never** invoke overridable instance methods (directly or indirectly). Only invoke `private`, `final`, or `static` methods from a constructor.

---

# Part 13: Deep-Dive Continuation: JVM Internals & Concurrency

## Scenario 37: `NoSuchMethodError` or `ClassCastException` in production despite compiling cleanly

**Interview question:** Your project compiles cleanly in CI, but fails at runtime with `java.lang.NoSuchMethodError` when calling a library method (e.g., Jackson, Guava, or SLF4J). Why does this happen, and how do you fix it?

### Answer

- **`ClassNotFoundException`** is a checked exception thrown when a dynamic call (`Class.forName(...)`) cannot find the class on the classpath.
- **`NoClassDefFoundError` / `NoSuchMethodError`** is a JVM linkage error thrown when a class was present at **compile time**, but a **different, incompatible version** of that class was loaded at **runtime** due to a transitive dependency conflict ("Classpath / Dependency Hell").

For example, Library A depends on `jackson-databind:2.17.0` and Library B pulls in an older `jackson-databind:2.11.0`. If the older JAR wins on the runtime classpath, any call to a method added in `2.15` throws `NoSuchMethodError`.

### How to investigate and fix

1. **Inspect the dependency tree:**
   ```bash
   mvn dependency:tree -Dverbose
   # or in Gradle:
   ./gradlew dependencies
   ```
2. **Log class loading at runtime:** Start the JVM with `-Xlog:class+load=info` (or inspect `Class.getProtectionDomain().getCodeSource().getLocation()`) to see which exact JAR file provided the conflicting class.
3. **Enforce version convergence:** Use the **Maven Enforcer Plugin** (`<dependencyConvergence/>` and `<requireUpperBoundDeps/>`) and manage versions centrally using a `<dependencyManagement>` BOM (Bill of Materials).

---

## Scenario 38: A background thread never stops when a boolean flag is set to `false`

**Interview question:** Why can the worker thread in this code run forever even after the main thread calls `stopWork()`?

```java
class Worker {
    private boolean running = true;

    void runLoop() {
        while (running) {
            // Do lightweight work without synchronization
        }
    }

    void stopWork() {
        running = false;
    }
}
```

### Answer

Without synchronization or the `volatile` keyword, the Java Memory Model (JMM) allows the JIT compiler (C2) to **hoist** the read of `running` out of the loop (transforming `while (running)` into `if (running) { while (true) { ... } }`) and allows CPU cores to read a stale value from their local L1/L2 cache instead of main memory.

### Fix 1: Declare the flag `volatile`

```java
private volatile boolean running = true;
```

In the Java Memory Model, a write to a `volatile` field **happens-before** every subsequent read of that same field. It disables JIT loop hoisting and inserts memory barriers so writes are immediately visible across all CPU cores.

### Fix 2: Use Cooperative Thread Interruption (`Thread.interrupt()`)

If the worker loop calls blocking methods like `Thread.sleep()`, `BlockingQueue.take()`, or `Future.get()`, a `volatile` boolean alone will not wake up the blocked thread until the blocking call returns. Always check `Thread.currentThread().isInterrupted()` and propagate/restore interrupts properly:

```java
while (running && !Thread.currentThread().isInterrupted()) {
    try {
        Task task = queue.take();
        process(task);
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt(); // Restore interrupt flag!
        break;
    }
}
```

---

## Scenario 39: User A sees User B's security context in a Spring web application (`ThreadLocal` leak)

**Interview question:** You store the current authenticated tenant ID in a custom `ThreadLocal<String>`. Under load, User A occasionally sees data belonging to User B. Why?

### Answer

Servlet containers like Tomcat reuse a pool of long-lived worker threads across thousands of incoming HTTP requests.

If Request 1 (for `"Tenant_B"`) executes on `http-nio-8080-exec-4`, sets `TenantContext.set("Tenant_B")`, and finishes **without calling `TenantContext.remove()`** (for example, because an exception was thrown mid-request), `http-nio-8080-exec-4` returns to the Tomcat pool still holding `"Tenant_B"` in its `ThreadLocalMap`. When Request 2 (for `"Tenant_A"`, or an unauthenticated endpoint) is assigned to `http-nio-8080-exec-4` and reads `TenantContext.get()`, it reads `"Tenant_B"`!

### Mandatory Pattern: Always Clear `ThreadLocal` in a `finally` Block

```java
public final class TenantContext {
    private static final ThreadLocal<String> CURRENT_TENANT = new ThreadLocal<>();

    public static void set(String tenantId) {
        CURRENT_TENANT.set(tenantId);
    }

    public static String get() {
        return CURRENT_TENANT.get();
    }

    public static void clear() {
        CURRENT_TENANT.remove();
    }
}
```

In your Servlet `Filter` or Spring `HandlerInterceptor`:

```java
try {
    TenantContext.set(resolveTenant(request));
    filterChain.doFilter(request, response);
} finally {
    TenantContext.clear(); // Guaranteed cleanup even if an exception occurs!
}
```

*(Note: In Java 21+, consider **Scoped Values** (`ScopedValue.where(TENANT, id).run(...)`), which are immutable and automatically bounded to the lexical execution scope of the call, making leaks impossible.)*

---

## Scenario 40: `CompletableFuture` tasks deadlock the entire thread pool

**Interview question:** You have a fixed thread pool of 4 threads. Inside an asynchronous task submitted to that pool, the code spawns a child task on the **same** pool and calls `.join()`. Under load, the application freezes completely. Why?

```java
ExecutorService pool = Executors.newFixedThreadPool(4);

// When 4 requests arrive simultaneously:
CompletableFuture.supplyAsync(() -> {
    // Parent task runs on pool thread, then submits child task to the SAME pool and blocks:
    String childResult = CompletableFuture.supplyAsync(() -> fetchSubData(), pool).join();
    return combine(childResult);
}, pool);
```

### Answer

This is **Thread Pool Induced Deadlock (Thread Starvation Deadlock)**:
1. All 4 threads in `pool` pick up 4 parent tasks concurrently.
2. Each of the 4 parent tasks enqueues a child task into `pool`'s queue and blocks waiting on `.join()`.
3. The 4 child tasks are sitting in the queue waiting for a thread in `pool` to become free—but all 4 threads in `pool` are blocked inside `.join()` waiting for those exact child tasks to finish!

### How to prevent it

1. **Never block synchronously (`.join()` / `.get()`) inside an async pipeline on the same bounded thread pool.** Instead, compose stages non-blockingly using **`thenCompose()`** or **`thenCombine()`**:
   ```java
   CompletableFuture<String> result = CompletableFuture
           .supplyAsync(() -> prepareContext(), pool)
           .thenComposeAsync(ctx -> fetchSubDataAsync(ctx, pool), pool)
           .thenApply(this::combine);
   ```
2. **Isolate thread pools by layer (Bulkhead Pattern):** Never share the same bounded executor between parent orchestrators and child I/O workers, and never block the shared `ForkJoinPool.commonPool()` with slow I/O calls.

---

## Scenario 41: Java 21 Virtual Threads stall due to Carrier Thread Pinning

**Interview question:** You migrate a Spring Boot 3.2+ service to Java 21 Virtual Threads (`spring.threads.virtual.enabled=true`), but throughput degrades sharply under load. What is **carrier thread pinning**, and how do you fix it?

### Answer

Virtual threads achieve massive scalability by **unmounting** from their underlying OS carrier thread (`ForkJoinPool`) whenever they block on I/O, freeing the carrier thread to run other virtual threads.

However, in Java 21–23, a virtual thread **cannot unmount** (it is **pinned** to its OS carrier thread) in two situations:
1. It executes a blocking operation inside a **`synchronized` block or method** (e.g., an older JDBC driver or caching library holding `synchronized (this)` while doing socket I/O).
2. It blocks while a **native method / JNI frame** is on the call stack.

When all carrier threads (typically equal to the number of CPU cores, e.g., 8) become pinned by virtual threads blocked inside `synchronized` I/O, no other virtual threads can be scheduled!

*(Note: JEP 491 in Java 24 resolves `synchronized` pinning in the JVM, but on Java 21 LTS it remains a critical production consideration.)*

### How to detect and fix on Java 21 LTS

1. **Detect pinning:** Run with `-Djdk.tracePinnedThreads=short` or inspect JFR event `jdk.VirtualThreadPinned`.
2. **Replace `synchronized` around I/O with `ReentrantLock`:** Virtual threads unmount cleanly when blocking inside `ReentrantLock.lock()`:
   ```java
   private final ReentrantLock lock = new ReentrantLock();

   public Data fetchExclusively() {
       lock.lock();
       try {
           return performBlockingNetworkCall(); // Unmounts cleanly from carrier thread!
       } finally {
           lock.unlock();
       }
   }
   ```

---

## Scenario 42: High GC Pause Times and Allocation Rate Spikes in a Low-Latency Service

**Interview question:** Your Java microservice has a p99 latency SLA of 50 ms, but every few minutes p99 spikes to 600 ms. Logs show long Stop-The-World (STW) garbage collection pauses. How do you diagnose and tune this?

### Answer

1. **Enable Unified GC Logging:**
   ```text
   -Xlog:gc*,gc+phases=debug:file=/var/log/app/gc.log:time,uptime,level,tags:filecount=5,filesize=20M
   ```
2. **Distinguish High Allocation Rate vs Humongous Allocations vs Premature Promotion:**
   - **Humongous Allocations (G1GC):** In G1GC, any single object allocation larger than **50% of a G1 Region size** (e.g., allocating a 6 MB `byte[]` or huge JSON string when region size is 8 MB) is allocated directly in Old Generation as a *Humongous Object*, triggering frequent concurrent marking and Full GCs. Fix by streaming large payloads or increasing `-XX:G1HeapRegionSize=16m`.
   - **High Allocation Rate:** Inspect JFR (`jdk.ObjectAllocationInNewTLAB`) to find hot loops allocating millions of temporary objects per second (e.g., `ObjectMapper` instantiated inside a loop instead of reused as a singleton, or regex `Pattern.compile()` on every request).
3. **Choose the right Garbage Collector for the SLA:**
   - **G1GC (Default in Java 9–21):** Balanced throughput and latency (`-XX:MaxGCPauseMillis=100`).
   - **Generational ZGC (`-XX:+UseZGC -XX:+ZGenerational` in Java 21+):** Sub-millisecond Stop-The-World pauses (`< 1 ms`) regardless of heap size (from gigabytes to terabytes), ideal for strict p99 latency SLAs.

---

# Part 14: Deep-Dive Continuation: Databases, Spring Boot & Microservices

## Scenario 43: `SQLTransientConnectionException: HikariPool-1 - Connection is not available, request timed out after 30000ms`

**Interview question:** In production, your Spring Boot service starts throwing HikariCP connection timeout exceptions (`request timed out after 30000ms`). Your DBA says the database CPU is only at 15%. Why is the connection pool exhausted?

### Answer

HikariCP pool exhaustion happens when application threads **acquire database connections and hold them for too long** (or leak them), starving incoming requests.

### Top 4 Production Root Causes

1. **External HTTP/RPC calls inside `@Transactional` methods:**
   ```java
   @Transactional // BAD: Holds a DB connection across a slow 3-second HTTP call!
   public void placeOrder(OrderRequest req) {
       orderRepository.save(order);
       paymentGatewayClient.charge(req); // If this takes 3s, 10 req/s exhausts a 20-conn pool!
   }
   ```
2. **`spring.jpa.open-in-view=true` (OSIV enabled by default in Spring Boot):**
   When `open-in-view` is `true`, Spring keeps the Hibernate session and database connection open for the **entire HTTP request lifecycle** (including controller logic and JSON view rendering) instead of releasing the connection as soon as the `@Transactional` repository method finishes. Always set:
   ```properties
   spring.jpa.open-in-view=false
   ```
3. **Database Lock Wait:** One uncommitted transaction holds a row lock (`UPDATE`), and 20 application threads block on `UPDATE` waiting for that lock, consuming all 20 HikariCP connections.
4. **Connection Leak:** Custom code calls `dataSource.getConnection()` directly without closing it in a `try-with-resources` block. Enable leak detection in HikariCP to log the exact stack trace where the connection was borrowed:
   ```properties
   spring.datasource.hikari.leak-detection-threshold=5000
   ```

---

## Scenario 44: Hibernate N+1 Select Problem Degrades an API Endpoint

**Interview question:** Fetching a list of 100 `Order` entities along with each order's `Customer` executes 101 SQL queries (`1` query for the 100 orders + `100` separate queries for each customer). How do you detect and fix the N+1 query problem in Spring Data JPA?

### Answer

The N+1 problem occurs when a parent query loads $N$ entities with a lazy association (`@ManyToOne(fetch = FetchType.LAZY)` or `@OneToMany`), and iterating over the results in Java triggers a separate `SELECT` statement for each of the $N$ parent rows.

### Solutions

1. **JPQL `JOIN FETCH`:** Fetch the association in a single SQL `JOIN` query when needed by that specific use case:
   ```java
   @Query("SELECT o FROM Order o JOIN FETCH o.customer WHERE o.status = :status")
   List<Order> findOrdersWithCustomerByStatus(@Param("status") OrderStatus status);
   ```
2. **`@EntityGraph`:** Declaratively specify which associations to fetch eagerly for a specific repository method without writing custom JPQL:
   ```java
   @EntityGraph(attributePaths = {"customer"})
   List<Order> findByStatus(OrderStatus status);
   ```
3. **DTO Projections (Best for Read-Only Endpoints):** Select only the required columns directly into a Java `record` DTO, bypassing Hibernate entity state tracking altogether:
   ```java
   @Query("""
          SELECT new com.example.OrderSummaryDto(o.id, o.totalAmount, c.name)
          FROM Order o JOIN o.customer c
          WHERE o.status = :status
          """)
   List<OrderSummaryDto> findOrderSummaries(@Param("status") OrderStatus status);
   ```

*(Note: Be careful when combining `JOIN FETCH` on a `@OneToMany` collection with pagination `Pageable`! Hibernate cannot apply SQL `LIMIT` accurately to a joined one-to-many result set and will load the entire table into memory with warning `HHH90003004: firstResult/maxResults specified with collection fetch; applying in memory`. Instead, paginate parent IDs first or use `@BatchSize(size = 50)`.)*

---

## Scenario 45: `@Transactional` or `@Async` Annotation is Completely Ignored

**Interview question:** You annotate a helper method with `@Transactional(propagation = Propagation.REQUIRES_NEW)` (or `@Async` / `@Cacheable`) and call it from another method inside the **same class**. The transaction never starts (or rolls back). Why?

```java
@Service
public class InvoiceService {

    public void generateBatch(List<Long> ids) {
        for (Long id : ids) {
            processSingleInvoice(id); // Direct self-invocation! Proxy is bypassed!
        }
    }

    @Transactional
    public void processSingleInvoice(Long id) {
        // ...
    }
}
```

### Answer

Spring implements declarative annotations (`@Transactional`, `@Async`, `@Cacheable`, `@Retryable`) using **Runtime AOP Proxies** (CGLIB subclass proxies or JDK Dynamic Proxies).

```text
External Caller ──> [ Spring AOP Proxy ] ──> (Starts Tx) ──> [ Target InvoiceService ]
                                                                     │
                                            this.processSingle() <───┘
                                       (Bypasses Proxy! No Tx Interceptor!)
```

1. When an **external bean** calls `invoiceService.processSingleInvoice(id)`, the call goes through the **Spring Proxy**, which intercepts the call, opens the database transaction, invokes the target method, and commits/rolls back on return.
2. When `generateBatch()` inside `InvoiceService` calls `this.processSingleInvoice(id)` directly, `this` refers to the **raw target instance**, completely bypassing the outer AOP proxy!
3. Additionally, `@Transactional` methods must be **`public`** (or package-protected/protected in newer Spring versions only if subclass proxying can intercept them; `private` methods can never be overridden by a CGLIB subclass proxy).

### Solutions

1. **Extract the transactional method into a separate focused component** (e.g., `InvoiceProcessor`) and inject it via constructor injection (Best practice—aligns with Single Responsibility).
2. **Use `TransactionTemplate`** programmatically when fine-grained transaction boundaries are needed inside a loop.

---

## Scenario 46: `@Transactional` Does Not Roll Back When a Checked Exception Is Thrown

**Interview question:** Your `@Transactional` method updates two tables and then throws a custom checked exception `InsufficientFundsException extends Exception`. The first table update is committed to the database instead of rolling back! Why?

### Answer

By default, Spring's `@Transactional` only rolls back automatically on **unchecked exceptions** (`RuntimeException` and `Error`), following legacy EJB conventions. When a **checked exception** (`extends Exception`, not `RuntimeException`) is thrown, Spring commits the transaction by default!

**Fix:** Always specify `rollbackFor = Exception.class` (or make domain exceptions extend `RuntimeException`):

```java
@Transactional(rollbackFor = Exception.class)
public void transferFunds(TransferRequest request) throws InsufficientFundsException {
    debit(request.fromAccount(), request.amount());
    credit(request.toAccount(), request.amount());
}
```

---

## Scenario 47: Spring `@Service` or `@RestController` Corrupts Data Across Concurrent Requests

**Interview question:** A developer adds a field `private BigDecimal currentDiscount;` inside a `@Service` class to share data between helper methods during a request. In production, customers receive each other's discounts. Why?

### Answer

By default, every Spring `@Service`, `@RestController`, `@Repository`, and `@Component` bean has **Singleton scope (`@Scope("singleton")`)**—Spring instantiates **only one shared instance** per `ApplicationContext`, and all concurrent HTTP request threads execute methods on that exact same object instance simultaneously.

Storing per-request mutable state in an instance field of a Singleton bean creates an immediate **race condition**.

**Fix:** Keep all Spring Singleton beans **stateless** (only `private final` injected dependencies and immutable constants as fields). Pass request-specific state as method parameters or inside an immutable request context object on the thread stack.

---

## Scenario 48: Authenticated User Accesses Another Customer's Order by Changing the ID in the URL (BOLA / IDOR)

**Interview question:** A user logs in validly, obtains a valid JWT token, and calls `GET /api/orders/10052` (which belongs to another customer). The endpoint returns the order details! How do you prevent this vulnerability?

### Answer

This is **Broken Object Level Authorization (BOLA)**, also known as **Insecure Direct Object Reference (IDOR)**—ranked #1 on the OWASP API Security Top 10.

**Authentication (`401`)** only proves *who* the caller is; **Authorization (`403` / `404`)** must verify that the authenticated caller actually owns or has permission to access the specific resource ID requested.

### Vulnerable Code

```java
@GetMapping("/orders/{orderId}")
public OrderDto getOrder(@PathVariable Long orderId) {
    // BUG: Never checks if orderId belongs to the authenticated user!
    return orderRepository.findById(orderId)
            .map(OrderDto::from)
            .orElseThrow(() -> new ResponseStatusException(HttpStatus.NOT_FOUND));
}
```

### Secure Solution: Scope Every Query by the Authenticated Principal

Extract the authenticated `userId` from the verified `SecurityContext` (`@AuthenticationPrincipal`), **never** from a client-supplied header or query parameter, and include it in the database query:

```java
@GetMapping("/orders/{orderId}")
public OrderDto getOrder(
        @PathVariable Long orderId,
        @AuthenticationPrincipal AuthenticatedUser currentUser) {

    return orderRepository.findByIdAndCustomerId(orderId, currentUser.id())
            .map(OrderDto::from)
            .orElseThrow(() -> new ResponseStatusException(
                    HttpStatus.NOT_FOUND, "Order not found"
            ));
}
```

Returning `404 Not Found` (rather than `403 Forbidden`) when `findByIdAndCustomerId` returns empty also prevents attackers from enumerating which order IDs exist in the database.

---

## Scenario 49: Database Commit Succeeds, but Publishing to Kafka Fails (Dual-Write Problem)

**Interview question:** Your `OrderService` saves a new order in PostgreSQL and then publishes an `OrderCreatedEvent` to Kafka. What goes wrong if the JVM crashes or Kafka is unreachable right after the database commit, and how do you solve it?

```java
@Transactional
public void createOrder(Order order) {
    orderRepository.save(order);
    kafkaTemplate.send("orders-topic", new OrderCreatedEvent(order.getId()));
}
```

### Answer

This is the **Dual-Write Anti-Pattern**:
- **Failure Case 1:** The database transaction commits, and 1 ms later the pod crashes or Kafka times out before `kafkaTemplate.send()` completes. The order exists in the database, but downstream inventory/shipping services never receive the `OrderCreatedEvent`.
- **Failure Case 2:** `kafkaTemplate.send()` sends the message over the network *before* the `@Transactional` method finishes, and then the database commit fails (e.g., deferred constraint violation or connection drop). Downstream services process an event for an order that rolled back!

### Solution: The Transactional Outbox Pattern

Write both the `Order` row and an `OutboxEvent` row into the **same relational database inside the same atomic local transaction**:

```java
@Transactional(rollbackFor = Exception.class)
public void createOrder(Order order) {
    orderRepository.save(order);

    OutboxEvent event = new OutboxEvent(
            UUID.randomUUID(),
            "ORDER",
            order.getId().toString(),
            "OrderCreatedEvent",
            toJson(order)
    );
    outboxRepository.save(event); // Commits atomically with the Order!
}
```

Then, an asynchronous **Outbox Relay** (either a Change Data Capture tool like **Debezium** reading the PostgreSQL WAL log, or a polling publisher using `SELECT ... FOR UPDATE SKIP LOCKED`) reads uncommitted outbox rows, publishes them to Kafka (**at-least-once delivery**), and marks them `PROCESSED`. Downstream Kafka consumers must be **idempotent** (tracking processed `eventId`s).

---

## Scenario 50: Kafka Consumer Group Continuously Rebalances and Reprocesses Messages

**Interview question:** Your Spring Kafka consumer processes messages that call a slow external API. Under heavy traffic, the consumer group enters a continuous **rebalance storm** and processes the same batch of records repeatedly without committing offsets. Why?

### Answer

Kafka's consumer group coordinator tracks consumer liveness using two separate mechanisms:
1. **Heartbeat thread (`session.timeout.ms`, default 45s):** Background thread proving the JVM is alive.
2. **Polling interval (`max.poll.interval.ms`, default 5 minutes):** Maximum time allowed between successive calls to `consumer.poll()`.

If `max.poll.records` is set to `500` (the default) and each message takes `1 second` to process synchronously, processing one batch takes `500 seconds` (> `300 seconds` `max.poll.interval.ms`). The broker assumes the consumer is stuck, **evicts it from the consumer group**, triggers a partition rebalance, and reassigns the exact same uncommitted 500 messages to another consumer—which also takes 500 seconds and gets evicted in an infinite rebalance loop!

### Solutions

1. **Lower `max.poll.records`** (e.g., from `500` to `20` or `50`) so worst-case batch processing time is well below `max.poll.interval.ms`.
2. **Enforce strict timeouts and circuit breakers** on any external HTTP call inside the consumer listener.
3. **Configure a Dead Letter Topic (`DefaultErrorHandler` + `DeadLetterPublishingRecoverer`)** with bounded exponential backoff so poison-pill messages do not block partition progress forever.

---

## Scenario 51: Redis Cache Expiration Causes a Database Meltdown (Cache Stampede / Thundering Herd)

**Interview question:** Your homepage product catalog is cached in Redis with a 10-minute TTL. The moment the key expires during peak traffic, 5,000 concurrent requests miss the cache simultaneously and execute the same expensive SQL query, crashing the database. How do you prevent a **Cache Stampede**?

### Answer

When a hot cache key expires under high concurrency, thousands of threads experience a cache miss at the exact same millisecond and all rush to recompute the value from the database (**Cache Stampede / Dogpiling**).

### 3 Production Solutions

1. **Single-Flight / Request Coalescing + Distributed Lock:**
   - Inside each JVM, use `Caffeine.AsyncLoadingCache` (`CompletableFuture` coalescing) so 500 threads on the same pod share **1** in-flight database call.
   - Across pods, acquire a short Redis lock (`SET lock:catalog NX PX 3000`) so only 1 pod queries the database while other pods either wait briefly or serve slightly stale data.
2. **Stale-While-Revalidate (Logical Expiration):**
   - Store a logical `refreshAt` timestamp inside the cached value while setting the physical Redis TTL much longer (e.g., logical refresh at 10 minutes, physical TTL at 60 minutes).
   - When a thread sees `now > refreshAt`, it tries to acquire a non-blocking lock to refresh the cache asynchronously in the background while **immediately returning the existing stale value** with zero latency!
3. **TTL Jitter for Bulk Keys:**
   - If caching 10,000 product keys at startup, never give all of them the exact same `TTL = 3600s`. Add random jitter (`3600 + ThreadLocalRandom.current().nextInt(0, 600)`) so keys expire smoothly over time instead of all at once.

---

## Scenario 52: A Slow Downstream Service Causes Cascading Failure Across All Microservices

**Interview question:** Your `CheckoutService` calls a third-party `ShippingRateService`. The shipping provider slows down from 50 ms to 25 seconds. Within 30 seconds, your entire `CheckoutService` becomes unresponsive, even for endpoints that do not use shipping rates! Why did this happen, and how do you prevent it?

### Answer

Without timeouts, bulkheads, and circuit breakers, all 200 Tomcat worker threads in `CheckoutService` quickly get stuck waiting 25 seconds on `ShippingRateService`. Once all 200 threads are occupied, **every other endpoint** on `CheckoutService` (even `/health` or digital-download checkouts) queues up and times out—causing a **Cascading Failure**.

### Defense-in-Depth Resilience Architecture (Resilience4j)

```text
Caller ──> [ Bulkhead (Max 25 concurrent calls) ]
                 │
                 v
           [ TimeLimiter / Socket Timeout (1.5s) ]
                 │
                 v
           [ CircuitBreaker (Opens when failure/slow rate > 50%) ]
                 │
                 +──> (CLOSED) ──> Call ShippingRateService
                 +──> (OPEN)   ──> Fail-fast (<1ms) to Fallback (Estimated flat shipping rate)
```

1. **Aggressive Connect & Read Timeouts:** Never use infinite default HTTP client timeouts. Configure `connectTimeout = 500ms`, `readTimeout = 1500ms`.
2. **Bulkhead Isolation:** Restrict `ShippingRateService` calls to a dedicated thread pool or semaphore of at most 25 concurrent calls so it can never consume all 200 Tomcat threads.
3. **Circuit Breaker (Resilience4j):** Monitor both failure rate (`failureRateThreshold: 50%`) and slow-call rate (`slowCallRateThreshold: 50%`, `slowCallDurationThreshold: 1500ms`). When tripped to `OPEN`, reject calls immediately in `< 1 ms` and invoke a graceful fallback while the downstream service recovers.
4. **Retries with Exponential Backoff + Jitter:** Never retry immediately on a struggling downstream service without backoff and jitter, or you will trigger a **Retry Storm** that amplifies traffic $3\times$–$5\times$.

---

## Scenario 53: Rolling Deployment Causes `502 Bad Gateway` Errors in Kubernetes

**Interview question:** Every time you deploy a new version of your Spring Boot service on Kubernetes, monitoring alerts on a brief spike of `502 Bad Gateway` / `504 Gateway Timeout` errors for 10 seconds, even though `RollingUpdate` is configured. Why does this happen, and how do you achieve true zero-downtime deployments?

### Answer

When Kubernetes terminates an old Pod during a rolling update, two things happen **concurrently in parallel**, not sequentially:
1. The kubelet sends `SIGTERM` to the Java container in the Pod.
2. The Kubernetes Control Plane removes the Pod's IP from `EndpointSlices` and notifies `kube-proxy` / Ingress Controllers (Nginx, Envoy, ALB) asynchronously.

If your Spring Boot application shuts down immediately on receiving `SIGTERM` (or refuses new connections immediately), two failure modes occur:
- **In-flight requests** currently executing on the pod are abruptly severed mid-transaction.
- **New requests** routed by the Ingress Controller during the 2–5 second endpoint propagation delay arrive at a closed socket (`Connection refused` -> `502 Bad Gateway`).

### Complete Zero-Downtime Configuration

1. **Enable Graceful Shutdown in Spring Boot (`application.yml`):**
   ```yaml
   server:
     shutdown: graceful
   spring:
     lifecycle:
       timeout-per-shutdown-phase: 30s
   ```
   When `server.shutdown=graceful` is enabled, Tomcat stops accepting new work after `SIGTERM` and waits up to 30 seconds for active in-flight HTTP requests to finish cleanly.
2. **Add a `preStop` Hook in Kubernetes:**
   Give the Ingress Controller / load balancer 5–10 seconds to remove the Pod IP from its routing tables *before* `SIGTERM` is even sent to the JVM:
   ```yaml
   lifecycle:
     preStop:
       exec:
         command: ["sh", "-c", "sleep 10"]
   terminationGracePeriodSeconds: 45
   ```
3. **Ensure the Dockerfile uses `ENTRYPOINT` exec form (`["java", "-jar", "app.jar"]`)** so the JVM is `PID 1` and actually receives the `SIGTERM` signal (shell form `/bin/sh -c` does not forward `SIGTERM` to child processes!).

---

# How to Answer Any Scenario-Based Java Interview Question

Use this seven-step framework in every interview:

1. **Clarify the requirement:** Identify the expected behavior, traffic scale, consistency needs, and constraints.
2. **Identify the failure or bottleneck:** Determine whether it concerns object identity, memory retention, concurrency/locks, I/O latency, database indexing, security, or distributed consensus.
3. **Explain the underlying mechanism:** Describe what Java, the JVM (Heap, Stack, JIT, GC, ClassLoader), the database engine, or the Linux kernel/network is actually doing under the hood.
4. **Present a clean solution:** Provide idiomatic, modern Java code (Records, `Optional`, `CompletableFuture`, Virtual Threads, Jakarta Validation, or atomic SQL) or a concrete debugging plan.
5. **Discuss alternatives and trade-offs:** Compare approaches (e.g., `synchronized` vs `AtomicInteger` vs `LongAdder`; Optimistic vs Pessimistic locking; G1GC vs ZGC).
6. **Consider production risks:** Think about partial failures, network timeouts, idempotency, connection pool limits, security (BOLA/IDOR), and graceful degradation.
7. **Explain verification and observability:** Describe the unit/concurrency tests, JFR profiles, heap dumps, `EXPLAIN ANALYZE` plans, OpenTelemetry traces, or Prometheus metrics that prove the fix works.
