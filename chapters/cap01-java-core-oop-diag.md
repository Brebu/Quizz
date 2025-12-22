# Capitolul 1 – Java Core & OOP (VERSIUNE EXTINSĂ)
## Q001–Q120 — Nivel Senior

> 📚 Ghid complet pentru interviuri Senior / Lead / Staff  
> 🎯 Focus: Teorie aprofundată + Diagrame + Exemple practice

---

## 🎯 Harta Conceptuală a Capitolului

```mermaid
mindmap
  root((Java Core & OOP))
    OOP Fundamentals
      Encapsulation
      Inheritance
      Polymorphism
      Abstraction
    SOLID Principles
      SRP
      OCP
      LSP
      ISP
      DIP
    Java Types
      Primitives
      Wrappers
      String
      Enums
      Records
    Memory & Safety
      Immutability
      JMM
      Thread Safety
    Modern Java
      Optional
      Generics
      Lambdas
      Streams
    Best Practices
      Clean Code
      Defensive Programming
      Error Handling
```

---

# 📦 SECȚIUNEA 1: OOP FUNDAMENTALS

## Q001-Q010: Bazele OOP

### Ce este OOP și de ce este fundamental în Java?

```mermaid
graph TB
    subgraph "Paradigma OOP"
        A[Obiect] --> B[Stare<br/>fields]
        A --> C[Comportament<br/>methods]
        A --> D[Identitate<br/>referință]
    end
    
    subgraph "Beneficii"
        E[Modelare domeniu]
        F[Extensibilitate]
        G[Reutilizare]
        H[Testabilitate]
    end
    
    A --> E
    A --> F
    A --> G
    A --> H
```

**Definiție completă:**
OOP structurează aplicațiile în jurul obiectelor care combină:
- **Stare** (date/fields)
- **Comportament** (metode)
- **Identitate** (referință unică)

**De ce Java este OOP pur:**
```java
// ❌ Nu există funcții globale în Java
// ✅ Totul este într-o clasă
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```

**Anti-pattern: Anemic Domain Model**
```java
// ❌ GREȘIT - doar date, fără comportament
public class User {
    private String email;
    private boolean active;
    
    // doar getters/setters - obiect "mort"
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}

// ✅ CORECT - Rich Domain Model
public class User {
    private String email;
    private boolean active;
    
    public void activate() {
        if (this.active) {
            throw new IllegalStateException("Already active");
        }
        this.active = true;
        // logică de business aici
    }
    
    public void changeEmail(String newEmail) {
        validateEmail(newEmail);
        this.email = newEmail;
    }
}
```

---

### Cele 4 Principii OOP

```mermaid
graph LR
    subgraph "4 Piloni OOP"
        A[🔒 Încapsulare] 
        B[🧬 Moștenire]
        C[🎭 Polimorfism]
        D[🎨 Abstractizare]
    end
    
    A --> A1[Ascunde starea]
    A --> A2[Expune comportament]
    
    B --> B1[Reutilizare cod]
    B --> B2[Ierarhii de tip]
    
    C --> C1[Aceeași interfață]
    C --> C2[Comportament diferit]
    
    D --> D1[Separă CE de CUM]
    D --> D2[Contracte clare]
```

---

### Încapsularea în Profunzime

```mermaid
sequenceDiagram
    participant Client
    participant BankAccount
    participant InternalState
    
    Note over BankAccount: Stare privată protejată
    
    Client->>BankAccount: withdraw(100)
    BankAccount->>BankAccount: validateAmount()
    BankAccount->>BankAccount: checkBalance()
    BankAccount->>InternalState: balance -= 100
    BankAccount-->>Client: success
    
    Note over Client,InternalState: Clientul NU accesează direct starea
```

**Exemplu corect de încapsulare:**

