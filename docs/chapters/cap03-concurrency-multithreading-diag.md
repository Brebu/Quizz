# Capitolul 3 – Concurrency & Multithreading (VERSIUNE EXTINSĂ)
## Q181–Q260 — Nivel Senior

> 📚 **Scop:** Interviuri Senior / Lead / Staff  
> 🎯 **Focus:** Thread Safety, Sincronizare, Java Memory Model  
> 💾 **Encoding:** UTF-8

---

## 🎯 HARTA MENTALĂ CONCURRENCY

```mermaid
mindmap
  root((Concurrency))
    Fundamentals
      Thread Lifecycle
      Runnable vs Callable
      ExecutorService
      Thread Pools
    Synchronization
      synchronized
      ReentrantLock
      ReadWriteLock
      StampedLock
    Thread Safety
      Atomicity
      Visibility
      Ordering
      volatile
    Coordination
      CountDownLatch
      CyclicBarrier
      Semaphore
      Phaser
    Modern APIs
      CompletableFuture
      Parallel Streams
      Virtual Threads
    Problems
      Race Condition
      Deadlock
      Livelock
      Starvation
```

---

# 📦 SECȚIUNEA 1: FUNDAMENTELE THREAD-URILOR

## Q181-Q185: Thread Basics

### Thread Lifecycle

```mermaid
stateDiagram-v2
    [*] --> NEW: new Thread()
    NEW --> RUNNABLE: start()
    RUNNABLE --> RUNNING: Scheduler picks
    RUNNING --> RUNNABLE: yield() / preempt
    RUNNING --> BLOCKED: synchronized (waiting for lock)
    BLOCKED --> RUNNABLE: Lock acquired
    RUNNING --> WAITING: wait() / join() / park()
    WAITING --> RUNNABLE: notify() / unpark()
    RUNNING --> TIMED_WAITING: sleep() / wait(timeout)
    TIMED_WAITING --> RUNNABLE: timeout / notify()
    RUNNING --> TERMINATED: run() completes
    TERMINATED --> [*]
```

### Thread Creation Patterns

```mermaid
graph TB
    subgraph "❌ Anti-pattern: Direct Thread"
        T1["new Thread(runnable).start()"]
        T1 --> P1["Cost mare creare"]
        T1 --> P2["Fără control"]
        T1 --> P3["Nu scalează"]
    end
    
    subgraph "✅ Best Practice: ExecutorService"
        E1["ExecutorService"]
        E1 --> E2["Thread reuse"]
        E1 --> E3["Queue management"]
        E1 --> E4["Lifecycle control"]
    end
```

### Runnable vs Callable

```mermaid
classDiagram
    class Runnable {
        <<interface>>
        +run() void
        Nu returnează rezultat
        Nu aruncă checked exceptions
    }
    
    class Callable~T~ {
        <<interface>>
        +call() T
        Returnează rezultat
        Poate arunca Exception
    }
    
    class Future~T~ {
        <<interface>>
        +get() T
        +get(timeout) T
        +isDone() boolean
        +cancel() boolean
    }
    
    Callable --> Future: produces
```

```java
// Runnable - fără rezultat
Runnable task = () -> System.out.println("Running");

// Callable - cu rezultat
Callable<Integer> callable = () -> {
    Thread.sleep(1000);
    return 42;
};

ExecutorService executor = Executors.newFixedThreadPool(4);
Future<Integer> future = executor.submit(callable);
Integer result = future.get(); // Blocking!
```

---

## Q187-Q190: Thread Pools

### Tipuri de Thread Pools

```mermaid
graph TB
    subgraph "Fixed Thread Pool"
        FTP["newFixedThreadPool(n)"]
        FTP --> F1["Exact n threads"]
        FTP --> F2["Unbounded queue"]
        FTP --> F3["Best for: CPU-bound tasks"]
    end
    
    subgraph "Cached Thread Pool"
        CTP["newCachedThreadPool()"]
        CTP --> C1["0 to ∞ threads"]
        CTP --> C2["60s keep-alive"]
        CTP --> C3["Best for: Short async tasks"]
    end
    
    subgraph "Single Thread Executor"
        STE["newSingleThreadExecutor()"]
        STE --> S1["Exactly 1 thread"]
        STE --> S2["Sequential execution"]
        STE --> S3["Best for: Event loop"]
    end
    
    subgraph "Scheduled Thread Pool"
        SCTP["newScheduledThreadPool(n)"]
        SCTP --> SC1["Delayed execution"]
        SCTP --> SC2["Periodic tasks"]
        SCTP --> SC3["Best for: Timers, cron"]
    end
    
    subgraph "Work Stealing Pool"
        WSP["newWorkStealingPool()"]
        WSP --> W1["ForkJoinPool based"]
        WSP --> W2["Work stealing algorithm"]
        WSP --> W3["Best for: Recursive tasks"]
    end
```

