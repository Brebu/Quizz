# Capitolul 4 – JVM, Garbage Collection & Performance
## Q261–Q340 — Nivel Senior

> 📚 **Scop:** Interviuri Senior / Lead / Staff  
> 🎯 **Focus:** JVM Internals, GC Tuning, Performance  
> 💾 **Encoding:** UTF-8

---

## 🎯 HARTA MENTALĂ JVM

```mermaid
mindmap
  root((JVM & Performance))
    JVM Architecture
      ClassLoader
      Runtime Data Areas
      Execution Engine
      Native Interface
    Memory Areas
      Heap
      Stack
      Metaspace
      Native Memory
    Garbage Collection
      Generational GC
      GC Algorithms
      GC Tuning
    Performance
      JIT Compilation
      Profiling
      Optimization
    Tools
      JFR
      JMC
      VisualVM
      jstat/jmap/jstack
```

---

# 📦 SECȚIUNEA 1: JVM ARCHITECTURE

## Q261: Ce este JVM și care este rolul ei?

### JVM Architecture Overview

```mermaid
graph TB
    subgraph "Source"
        SRC[".java files"]
    end
    
    subgraph "Compile Time"
        SRC --> JAVAC["javac<br/>(compiler)"]
        JAVAC --> BC[".class files<br/>(bytecode)"]
    end
    
    subgraph "JVM Runtime"
        BC --> CL["ClassLoader<br/>Subsystem"]
        
        CL --> RDA["Runtime Data Areas"]
        
        subgraph RDA["Runtime Data Areas"]
            HEAP["Heap<br/>(shared)"]
            META["Metaspace<br/>(shared)"]
            STACK["Stack<br/>(per thread)"]
            PC["PC Register<br/>(per thread)"]
            NMS["Native Stack<br/>(per thread)"]
        end
        
        RDA --> EE["Execution Engine"]
        
        subgraph EE["Execution Engine"]
            INT["Interpreter"]
            JIT["JIT Compiler"]
            GC["Garbage Collector"]
        end
    end
    
    EE --> NI["Native Interface (JNI)"]
    NI --> NL["Native Libraries"]
```

**Răspuns Complet:**
JVM (Java Virtual Machine) este motorul de execuție care:
- **Abstractizează hardware-ul** - "Write once, run anywhere"
- **Gestionează memoria** - Alocare automată + Garbage Collection
- **Execută bytecode** - Interpretare + JIT compilation
- **Oferă securitate** - Sandboxing, verificare bytecode

---

## Q264-Q265: ClassLoader System

### ClassLoader Hierarchy (Delegation Model)

```mermaid
graph TB
    subgraph "ClassLoader Hierarchy"
        BCL["🔷 Bootstrap ClassLoader<br/>rt.jar, core Java classes<br/>(implemented in native code)"]
        
        PCL["🔶 Platform ClassLoader<br/>(Java 9+, fost Extension)<br/>java.sql, java.xml, etc."]
        
        ACL["🔷 Application ClassLoader<br/>Classpath classes<br/>Your application code"]
        
        CCL["⚪ Custom ClassLoader<br/>User-defined<br/>Hot reload, plugins"]
    end
    
    CCL -->|"2. delegate to parent"| ACL
    ACL -->|"2. delegate to parent"| PCL
    PCL -->|"2. delegate to parent"| BCL
    
    BCL -->|"3. if not found"| PCL
    PCL -->|"3. if not found"| ACL
    ACL -->|"3. if not found"| CCL
```

**Parent-First Delegation:**
1. ClassLoader primește cerere de încărcare
2. Delegă la parent (recursiv până la Bootstrap)
3. Dacă parent-ul nu găsește, încearcă singur

```java
// Verificare ClassLoader
public class ClassLoaderDemo {
    public static void main(String[] args) {
        // Bootstrap - returnează null (e nativ)
        System.out.println(String.class.getClassLoader()); 
        // null
        
        // Platform ClassLoader
        System.out.println(java.sql.Driver.class.getClassLoader());
        // jdk.internal.loader.ClassLoaders$PlatformClassLoader
        
        // Application ClassLoader  
        System.out.println(ClassLoaderDemo.class.getClassLoader());
        // jdk.internal.loader.ClassLoaders$AppClassLoader
    }
}
```