```java
public class BankAccount {
    // Stare privată - INVARIANTA: balance >= 0
    private BigDecimal balance;
    private final String accountNumber;
    private AccountStatus status;
    
    public BankAccount(String accountNumber, BigDecimal initialDeposit) {
        this.accountNumber = accountNumber;
        this.balance = Objects.requireNonNull(initialDeposit);
        this.status = AccountStatus.ACTIVE;
        
        if (initialDeposit.compareTo(BigDecimal.ZERO) < 0) {
            throw new IllegalArgumentException("Initial deposit cannot be negative");
        }
    }
    
    // ✅ Comportament, nu getter/setter
    public void deposit(BigDecimal amount) {
        validateActive();
        validatePositive(amount);
        this.balance = this.balance.add(amount);
    }
    
    public void withdraw(BigDecimal amount) {
        validateActive();
        validatePositive(amount);
        if (this.balance.compareTo(amount) < 0) {
            throw new InsufficientFundsException(this.balance, amount);
        }
        this.balance = this.balance.subtract(amount);
    }
    
    // Query method - OK să returneze valoare
    public BigDecimal getBalance() {
        return this.balance; // BigDecimal e immutable
    }
    
    private void validateActive() {
        if (this.status != AccountStatus.ACTIVE) {
            throw new AccountClosedException();
        }
    }
    
    private void validatePositive(BigDecimal amount) {
        if (amount.compareTo(BigDecimal.ZERO) <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
    }
}
```

---

### Polimorfism: Static vs Dinamic

```mermaid
graph TB
    subgraph "Polimorfism Static (Compile-time)"
        A[Overloading]
        A --> A1["print(int)"]
        A --> A2["print(String)"]
        A --> A3["print(int, String)"]
    end
    
    subgraph "Polimorfism Dinamic (Runtime)"
        B[Overriding]
        B --> B1[Animal.speak]
        B1 --> B2[Dog.speak → 'Woof']
        B1 --> B3[Cat.speak → 'Meow']
    end
    
    style B fill:#90EE90
```

```java
// Polimorfism DINAMIC - baza OOP
public interface PaymentProcessor {
    PaymentResult process(Payment payment);
}

public class CreditCardProcessor implements PaymentProcessor {
    @Override
    public PaymentResult process(Payment payment) {
        // Logică specifică card
        return new PaymentResult(/* ... */);
    }
}

public class PayPalProcessor implements PaymentProcessor {
    @Override
    public PaymentResult process(Payment payment) {
        // Logică specifică PayPal
        return new PaymentResult(/* ... */);
    }
}

// ✅ Codul client NU știe implementarea
public class PaymentService {
    private final PaymentProcessor processor;
    
    public PaymentService(PaymentProcessor processor) {
        this.processor = processor; // Dependency Injection
    }
    
    public void pay(Payment payment) {
        // Polimorfism în acțiune - metoda corectă la runtime
        PaymentResult result = processor.process(payment);
    }
}
```

---

### Liskov Substitution Principle (LSP)

```mermaid
graph TB
    subgraph "✅ Corect LSP"
        A[Bird] --> B[Sparrow]
        A --> C[Eagle]
        D[FlyingBird] --> B
        D --> C
        E[NonFlyingBird] --> F[Penguin]
        E --> G[Ostrich]
    end
    
    subgraph "❌ Încălcare LSP"
        X[Bird with fly] --> Y[Penguin]
        Y -. "throw Exception" .-> Z[💥]
    end
```

```java
// ❌ ÎNCĂLCARE LSP
public class Bird {
    public void fly() { /* ... */ }
}

public class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly!");
    }
}

// ✅ CORECT - Separare responsabilități
public interface Bird {
    void eat();
    void sleep();
}

public interface FlyingBird extends Bird {
    void fly();
}

public class Sparrow implements FlyingBird {
    @Override public void fly() { /* ... */ }
    @Override public void eat() { /* ... */ }
    @Override public void sleep() { /* ... */ }
}

public class Penguin implements Bird {
    @Override public void eat() { /* ... */ }
    @Override public void sleep() { /* ... */ }
    // Nu are fly() - corect semantic
}
```

---

### Compoziție vs Moștenire

