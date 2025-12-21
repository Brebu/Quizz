# Capitolul 2 – Collections Framework (VERSIUNE EXTINSĂ)
## Q121–Q200 — Nivel Senior

> 📚 **Scop:** Interviuri Senior / Lead / Staff  
> 🎯 **Focus:** Performanță, Trade-offs, Diagrame Mermaid  
> 💾 **Encoding:** UTF-8

---

## 🎯 HARTA MENTALĂ COLLECTIONS

```mermaid
mindmap
  root((Collections Framework))
    Interfaces
      Collection
        List
        Set
        Queue/Deque
      Map
    List Implementations
      ArrayList
      LinkedList
      CopyOnWriteArrayList
    Set Implementations
      HashSet
      LinkedHashSet
      TreeSet
      EnumSet
    Map Implementations
      HashMap
      LinkedHashMap
      TreeMap
      ConcurrentHashMap
      WeakHashMap
      EnumMap
    Queue Implementations
      ArrayDeque
      PriorityQueue
      BlockingQueue
    Utilities
      Collections
      Arrays
      Comparator
```

---

## 📊 IERARHIA COMPLETĂ

```mermaid
classDiagram
    class Iterable {
        <<interface>>
        +iterator() Iterator
    }
    
    class Collection {
        <<interface>>
        +add(E) boolean
        +remove(Object) boolean
        +contains(Object) boolean
        +size() int
        +isEmpty() boolean
    }
    
    class List {
        <<interface>>
        +get(int) E
        +set(int, E) E
        +indexOf(Object) int
    }
    
    class Set {
        <<interface>>
        Fără duplicate
    }
    
    class Queue {
        <<interface>>
        +offer(E) boolean
        +poll() E
        +peek() E
    }
    
    class Deque {
        <<interface>>
        +addFirst(E)
        +addLast(E)
        +removeFirst() E
        +removeLast() E
    }
    
    class Map {
        <<interface>>
        +put(K,V) V
        +get(K) V
        +containsKey(K) boolean
        +keySet() Set
    }
    
    Iterable <|-- Collection
    Collection <|-- List
    Collection <|-- Set
    Collection <|-- Queue
    Queue <|-- Deque
    
    List <|.. ArrayList
    List <|.. LinkedList
    List <|.. CopyOnWriteArrayList
    
    Set <|.. HashSet
    Set <|.. LinkedHashSet
    Set <|.. TreeSet
    Set <|.. EnumSet
    
    Deque <|.. ArrayDeque
    Deque <|.. LinkedList
    Queue <|.. PriorityQueue
    
    Map <|.. HashMap
    Map <|.. LinkedHashMap
    Map <|.. TreeMap
    Map <|.. ConcurrentHashMap
```

---

# 📦 SECȚIUNEA 1: LIST IMPLEMENTATIONS

## Q124-Q128: ArrayList vs LinkedList

### Structura Internă

```mermaid
graph TB
    subgraph "ArrayList - Array Continuu"
        A[Index 0<br/>Element A] --- B[Index 1<br/>Element B] --- C[Index 2<br/>Element C] --- D[Index 3<br/>Element D]
    end
    
    subgraph "LinkedList - Noduri Înlănțuite"
        N1[◀ prev | A | next ▶] <--> N2[◀ prev | B | next ▶] <--> N3[◀ prev | C | next ▶]
    end
```

### Comparație Vizuală Performanță

```mermaid
graph LR
    subgraph "ArrayList"
        AL_GET["get(i): O(1) ⚡"]
        AL_ADD["add(E): O(1)*"]
        AL_INS["add(i,E): O(n) 🐌"]
        AL_REM["remove(i): O(n) 🐌"]
        AL_MEM["Memorie: LOW ✅"]
    end
    
    subgraph "LinkedList"
        LL_GET["get(i): O(n) 🐌"]
        LL_ADD["add(E): O(1) ⚡"]
        LL_INS["add(i,E): O(n) 🐌"]
        LL_REM["remove(i): O(n) 🐌"]
        LL_MEM["Memorie: HIGH ❌"]
    end
    
    style AL_GET fill:#90EE90
    style AL_ADD fill:#90EE90
    style AL_MEM fill:#90EE90
    style LL_ADD fill:#90EE90
    style LL_GET fill:#FFB6C1
    style LL_MEM fill:#FFB6C1
```

### Tabel Complexitate Completă