---

## Q266-Q267: Metaspace (înlocuiește PermGen)

```mermaid
graph LR
    subgraph "Java 7 și anterior"
        PG["PermGen<br/>Fixed size<br/>Part of Heap<br/>-XX:MaxPermSize"]
    end
    
    subgraph "Java 8+"
        MS["Metaspace<br/>Auto-grows<br/>Native memory<br/>-XX:MaxMetaspaceSize"]
    end
    
    PG -->|"Java 8"| MS
```

**De ce PermGen a fost eliminat:**
- Dimensiune fixă → OutOfMemoryError frecvent
- Greu de tunat corect
- Hot deploy problematic

**Metaspace conține:**
- Class metadata
- Method metadata  
- Constant pool
- Annotations

---

## Q268-Q271: Memory Structure

### JVM Memory Layout Detaliat

```mermaid
graph TB
    subgraph "HEAP (Shared, GC-managed)"
        subgraph "Young Generation"
            EDEN["Eden Space<br/>New objects allocated here"]
            S0["Survivor 0<br/>(From)"]
            S1["Survivor 1<br/>(To)"]
        end
        
        subgraph "Old Generation"
            OLD["Tenured Space<br/>Long-lived objects"]
        end
    end
    
    subgraph "NON-HEAP"
        META["Metaspace<br/>Class metadata"]
        CCS["Compressed Class Space"]
        CC["Code Cache<br/>JIT compiled code"]
    end
    
    subgraph "PER-THREAD (Not shared)"
        STK["JVM Stack<br/>Method frames<br/>Local variables"]
        PC["PC Register<br/>Current instruction"]
        NMS["Native Method Stack"]
    end
```

### Heap vs Stack Comparison

```mermaid
graph TB
    subgraph "HEAP"
        H1["✓ Stores: Objects, arrays"]
        H2["✓ Shared: Between all threads"]
        H3["✓ Managed: By Garbage Collector"]
        H4["✓ Size: -Xms, -Xmx"]
        H5["✗ Error: OutOfMemoryError"]
        H6["✓ Lifetime: Until unreachable"]
    end
    
    subgraph "STACK"
        S1["✓ Stores: Frames, locals, refs"]
        S2["✓ Scope: Per thread (isolated)"]
        S3["✓ Managed: Automatic (LIFO)"]
        S4["✓ Size: -Xss"]
        S5["✗ Error: StackOverflowError"]
        S6["✓ Lifetime: Method execution"]
    end
```

```java
public void memoryDemo() {
    // STACK: primitive locală
    int counter = 10;
    
    // STACK: referință (8 bytes pe 64-bit)
    // HEAP: obiectul String
    String name = "Alexandru";
    
    // STACK: referință
    // HEAP: obiectul User + toate field-urile sale
    User user = new User("Alex", 30);
    
    // STACK: referință
    // HEAP: array object + 100 Integer objects
    List<Integer> numbers = new ArrayList<>(100);
}

// Stack frame pentru metodă conține:
// 1. Local Variable Array (counter, name ref, user ref, numbers ref)
// 2. Operand Stack (pentru calcule)
// 3. Frame Data (return address, exception table ref)
```

---

# 📦 SECȚIUNEA 2: GARBAGE COLLECTION

## Q272-Q275: GC Fundamentals

### Ce este Garbage Collection?

```mermaid
graph LR
    subgraph "Fără GC (C/C++)"
        M1["malloc()"] --> M2["use memory"]
        M2 --> M3["free()"]
        M3 --> M4["❌ Forget = Memory Leak"]
        M3 --> M5["❌ Double free = Crash"]
    end
    
    subgraph "Cu GC (Java)"
        J1["new Object()"] --> J2["use object"]
        J2 --> J3["object unreachable"]
        J3 --> J4["✅ GC cleans automatically"]
    end
```

### Object Reachability (GC Roots)