```mermaid
graph LR
    subgraph "❌ Moștenire Excesivă"
        A[Vehicle] --> B[Car]
        B --> C[ElectricCar]
        C --> D[TeslaModelS]
        D --> E[TeslaModelSPlaid]
    end
    
    subgraph "✅ Compoziție"
        F[Car]
        F --> G[Engine]
        F --> H[Transmission]
        F --> I[Battery]
        G --> G1[GasEngine]
        G --> G2[ElectricEngine]
    end
```

```java
// ✅ Compoziție - flexibilitate maximă
public class Car {
    private final Engine engine;
    private final Transmission transmission;
    private final List<Wheel> wheels;
    
    public Car(Engine engine, Transmission transmission, List<Wheel> wheels) {
        this.engine = engine;
        this.transmission = transmission;
        this.wheels = List.copyOf(wheels);
    }
    
    public void start() {
        engine.start();
        transmission.engage();
    }
}

// Poți schimba componente fără a modifica Car
Engine electricEngine = new ElectricEngine(batteryCapacity);
Engine gasEngine = new GasEngine(fuelType);

Car tesla = new Car(electricEngine, new AutoTransmission(), wheels);
Car mustang = new Car(gasEngine, new ManualTransmission(), wheels);
```

---

# 📦 SECȚIUNEA 2: JAVA TYPE SYSTEM

## Q011-Q040: Tipuri și Memorie

### String Immutability & Pool

```mermaid
graph TB
    subgraph "Heap Memory"
        subgraph "String Pool"
            S1["'hello'"]
            S2["'world'"]
        end
        
        subgraph "Regular Heap"
            S3["new String('hello')"]
        end
    end
    
    R1[s1] --> S1
    R2[s2] --> S1
    R3[s3] --> S3
    
    Note1["s1 == s2 → true<br/>s1 == s3 → false<br/>s1.equals(s3) → true"]
```

```java
// Demonstrație String Pool
String s1 = "hello";           // Pool
String s2 = "hello";           // Aceeași referință din pool
String s3 = new String("hello"); // Obiect nou pe heap
String s4 = s3.intern();       // Forțează în pool

System.out.println(s1 == s2);  // true - aceeași referință
System.out.println(s1 == s3);  // false - obiecte diferite
System.out.println(s1 == s4);  // true - intern() returnează din pool
System.out.println(s1.equals(s3)); // true - aceeași valoare
```

---

### equals() și hashCode() Contract

```mermaid
graph TB
    subgraph "Contract"
        A[equals true] --> B[hashCode TREBUIE egal]
        C[hashCode egal] -. "NU implică" .-> D[equals true]
    end
    
    subgraph "HashMap Lookup"
        E[key.hashCode] --> F[Find bucket]
        F --> G[key.equals pe fiecare element]
        G --> H[Return value]
    end
```

```java
public class Employee {
    private final Long id;
    private final String email;
    private String name; // mutable, NU include în equals/hashCode
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Employee employee = (Employee) o;
        return Objects.equals(id, employee.id) 
            && Objects.equals(email, employee.email);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(id, email);
    }
    
    // ⚠️ REGULĂ: Folosește doar câmpuri IMMUTABLE în equals/hashCode
}
```

---

### Wrapper Classes și Autoboxing

```mermaid
graph LR
    subgraph "Integer Cache [-128, 127]"
        C1[-128]
        C2[...]
        C3[127]
    end
    
    A[Integer.valueOf 100] --> C1
    B[Integer.valueOf 100] --> C1
    
    D[Integer.valueOf 200] --> E[new Integer 200]
    F[Integer.valueOf 200] --> G[new Integer 200]
    
    style C1 fill:#90EE90
    style E fill:#FFB6C1
    style G fill:#FFB6C1
```

```java
// ⚠️ CAPCANĂ CLASICĂ
Integer a = 100;
Integer b = 100;
System.out.println(a == b);  // true (cache)

Integer c = 200;
Integer d = 200;
System.out.println(c == d);  // false! (obiecte diferite)
System.out.println(c.equals(d)); // true

// ✅ REGULĂ: Întotdeauna folosește equals() pentru Wrapper
```

---

### Immutability Pattern