| Operație | ArrayList | LinkedList | Câștigător |
|----------|-----------|------------|------------|
| `get(index)` | **O(1)** | O(n) | ArrayList |
| `add(E)` la final | **O(1)** amortizat | O(1) | Egal |
| `add(index, E)` | O(n) | O(n)* | Egal |
| `remove(index)` | O(n) | O(n)* | Egal |
| `contains(E)` | O(n) | O(n) | Egal |
| `iterator.remove()` | O(n) | **O(1)** | LinkedList |
| Memory per element | **~4 bytes** | ~24 bytes | ArrayList |
| Cache locality | **Excelent** | Slab | ArrayList |

> *LinkedList: O(n) pentru a găsi poziția + O(1) pentru operație

### Cod Exemplu

```java
// ✅ ArrayList - alegerea DEFAULT pentru 99% din cazuri
List<String> names = new ArrayList<>();

// ✅ Pre-sizing pentru performanță (evită resize)
List<Order> orders = new ArrayList<>(10_000);

// ❌ LinkedList - RAREORI alegerea corectă
// Folosește DOAR pentru:
// 1. Operații Queue/Deque la capete
// 2. Iterator.remove() frecvent în mijlocul listei
Deque<Task> taskQueue = new LinkedList<>();
```

---

## Q129: ArrayList Resize Mechanism

```mermaid
sequenceDiagram
    participant Client
    participant ArrayList
    participant OldArray as Array[10]
    participant NewArray as Array[15]
    
    Note over ArrayList: capacity=10, size=10
    
    Client->>ArrayList: add(element)
    ArrayList->>ArrayList: ensureCapacity(11)
    ArrayList->>ArrayList: newCapacity = 10 + 10/2 = 15
    ArrayList->>NewArray: Create new array[15]
    ArrayList->>OldArray: System.arraycopy()
    OldArray-->>NewArray: Copy all elements
    ArrayList->>NewArray: array[10] = element
    
    Note over ArrayList: capacity=15, size=11
```

**Formula de creștere:** `newCapacity = oldCapacity + (oldCapacity >> 1)` = **+50%**

---

# 📦 SECȚIUNEA 2: SET IMPLEMENTATIONS

## Q130-Q134: HashSet, LinkedHashSet, TreeSet

### Comparație Vizuală

```mermaid
graph TB
    subgraph "HashSet"
        HS[Fără ordine garantată<br/>Operations: O*1*]
        HS --> HSE1[C]
        HS --> HSE2[A]
        HS --> HSE3[B]
    end
    
    subgraph "LinkedHashSet"
        LHS[Ordine de INSERARE<br/>Operations: O*1*]
        LHS --> LHSE1[A] --> LHSE2[B] --> LHSE3[C]
    end
    
    subgraph "TreeSet"
        TS[Ordine SORTATĂ<br/>Operations: O*log n*]
        TS --> TSE1[A]
        TSE1 --> TSE2[B]
        TSE1 --> TSE3[C]
    end
    
    style HS fill:#E8E8E8
    style LHS fill:#E8F5E9
    style TS fill:#FFF3E0
```

### Structura Internă HashSet

```mermaid
graph TB
    subgraph "HashSet = HashMap cu valoare PRESENT"
        HM[HashMap Intern]
        HM --> B0["Bucket 0"]
        HM --> B1["Bucket 1"]
        HM --> B2["Bucket 2"]
        HM --> B3["Bucket ..."]
        
        B0 --> E1["Element X<br/>hash=0"]
        B1 --> E2["Element Y<br/>hash=1"] --> E3["Element Z<br/>hash=1"]
        B2 --> E4["Element W<br/>hash=2"]
    end
    
    Note["add(E) → map.put(E, PRESENT)<br/>contains(E) → map.containsKey(E)"]
```

### Cod Exemplu

```java
// HashSet - cel mai rapid, fără ordine
Set<String> emails = new HashSet<>();
emails.add("z@test.com");
emails.add("a@test.com");
emails.add("m@test.com");
// Iterare: ordine IMPREVIZIBILĂ

// LinkedHashSet - păstrează ordinea inserării
Set<String> orderedTags = new LinkedHashSet<>();
orderedTags.add("java");
orderedTags.add("spring");
orderedTags.add("boot");
// Iterare: java → spring → boot (ordine inserare)

// TreeSet - sortat automat
Set<String> sortedNames = new TreeSet<>();
sortedNames.add("Charlie");
sortedNames.add("Alice");
sortedNames.add("Bob");
// Iterare: Alice → Bob → Charlie (ordine alfabetică)

// TreeSet cu Comparator custom
Set<Person> byAge = new TreeSet<>(
    Comparator.comparingInt(Person::getAge)
);

// EnumSet - SUPER eficient pentru enum-uri (bit vector)
enum Permission { READ, WRITE, DELETE, ADMIN }
Set<Permission> perms = EnumSet.of(Permission.READ, Permission.WRITE);
```