```mermaid
graph TB
    subgraph "GC Roots"
        R1["📍 Local variables<br/>(stack frames)"]
        R2["📍 Static fields<br/>(class variables)"]
        R3["📍 Active threads"]
        R4["📍 JNI references"]
    end
    
    subgraph "Heap Objects"
        A["Object A<br/>✅ REACHABLE"]
        B["Object B<br/>✅ REACHABLE"]
        C["Object C<br/>✅ REACHABLE"]
        D["Object D<br/>❌ UNREACHABLE"]
        E["Object E<br/>❌ UNREACHABLE"]
        F["Object F<br/>❌ UNREACHABLE"]
    end
    
    R1 --> A
    R2 --> B
    A --> C
    
    D --> E
    E --> F
    F --> D
    
    style D fill:#FFB6C1
    style E fill:#FFB6C1
    style F fill:#FFB6C1
```

**Regula:** Un obiect este LIVE dacă este accesibil (direct sau indirect) din orice GC Root.

---

## Q276-Q277: Reference Types

```mermaid
graph TB
    subgraph "Reference Strength (Strong → Weak)"
        STRONG["🔴 Strong Reference<br/>Object obj = new Object()<br/>GC: NEVER collected while reachable"]
        
        SOFT["🟠 Soft Reference<br/>SoftReference&lt;Object&gt;<br/>GC: Collected at memory pressure<br/>Use: Memory-sensitive caches"]
        
        WEAK["🟡 Weak Reference<br/>WeakReference&lt;Object&gt;<br/>GC: Collected at any GC<br/>Use: WeakHashMap, canonicalizing"]
        
        PHANTOM["⚪ Phantom Reference<br/>PhantomReference&lt;Object&gt;<br/>GC: After finalization<br/>Use: Pre-mortem cleanup"]
    end
    
    STRONG --> SOFT --> WEAK --> PHANTOM
```

```java
// Strong Reference - normal
Object strong = new Object(); // Nu se colectează cât strong != null

// Soft Reference - cache memory-sensitive
SoftReference<byte[]> cache = new SoftReference<>(new byte[1024*1024]);
byte[] data = cache.get(); // poate fi null dacă GC a curățat

// Weak Reference - nu previne GC
WeakReference<Object> weak = new WeakReference<>(new Object());
// Obiectul poate fi colectat la orice GC

// WeakHashMap - cheile sunt weak references
Map<Key, Value> cache = new WeakHashMap<>();
// Entry-urile dispar când cheia nu mai e referențiată altundeva
```

---

## Q278-Q282: Generational GC

### Generational Hypothesis

```mermaid
graph LR
    subgraph "Observație Empirică"
        OBS["Majoritatea obiectelor<br/>mor TINERE"]
    end
    
    subgraph "Implicație"
        IMP1["Young Gen: colectat frecvent, rapid"]
        IMP2["Old Gen: colectat rar, mai lent"]
    end
    
    OBS --> IMP1
    OBS --> IMP2
```

### Object Lifecycle în Generational GC

```mermaid
sequenceDiagram
    participant App as Application
    participant E as Eden
    participant S0 as Survivor 0
    participant S1 as Survivor 1
    participant O as Old Gen
    
    Note over E: Toate obiectele noi aici
    
    App->>E: new Object() #1
    App->>E: new Object() #2
    App->>E: new Object() #3
    
    Note over E: Eden FULL → Minor GC
    
    rect rgb(255, 200, 200)
        Note over E,S0: Minor GC #1
        E->>S0: Survivors (age=1)
        Note over E: Dead objects cleared
    end
    
    App->>E: new Object() #4
    App->>E: new Object() #5
    
    Note over E: Eden FULL → Minor GC
    
    rect rgb(255, 200, 200)
        Note over E,S1: Minor GC #2
        E->>S1: New survivors (age=1)
        S0->>S1: Old survivors (age=2)
        Note over S0: S0 cleared
    end
    
    Note over S1: Objects reach age threshold (default 15)
    
    rect rgb(200, 200, 255)
        S1->>O: PROMOTION to Old Gen
    end
```

### Minor GC vs Major GC vs Full GC