```mermaid
classDiagram
    class ImmutablePerson {
        -String name
        -LocalDate birthDate
        -List~String~ hobbies
        +ImmutablePerson(name, birthDate, hobbies)
        +getName() String
        +getBirthDate() LocalDate
        +getHobbies() List~String~
        +withName(newName) ImmutablePerson
    }
    
    note for ImmutablePerson "✅ All fields final\n✅ No setters\n✅ Defensive copy in constructor\n✅ Return copies for mutable fields\n✅ with* methods return new instance"
```

```java
public final class ImmutablePerson {
    private final String name;
    private final LocalDate birthDate;
    private final List<String> hobbies;
    
    public ImmutablePerson(String name, LocalDate birthDate, List<String> hobbies) {
        this.name = Objects.requireNonNull(name);
        this.birthDate = Objects.requireNonNull(birthDate);
        // Defensive copy - protejează de modificări externe
        this.hobbies = List.copyOf(hobbies);
    }
    
    public String getName() { return name; }
    public LocalDate getBirthDate() { return birthDate; } // LocalDate e immutable
    public List<String> getHobbies() { return hobbies; } // List.copyOf e immutable
    
    // "Wither" pattern pentru modificări
    public ImmutablePerson withName(String newName) {
        return new ImmutablePerson(newName, this.birthDate, this.hobbies);
    }
}
```

---

# 📦 SECȚIUNEA 3: GENERICS & TYPE SAFETY

## Q051-Q060: Generics în Profunzime

### Type Erasure

```mermaid
graph LR
    subgraph "Compile Time"
        A["List&lt;String&gt;"]
        B["List&lt;Integer&gt;"]
    end
    
    subgraph "Runtime (Type Erasure)"
        C[List]
    end
    
    A --> C
    B --> C
    
    Note["La runtime, toate sunt doar 'List'"]
```

```java
// Type erasure în acțiune
List<String> strings = new ArrayList<>();
List<Integer> integers = new ArrayList<>();

// La runtime, ambele sunt ArrayList
System.out.println(strings.getClass() == integers.getClass()); // true!

// ❌ Nu poți face
// if (obj instanceof List<String>) { } // Compile error
// new T() // Compile error
// T[] array = new T[10]; // Compile error

// ✅ Workaround cu Class<T>
public class Factory<T> {
    private final Class<T> type;
    
    public Factory(Class<T> type) {
        this.type = type;
    }
    
    public T create() throws Exception {
        return type.getDeclaredConstructor().newInstance();
    }
}
```

---

### PECS: Producer Extends, Consumer Super

```mermaid
graph TB
    subgraph "? extends T (Producer/Covariant)"
        A[List ? extends Number]
        A --> B["✅ Read: Number n = list.get(0)"]
        A --> C["❌ Write: list.add(1) - COMPILE ERROR"]
    end
    
    subgraph "? super T (Consumer/Contravariant)"
        D[List ? super Integer]
        D --> E["✅ Write: list.add(1)"]
        D --> F["❌ Read: Integer i = list.get(0) - needs cast"]
    end
```

```java
// PECS în practică
public class Collections {
    
    // Producer - citești din src
    public static <T> void copy(
        List<? super T> dest,    // Consumer - scrii în dest
        List<? extends T> src    // Producer - citești din src
    ) {
        for (int i = 0; i < src.size(); i++) {
            dest.set(i, src.get(i));
        }
    }
}

// Exemplu utilizare
List<Number> numbers = new ArrayList<>();
List<Integer> integers = List.of(1, 2, 3);

Collections.copy(numbers, integers); // OK!
// numbers (super Integer) primește
// integers (extends Number) oferă
```

---

# 📦 SECȚIUNEA 4: EXCEPTIONS & ERROR HANDLING

## Q043-Q050: Exception Handling

### Exception Hierarchy

```mermaid
classDiagram
    Throwable <|-- Error
    Throwable <|-- Exception
    Exception <|-- RuntimeException
    Exception <|-- IOException
    Exception <|-- SQLException
    RuntimeException <|-- NullPointerException
    RuntimeException <|-- IllegalArgumentException
    RuntimeException <|-- IllegalStateException
    Error <|-- OutOfMemoryError
    Error <|-- StackOverflowError
    
    class Throwable {
        <<abstract>>
    }
    class Error {
        ❌ Nu prinde
    }
    class Exception {
        Checked
    }
    class RuntimeException {
        Unchecked
    }
```