---

# 📦 SECȚIUNEA 3: MAP IMPLEMENTATIONS

## Q136-Q144: HashMap Deep Dive

### Structura Internă HashMap (Java 8+)

```mermaid
graph TB
    subgraph "HashMap Structure"
        HM[HashMap<br/>capacity=16, loadFactor=0.75]
        HM --> BA[Bucket Array]
        
        BA --> B0["Bucket 0<br/>null"]
        BA --> B1["Bucket 1<br/>LinkedList"]
        BA --> B2["Bucket 2<br/>null"]
        BA --> B7["Bucket 7<br/>Red-Black Tree"]
        BA --> BN["Bucket 15<br/>null"]
        
        B1 --> N1["K1:V1"] --> N2["K2:V2"]
        B7 --> T1["TreeNode"]
        T1 --> T2["Left"]
        T1 --> T3["Right"]
    end
    
    Note["< 8 noduri: LinkedList<br/>>= 8 noduri: Red-Black Tree<br/>< 6 noduri: revine la LinkedList"]
```

### Flux Put Operation

```mermaid
flowchart TD
    A["put(key, value)"] --> B["hash = key.hashCode()"]
    B --> C["index = hash & (capacity-1)"]
    C --> D{Bucket gol?}
    
    D -->|Da| E["Inserează direct"]
    D -->|Nu| F{Găsește key existent?}
    
    F -->|Da, equals=true| G["Înlocuiește valoarea"]
    F -->|Nu| H{Noduri >= 8?}
    
    H -->|Nu| I["Adaugă la LinkedList"]
    H -->|Da| J["Treeify: convertește la Red-Black Tree"]
    
    E --> K{size > threshold?}
    I --> K
    J --> K
    G --> K
    
    K -->|Da| L["resize(): capacity *= 2"]
    K -->|Nu| M["Done"]
    L --> M
```

### Load Factor și Rehashing

```mermaid
graph LR
    subgraph "Load Factor = 0.75"
        LF["threshold = capacity × 0.75"]
        LF --> T1["capacity=16 → threshold=12"]
        LF --> T2["capacity=32 → threshold=24"]
        LF --> T3["capacity=64 → threshold=48"]
    end
    
    subgraph "Rehashing (costisitor!)"
        R1["size > threshold"]
        R1 --> R2["new capacity = old × 2"]
        R2 --> R3["Recalculează TOATE pozițiile"]
        R3 --> R4["Mută elementele"]
    end
```

### Cod Exemplu

```java
// HashMap standard
Map<String, User> userMap = new HashMap<>();

// ✅ Pre-sizing pentru a evita rehashing
int expectedSize = 10_000;
Map<String, Order> orders = new HashMap<>(
    (int) (expectedSize / 0.75) + 1
);

// LinkedHashMap - păstrează ordinea inserării
Map<String, Integer> accessOrder = new LinkedHashMap<>(16, 0.75f, true);
// true = access order (pentru LRU cache)

// TreeMap - chei sortate
Map<String, Integer> sorted = new TreeMap<>();
sorted.put("banana", 2);
sorted.put("apple", 1);
sorted.put("cherry", 3);
// Iterare: apple → banana → cherry
```

---

## Q143-Q144: ConcurrentHashMap vs Synchronized

### Comparație Blocking

```mermaid
graph TB
    subgraph "Collections.synchronizedMap ❌"
        SM[Single Lock Global]
        SM --> T1["Thread 1: BLOCKED"]
        SM --> T2["Thread 2: BLOCKED"]
        SM --> T3["Thread 3: WORKING"]
    end
    
    subgraph "ConcurrentHashMap ✅"
        CHM[Lock Striping / CAS]
        CHM --> CT1["Thread 1: Bucket 1 ✅"]
        CHM --> CT2["Thread 2: Bucket 2 ✅"]
        CHM --> CT3["Thread 3: Bucket 3 ✅"]
    end
    
    style SM fill:#FFB6C1
    style CHM fill:#90EE90
```

### Operații Atomice ConcurrentHashMap

```java
ConcurrentHashMap<String, Long> counters = new ConcurrentHashMap<>();

// ✅ Atomic compute
counters.computeIfAbsent("views", k -> 0L);
counters.computeIfPresent("views", (k, v) -> v + 1);
counters.merge("views", 1L, Long::sum);

// ⚠️ ConcurrentHashMap NU permite null!
// counters.put("key", null); // NullPointerException!

// Pattern: Counter atomic
counters.compute("pageHits", (k, v) -> v == null ? 1 : v + 1);
```