### Configurare ThreadPoolExecutor

```mermaid
graph LR
    subgraph "ThreadPoolExecutor Parameters"
        TPE[ThreadPoolExecutor]
        TPE --> CORE["corePoolSize<br/>Minimum threads"]
        TPE --> MAX["maxPoolSize<br/>Maximum threads"]
        TPE --> ALIVE["keepAliveTime<br/>Idle timeout"]
        TPE --> QUEUE["workQueue<br/>Task queue"]
        TPE --> HANDLER["rejectionHandler<br/>When full"]
    end
```

```java
// Custom ThreadPoolExecutor
ThreadPoolExecutor executor = new ThreadPoolExecutor(
    4,                      // corePoolSize
    8,                      // maxPoolSize
    60L, TimeUnit.SECONDS,  // keepAliveTime
    new ArrayBlockingQueue<>(100),  // bounded queue
    new ThreadPoolExecutor.CallerRunsPolicy() // rejection policy
);

// Rejection Policies:
// - AbortPolicy: throw RejectedExecutionException (default)
// - CallerRunsPolicy: caller thread executes task
// - DiscardPolicy: silently discard
// - DiscardOldestPolicy: discard oldest, retry
```

---

# 📦 SECȚIUNEA 2: SINCRONIZARE

## Q191-Q200: Race Conditions și Locks

### Race Condition Visualization

```mermaid
sequenceDiagram
    participant T1 as Thread 1
    participant Mem as counter = 0
    participant T2 as Thread 2
    
    Note over T1, T2: Race Condition: counter++
    
    T1->>Mem: READ counter (0)
    T2->>Mem: READ counter (0)
    T1->>T1: ADD 1 (result: 1)
    T2->>T2: ADD 1 (result: 1)
    T1->>Mem: WRITE counter = 1
    T2->>Mem: WRITE counter = 1
    
    Note over Mem: Expected: 2, Actual: 1 💥
```

### synchronized Keyword

```mermaid
graph TB
    subgraph "synchronized Method"
        SM["synchronized void method()"]
        SM --> SM1["Lock pe 'this'"]
        SM --> SM2["Tot body-ul protejat"]
    end
    
    subgraph "synchronized Block"
        SB["synchronized(lockObject) { }"]
        SB --> SB1["Lock explicit"]
        SB --> SB2["Granularitate fină"]
        SB --> SB3["Preferred ✅"]
    end
    
    subgraph "static synchronized"
        SS["static synchronized method()"]
        SS --> SS1["Lock pe Class object"]
        SS --> SS2["Toate instanțele blocked"]
    end
```

```java
public class Counter {
    private int count = 0;
    private final Object lock = new Object();
    
    // ❌ Synchronized pe metodă - lock pe 'this'
    public synchronized void incrementBad() {
        count++;
    }
    
    // ✅ Synchronized block - granularitate fină
    public void increment() {
        synchronized (lock) {
            count++;
        }
    }
    
    // ✅ Sau folosește AtomicInteger
    private final AtomicInteger atomicCount = new AtomicInteger(0);
    
    public void incrementAtomic() {
        atomicCount.incrementAndGet(); // Lock-free!
    }
}
```

---

## Q196-Q199: Deadlock, Livelock, Starvation

### Deadlock - Cele 4 Condiții Coffman

```mermaid
graph TB
    subgraph "4 Condiții NECESARE pentru Deadlock"
        C1["1. Mutual Exclusion<br/>Resursa e exclusivă"]
        C2["2. Hold and Wait<br/>Ține o resursă, așteaptă alta"]
        C3["3. No Preemption<br/>Nu poate fi forțat să elibereze"]
        C4["4. Circular Wait<br/>Lanț circular de așteptare"]
    end
    
    C1 --> D[DEADLOCK]
    C2 --> D
    C3 --> D
    C4 --> D
```

### Deadlock Example

```mermaid
sequenceDiagram
    participant T1 as Thread 1
    participant LA as Lock A
    participant LB as Lock B
    participant T2 as Thread 2
    
    T1->>LA: acquire Lock A ✅
    T2->>LB: acquire Lock B ✅
    T1->>LB: waiting for Lock B... 🔄
    T2->>LA: waiting for Lock A... 🔄
    
    Note over T1, T2: 💀 DEADLOCK - ambele blocked forever
```