### Exception Strategy Decision Tree

```mermaid
flowchart TD
    A[Eroare detectată] --> B{Este recuperabilă?}
    B -->|Da| C{Caller poate gestiona?}
    B -->|Nu| D[Throw RuntimeException]
    C -->|Da, întotdeauna| E[Checked Exception]
    C -->|Depinde| F[Unchecked + Document]
    
    D --> G[IllegalStateException<br/>IllegalArgumentException]
    E --> H[IOException<br/>Custom checked]
    F --> I[Custom RuntimeException]
```

```java
// ✅ Modern Exception Handling
public class UserService {
    
    public User findById(Long id) {
        // Precondition check - fail fast
        if (id == null || id <= 0) {
            throw new IllegalArgumentException("Invalid user ID: " + id);
        }
        
        return userRepository.findById(id)
            .orElseThrow(() -> new UserNotFoundException(id));
    }
    
    public void updateEmail(Long userId, String newEmail) {
        User user = findById(userId);
        
        if (!user.isActive()) {
            throw new IllegalStateException(
                "Cannot update email for inactive user: " + userId
            );
        }
        
        try {
            emailValidator.validate(newEmail);
            user.setEmail(newEmail);
            userRepository.save(user);
        } catch (ValidationException e) {
            // Exception translation
            throw new InvalidEmailException(newEmail, e);
        }
    }
}

// Custom exception cu context
public class UserNotFoundException extends RuntimeException {
    private final Long userId;
    
    public UserNotFoundException(Long userId) {
        super("User not found with ID: " + userId);
        this.userId = userId;
    }
    
    public Long getUserId() { return userId; }
}
```

---

# 📦 SECȚIUNEA 5: MODERN JAVA FEATURES

## Q063-Q080: Records, Sealed, Pattern Matching

### Records (Java 16+)

```mermaid
classDiagram
    class RecordPoint {
        <<record>>
        -int x
        -int y
        +x() int
        +y() int
        +equals() boolean
        +hashCode() int
        +toString() String
    }
    
    note for RecordPoint "✅ Immutable by default\n✅ Auto-generated methods\n✅ Compact constructors\n❌ Cannot extend classes\n❌ No additional instance fields"
```

```java
// Record simplu
public record Point(int x, int y) {
    // Compact constructor pentru validare
    public Point {
        if (x < 0 || y < 0) {
            throw new IllegalArgumentException("Coordinates must be positive");
        }
    }
    
    // Metode adiționale permise
    public double distanceFromOrigin() {
        return Math.sqrt(x * x + y * y);
    }
}

// Record cu componente complexe
public record Order(
    String orderId,
    List<OrderItem> items,
    LocalDateTime createdAt
) {
    public Order {
        Objects.requireNonNull(orderId);
        items = List.copyOf(items); // Defensive copy
        createdAt = createdAt != null ? createdAt : LocalDateTime.now();
    }
    
    public BigDecimal total() {
        return items.stream()
            .map(OrderItem::subtotal)
            .reduce(BigDecimal.ZERO, BigDecimal::add);
    }
}
```

---

### Sealed Classes (Java 17+)

```mermaid
classDiagram
    class Shape {
        <<sealed>>
    }
    class Circle {
        <<final>>
    }
    class Rectangle {
        <<final>>
    }
    class Triangle {
        <<final>>
    }
    
    Shape <|-- Circle
    Shape <|-- Rectangle
    Shape <|-- Triangle
    
    note for Shape "permits Circle, Rectangle, Triangle\nExhaustive in switch expressions"
```