---

# 📦 SECȚIUNEA 4: QUEUE & DEQUE

## Q148-Q151: Cozi și Stive

### Tipuri de Queue

```mermaid
graph TB
    subgraph "Queue Interface"
        Q[Queue]
        Q --> AQ["ArrayDeque<br/>✅ General purpose"]
        Q --> PQ["PriorityQueue<br/>Heap-based"]
        Q --> LL["LinkedList<br/>❌ Avoid"]
    end
    
    subgraph "BlockingQueue (thread-safe)"
        BQ[BlockingQueue]
        BQ --> ABQ["ArrayBlockingQueue<br/>Bounded, fair"]
        BQ --> LBQ["LinkedBlockingQueue<br/>Optionally bounded"]
        BQ --> PBQ["PriorityBlockingQueue<br/>Unbounded, ordered"]
        BQ --> SQ["SynchronousQueue<br/>Zero capacity"]
    end
```

### ArrayDeque ca Stack și Queue

```mermaid
graph LR
    subgraph "Ca STACK (LIFO)"
        S1["push()"] --> AD1[ArrayDeque]
        AD1 --> S2["pop()"]
    end
    
    subgraph "Ca QUEUE (FIFO)"
        Q1["offer()"] --> AD2[ArrayDeque] --> Q2["poll()"]
    end
```

```java
// ✅ ArrayDeque - înlocuiește Stack și LinkedList
Deque<String> stack = new ArrayDeque<>();
stack.push("first");
stack.push("second");
stack.pop(); // "second" (LIFO)

Deque<String> queue = new ArrayDeque<>();
queue.offer("first");
queue.offer("second");
queue.poll(); // "first" (FIFO)

// PriorityQueue - MIN heap by default
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.offer(3);
minHeap.offer(1);
minHeap.offer(2);
minHeap.poll(); // 1 (cel mai mic)

// MAX heap
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(
    Comparator.reverseOrder()
);
```

---

# 📦 SECȚIUNEA 5: ITERATORI ȘI FAIL-FAST

## Q152-Q154: Fail-Fast vs Fail-Safe

```mermaid
sequenceDiagram
    participant Client
    participant Iterator
    participant ArrayList
    
    Note over Client, ArrayList: FAIL-FAST Behavior
    
    Client->>ArrayList: iterator()
    ArrayList->>Iterator: expectedModCount = modCount (5)
    Client->>ArrayList: list.add("new") 
    Note over ArrayList: modCount = 6
    Client->>Iterator: next()
    Iterator->>Iterator: Check: modCount != expectedModCount
    Iterator-->>Client: 💥 ConcurrentModificationException!
```

```mermaid
sequenceDiagram
    participant Client
    participant Iterator
    participant CopyOnWriteArrayList
    
    Note over Client, CopyOnWriteArrayList: FAIL-SAFE Behavior
    
    Client->>CopyOnWriteArrayList: iterator()
    CopyOnWriteArrayList->>Iterator: Snapshot of array
    Client->>CopyOnWriteArrayList: list.add("new")
    Note over CopyOnWriteArrayList: Creates NEW array
    Client->>Iterator: next()
    Iterator-->>Client: ✅ Returns from snapshot (OK)
```

### Cod Exemplu

```java
// ❌ ConcurrentModificationException
List<String> list = new ArrayList<>(List.of("a", "b", "c"));
for (String s : list) {
    if (s.equals("b")) {
        list.remove(s); // 💥 BOOM!
    }
}

// ✅ Soluția 1: Iterator.remove()
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    if (it.next().equals("b")) {
        it.remove(); // ✅ Safe
    }
}

// ✅ Soluția 2: removeIf() (Java 8+)
list.removeIf(s -> s.equals("b"));

// ✅ Soluția 3: CopyOnWriteArrayList (pentru concurrency)
List<String> cowList = new CopyOnWriteArrayList<>();
// Safe pentru iterare cu modificări concurente
// DAR: costisitor pentru multe scrieri
```

---

# 📦 SECȚIUNEA 6: COLECȚII IMUTABILE

## Q156-Q159: Immutable Collections

```mermaid
graph TB
    subgraph "Unmodifiable View ⚠️"
        O[Original List] --> UV["unmodifiableList()"]
        UV --> V[View]
        O -.-> |"modifică"| V
        Note1["View-ul se schimbă!"]
    end
    
    subgraph "Truly Immutable ✅"
        LO["List.of()"] --> IM[Immutable List]
        Note2["Nu poate fi modificată"]
    end
    
    style UV fill:#FFE4B5
    style IM fill:#90EE90
```