```java
// ❌ Deadlock prone
public void transfer(Account from, Account to, int amount) {
    synchronized (from) {  // Lock 1
        synchronized (to) { // Lock 2 - ordine diferită = deadlock!
            from.debit(amount);
            to.credit(amount);
        }
    }
}

// ✅ Fix: Ordine consistentă a lock-urilor
public void transferSafe(Account from, Account to, int amount) {
    Account first = from.getId() < to.getId() ? from : to;
    Account second = from.getId() < to.getId() ? to : from;
    
    synchronized (first) {
        synchronized (second) {
            from.debit(amount);
            to.credit(amount);
        }
    }
}
```

### Livelock vs Starvation

```mermaid
graph TB
    subgraph "Livelock"
        LL1["Thread-urile sunt ACTIVE"]
        LL2["Dar NU fac progres"]
        LL3["Se cedează reciproc"]
        LL1 --> LL2 --> LL3
    end
    
    subgraph "Starvation"
        ST1["Thread cu prioritate mică"]
        ST2["Nu primește niciodată CPU"]
        ST3["Thread-uri prioritare domină"]
        ST1 --> ST2 --> ST3
    end
```

---

# 📦 SECȚIUNEA 3: JAVA MEMORY MODEL

## Q201-Q203: volatile și happens-before

### Java Memory Model

```mermaid
graph TB
    subgraph "Thread 1"
        T1C[CPU Cache 1]
        T1R[Registers]
    end
    
    subgraph "Thread 2"
        T2C[CPU Cache 2]
        T2R[Registers]
    end
    
    subgraph "Main Memory"
        MM[Shared Variables]
    end
    
    T1C <--> MM
    T2C <--> MM
    T1R <--> T1C
    T2R <--> T2C
    
    Note["Problema: Cache-urile pot fi DESINCRONIZATE!"]
```

### volatile Guarantees

```mermaid
graph LR
    subgraph "volatile"
        V1["✅ VISIBILITY<br/>Toate thread-urile văd ultima valoare"]
        V2["✅ ORDERING<br/>Happens-before relationship"]
        V3["❌ ATOMICITY<br/>volatile++ NU e atomic!"]
    end
```

```java
// ❌ Visibility problem fără volatile
class BrokenVisibility {
    private boolean running = true; // Cache-uit local!
    
    public void stop() { running = false; }
    
    public void run() {
        while (running) { // Poate să nu vadă niciodată false!
            // ...
        }
    }
}

// ✅ Cu volatile - visibility garantată
class FixedVisibility {
    private volatile boolean running = true;
    
    public void stop() { running = false; } // Scrie în main memory
    
    public void run() {
        while (running) { // Citește din main memory
            // ...
        }
    }
}

// ⚠️ volatile NU garantează atomicitate!
private volatile int counter = 0;
counter++; // NU E ATOMIC! (read-modify-write)
// Folosește AtomicInteger pentru operații atomice
```

### Happens-Before Relationships

```mermaid
graph TB
    subgraph "Happens-Before Rules"
        R1["1. Program order<br/>În același thread"]
        R2["2. Monitor lock<br/>unlock → lock"]
        R3["3. volatile<br/>write → read"]
        R4["4. Thread start<br/>start() → run()"]
        R5["5. Thread join<br/>run() end → join() return"]
        R6["6. Transitivity<br/>A→B și B→C implică A→C"]
    end
```

---

# 📦 SECȚIUNEA 4: LOCK-URI AVANSATE

## Q207-Q210: ReentrantLock și variante

### Lock Types Comparison

```mermaid
graph TB
    subgraph "synchronized"
        S1["Simplu"]
        S2["Reentrant"]
        S3["❌ Fără timeout"]
        S4["❌ Fără try-lock"]
        S5["❌ Fără fairness"]
    end
    
    subgraph "ReentrantLock"
        R1["Flexibil"]
        R2["Reentrant"]
        R3["✅ Timeout support"]
        R4["✅ tryLock()"]
        R5["✅ Fair option"]
        R6["✅ Multiple conditions"]
    end
    
    subgraph "ReadWriteLock"
        RW1["Read: shared"]
        RW2["Write: exclusive"]
        RW3["Multiple readers OK"]
        RW4["Writer blocks all"]
    end
    
    subgraph "StampedLock"
        SL1["Optimistic reads"]
        SL2["Cel mai performant"]
        SL3["Nu e reentrant!"]
    end
```