```mermaid
graph TB
    subgraph "Minor GC (Young GC)"
        MN1["📍 Scope: Young Generation only"]
        MN2["⚡ Frequency: Very frequent"]
        MN3["⏱️ Duration: Milliseconds"]
        MN4["🛑 STW: Short pause"]
        MN5["✅ Impact: Low"]
    end
    
    subgraph "Major GC (Old GC)"
        MJ1["📍 Scope: Old Generation"]
        MJ2["⚡ Frequency: Less frequent"]
        MJ3["⏱️ Duration: Longer"]
        MJ4["🛑 STW: Noticeable pause"]
        MJ5["⚠️ Impact: Medium"]
    end
    
    subgraph "Full GC"
        FG1["📍 Scope: ENTIRE Heap + Metaspace"]
        FG2["⚡ Frequency: Rare (should be!)"]
        FG3["⏱️ Duration: Seconds possible"]
        FG4["🛑 STW: MAJOR pause"]
        FG5["❌ Impact: HIGH - avoid!"]
    end
    
    style FG1 fill:#FFB6C1
    style FG4 fill:#FFB6C1
    style FG5 fill:#FFB6C1
```

---

## Q286-Q292: GC Algorithms

### GC Algorithms Comparison

```mermaid
graph TB
    subgraph SerialGC["Serial GC"]
        SER["Single-threaded<br/>-XX:+UseSerialGC"]
        SER --> SER1["✅ Simple, low overhead"]
        SER --> SER2["✅ Good for small heaps"]
        SER --> SER3["❌ Long STW pauses"]
        SER --> SER4["📱 Use: Client apps, containers"]
    end
    
    subgraph ParallelGC["Parallel GC"]
        PAR["Multi-threaded<br/>-XX:+UseParallelGC"]
        PAR --> PAR1["✅ High throughput"]
        PAR --> PAR2["✅ Good for batch"]
        PAR --> PAR3["❌ STW pauses"]
        PAR --> PAR4["🖥️ Use: Batch processing"]
    end
    
    subgraph G1GC["G1 GC (Default Java 9+)"]
        G1["Region-based<br/>-XX:+UseG1GC"]
        G1 --> G11["✅ Predictable pauses"]
        G1 --> G12["✅ Good balance"]
        G1 --> G13["✅ Large heaps OK"]
        G1 --> G14["🌐 Use: Most applications"]
    end
    
    subgraph ZGCGC["ZGC"]
        Z["Concurrent<br/>-XX:+UseZGC"]
        Z --> Z1["✅ Sub-ms pauses"]
        Z --> Z2["✅ TB-scale heaps"]
        Z --> Z3["⚠️ Some CPU overhead"]
        Z --> Z4["💹 Use: Low-latency critical"]
    end
    
    style SerialGC fill:#ffebee
    style ParallelGC fill:#e3f2fd
    style G1GC fill:#e8f5e9
    style ZGCGC fill:#fff3e0
```

### G1 GC Region-Based Architecture

```mermaid
graph TB
    subgraph "G1 Heap = Grid of Regions"
        R1["E"] 
        R2["E"]
        R3["E"]
        R4["S"]
        R5["O"]
        R6["O"]
        R7["O"]
        R8["H"]
        R9["H"]
        R10["—"]
        R11["O"]
        R12["E"]
        R13["—"]
        R14["O"]
        R15["S"]
        R16["—"]
    end
    
    Legend["E=Eden 🟢  S=Survivor 🟡  O=Old 🔵  H=Humongous 🟣  —=Free ⚪"]
    
    style R1 fill:#90EE90
    style R2 fill:#90EE90
    style R3 fill:#90EE90
    style R12 fill:#90EE90
    style R4 fill:#FFFF00
    style R15 fill:#FFFF00
    style R5 fill:#87CEEB
    style R6 fill:#87CEEB
    style R7 fill:#87CEEB
    style R11 fill:#87CEEB
    style R14 fill:#87CEEB
    style R8 fill:#DDA0DD
    style R9 fill:#DDA0DD
```

**G1 Key Features:**
- Heap împărțit în regiuni egale (1-32MB)
- Colectează regiunile cu cel mai mult "garbage" first
- Target pause time: `-XX:MaxGCPauseMillis=200`

### When to Use Which GC

```mermaid
flowchart TD
    A["Choose GC"] --> B{"Heap Size?"}
    
    B -->|"< 100MB"| C["Serial GC"]
    B -->|"100MB - 4GB"| D{"Latency matters?"}
    B -->|"4GB - 32GB"| E["G1 GC ✅"]
    B -->|"> 32GB"| F{"Max pause tolerance?"}
    
    D -->|"No, throughput"| G["Parallel GC"]
    D -->|"Yes"| E
    
    F -->|"< 1ms required"| H["ZGC"]
    F -->|"< 10ms OK"| I["ZGC or Shenandoah"]
    F -->|"Throughput priority"| J["Parallel GC"]
    
    style E fill:#90EE90
    style H fill:#90EE90
```