```java
// ⚠️ Unmodifiable ≠ Immutable
List<String> original = new ArrayList<>();
original.add("a");
List<String> unmodifiable = Collections.unmodifiableList(original);
original.add("b"); // ⚠️ Modifică și view-ul!
System.out.println(unmodifiable); // [a, b]

// ✅ Truly Immutable (Java 9+)
List<String> immutable = List.of("a", "b", "c");
Set<Integer> immutableSet = Set.of(1, 2, 3);
Map<String, Integer> immutableMap = Map.of(
    "one", 1,
    "two", 2,
    "three", 3
);

// ⚠️ Null NU e permis în List.of(), Set.of(), Map.of()
// List.of("a", null); // NullPointerException!
```

---

# 📦 SECȚIUNEA 7: ALEGEREA CORECTĂ

## Decision Tree pentru Alegerea Colecției

```mermaid
flowchart TD
    A[Ce tip de date?] --> B{Perechi key-value?}
    B -->|Da| MAP[MAP]
    B -->|Nu| C{Ordine contează?}
    
    C -->|Da| D{Acces prin index?}
    C -->|Nu| E{Duplicate permise?}
    
    D -->|Da| LIST[LIST]
    D -->|Nu| QUEUE[QUEUE/DEQUE]
    
    E -->|Da| LIST
    E -->|Nu| SET[SET]
    
    MAP --> M1{Sortare după cheie?}
    M1 -->|Da| M2[TreeMap]
    M1 -->|Nu| M3{Thread-safe?}
    M3 -->|Da| M4[ConcurrentHashMap]
    M3 -->|Nu| M5{Ordine inserare?}
    M5 -->|Da| M6[LinkedHashMap]
    M5 -->|Nu| M7[HashMap ✅]
    
    LIST --> L1{Thread-safe reads?}
    L1 -->|Da| L2[CopyOnWriteArrayList]
    L1 -->|Nu| L3[ArrayList ✅]
    
    SET --> S1{Sortare?}
    S1 -->|Da| S2[TreeSet]
    S1 -->|Nu| S3{Ordine inserare?}
    S3 -->|Da| S4[LinkedHashSet]
    S3 -->|Nu| S5[HashSet ✅]
    
    QUEUE --> Q1{Prioritate?}
    Q1 -->|Da| Q2[PriorityQueue]
    Q1 -->|Nu| Q3[ArrayDeque ✅]
```

---

# 🎯 COMPLEXITY CHEAT SHEET

| Colecție | add | remove | get | contains | Notă |
|----------|-----|--------|-----|----------|------|
| **ArrayList** | O(1)* | O(n) | **O(1)** | O(n) | Default pentru List |
| **LinkedList** | O(1) | O(1)† | O(n) | O(n) | Evită în general |
| **HashSet** | O(1) | O(1) | - | **O(1)** | Default pentru Set |
| **TreeSet** | O(log n) | O(log n) | - | O(log n) | Când ai nevoie de sortare |
| **HashMap** | O(1) | O(1) | **O(1)** | O(1) | Default pentru Map |
| **TreeMap** | O(log n) | O(log n) | O(log n) | O(log n) | Chei sortate |
| **ArrayDeque** | O(1) | O(1) | - | O(n) | Default pentru Stack/Queue |
| **PriorityQueue** | O(log n) | O(log n) | O(1)‡ | O(n) | Heap |

*Amortizat †La poziția iteratorului ‡Doar peek()

---

# 🎯 ANTI-PATTERNS DE EVITAT

```java
// ❌ Anti-pattern 1: contains() pe List în loop
for (Order order : orders) {
    if (validIds.contains(order.getId())) { // O(n²)!
        process(order);
    }
}

// ✅ Fix: Folosește Set pentru lookup
Set<Long> validIdSet = new HashSet<>(validIds); // O(n)
for (Order order : orders) {
    if (validIdSet.contains(order.getId())) { // O(1)
        process(order);
    }
}

// ❌ Anti-pattern 2: Nested loops pentru join
for (User u : users) {
    for (Order o : orders) {
        if (o.getUserId().equals(u.getId())) { // O(n×m)
            // ...
        }
    }
}

// ✅ Fix: Indexează cu Map
Map<Long, List<Order>> ordersByUser = orders.stream()
    .collect(Collectors.groupingBy(Order::getUserId)); // O(n+m)

for (User u : users) {
    List<Order> userOrders = ordersByUser.get(u.getId());
}
```

---

> 💡 **Regula de Aur:**  
> *"Alege colecția pe baza pattern-ului de ACCES dominant, nu pe baza dimensiunii datelor."*