```java
// ReentrantLock usage
private final ReentrantLock lock = new ReentrantLock(true); // fair=true

public void safeMethod() {
    lock.lock();
    try {
        // critical section
    } finally {
        lock.unlock(); // ÎNTOTDEAUNA în finally!
    }
}

// tryLock cu timeout
public boolean tryOperation() {
    try {
        if (lock.tryLock(1, TimeUnit.SECONDS)) {
            try {
                // operație
                return true;
            } finally {
                lock.unlock();
            }
        }
        return false; // Nu am obținut lock-ul
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
        return false;
    }
}

// ReadWriteLock pentru read-heavy scenarios
private final ReadWriteLock rwLock = new ReentrantReadWriteLock();
private final Lock readLock = rwLock.readLock();
private final Lock writeLock = rwLock.writeLock();

public Data read() {
    readLock.lock();
    try {
        return data; // Multiple readers simultani
    } finally {
        readLock.unlock();
    }
}

public void write(Data newData) {
    writeLock.lock();
    try {
        data = newData; // Exclusive access
    } finally {
        writeLock.unlock();
    }
}
```

---

# 📦 SECȚIUNEA 5: SYNCHRONIZERS

## Q211-Q213: CountDownLatch, CyclicBarrier, Semaphore

### Synchronizers Comparison

```mermaid
graph TB
    subgraph "CountDownLatch"
        CDL["One-shot<br/>count down to 0"]
        CDL --> CDL1["await() blocks"]
        CDL --> CDL2["countDown() decrements"]
        CDL --> CDL3["Nu poate fi refolosit"]
    end
    
    subgraph "CyclicBarrier"
        CB["Reusable<br/>all parties arrive"]
        CB --> CB1["await() blocks"]
        CB --> CB2["Releases when N arrive"]
        CB --> CB3["Poate fi refolosit"]
    end
    
    subgraph "Semaphore"
        SEM["Permits counter"]
        SEM --> SEM1["acquire() takes permit"]
        SEM --> SEM2["release() returns permit"]
        SEM --> SEM3["Rate limiting"]
    end
```

```java
// CountDownLatch - așteptare pentru N evenimente
CountDownLatch latch = new CountDownLatch(3);

// Worker threads
for (int i = 0; i < 3; i++) {
    executor.submit(() -> {
        doWork();
        latch.countDown(); // Decrement
    });
}

latch.await(); // Main thread waits for all 3
System.out.println("All done!");

// CyclicBarrier - sincronizare repetată
CyclicBarrier barrier = new CyclicBarrier(3, () -> {
    System.out.println("All arrived, proceeding!");
});

// Each thread
barrier.await(); // Blocks until 3 threads arrive

// Semaphore - limită de concurrency
Semaphore semaphore = new Semaphore(10); // Max 10 concurrent

public void limitedAccess() {
    try {
        semaphore.acquire();
        // Only 10 threads here at once
        accessResource();
    } finally {
        semaphore.release();
    }
}
```

---

# 📦 SECȚIUNEA 6: COMPLETABLEFUTURE

## Q214-Q215: Async Programming

### CompletableFuture Pipeline

```mermaid
graph LR
    A["supplyAsync()"] --> B["thenApply()"]
    B --> C["thenApply()"]
    C --> D["thenAccept()"]
    
    E["supplyAsync()"] --> F["thenCombine()"]
    A --> F
    F --> G["thenCompose()"]
    
    style A fill:#90EE90
    style E fill:#90EE90
```

### Composition Methods

```mermaid
graph TB
    subgraph "Transformation"
        T1["thenApply(fn)<br/>T → U"]
        T2["thenCompose(fn)<br/>T → CompletableFuture U"]
        T3["thenCombine(other, fn)<br/>T + U → V"]
    end
    
    subgraph "Consumption"
        C1["thenAccept(consumer)<br/>T → void"]
        C2["thenRun(runnable)<br/>() → void"]
    end
    
    subgraph "Error Handling"
        E1["exceptionally(fn)<br/>Exception → T"]
        E2["handle(fn)<br/>(T, Exception) → U"]
        E3["whenComplete(fn)<br/>(T, Exception) → void"]
    end
```