---

# 📦 SECȚIUNEA 3: MEMORY ISSUES

## Q294-Q297: OutOfMemoryError Types

```mermaid
graph TB
    OOM["OutOfMemoryError"]
    
    OOM --> HEAP["Java heap space<br/>━━━━━━━━━━━━━━<br/>Heap is full<br/>Objects can't be allocated"]
    
    OOM --> META["Metaspace<br/>━━━━━━━━━━━━━━<br/>Too many classes loaded<br/>Dynamic class generation"]
    
    OOM --> GCOH["GC overhead limit exceeded<br/>━━━━━━━━━━━━━━<br/>98% time in GC<br/>< 2% heap recovered"]
    
    OOM --> NATIVE["Direct buffer memory<br/>━━━━━━━━━━━━━━<br/>NIO direct buffers<br/>Native memory exhausted"]
    
    OOM --> THREAD["Unable to create native thread<br/>━━━━━━━━━━━━━━<br/>OS thread limit reached"]
    
    HEAP --> FIX1["Fix: -Xmx increase<br/>Fix: Check memory leak"]
    META --> FIX2["Fix: -XX:MaxMetaspaceSize<br/>Fix: Check class loading"]
    GCOH --> FIX3["Fix: Memory leak likely<br/>Fix: Increase heap"]
```

## Q294-Q295: Memory Leak Patterns

```mermaid
graph TB
    subgraph "Common Memory Leak Causes"
        ML1["📍 Static Collections<br/>care cresc nelimitat"]
        ML2["📍 Listeners/Callbacks<br/>neînregistrați"]
        ML3["📍 Cache fără eviction<br/>sau size limit"]
        ML4["📍 ThreadLocal<br/>fără cleanup în thread pools"]
        ML5["📍 Unclosed Resources<br/>streams, connections"]
        ML6["📍 Inner Class References<br/>la outer class"]
    end
```

```java
// ❌ MEMORY LEAK: Static collection grows forever
public class LeakyService {
    // Crește la infinit!
    private static final List<Event> eventLog = new ArrayList<>();
    
    public void logEvent(Event e) {
        eventLog.add(e); // Never removed!
    }
}

// ✅ FIX: Bounded cache
public class FixedService {
    private static final int MAX_SIZE = 1000;
    private static final Queue<Event> eventLog = new LinkedBlockingQueue<>(MAX_SIZE);
    
    public void logEvent(Event e) {
        if (!eventLog.offer(e)) {
            eventLog.poll(); // Remove oldest
            eventLog.offer(e);
        }
    }
}

// ❌ MEMORY LEAK: ThreadLocal in thread pool
public class LeakyThreadLocal {
    private static final ThreadLocal<HeavyObject> context = new ThreadLocal<>();
    
    public void process() {
        context.set(new HeavyObject());
        doWork();
        // FORGOT context.remove()!
        // Thread goes back to pool with HeavyObject still attached
    }
}

// ✅ FIX: Always remove in finally
public void processSafe() {
    try {
        context.set(new HeavyObject());
        doWork();
    } finally {
        context.remove(); // CRITICAL in thread pools!
    }
}

// ❌ MEMORY LEAK: Listener not removed
public class LeakyObserver {
    public void init() {
        eventBus.register(this); // Registered
    }
    // No unregister on destroy → this object never GC'd
}

// ✅ FIX: Proper lifecycle
public class FixedObserver implements AutoCloseable {
    public void init() {
        eventBus.register(this);
    }
    
    @Override
    public void close() {
        eventBus.unregister(this); // Clean up!
    }
}
```

---

# 📦 SECȚIUNEA 4: JIT COMPILATION

## Q299-Q305: JIT Compiler

### Tiered Compilation