```java
// Sealed hierarchy pentru expresii
public sealed interface Expression 
    permits Constant, Variable, BinaryOp, UnaryOp {
}

public record Constant(double value) implements Expression {}
public record Variable(String name) implements Expression {}
public record BinaryOp(Expression left, String op, Expression right) implements Expression {}
public record UnaryOp(String op, Expression operand) implements Expression {}

// Pattern matching exhaustiv
public double evaluate(Expression expr, Map<String, Double> env) {
    return switch (expr) {
        case Constant(var value) -> value;
        case Variable(var name) -> env.getOrDefault(name, 0.0);
        case BinaryOp(var l, var op, var r) -> switch (op) {
            case "+" -> evaluate(l, env) + evaluate(r, env);
            case "-" -> evaluate(l, env) - evaluate(r, env);
            case "*" -> evaluate(l, env) * evaluate(r, env);
            case "/" -> evaluate(l, env) / evaluate(r, env);
            default -> throw new IllegalArgumentException("Unknown op: " + op);
        };
        case UnaryOp(var op, var operand) -> switch (op) {
            case "-" -> -evaluate(operand, env);
            case "+" -> evaluate(operand, env);
            default -> throw new IllegalArgumentException("Unknown op: " + op);
        };
        // Nu e nevoie de default - exhaustiv!
    };
}
```

---

### Optional Best Practices

```mermaid
flowchart TD
    A[Ai o valoare care poate lipsi?] --> B{Este return type?}
    B -->|Da| C[✅ Folosește Optional]
    B -->|Nu| D{Este field?}
    D -->|Da| E[❌ Nu folosi Optional ca field]
    D -->|Nu| F{Este parametru?}
    F -->|Da| G[❌ Nu folosi Optional ca parametru]
    
    C --> H{Cum procesezi?}
    H --> I[map/flatMap pentru transformări]
    H --> J[orElse/orElseGet pentru default]
    H --> K[orElseThrow pentru erori]
    H --> L[ifPresent pentru side effects]
```

```java
public class OptionalBestPractices {
    
    // ✅ Return type - perfect use case
    public Optional<User> findByEmail(String email) {
        return Optional.ofNullable(userMap.get(email));
    }
    
    // ✅ Chaining transformations
    public String getUserDisplayName(Long userId) {
        return findById(userId)
            .map(User::getProfile)
            .map(Profile::getDisplayName)
            .orElse("Anonymous");
    }
    
    // ✅ orElseGet pentru operații costisitoare
    public User getOrCreate(String email) {
        return findByEmail(email)
            .orElseGet(() -> createNewUser(email)); // Lazy evaluation
    }
    
    // ✅ orElseThrow pentru cazuri obligatorii
    public User getRequired(Long id) {
        return findById(id)
            .orElseThrow(() -> new UserNotFoundException(id));
    }
    
    // ❌ ANTI-PATTERNS
    
    // ❌ Nu folosi Optional.get() fără verificare
    // user.get() // Poate arunca NoSuchElementException
    
    // ❌ Nu folosi Optional pentru fields
    // private Optional<Address> address; // GREȘIT
    
    // ❌ Nu folosi Optional ca parametru
    // public void process(Optional<Config> config) // GREȘIT
    // public void process(Config config) // config poate fi null, documentează
    
    // ❌ Nu folosi Optional cu colecții
    // Optional<List<User>> // GREȘIT
    // List<User> // returnează listă goală în loc de Optional
}
```

---

# 📦 SECȚIUNEA 6: FUNCTIONAL PROGRAMMING

## Q073-Q080: Lambdas & Streams

### Stream Pipeline

```mermaid
graph LR
    subgraph "Source"
        A[Collection/Array]
    end
    
    subgraph "Intermediate Operations (Lazy)"
        B[filter]
        C[map]
        D[flatMap]
        E[sorted]
        F[distinct]
    end
    
    subgraph "Terminal Operation (Eager)"
        G[collect]
        H[reduce]
        I[forEach]
        J[count]
    end
    
    A --> B --> C --> D --> E --> F --> G
    
    style B fill:#FFE4B5
    style C fill:#FFE4B5
    style D fill:#FFE4B5
    style E fill:#FFE4B5
    style F fill:#FFE4B5
    style G fill:#90EE90
```