```java
// Pipeline async
CompletableFuture<String> future = CompletableFuture
    .supplyAsync(() -> fetchData())           // Async fetch
    .thenApply(data -> transform(data))       // Transform
    .thenApply(result -> format(result))      // Format
    .exceptionally(ex -> "Error: " + ex);     // Error handling

// Combining futures
CompletableFuture<User> userFuture = fetchUserAsync(userId);
CompletableFuture<List<Order>> ordersFuture = fetchOrdersAsync(userId);

CompletableFuture<UserProfile> profile = userFuture
    .thenCombine(ordersFuture, (user, orders) -> 
        new UserProfile(user, orders)
    );

// Parallel execution cu allOf
CompletableFuture<Void> allDone = CompletableFuture.allOf(
    task1, task2, task3
);
allDone.thenRun(() -> System.out.println("All completed!"));

// anyOf - primul care termină
CompletableFuture<Object> fastest = CompletableFuture.anyOf(
    fetchFromServer1(),
    fetchFromServer2(),
    fetchFromServer3()
);
```

---

# 📦 SECȚIUNEA 7: THREAD SAFETY PATTERNS

## Q218-Q222: Strategii de Thread Safety

### Thread Safety Decision Tree

```mermaid
flowchart TD
    A[Ai stare partajată?] -->|Nu| B[✅ Thread-safe by default]
    A -->|Da| C{Starea e mutabilă?}
    
    C -->|Nu| D[✅ Immutable - safe]
    C -->|Da| E{O singură variabilă?}
    
    E -->|Da| F{Operație atomică simplă?}
    E -->|Nu| G[synchronized / Lock]
    
    F -->|Da| H[Atomic classes]
    F -->|Nu| G
    
    G --> I{Read-heavy?}
    I -->|Da| J[ReadWriteLock]
    I -->|Nu| K[ReentrantLock / synchronized]
```

### Thread Safety Strategies

```java
// 1. IMMUTABILITY - cea mai bună strategie
public final class ImmutableUser {
    private final String name;
    private final List<String> roles;
    
    public ImmutableUser(String name, List<String> roles) {
        this.name = name;
        this.roles = List.copyOf(roles); // Defensive copy
    }
    
    public ImmutableUser withName(String newName) {
        return new ImmutableUser(newName, this.roles);
    }
}

// 2. THREAD CONFINEMENT - ThreadLocal
private static final ThreadLocal<DateFormat> dateFormat =
    ThreadLocal.withInitial(() -> new SimpleDateFormat("yyyy-MM-dd"));

public String format(Date date) {
    return dateFormat.get().format(date); // Each thread has its own
}

// 3. ATOMIC CLASSES - pentru contoare simple
private final AtomicLong counter = new AtomicLong();
public long increment() {
    return counter.incrementAndGet();
}

// 4. CONCURRENT COLLECTIONS - pentru colecții partajate
private final ConcurrentHashMap<String, User> cache = new ConcurrentHashMap<>();
public User getOrLoad(String id) {
    return cache.computeIfAbsent(id, this::loadFromDb);
}
```

---

# 🎯 ANTI-PATTERNS & BEST PRACTICES

### Common Mistakes

```java
// ❌ Double-Checked Locking GREȘIT (pre-Java 5)
private static Singleton instance;
public static Singleton getInstance() {
    if (instance == null) {              // Check 1
        synchronized (Singleton.class) {
            if (instance == null) {      // Check 2
                instance = new Singleton(); // NOT SAFE fără volatile!
            }
        }
    }
    return instance;
}

// ✅ Double-Checked Locking CORECT
private static volatile Singleton instance;

// ✅ SAU: Initialization-on-demand holder (preferred)
public class Singleton {
    private Singleton() {}
    
    private static class Holder {
        static final Singleton INSTANCE = new Singleton();
    }
    
    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}

// ❌ Synchronized pe String literal
synchronized ("lock") { } // GREȘIT - String pool sharing!

// ✅ Lock object privat
private final Object lock = new Object();
synchronized (lock) { }
```

---

# 🎯 CHEAT SHEET CONCURRENCY

| Problemă | Soluție | Când folosești |
|----------|---------|----------------|
| Counter simplu | `AtomicLong` | Incrementări atomice |
| Flag stop | `volatile boolean` | Un writer, mulți readers |
| Cache thread-safe | `ConcurrentHashMap` | Read-heavy, writes moderate |
| Queue producer-consumer | `BlockingQueue` | Decuplare threads |
| Wait for N tasks | `CountDownLatch` | One-time sync |
| Sync N threads | `CyclicBarrier` | Repeated sync points |
| Limit concurrency | `Semaphore` | Resource pooling |
| Async pipeline | `CompletableFuture` | Non-blocking composition |
| Read-heavy data | `ReadWriteLock` | Mulți readers, puțini writers |

---

> 💡 **Regula de Aur Concurrency:**  
> *"Evită starea partajată dacă poți. Dacă nu poți, fă-o IMMUTABLE. Dacă nu poți, SINCRONIZEAZĂ corect."*