```mermaid
graph LR
    subgraph "Execution Modes"
        INT["Interpreter<br/>Bytecode direct<br/>No optimization"]
        
        C1["C1 Compiler<br/>(Client)<br/>Fast compile<br/>Basic optimizations"]
        
        C2["C2 Compiler<br/>(Server)<br/>Slow compile<br/>Aggressive optimizations"]
    end
    
    INT -->|"Method called"| INT
    INT -->|"Invocation threshold"| C1
    C1 -->|"Hot method"| C2
    
    PROF["Profiling Data"] --> C1
    PROF --> C2
```

### JIT Optimization Techniques

```mermaid
graph TB
    subgraph "Key JIT Optimizations"
        OPT1["🔹 Method Inlining<br/>Replace call with method body<br/>Eliminate call overhead"]
        
        OPT2["🔹 Escape Analysis<br/>Object doesn't escape method?<br/>→ Allocate on STACK, not heap"]
        
        OPT3["🔹 Loop Unrolling<br/>Reduce loop iterations<br/>More code, fewer jumps"]
        
        OPT4["🔹 Dead Code Elimination<br/>Remove unreachable code"]
        
        OPT5["🔹 Lock Elision<br/>Remove locks on thread-local objects"]
        
        OPT6["🔹 Null Check Elimination<br/>Proven non-null → skip check"]
    end
```

```java
// Escape Analysis Example
public long sumPoints() {
    long sum = 0;
    for (int i = 0; i < 1000; i++) {
        // Point nu "scapă" din metodă
        // JIT poate aloca pe STACK (nu heap)
        // → No GC pressure!
        Point p = new Point(i, i * 2);
        sum += p.x + p.y;
    }
    return sum;
}

// Lock Elision Example
public void processLocal() {
    // Lock object e local, nu e partajat
    Object lock = new Object();
    synchronized (lock) {
        // JIT elimină lock-ul complet
        // pentru că lock nu e partajat
        doWork();
    }
}

// Method Inlining Example
public int calculate(int x) {
    return double(x) + 1; // Call to double()
}
private int double(int x) {
    return x * 2;
}
// After inlining:
// public int calculate(int x) {
//     return (x * 2) + 1;  // No method call
// }
```

---

# 📦 SECȚIUNEA 5: PROFILING & TOOLS

## Q306-Q311: Diagnostic Tools

### JVM Diagnostic Tools Overview

```mermaid
graph TB
    subgraph "Command Line Tools"
        JPS["jps<br/>List Java processes"]
        JSTAT["jstat<br/>GC statistics"]
        JMAP["jmap<br/>Heap dump, histogram"]
        JSTACK["jstack<br/>Thread dump"]
        JCMD["jcmd<br/>All-in-one diagnostic"]
        JINFO["jinfo<br/>JVM configuration"]
    end
    
    subgraph "GUI/Profilers"
        JMC["Java Mission Control<br/>+ Flight Recorder"]
        VVVM["VisualVM"]
        JPROF["JProfiler<br/>(commercial)"]
        YKIT["YourKit<br/>(commercial)"]
    end
    
    subgraph "Built-in"
        JFR["Java Flight Recorder<br/>Low-overhead recording"]
    end
```

### Essential Commands

```bash
# List all Java processes
jps -l
# Output: 12345 com.example.MyApp

# GC statistics (every 1 second, 10 samples)
jstat -gcutil <pid> 1000 10
#  S0     S1     E      O      M     CCS    YGC   YGCT   FGC  FGCT   CGC  CGCT   GCT
# 45.23   0.00  67.89  34.56  95.12  91.23   145  1.234    3  0.567    -     -  1.801

# Heap histogram (quick memory check)
jmap -histo <pid> | head -30
# Shows top memory consumers by class

# Heap dump (for detailed analysis)
jmap -dump:format=b,file=heap.hprof <pid>

# Thread dump (for deadlock analysis)
jstack <pid> > threads.txt

# Flight Recording (low overhead profiling)
jcmd <pid> JFR.start duration=60s filename=recording.jfr

# View JVM flags
jcmd <pid> VM.flags
```

### Reading jstat Output