```java
public class StreamExamples {
    
    // Transformări complexe
    public Map<String, List<Order>> getOrdersByCustomerCity(List<Order> orders) {
        return orders.stream()
            .filter(order -> order.getStatus() == OrderStatus.COMPLETED)
            .collect(Collectors.groupingBy(
                order -> order.getCustomer().getCity()
            ));
    }
    
    // flatMap pentru nested structures
    public List<String> getAllProductNames(List<Order> orders) {
        return orders.stream()
            .flatMap(order -> order.getItems().stream())
            .map(OrderItem::getProductName)
            .distinct()
            .sorted()
            .collect(Collectors.toList());
    }
    
    // Reduce pentru agregări
    public BigDecimal calculateTotalRevenue(List<Order> orders) {
        return orders.stream()
            .map(Order::getTotal)
            .reduce(BigDecimal.ZERO, BigDecimal::add);
    }
    
    // Parallel stream - cu grijă!
    public long countExpensiveOrders(List<Order> orders) {
        return orders.parallelStream() // ⚠️ Doar pentru colecții MARI
            .filter(order -> order.getTotal().compareTo(threshold) > 0)
            .count();
    }
}
```

---

# 📦 SECȚIUNEA 7: CLEAN CODE & BEST PRACTICES

## Q100-Q120: Professional Development

### SOLID Quick Reference

```mermaid
graph TB
    subgraph "S - Single Responsibility"
        S1[O clasă = un motiv de schimbare]
    end
    
    subgraph "O - Open/Closed"
        O1[Deschis pentru extensie]
        O2[Închis pentru modificare]
    end
    
    subgraph "L - Liskov Substitution"
        L1[Subtipurile pot înlocui părinții]
    end
    
    subgraph "I - Interface Segregation"
        I1[Interfețe mici și specifice]
    end
    
    subgraph "D - Dependency Inversion"
        D1[Depinde de abstracții]
        D2[Nu de implementări]
    end
```

### Code Quality Checklist

```mermaid
flowchart TD
    A[Cod nou] --> B{Are teste?}
    B -->|Nu| C[❌ Scrie teste întâi]
    B -->|Da| D{Respectă SRP?}
    D -->|Nu| E[Refactor: extrage clase]
    D -->|Da| F{Metode < 20 linii?}
    F -->|Nu| G[Refactor: extrage metode]
    F -->|Da| H{Nume clare?}
    H -->|Nu| I[Rename pentru claritate]
    H -->|Da| J{Documentație necesară?}
    J -->|Da| K[Adaugă Javadoc]
    J -->|Nu| L[✅ Ready for review]
```

---

# 🎯 CHEAT SHEET FINAL

## Reguli de Aur Java Senior

| Concept | Regulă | Anti-pattern |
|---------|--------|--------------|
| **OOP** | Rich Domain Model | Anemic Domain Model |
| **Încapsulare** | Comportament, nu date | Getter/Setter pentru tot |
| **Moștenire** | Preferă compoziția | Ierarhii adânci |
| **Comparații** | `equals()` pentru valori | `==` pentru obiecte |
| **Null** | `Optional` pentru return | Null checks everywhere |
| **Exceptions** | Unchecked + documentație | Checked pentru tot |
| **Immutability** | Default immutable | Mutable by default |
| **Generics** | PECS | Raw types |

---

## Quick Reference: Când folosești ce?

```
ArrayList vs LinkedList?
  → ArrayList (aproape întotdeauna)

HashMap vs TreeMap?
  → HashMap (O(1) vs O(log n))
  → TreeMap doar dacă ai nevoie de sortare

Checked vs Unchecked Exception?
  → Unchecked pentru erori de programare
  → Checked doar când caller TREBUIE să gestioneze

Optional vs null?
  → Optional pentru return values
  → Evită Optional ca field sau parametru

Record vs Class?
  → Record pentru date imutabile simple
  → Class pentru comportament complex
```

---

> 💡 **Regula supremă pentru Senior Engineer:**  
> *"Judecata tehnică: a ști CE SĂ NU FACI este mai important decât a ști ce să faci."*