```
jstat -gcutil <pid> 1000

 S0     S1     E      O      M     CCS    YGC    YGCT   FGC   FGCT   CGC  CGCT    GCT
 0.00  45.23  78.56  34.12  95.45  92.34   234   2.345    5   1.234    12  0.123  3.702
 │      │      │      │      │      │       │      │      │     │      │    │      │
 │      │      │      │      │      │       │      │      │     │      │    │      └─ Total GC time
 │      │      │      │      │      │       │      │      │     │      │    └─ Concurrent GC time  
 │      │      │      │      │      │       │      │      │     │      └─ Concurrent GC count
 │      │      │      │      │      │       │      │      │     └─ Full GC time
 │      │      │      │      │      │       │      │      └─ Full GC count (should be LOW!)
 │      │      │      │      │      │       │      └─ Young GC time
 │      │      │      │      │      │       └─ Young GC count
 │      │      │      │      │      └─ Compressed Class Space %
 │      │      │      │      └─ Metaspace %
 │      │      │      └─ Old Gen % (watch this!)
 │      │      └─ Eden %
 │      └─ Survivor 1 %
 └─ Survivor 0 %
```

---

# 📦 SECȚIUNEA 6: JVM TUNING

## Q329-Q332: GC Tuning

### Tuning Decision Flowchart

```mermaid
flowchart TD
    A["Performance Problem?"] --> B{"High GC Pause?"}
    
    B -->|Yes| C{"Current GC?"}
    B -->|No| D{"High GC Frequency?"}
    
    C -->|"Parallel/Serial"| E["Switch to G1 or ZGC"]
    C -->|"G1"| F["Tune -XX:MaxGCPauseMillis<br/>Increase heap"]
    C -->|"ZGC"| G["Already optimal for latency"]
    
    D -->|Yes| H{"Allocation rate high?"}
    D -->|No| I["Check application logic"]
    
    H -->|Yes| J["Increase Young Gen<br/>-XX:NewRatio=2"]
    H -->|"Normal"| K["Possible memory leak!<br/>Heap dump analysis"]
```

### Key JVM Parameters

```java
// ═══════════════════════════════════════
// MEMORY SIZING
// ═══════════════════════════════════════

-Xms4g                    // Initial heap size
-Xmx4g                    // Maximum heap size
                          // TIP: Set equal for predictability

-XX:NewRatio=2            // Old:Young = 2:1
-XX:SurvivorRatio=8       // Eden:Survivor = 8:1

-XX:MaxMetaspaceSize=256m // Limit metaspace

-Xss256k                  // Thread stack size

// ═══════════════════════════════════════
// GC SELECTION
// ═══════════════════════════════════════

-XX:+UseG1GC              // G1 (default Java 9+)
-XX:+UseZGC               // ZGC (Java 15+)
-XX:+UseParallelGC        // Parallel (throughput)
-XX:+UseShenandoahGC      // Shenandoah

// ═══════════════════════════════════════
// G1 TUNING
// ═══════════════════════════════════════

-XX:MaxGCPauseMillis=200  // Target pause (default 200ms)
-XX:G1HeapRegionSize=16m  // Region size (1-32MB, auto)
-XX:InitiatingHeapOccupancyPercent=45  // Start concurrent GC

// ═══════════════════════════════════════
// GC LOGGING (Java 9+ Unified Logging)
// ═══════════════════════════════════════

-Xlog:gc*:file=gc.log:time,uptime,level:filecount=5,filesize=20m

// ═══════════════════════════════════════
// DIAGNOSTICS
// ═══════════════════════════════════════

-XX:+HeapDumpOnOutOfMemoryError
-XX:HeapDumpPath=/var/log/myapp/
-XX:+ExitOnOutOfMemoryError      // Restart container

-XX:NativeMemoryTracking=summary // Track native memory
```

### Production-Ready JVM Configuration

```bash
#!/bin/bash
# Production JVM flags for a typical web service

java \
  -Xms4g \
  -Xmx4g \
  -XX:+UseG1GC \
  -XX:MaxGCPauseMillis=200 \
  -XX:+UseStringDeduplication \
  -XX:+HeapDumpOnOutOfMemoryError \
  -XX:HeapDumpPath=/var/log/app/heap-dump.hprof \
  -XX:+ExitOnOutOfMemoryError \
  -Xlog:gc*:file=/var/log/app/gc.log:time,uptime:filecount=5,filesize=20m \
  -jar myapp.jar
```

---

# 📦 SECȚIUNEA 7: PERFORMANCE BEST PRACTICES

## Q326-Q340: Optimization Guidelines

### Performance Anti-Patterns

```mermaid
graph TB
    subgraph "❌ ANTI-PATTERNS"
        AP1["Premature Optimization<br/>Optimizing without measuring"]
        AP2["Object Pooling (usually)<br/>GC is very efficient"]
        AP3["Naive Microbenchmarks<br/>JIT not warmed up"]
        AP4["String += in loops<br/>Creates many objects"]
        AP5["Autoboxing in hot paths<br/>Integer vs int overhead"]
    end
    
    subgraph "✅ BEST PRACTICES"
        BP1["Measure First<br/>Profile before optimizing"]
        BP2["Trust the GC<br/>Let it manage memory"]
        BP3["Use JMH<br/>Proper benchmarking"]
        BP4["StringBuilder in loops<br/>Single buffer"]
        BP5["Primitive streams<br/>IntStream, LongStream"]
    end
```

```java
// ❌ String concatenation in loop
String result = "";
for (String item : items) {
    result += item;  // New String object each iteration!
}

// ✅ StringBuilder
StringBuilder sb = new StringBuilder();
for (String item : items) {
    sb.append(item);  // Single buffer
}
String result = sb.toString();

// ❌ Autoboxing overhead
List<Integer> numbers = getNumbers();
long sum = 0;
for (Integer n : numbers) {
    sum += n;  // Unboxing on every iteration!
}

// ✅ Primitive stream
long sum = numbers.stream()
    .mapToLong(Integer::longValue)
    .sum();

// ❌ Naive benchmark (WRONG!)
long start = System.currentTimeMillis();
for (int i = 0; i < 1000; i++) {
    doSomething();  // JIT hasn't optimized yet!
}
System.out.println("Time: " + (System.currentTimeMillis() - start));

// ✅ Use JMH (Java Microbenchmark Harness)
@Benchmark
@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.NANOSECONDS)
public void properBenchmark(Blackhole bh) {
    bh.consume(doSomething());
}
```

---

# 🎯 CHEAT SHEET - JVM & GC

## Quick Reference Table

| Problemă | Simptom | Diagnostic | Soluție |
|----------|---------|------------|---------|
| **OOM Heap** | "Java heap space" | `jmap -histo` | Crește -Xmx, fix leak |
| **OOM Metaspace** | "Metaspace" | Class histogram | -XX:MaxMetaspaceSize |
| **GC Overhead** | "GC overhead limit" | `jstat -gcutil` | Fix memory leak |
| **Long GC Pauses** | Latency spikes | GC logs | G1/ZGC, tune pause |
| **Frequent GC** | High CPU | `jstat -gcutil` | Increase heap |
| **Memory Leak** | Heap grows | Heap dump | MAT analysis |
| **Deadlock** | App frozen | `jstack` | Thread dump |

## GC Selection Quick Guide

```
┌─────────────────────────────────────────────────────────┐
│                   CHOOSE YOUR GC                        │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  Heap < 4GB, simple app?     →  Serial GC              │
│                                                         │
│  Throughput priority?        →  Parallel GC            │
│  (batch, offline processing)                            │
│                                                         │
│  Balanced, general purpose?  →  G1 GC ✅ (default)     │
│  (most applications)                                    │
│                                                         │
│  Ultra-low latency?          →  ZGC                    │
│  (< 1ms pause required)                                 │
│                                                         │
│  Large heap, low latency?    →  Shenandoah             │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

## Essential Flags Template

```bash
# Development
java -Xms512m -Xmx512m -XX:+UseG1GC -jar app.jar

# Production
java -Xms4g -Xmx4g \
     -XX:+UseG1GC \
     -XX:MaxGCPauseMillis=200 \
     -XX:+HeapDumpOnOutOfMemoryError \
     -Xlog:gc*:file=gc.log:time \
     -jar app.jar

# Low-latency
java -Xms8g -Xmx8g \
     -XX:+UseZGC \
     -jar app.jar
```

---

> 💡 **Regula de Aur JVM Performance:**  
> *"Măsoară înainte să optimizezi. Profilează în producție. Lasă GC-ul să-și facă treaba. Optimizează algoritmii, nu micro-detaliile."*
