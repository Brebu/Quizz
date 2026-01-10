# Miscellaneous Topics – Întrebări & Răspunsuri

## 📋 Cuprins
- [Metodologii & Practici](#metodologii--practici)
  - [Agile & Kanban](#agile--kanban)
  - [Observability](#observability)
- [Java Evolution](#java-evolution)
  - [Java 11 vs 17 vs 21](#java-11-vs-17-vs-21)
  - [Records](#records)
  - [Sealed Classes](#sealed-classes)
- [Java Core Features](#java-core-features)
  - [Streams API](#streams-api)
  - [Optional](#optional)
  - [Thread vs ExecutorService](#thread-vs-executorservice)
  - [ObjectMapper](#objectmapper)
  - [Interfaces & Default Methods](#interfaces--default-methods)
- [Spring Framework](#spring-framework)
  - [@Controller vs @RestController](#controller-vs-restcontroller)
  - [Dependency Injection](#dependency-injection)
  - [Bean Scopes](#bean-scopes)
  - [API Definition în Spring](#api-definition-în-spring)
  - [Spring Batch](#spring-batch)
- [Microservices](#microservices)
  - [Microservices vs Monolith](#microservices-vs-monolith)
  - [Communication Patterns](#communication-patterns)
  - [Monitoring & Observability](#monitoring--observability)
  - [Thread Control în Microservices](#thread-control-în-microservices)
- [Databases](#databases)
  - [Relational vs Non-Relational](#relational-vs-non-relational)
  - [PostgreSQL vs Couchbase](#postgresql-vs-couchbase)
- [DevOps & Tools](#devops--tools)
  - [Docker](#docker)
  - [Gradle](#gradle)
  - [Java SE vs Java EE](#java-se-vs-java-ee)
- [System Design](#system-design)
  - [URL Shortener](#url-shortener)

---

## Metodologii & Practici

### Agile & Kanban

#### Ce este Agile și care sunt principiile sale fundamentale?

**Răspuns:**

**Agile** este o **filozofie de dezvoltare software** bazată pe iterații scurte, feedback continuu și adaptabilitate la schimbare.

**Agile Manifesto - 4 Valori Fundamentale:**

1. **Indivizi și interacțiuni** > procese și unelte
   - Echipe auto-organizate
   - Comunicare face-to-face
   - Colaborare strânsă

2. **Software funcțional** > documentație comprehensivă
   - Working software este măsura primară a progresului
   - Documentația minimă necesară
   - Demo-uri regulate

3. **Colaborare cu clientul** > negociere contractuală
   - Client parte din echipă
   - Feedback constant
   - Prioritizare împreună

4. **Răspuns la schimbare** > urmărire plan
   - Planuri flexibile
   - Adaptare rapidă
   - Îmbrățișarea schimbării

**12 Principii Agile (cele mai importante pentru interviuri):**

1. **Livrare continuă** - software valoros des și regulat
2. **Acceptarea schimbărilor** - chiar și târziu în dezvoltare
3. **Livrări frecvente** - săptămâni vs luni
4. **Colaborare zilnică** - business și development împreună
5. **Oameni motivați** - încredere și suport
6. **Comunicare față-în-față** - cea mai eficientă
7. **Software funcțional** - măsura primară a progresului
8. **Ritm sustenabil** - echilibru muncă-viață
9. **Excelență tehnică** - design bun, refactoring continuu
10. **Simplitate** - arta de a maximiza munca nefăcută
11. **Echipe auto-organizate** - cele mai bune arhitecturi vin de la echipe auto-organizate
12. **Reflecție și ajustare** - retrospective regulate

**Framework-uri Agile:**

**1. Scrum**
- **Sprint-uri** fixe (2-4 săptămâni)
- **Roluri definite:**
  - Scrum Master - facilitator, remove impediments
  - Product Owner - prioritizare, vision
  - Development Team - self-organizing, cross-functional
- **Ceremonies:**
  - Sprint Planning
  - Daily Standup (15 min)
  - Sprint Review (demo)
  - Sprint Retrospective
- **Artifacts:**
  - Product Backlog
  - Sprint Backlog
  - Increment (potentially shippable)

**2. Kanban**
- Flow continuu (detaliat mai jos)
- Fără sprint-uri fixe
- Fără roluri prescrise

**3. XP (Extreme Programming)**
- TDD (Test-Driven Development)
- Pair Programming
- Continuous Integration
- Refactoring continuu
- Simple Design
- Collective Code Ownership

**4. SAFe (Scaled Agile Framework)**
- Pentru organizații mari (100+ oameni)
- Multiple nivele: Team, Program, Portfolio
- PI Planning (Program Increment)
- Agile Release Trains

---

#### Ce este Kanban și cum diferă de Scrum în detaliu?

**Răspuns:**

**Kanban** este o metodă vizuală de management al workflow-ului, bazată pe principiile sistemului de producție Toyota.

**6 Principii Kanban:**

1. **Vizualizare workflow** - board cu coloane
2. **Limitare WIP** (Work In Progress) - max items per coloană
3. **Management flow** - măsurare și optimizare
4. **Politici explicite** - reguli clare
5. **Feedback loops** - meetinguri regulate
6. **Îmbunătățire colaborativă** - Kaizen

**Kanban Board tipic:**

```
┌─────────────┬─────────────┬─────────────┬─────────────┬─────────────┐
│   Backlog   │   To Do     │ In Progress │   Review    │    Done     │
│             │  (WIP: 3)   │  (WIP: 2)   │  (WIP: 2)   │             │
├─────────────┼─────────────┼─────────────┼─────────────┼─────────────┤
│ Task A      │ Task D  🔴  │ Task F ⚙️   │ Task H 👁    │ Task J ✓    │
│ Task B      │ Task E  🟡  │ Task G ⚙️   │ Task I 👁    │ Task K ✓    │
│ Task C      │ Task M  🟢  │ (2/2) FULL  │ (2/2) FULL  │ Task L ✓    │
│             │ (3/3) FULL  │             │             │             │
└─────────────┴─────────────┴─────────────┴─────────────┴─────────────┘

Legend:
🔴 Blocker
🟡 High Priority
🟢 Normal
⚙️ In Progress
👁 In Review
✓ Done
```

**WIP Limits - De ce sunt importante:**

```
Fără WIP limits:
To Do: 10 tasks → In Progress: 8 tasks → Review: 5 tasks → Done: 2 tasks
Rezultat: Multitasking, context switching, nimic se termină

Cu WIP limits:
To Do: 10 tasks → In Progress: 2 tasks → Review: 2 tasks → Done: 10 tasks
Rezultat: Focus, completion, flow continuu
```

**Metrici Kanban:**

1. **Lead Time** - timpul total de la cerere până la livrare
   ```
   Lead Time = Timpul când task intră în sistem → task finalizat
   ```

2. **Cycle Time** - timpul de lucru activ
   ```
   Cycle Time = Timpul când task începe → task finalizat
   ```

3. **Throughput** - items completate per perioadă
   ```
   Throughput = Tasks Done / Week
   ```

4. **WIP** - work in progress actual

**Kanban vs Scrum - Comparație Detaliată:**

| Aspect | Kanban | Scrum |
|--------|--------|-------|
| **Cadență** | Continuă, fără timebox | Sprint-uri fixe (2-4 săpt) |
| **Roluri** | Fără roluri prescrise | SM, PO, Dev Team (mandatory) |
| **Planificare** | Just-in-time, la cerere | Sprint Planning (fix) |
| **Estimări** | Opționale | Story points mandatory |
| **Changes mid-iteration** | Oricând | Doar între sprint-uri |
| **WIP Limits** | Explicit, enforced | Implicit (Sprint capacity) |
| **Board** | Persistent, continuous | Reset la fiecare sprint |
| **Metrici cheie** | Lead Time, Cycle Time, Throughput | Velocity, Burndown |
| **Commitments** | Fără commitments | Sprint commitment |
| **Release** | Continuous delivery | La final de sprint |
| **Ceremonies** | Optional (standup, replenishment) | Mandatory (Planning, Review, Retro) |
| **Best for** | Support, ops, unpredictable work | Product development, fixed sprints |

**Când folosești Kanban:**

✅ **DA:**
- Support/maintenance teams
- Operations teams
- Flux continuu de task-uri mici
- Priorități schimbătoare des
- Echipă matură, auto-organizată
- SLA-uri stricte
- Bug fixing

**Exemplu:** Customer support, DevOps, incident management

**Când folosești Scrum:**

✅ **DA:**
- Dezvoltare produs nou
- Proiecte cu unknowns complexe
- Ai nevoie de ritm și cadență
- Echipă nouă, needs structure
- Stakeholder demos regulate
- Fixed release cycles

**Exemplu:** Nou feature development, product launches

**Hybrid - Scrumban:**

Combină best of both:
- Sprint-uri din Scrum (opțional, mai scurte)
- WIP limits din Kanban
- Flow continuu
- Ceremonies selective

```
Sprint 1 (2 săpt) cu WIP limits
→ Retrospective
→ Sprint 2 (2 săpt) cu WIP limits
→ Retrospective
```

**Flow Efficiency:**

```
Flow Efficiency = Active Time / Lead Time

Exemplu:
Task A:
- Lead Time: 10 zile (de la To Do → Done)
- Active Time: 2 zile (actual work)
- Wait Time: 8 zile (în cozi)
- Flow Efficiency: 2/10 = 20%

Target: >40% flow efficiency
```

**Kanban în practică - Exemple:**

**1. Software Development Team:**
```
Backlog → Analysis (WIP:2) → Development (WIP:3) → Code Review (WIP:2) → Testing (WIP:2) → Deploy → Done
```

**2. Support Team:**
```
Incidents → Triage (WIP:5) → Investigation (WIP:3) → Resolution (WIP:2) → Verification → Closed
```

**3. DevOps Team:**
```
Requests → To Do (WIP:∞) → In Progress (WIP:3) → Review (WIP:2) → Deployed → Done
```

---

### Observability

#### Ce este Observability și cum diferă de Monitoring în profunzime?

**Răspuns:**

**Observability** = abilitatea de a **înțelege starea internă** a unui sistem bazat pe outputurile externe (logs, metrics, traces). Permite să răspunzi la întrebări pe care nu le-ai anticipat.

**Monitoring** = verificarea constantă a unor metrici predefinite. Răspunde la întrebări cunoscute ("is the service up?").

**Diferențe fundamentale:**

| Monitoring | Observability |
|------------|---------------|
| **Known unknowns** | **Unknown unknowns** |
| Predefined queries | Ad-hoc queries |
| "Is it working?" | "Why is it not working?" |
| Dashboards + Alerts | Root cause analysis |
| Reactive | Proactive + Reactive |
| Checks predefinite | Exploratory investigation |
| Metrici agregate | High-cardinality data |

**Exemplu concret:**

**Monitoring:**
```
Alert: "API latency > 500ms"
→ Știi că e lent
→ Nu știi DE CE
```

**Observability:**
```
Alert: "API latency > 500ms"
→ Check distributed trace
→ See: Database query took 480ms
→ Check query logs
→ Find: Missing index on new column added yesterday
→ ROOT CAUSE identified
```

---

**Cele 3 Piloni ai Observability (în detaliu):**

**1. Logs (Jurnale)**

**Ce sunt:**
- Event-uri discrete cu timestamp
- Detalii despre ce s-a întâmplat
- Structured sau unstructured

**Tipuri:**
```
Application Logs:
2025-01-10 10:23:45.123 ERROR [UserService] Failed to save user id=123
  Exception: java.sql.SQLException: Connection timeout
  at UserRepository.save(UserRepository.java:45)
  userId=123, email=john@example.com, action=CREATE

Access Logs:
192.168.1.100 - - [10/Jan/2025:10:23:45 +0000] "POST /api/users HTTP/1.1" 500 1234

Audit Logs:
{"timestamp":"2025-01-10T10:23:45Z","user":"admin","action":"DELETE_USER","resource":"user:123","result":"SUCCESS"}
```

**Structured Logging (JSON):**
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import net.logstash.logback.argument.StructuredArguments;

@Service
public class UserService {
    private static final Logger log = LoggerFactory.getLogger(UserService.class);
    
    public User createUser(User user) {
        log.info("Creating user",
            StructuredArguments.keyValue("userId", user.getId()),
            StructuredArguments.keyValue("email", user.getEmail()),
            StructuredArguments.keyValue("action", "CREATE")
        );
        
        try {
            User saved = userRepository.save(user);
            log.info("User created successfully",
                StructuredArguments.keyValue("userId", saved.getId()),
                StructuredArguments.keyValue("duration", timer.stop())
            );
            return saved;
        } catch (Exception e) {
            log.error("Failed to create user",
                StructuredArguments.keyValue("userId", user.getId()),
                StructuredArguments.keyValue("error", e.getMessage()),
                e
            );
            throw e;
        }
    }
}
```

**Log Levels:**
```
TRACE: Extremely detailed (entry/exit methods)
DEBUG: Detailed for debugging
INFO: Important events (user login, order created)
WARN: Potentially harmful (deprecated API used)
ERROR: Error events (exceptions)
FATAL: Severe errors (app crash)
```

**Best Practices:**
- Use structured logging (JSON)
- Include context (userId, requestId, traceId)
- Don't log sensitive data (passwords, credit cards)
- Use appropriate log levels
- Centralize logs (ELK, Splunk)

---

**2. Metrics (Metrici)**

**Ce sunt:**
- Valori numerice măsurate în timp
- Time-series data
- Agregabile și alertabile

**Tipuri de Metrici:**

**a) Counter** - valoare care crește
```java
@RestController
public class UserController {
    
    private final Counter userCreatedCounter;
    
    public UserController(MeterRegistry registry) {
        this.userCreatedCounter = registry.counter("users.created.total",
            "service", "user-service",
            "environment", "production"
        );
    }
    
    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        User created = userService.save(user);
        userCreatedCounter.increment();
        return created;
    }
}
```

**b) Gauge** - valoare instantanee (poate crește sau scădea)
```java
registry.gauge("database.connections.active", connectionPool, ConnectionPool::getActiveConnections);
registry.gauge("jvm.memory.used", memoryMXBean, bean -> bean.getHeapMemoryUsage().getUsed());
```

**c) Histogram** - distribuție valori
```java
@Timed(value = "http.request.duration", percentiles = {0.5, 0.95, 0.99})
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.findById(id);
}
```

**d) Summary** - similar histogram, client-side quantiles

**Metrici cheie pentru microservices:**

**RED Method** (Request-oriented):
- **Rate** - requests per second
- **Errors** - error rate
- **Duration** - latency

**USE Method** (Resource-oriented):
- **Utilization** - % time busy
- **Saturation** - queue length
- **Errors** - error count

**Four Golden Signals** (Google SRE):
- **Latency** - request duration
- **Traffic** - requests per second
- **Errors** - error rate
- **Saturation** - resource usage

**Exemplu metrici HTTP:**
```
http_requests_total{method="POST", endpoint="/api/users", status="200"} 15234
http_requests_total{method="POST", endpoint="/api/users", status="500"} 42
http_request_duration_seconds{method="POST", endpoint="/api/users", quantile="0.5"} 0.123
http_request_duration_seconds{method="POST", endpoint="/api/users", quantile="0.95"} 0.456
http_request_duration_seconds{method="POST", endpoint="/api/users", quantile="0.99"} 0.890
```

**Alerting pe metrici:**
```yaml
# Prometheus Alert
groups:
- name: api_alerts
  rules:
  - alert: HighErrorRate
    expr: |
      rate(http_requests_total{status=~"5.."}[5m]) / 
      rate(http_requests_total[5m]) > 0.05
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "High error rate detected"
      description: "Error rate is {{ $value | humanizePercentage }}"
```

---

**3. Traces (Urmăriri Distribuite)**

**Ce sunt:**
- Request flow prin multiple servicii
- Înregistrează latența fiecărei operații
- Relații parent-child între operații

**Concepte:**

**Trace** = end-to-end request journey
```
TraceID: abc-123-def-456
Total Duration: 523ms
```

**Span** = single operation în trace
```
Span 1: API Gateway           50ms   (parent)
  Span 2: User Service        120ms  (child of 1)
    Span 3: DB Query          80ms   (child of 2)
    Span 4: Cache Lookup      10ms   (child of 2)
  Span 5: Email Service       200ms  (child of 1)
    Span 6: SMTP Send         180ms  (child of 5)
  Span 7: Response Format     10ms   (child of 1)
```

**Implementare cu Spring Cloud Sleuth:**

```java
// application.yml
spring:
  sleuth:
    sampler:
      probability: 1.0  # 100% - toate request-urile (reduce în prod la 0.1)
  zipkin:
    base-url: http://zipkin:9411

// Logs automat includ trace/span IDs
[user-service,abc123def456,1234567890ab,true] Creating user john@example.com
                ↑ traceId    ↑ spanId     ↑ exported to Zipkin
```

**Propagare context între servicii:**

```java
// Service A → Service B (HTTP)
@Service
public class OrderService {
    
    @Autowired
    private RestTemplate restTemplate;  // Auto-configured cu Sleuth interceptors
    
    public Order createOrder(Order order) {
        // TraceID și SpanID propagate automat în HTTP headers
        // B3 Headers: X-B3-TraceId, X-B3-SpanId, X-B3-ParentSpanId
        User user = restTemplate.getForObject(
            "http://user-service/users/" + order.getUserId(),
            User.class
        );
        return orderRepository.save(order);
    }
}
```

**Custom Spans:**

```java
import org.springframework.cloud.sleuth.Tracer;
import org.springframework.cloud.sleuth.Span;

@Service
public class PaymentService {
    
    @Autowired
    private Tracer tracer;
    
    public void processPayment(Payment payment) {
        Span span = tracer.nextSpan().name("process-payment").start();
        try (Tracer.SpanInScope ws = tracer.withSpan(span)) {
            span.tag("payment.amount", payment.getAmount().toString());
            span.tag("payment.currency", payment.getCurrency());
            
            // Business logic
            paymentGateway.charge(payment);
            
            span.event("payment.charged");
        } finally {
            span.end();
        }
    }
}
```

**Visualizare în Jaeger/Zipkin:**

```
Request: POST /api/orders
TraceID: abc-123-def-456
Duration: 523ms

┌─ api-gateway (50ms) ──────────────────────────────────────────┐
│  ├─ user-service (120ms) ────────────┐                        │
│  │  ├─ db:findById (80ms) ─────┐     │                        │
│  │  └─ cache:get (10ms) ──┐    │     │                        │
│  ├─ product-service (80ms) ────┐     │                        │
│  │  └─ db:findById (70ms) ─┐   │     │                        │
│  ├─ payment-service (200ms) ──────────┐                       │
│  │  ├─ validate (20ms) ─┐    │  │     │                       │
│  │  └─ gateway:charge (180ms) ──────┐ │                       │
│  └─ email-service (100ms) ──────────────┐                     │
│     └─ smtp:send (90ms) ───────┐   │  │ │                     │
└────────────────────────────────────────────────────────────────┘
    0ms    100ms   200ms   300ms   400ms   500ms

Bottleneck: email-service → smtp:send (90ms)
```

---

**Tools & Platforms:**

**1. ELK Stack** (Logs)
```
Elasticsearch: Search & analytics
Logstash: Log processing pipeline
Kibana: Visualization
```

**2. Prometheus + Grafana** (Metrics)
```
Prometheus: Metrics collection & storage (pull-based)
Grafana: Dashboards & visualization
AlertManager: Alerting
```

**3. Jaeger / Zipkin** (Traces)
```
Jaeger: Distributed tracing (Uber)
Zipkin: Distributed tracing (Twitter)
OpenTelemetry: Vendor-neutral standard
```

**4. APM All-in-One:**
```
New Relic: Full-stack observability
Datadog: Infrastructure + APM
Dynatrace: AI-powered observability
AppDynamics: Business transaction monitoring
Elastic APM: Part of Elastic Stack
```

---

**Observability în practică - Troubleshooting Flow:**

**Scenario:** API slow (latency spike)

**Step 1: Metrics (What's happening?)**
```
Grafana Dashboard shows:
- p95 latency: 2000ms (was 200ms)
- Error rate: 5% (was 0.1%)
- Traffic: same
→ Something slowed down
```

**Step 2: Logs (Where's the problem?)**
```
Kibana search: level:ERROR AND timestamp:[now-15m TO now]
Results:
- 200 errors from "payment-service"
- Exception: "Connection timeout to payment-gateway"
→ Payment gateway is slow/down
```

**Step 3: Traces (Why is it slow?)**
```
Jaeger trace analysis:
- payment-service → payment-gateway: 1950ms (was 50ms)
- Retry attempts: 3 (exponential backoff)
→ Payment gateway is timing out, retries adding latency
```

**Step 4: Infrastructure (Root cause?)**
```
Check payment-gateway logs:
- Database connection pool exhausted
- Max connections: 10
- Active connections: 10
- Waiting: 50
→ Need to scale DB connection pool
```

**Resolution:**
```
1. Immediate: Circuit breaker on payment-gateway
2. Short-term: Increase DB connection pool
3. Long-term: Cache payment validation results
```

---

**Best Practices Observability:**

**1. Correlation IDs**
```java
@Component
public class CorrelationIdFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
        String correlationId = UUID.randomUUID().toString();
        MDC.put("correlationId", correlationId);
        ((HttpServletResponse) response).setHeader("X-Correlation-ID", correlationId);
        try {
            chain.doFilter(request, response);
        } finally {
            MDC.clear();
        }
    }
}
```

**2. Structured Data**
```java
// ❌ BAD
log.info("User " + userId + " created order " + orderId);

// ✅ GOOD
log.info("Order created",
    kv("userId", userId),
    kv("orderId", orderId),
    kv("amount", order.getTotal()),
    kv("items", order.getItemCount())
);
```

**3. High-Cardinality Tagging**
```java
// Permite query-uri detaliate
counter.increment(
    "userId", userId,           // High cardinality
    "country", user.getCountry(), // Medium cardinality
    "plan", user.getPlan()      // Low cardinality
);
```

**4. SLOs (Service Level Objectives)**
```
SLO: 99.9% of requests < 500ms
SLI (Indicator): Actual measured latency
Error Budget: 0.1% (43 minutes/month downtime allowed)
```

---

## Java Evolution

### Java 11 vs 17 vs 21

#### Care sunt diferențele majore și ce features noi au adus fiecare?

**Răspuns:**

**Context:** Java folosește un release cycle de 6 luni, cu versiuni LTS (Long Term Support) la fiecare 3 ani.

**LTS Versions:**
- Java 8 (2014) - support până 2030
- Java 11 (2018) - support până 2026
- Java 17 (2021) - support până 2029
- Java 21 (2023) - support până 2031

---

**Java 11 (LTS - Septembrie 2018)**

**Features majore:**

**1. Local-Variable Syntax for Lambda Parameters**
```java
// Java 10: var pentru variabile locale
var list = List.of("a", "b", "c");

// Java 11: var în lambda parameters
list.forEach((var item) -> System.out.println(item));

// Beneficiu: annotations pe lambda params
list.forEach((@NotNull var item) -> System.out.println(item));
```

**2. String API Enhancements**
```java
String text = "  Hello World  ";

// isBlank() - true dacă empty sau doar whitespace
" ".isBlank();      // true
"".isBlank();       // true
"  ".isBlank();     // true
"a".isBlank();      // false

// strip() - remove leading/trailing whitespace (Unicode-aware)
text.strip();       // "Hello World"
text.stripLeading(); // "Hello World  "
text.stripTrailing();// "  Hello World"

// lines() - split by line terminators
"A\nB\nC".lines().count();  // 3
"A\nB\nC".lines().forEach(System.out::println);

// repeat() - repeat string N times
"Ha".repeat(3);     // "HaHaHa"
"-".repeat(10);     // "----------"
```

**3. Files API Enhancements**
```java
// readString() / writeString()
Path path = Path.of("/tmp/file.txt");
String content = Files.readString(path);
Files.writeString(path, "Hello World", StandardOpenOption.CREATE);

// isSameFile() helper
boolean same = Files.isSameFile(path1, path2);
```

**4. Collection.toArray(IntFunction)**
```java
List<String> list = List.of("a", "b", "c");

// Java 8
String[] array = list.toArray(new String[0]);

// Java 11 - cleaner
String[] array = list.toArray(String[]::new);
```

**5. HTTP Client API (Standard)**

Din Java 9 era incubator, în Java 11 devine standard.

```java
HttpClient client = HttpClient.newBuilder()
    .version(HttpClient.Version.HTTP_2)
    .connectTimeout(Duration.ofSeconds(10))
    .build();

HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/users"))
    .header("Authorization", "Bearer token")
    .GET()
    .build();

// Synchronous
HttpResponse<String> response = client.send(request, 
    HttpResponse.BodyHandlers.ofString());
System.out.println(response.body());

// Asynchronous
CompletableFuture<HttpResponse<String>> futureResponse = 
    client.sendAsync(request, HttpResponse.BodyHandlers.ofString());

futureResponse.thenAccept(res -> System.out.println(res.body()));
```

**6. Removed Features**
- **Java EE modules:** JAXB, JAX-WS, CORBA
  ```java
  // ❌ No longer included
  import javax.xml.bind.JAXBContext;
  
  // ✅ Add dependency manually
  <dependency>
      <groupId>javax.xml.bind</groupId>
      <artifactId>jaxb-api</artifactId>
      <version>2.3.1</version>
  </dependency>
  ```
- **JavaFX** - moved to separate project
- **Nashorn JavaScript Engine** - deprecated

**7. New GC: Epsilon (No-Op GC)**
```
-XX:+UnlockExperimentalVMOptions -XX:+UseEpsilonGC

Use case: Performance testing, short-lived apps
```

**8. Flight Recorder (JFR) - Open Source**
```
Low-overhead profiling tool
-XX:StartFlightRecording=duration=60s,filename=recording.jfr
```

---

**Java 17 (LTS - Septembrie 2021)**

**Features majore:**

**1. Sealed Classes (JEP 409) - FINALIZAT**

Restricționează ce clase pot extinde/implementa.

```java
public sealed class Shape 
    permits Circle, Rectangle, Triangle {
    
    public abstract double area();
}

// Subclasele TREBUIE să fie: final, sealed, sau non-sealed
public final class Circle extends Shape {
    private final double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

public final class Rectangle extends Shape {
    private final double width, height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double area() {
        return width * height;
    }
}

public non-sealed class Triangle extends Shape {
    // non-sealed allows further extension
    private final double base, height;
    
    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }
    
    @Override
    public double area() {
        return 0.5 * base * height;
    }
}

// ❌ Compilation error
public class Hexagon extends Shape { } // Not permitted!
```

**Beneficii:**
- Domain modeling cu type-safe
- Pattern matching exhaustiveness checking
- Prevent unwanted inheritance

**2. Pattern Matching for switch (Preview în 17, Final în 21)**

```java
// Java 17 (preview)
public String format(Object obj) {
    return switch (obj) {
        case Integer i -> String.format("int %d", i);
        case Long l    -> String.format("long %d", l);
        case Double d  -> String.format("double %f", d);
        case String s  -> String.format("String %s", s);
        default        -> obj.toString();
    };
}

// Cu guards (when clause)
public String describe(Object obj) {
    return switch (obj) {
        case String s when s.length() > 5 -> "Long string: " + s;
        case String s -> "Short string: " + s;
        case Integer i when i > 0 -> "Positive: " + i;
        case Integer i -> "Non-positive: " + i;
        default -> "Unknown";
    };
}

// Cu sealed classes - exhaustive
public double calculateArea(Shape shape) {
    return switch (shape) {
        case Circle c    -> Math.PI * c.radius() * c.radius();
        case Rectangle r -> r.width() * r.height();
        case Triangle t  -> 0.5 * t.base() * t.height();
        // No default needed - compiler knows all cases
    };
}
```

**3. Records (JEP 395) - FINALIZAT**

Introduse în Java 14 (preview), finalizate în Java 16, incluse în LTS 17.

```java
// Înainte de Records
public final class Point {
    private final int x;
    private final int y;
    
    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    public int x() { return x; }
    public int y() { return y; }
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Point)) return false;
        Point point = (Point) o;
        return x == point.x && y == point.y;
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(x, y);
    }
    
    @Override
    public String toString() {
        return "Point[x=" + x + ", y=" + y + "]";
    }
}

// Cu Records - 1 linie!
public record Point(int x, int y) { }

// Auto-generated:
// - Constructor: Point(int x, int y)
// - Accessors: x(), y()
// - equals(), hashCode(), toString()
// - Final class, final fields
```

**Records cu customizare:**
```java
public record Person(String name, int age) {
    
    // Compact constructor - validation
    public Person {
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
        if (name == null || name.isBlank()) {
            throw new IllegalArgumentException("Name is required");
        }
        // Auto-assignment happens after this
    }
    
    // Additional constructor
    public Person(String name) {
        this(name, 0);
    }
    
    // Custom methods
    public boolean isAdult() {
        return age >= 18;
    }
    
    // Static factory
    public static Person unknown() {
        return new Person("Unknown", 0);
    }
    
    // Override toString
    @Override
    public String toString() {
        return name + " (" + age + " years)";
    }
}
```

**Records în Collections:**
```java
public record User(Long id, String name, String email) { }

List<User> users = List.of(
    new User(1L, "Alice", "alice@example.com"),
    new User(2L, "Bob", "bob@example.com"),
    new User(3L, "Charlie", "charlie@example.com")
);

// Filtering
List<User> filtered = users.stream()
    .filter(user -> user.name().startsWith("A"))
    .toList();

// Mapping
List<String> emails = users.stream()
    .map(User::email)
    .toList();

// Grouping
Map<String, List<User>> byDomain = users.stream()
    .collect(Collectors.groupingBy(
        user -> user.email().split("@")[1]
    ));
```

**4. Text Blocks (JEP 378) - FINALIZAT**

Introduse în Java 13 (preview), finalizate în Java 15.

```java
// Înainte
String html = "<html>\n" +
              "    <body>\n" +
              "        <p>Hello, World!</p>\n" +
              "    </body>\n" +
              "</html>\n";

// Cu Text Blocks
String html = """
    <html>
        <body>
            <p>Hello, World!</p>
        </body>
    </html>
    """;

// JSON
String json = """
    {
        "name": "John",
        "age": 30,
        "email": "john@example.com"
    }
    """;

// SQL
String query = """
    SELECT u.id, u.name, o.total
    FROM users u
    JOIN orders o ON u.id = o.user_id
    WHERE u.active = true
    ORDER BY o.created_at DESC
    """;
```

**Text Blocks - String interpolation (nu există direct, folosești formatted()):**
```java
String name = "Alice";
int age = 30;

String message = """
    Hello, %s!
    You are %d years old.
    """.formatted(name, age);

// Sau String.format()
String message = String.format("""
    Hello, %s!
    You are %d years old.
    """, name, age);
```

**5. Enhanced Pseudo-Random Number Generators (JEP 356)**

```java
// Old way
Random random = new Random();
int value = random.nextInt();

// New way - interface-based
RandomGenerator generator = RandomGenerator.of("L64X128MixRandom");
int value = generator.nextInt();

// Different algorithms
RandomGenerator.StreamableGenerator streamable = 
    RandomGenerator.of("L128X256MixRandom").splits().findFirst().orElseThrow();

// For cryptographic use
SecureRandom secureRandom = new SecureRandom();
```

**6. Strong Encapsulation of JDK Internals (JEP 403)**

```java
// ❌ No longer accessible by default
import sun.misc.Unsafe;
Unsafe unsafe = Unsafe.getUnsafe(); // IllegalAccessError

// Need to explicitly open with:
--add-opens java.base/sun.misc=ALL-UNNAMED

// Better: Use official APIs
```

**7. Context-Specific Deserialization Filters (JEP 415)**

Security improvement pentru Object deserialization.

```java
ObjectInputFilter filter = ObjectInputFilter.Config.createFilter(
    "example.AllowedClass;!*"
);

ObjectInputStream ois = new ObjectInputStream(inputStream);
ois.setObjectInputFilter(filter);
```

**8. macOS/AArch64 Support**

Native support pentru Apple Silicon (M1/M2/M3 chips).

---

**Java 21 (LTS - Septembrie 2023)**

**Features revoluționare:**

**1. Virtual Threads (Project Loom - JEP 444) - FINALIZAT**

**Problema:** Platform threads sunt scumpe (1MB stack, OS-managed).

```java
// Platform Thread - heavyweight
Thread thread = new Thread(() -> {
    // 1MB stack allocated
    // OS scheduling
});

// Limită practică: ~10,000 threads
```

**Soluția:** Virtual Threads - lightweight, JVM-managed.

```java
// Virtual Thread - foarte lightweight
Thread vThread = Thread.ofVirtual().start(() -> {
    // ~1KB stack
    // JVM scheduling
    System.out.println("Hello from virtual thread!");
});

// Poți avea MILIOANE de virtual threads
```

**Exemplu: Server cu 1 million de conexiuni simultane**

```java
// ❌ Platform threads - impossible
// 1 million threads × 1MB = 1TB RAM!

// ✅ Virtual threads - easy
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    IntStream.range(0, 1_000_000).forEach(i -> {
        executor.submit(() -> {
            // Handle connection
            Thread.sleep(Duration.ofSeconds(10));
            return "Task " + i + " completed";
        });
    });
} // Auto-closes, waits for all tasks
```

**Virtual Thread creation:**

```java
// 1. Thread.ofVirtual()
Thread vt = Thread.ofVirtual().start(() -> System.out.println("VT 1"));

// 2. Thread.startVirtualThread()
Thread.startVirtualThread(() -> System.out.println("VT 2"));

// 3. Virtual thread executor
ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor();
executor.submit(() -> System.out.println("VT 3"));

// 4. Thread builder
Thread.Builder builder = Thread.ofVirtual().name("vt-", 0);
Thread vt1 = builder.start(() -> System.out.println("VT 4"));
Thread vt2 = builder.start(() -> System.out.println("VT 5"));
```

**Când folosești Virtual Threads:**

✅ **DA:**
- I/O-bound tasks (DB queries, HTTP calls, file I/O)
- High concurrency (many concurrent requests)
- Blocking operations

```java
// Web server - one virtual thread per request
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    while (true) {
        Socket socket = serverSocket.accept();
        executor.submit(() -> handleRequest(socket));
    }
}
```

❌ **NU:**
- CPU-bound tasks (calculations, image processing)
- Tasks that hold locks for long time
- Code with synchronized blocks on shared objects

**2. Sequenced Collections (JEP 431)**

**Problema:** Nu existau metode comune pentru acces la primul/ultimul element.

```java
// Înainte - inconsistent
List<String> list = new ArrayList<>();
String first = list.get(0);              // List
String last = list.get(list.size() - 1);

Deque<String> deque = new ArrayDeque<>();
String first = deque.getFirst();         // Deque
String last = deque.getLast();

SortedSet<String> set = new TreeSet<>();
String first = set.first();              // SortedSet
String last = set.last();
```

**Soluția:** Interface nou `SequencedCollection`

```java
interface SequencedCollection<E> extends Collection<E> {
    // Access
    E getFirst();
    E getLast();
    
    // Modification
    void addFirst(E e);
    void addLast(E e);
    E removeFirst();
    E removeLast();
    
    // Reverse view
    SequencedCollection<E> reversed();
}

// Similar: SequencedSet, SequencedMap
```

**Exemplu:**

```java
List<String> list = new ArrayList<>(List.of("A", "B", "C", "D"));

// Access
String first = list.getFirst();  // "A"
String last = list.getLast();    // "D"

// Modification
list.addFirst("Z");  // [Z, A, B, C, D]
list.addLast("Y");   // [Z, A, B, C, D, Y]

// Reverse
List<String> reversed = list.reversed();  // [Y, D, C, B, A, Z]

// Iteration
for (String s : reversed) {
    System.out.println(s);  // Y, D, C, B, A, Z
}
```

**SequencedMap:**

```java
SequencedMap<String, Integer> map = new LinkedHashMap<>();
map.put("one", 1);
map.put("two", 2);
map.put("three", 3);

// First/Last entry
Map.Entry<String, Integer> first = map.firstEntry();  // one=1
Map.Entry<String, Integer> last = map.lastEntry();    // three=3

// Reversed map
SequencedMap<String, Integer> reversed = map.reversed();
// {three=3, two=2, one=1}

// Poll first/last
Map.Entry<String, Integer> polled = map.pollFirstEntry();  // Removes and returns
```

**3. Pattern Matching for switch (JEP 441) - FINALIZAT**

```java
public String describe(Object obj) {
    return switch (obj) {
        case null      -> "null value";
        case String s when s.isEmpty() -> "empty string";
        case String s when s.length() > 10 -> "long string: " + s.substring(0, 10) + "...";
        case String s  -> "string: " + s;
        case Integer i when i > 0 -> "positive integer: " + i;
        case Integer i -> "non-positive integer: " + i;
        case Long l    -> "long: " + l;
        case Double d  -> "double: " + d;
        case int[] arr -> "int array of length " + arr.length;
        default        -> "unknown: " + obj.toString();
    };
}
```

**4. Record Patterns (JEP 440)**

Deconstrucție records în pattern matching.

```java
public record Point(int x, int y) { }
public record Rectangle(Point topLeft, Point bottomRight) { }

// Nested pattern matching
public int area(Shape shape) {
    return switch (shape) {
        case Rectangle(Point(int x1, int y1), Point(int x2, int y2)) ->
            (x2 - x1) * (y2 - y1);
        case Circle(Point(int x, int y), int radius) ->
            (int) (Math.PI * radius * radius);
    };
}

// instanceof cu pattern
if (obj instanceof Point(int x, int y)) {
    System.out.println("Point at: " + x + ", " + y);
}

// Nested records
public record Order(String id, Customer customer, List<Item> items) { }
public record Customer(String name, Address address) { }
public record Address(String city, String country) { }

if (order instanceof Order(String id, Customer(String name, Address(String city, _)), _)) {
    System.out.println("Order " + id + " for " + name + " in " + city);
}
```

**5. String Templates (JEP 430) - PREVIEW**

**IMPORTANT:** Preview feature, needs `--enable-preview`

```java
// Old way - concatenation
String message = "Hello, " + name + "! You have " + count + " messages.";

// Old way - format
String message = String.format("Hello, %s! You have %d messages.", name, count);

// New way - String Templates
String message = STR."Hello, \{name}! You have \{count} messages.";

// Multi-line
String html = STR."""
    <html>
        <body>
            <h1>Welcome, \{user.name()}!</h1>
            <p>You have \{user.messageCount()} new messages.</p>
        </body>
    </html>
    """;

// Expressions
int x = 10, y = 20;
String result = STR."\{x} + \{y} = \{x + y}";  // "10 + 20 = 30"

// Method calls
String greeting = STR."Hello, \{user.getName().toUpperCase()}!";
```

**Custom template processors:**

```java
// JSON template
JSONObject json = JSON."""
    {
        "name": "\{user.name()}",
        "age": \{user.age()},
        "email": "\{user.email()}"
    }
    """;

// SQL template (safe from SQL injection)
ResultSet rs = DB."""
    SELECT * FROM users
    WHERE name = \{userName}
    AND age > \{minAge}
    """;
```

**6. Unnamed Patterns and Variables (JEP 443)**

Când nu te interesează valoarea.

```java
// Unnamed variable with _
if (obj instanceof Point(int x, int _)) {
    // Only care about x, ignore y
    System.out.println("x = " + x);
}

// In switch
switch (shape) {
    case Rectangle(Point(int x, int _), Point(_, int y2)) ->
        // Only care about x and y2
        System.out.println("Width: " + x + ", Height: " + y2);
}

// In for loops
for (int i = 0, _ = sideEffect(); i < 10; i++) {
    // Don't care about second variable, just side effect
}

// In try-with-resources
try (var _ = lock.acquire()) {
    // Don't use the resource, just need it acquired
    doWork();
}

// In lambda (when you don't use parameter)
list.forEach(_ -> System.out.println("Item"));
```

**7. Unnamed Classes and Instance Main Methods (JEP 445) - PREVIEW**

Simplified Java for beginners/scripts.

```java
// Traditional
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}

// Java 21 (preview)
void main() {
    System.out.println("Hello, World!");
}

// With arguments
void main(String[] args) {
    System.out.println("Args: " + Arrays.toString(args));
}
```

**8. Scoped Values (JEP 446) - PREVIEW**

Alternative la ThreadLocal, better for virtual threads.

```java
// Old way - ThreadLocal
private static final ThreadLocal<User> currentUser = new ThreadLocal<>();

public void handleRequest(Request request) {
    currentUser.set(request.getUser());
    try {
        processRequest(request);
    } finally {
        currentUser.remove();  // Must cleanup!
    }
}

// New way - Scoped Values
private static final ScopedValue<User> CURRENT_USER = ScopedValue.newInstance();

public void handleRequest(Request request) {
    ScopedValue.where(CURRENT_USER, request.getUser())
        .run(() -> processRequest(request));
    // Auto-cleanup when scope ends
}
```

---

**Comparație Sumar:**

| Feature | Java 11 | Java 17 | Java 21 |
|---------|---------|---------|---------|
| **LTS** | ✅ | ✅ | ✅ |
| **Support până** | 2026 | 2029 | 2031 |
| **var** | ✅ Lambda params | ✅ | ✅ |
| **String APIs** | ✅ isBlank, strip, etc | ✅ | ✅ |
| **HTTP Client** | ✅ Standard | ✅ | ✅ |
| **Records** | ❌ | ✅ Final | ✅ |
| **Sealed Classes** | ❌ | ✅ Final | ✅ |
| **Pattern Matching** | ❌ | Preview | ✅ Final |
| **Text Blocks** | ❌ | ✅ Final | ✅ |
| **Virtual Threads** | ❌ | ❌ | ✅ Final |
| **Sequenced Collections** | ❌ | ❌ | ✅ |
| **String Templates** | ❌ | ❌ | Preview |
| **Record Patterns** | ❌ | ❌ | ✅ |

**Migration path:**

```
Java 8 → Java 11:
- Prepare: Remove Java EE dependencies
- Add: JAXB if needed
- Test: Especially reflection/internals usage

Java 11 → Java 17:
- Benefit: Records for DTOs
- Benefit: Sealed classes for domain model
- Benefit: Pattern matching (preview)
- Test: Strong encapsulation

Java 17 → Java 21:
- Benefit: Virtual threads for high concurrency
- Benefit: Pattern matching (final)
- Benefit: Sequenced collections
- Test: Virtual thread compatibility
```

**Production recommendations:**

- **Java 11:** Stable, well-tested, good for conservative environments
- **Java 17:** Sweet spot - modern features, LTS support
- **Java 21:** Latest LTS, best features, adopt if starting new project

---

## Java Core Features

### Streams API

#### Ai folosit Streams? Cum le compari cu buclele clasice? Avantaje și dezavantaje?

**Răspuns:**

Da, folosesc Streams extensiv pentru procesarea colecțiilor. Streams oferă o **API declarativă** pentru operații pe date.

**Concepte fundamentale:**

**Stream** = secvență de elemente ce suportă operații agregate
- **Nu e data structure** (nu stochează elemente)
- **Nu modifică sursa** (operații non-mutative)
- **Lazy evaluation** (operații intermediate nu se execută până la terminal operation)
- **Posibil infinite** (IntStream.iterate())

**Tipuri de operații:**

**1. Intermediate** (returnează Stream, lazy)
- `filter()`, `map()`, `flatMap()`
- `distinct()`, `sorted()`, `limit()`, `skip()`
- `peek()` - debugging

**2. Terminal** (returnează rezultat, trigger execution)
- `forEach()`, `collect()`, `reduce()`
- `count()`, `min()`, `max()`, `sum()`, `average()`
- `anyMatch()`, `allMatch()`, `noneMatch()`
- `findFirst()`, `findAny()`

---

**Comparație detaliată: Loop vs Stream**

**Exemplu 1: Filtrare și transformare**

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve");

// ❌ Imperative - HOW (cum să faci)
List<String> result = new ArrayList<>();
for (String name : names) {
    if (name.length() > 3) {
        String upper = name.toUpperCase();
        result.add(upper);
    }
}
// [ALICE, CHARLIE, DAVID]

// ✅ Declarative - WHAT (ce vrei)
List<String> result = names.stream()
    .filter(name -> name.length() > 3)
    .map(String::toUpperCase)
    .collect(Collectors.toList());
// [ALICE, CHARLIE, DAVID]
```

**Exemplu 2: Găsirea primului element**

```java
List<User> users = getUserList();

// ❌ Loop - procesează toată lista
User admin = null;
for (User user : users) {
    if (user.isAdmin()) {
        admin = user;
        break;  // Trebuie explicit break
    }
}

// ✅ Stream - lazy, oprește la primul
Optional<User> admin = users.stream()
    .filter(User::isAdmin)
    .findFirst();  // Short-circuit, nu procesează tot
```

**Exemplu 3: Operații complexe**

```java
List<Order> orders = getOrders();

// ❌ Loop - multe variabile temporare
Map<String, BigDecimal> totalByCustomer = new HashMap<>();
for (Order order : orders) {
    if (order.getStatus() == Status.COMPLETED) {
        String customer = order.getCustomerId();
        BigDecimal total = order.getTotal();
        totalByCustomer.merge(customer, total, BigDecimal::add);
    }
}

// ✅ Stream - fluent, expresiv
Map<String, BigDecimal> totalByCustomer = orders.stream()
    .filter(order -> order.getStatus() == Status.COMPLETED)
    .collect(Collectors.groupingBy(
        Order::getCustomerId,
        Collectors.reducing(BigDecimal.ZERO, Order::getTotal, BigDecimal::add)
    ));
```

---

**Avantaje Streams:**

**✅ 1. Declarativ și lizibil**

```java
// Ce vrei să faci, nu cum
users.stream()
    .filter(User::isActive)
    .map(User::getEmail)
    .distinct()
    .sorted()
    .limit(10)
    .collect(Collectors.toList());
```

**✅ 2. Composable - chain operations**

```java
// Ușor de extins și modificat
Stream<User> baseQuery = users.stream()
    .filter(User::isActive);

List<String> emails = baseQuery
    .map(User::getEmail)
    .collect(Collectors.toList());

List<String> names = baseQuery
    .map(User::getName)
    .collect(Collectors.toList());
```

**✅ 3. Lazy evaluation - optimizări**

```java
// Doar 5 elemente procesate, nu 1000
List<Integer> result = IntStream.range(1, 1000)
    .filter(n -> n % 2 == 0)      // Not executed yet
    .map(n -> n * 2)               // Not executed yet
    .limit(5)                      // Not executed yet
    .boxed()
    .collect(Collectors.toList()); // NOW executes, stops at 5

// Loop ar procesa tot, apoi take 5
```

**✅ 4. Parallelism trivial**

```java
// Sequential
long count = list.stream()
    .filter(expensivePredicate)
    .count();

// Parallel - just add parallel()
long count = list.parallelStream()
    .filter(expensivePredicate)
    .count();

// Folosește ForkJoinPool.commonPool()
// Divide and conquer automatic
```

**✅ 5. Operații complexe simplificate**

**Grouping:**
```java
// Group users by role
Map<String, List<User>> byRole = users.stream()
    .collect(Collectors.groupingBy(User::getRole));

// Group and count
Map<String, Long> countByRole = users.stream()
    .collect(Collectors.groupingBy(
        User::getRole,
        Collectors.counting()
    ));

// Group and sum
Map<String, Integer> ageByRole = users.stream()
    .collect(Collectors.groupingBy(
        User::getRole,
        Collectors.summingInt(User::getAge)
    ));
```

**Partitioning:**
```java
// Split into two groups
Map<Boolean, List<User>> activeInactive = users.stream()
    .collect(Collectors.partitioningBy(User::isActive));

List<User> active = activeInactive.get(true);
List<User> inactive = activeInactive.get(false);
```

**Statistics:**
```java
IntSummaryStatistics stats = users.stream()
    .mapToInt(User::getAge)
    .summaryStatistics();

System.out.println("Average: " + stats.getAverage());
System.out.println("Min: " + stats.getMin());
System.out.println("Max: " + stats.getMax());
System.out.println("Sum: " + stats.getSum());
System.out.println("Count: " + stats.getCount());
```

**✅ 6. Infinite streams**

```java
// Generate infinite stream
Stream.iterate(0, n -> n + 2)
    .limit(10)
    .forEach(System.out::println);
// 0, 2, 4, 6, 8, 10, 12, 14, 16, 18

// Random numbers
Stream.generate(Math::random)
    .limit(5)
    .forEach(System.out::println);
```

---

**Dezavantaje Streams:**

**❌ 1. Performance pentru colecții mici (<100 elemente)**

```java
List<Integer> small = IntStream.range(1, 10).boxed().collect(Collectors.toList());

// Loop: ~5-10 nanoseconds
int sum = 0;
for (int n : small) {
    sum += n;
}

// Stream: ~50-100 nanoseconds (10x mai lent)
int sum = small.stream()
    .mapToInt(Integer::intValue)
    .sum();

// Overhead: pipeline creation, boxing/unboxing, lambda invocation
```

**Benchmark:**
```
List size   | Loop time | Stream time | Ratio
10          | 5ns       | 50ns        | 10x slower
100         | 50ns      | 200ns       | 4x slower
1,000       | 500ns     | 1,500ns     | 3x slower
10,000      | 5µs       | 12µs        | 2.4x slower
1,000,000   | 500µs     | 900µs       | 1.8x slower
```

**❌ 2. Debugging mai greu**

```java
// Loop - easy to debug, step through
List<String> result = new ArrayList<>();
for (String name : names) {
    if (name.length() > 4) {  // BREAKPOINT here
        String upper = name.toUpperCase();  // BREAKPOINT here
        result.add(upper);  // BREAKPOINT here
    }
}

// Stream - hard to debug
List<String> result = names.stream()
    .filter(name -> name.length() > 4)  // Can't easily see intermediate values
    .map(String::toUpperCase)
    .collect(Collectors.toList());

// Soluție: peek() pentru debugging
List<String> result = names.stream()
    .peek(name -> System.out.println("Original: " + name))
    .filter(name -> name.length() > 4)
    .peek(name -> System.out.println("After filter: " + name))
    .map(String::toUpperCase)
    .peek(name -> System.out.println("After map: " + name))
    .collect(Collectors.toList());
```

**❌ 3. Stack traces complexe și neclare**

```java
// Exception în stream
List<Integer> numbers = Arrays.asList(1, 2, 0, 4);

numbers.stream()
    .map(n -> 10 / n)  // Division by zero
    .collect(Collectors.toList());

// Stack trace:
Exception in thread "main" java.lang.ArithmeticException: / by zero
    at MyClass.lambda$main$0(MyClass.java:15)
    at java.base/java.util.stream.ReferencePipeline$3$1.accept(ReferencePipeline.java:197)
    at java.base/java.util.Spliterators$ArraySpliterator.forEachRemaining(Spliterators.java:948)
    at java.base/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:509)
    at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:499)
    at java.base/java.util.stream.ReduceOps$ReduceOp.evaluateSequential(ReduceOps.java:921)
    at java.base/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:234)
    at java.base/java.util.stream.ReferencePipeline.collect(ReferencePipeline.java:682)
    at MyClass.main(MyClass.java:15)
// 😵 Multe frames interne, greu de înțeles
```

**❌ 4. Side effects - anti-pattern**

```java
// ❌ BAD - mutating external state
List<String> results = new ArrayList<>();
stream.forEach(results::add);  // Side effect!

List<Integer> numbers = new ArrayList<>();
IntStream.range(1, 10)
    .forEach(numbers::add);  // Side effect!

// ✅ GOOD - use collector
List<String> results = stream.collect(Collectors.toList());

List<Integer> numbers = IntStream.range(1, 10)
    .boxed()
    .collect(Collectors.toList());
```

**❌ 5. Nu poate break/continue/return**

```java
// Loop - can break early
for (String item : items) {
    if (item.equals("STOP")) {
        break;  // Exit loop
    }
    process(item);
}

// Stream - cannot break (compilation error)
items.stream().forEach(item -> {
    if (item.equals("STOP")) {
        break;  // ❌ COMPILATION ERROR
    }
    process(item);
});

// Soluție 1: takeWhile (Java 9+)
items.stream()
    .takeWhile(item -> !item.equals("STOP"))
    .forEach(this::process);

// Soluție 2: findFirst with filter
Optional<String> found = items.stream()
    .filter(item -> item.equals("TARGET"))
    .findFirst();

// Soluție 3: anyMatch for early termination
boolean hasTarget = items.stream()
    .anyMatch(item -> item.equals("TARGET"));
```

**❌ 6. Parallel streams - shared mutable state dangerous**

```java
// ❌ DANGEROUS - race condition
List<Integer> list = new ArrayList<>();
IntStream.range(1, 1000)
    .parallel()
    .forEach(list::add);  // NOT thread-safe! ConcurrentModificationException

// ❌ DANGEROUS - non-deterministic result
AtomicInteger counter = new AtomicInteger(0);
IntStream.range(1, 1000)
    .parallel()
    .forEach(n -> counter.incrementAndGet());
// counter might not be 999 due to race conditions

// ✅ GOOD - use concurrent collector
List<Integer> list = IntStream.range(1, 1000)
    .parallel()
    .boxed()
    .collect(Collectors.toCollection(CopyOnWriteArrayList::new));

// ✅ GOOD - use reduction
int sum = IntStream.range(1, 1000)
    .parallel()
    .sum();  // Thread-safe reduction
```

**❌ 7. Boxing overhead pentru primitive streams**

```java
// ❌ BAD - boxing overhead
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.stream()
    .mapToInt(Integer::intValue)  // Unboxing
    .sum();

// ✅ BETTER - use IntStream directly
int sum = IntStream.of(1, 2, 3, 4, 5)
    .sum();

// ✅ BETTER - primitive stream from range
int sum = IntStream.rangeClosed(1, 5)
    .sum();
```

**❌ 8. Reusing streams - IllegalStateException**

```java
// ❌ ERROR - stream already consumed
Stream<String> stream = names.stream();
long count = stream.count();  // Terminal operation
List<String> list = stream.collect(Collectors.toList());  // IllegalStateException!

// ✅ GOOD - create new stream
long count = names.stream().count();
List<String> list = names.stream().collect(Collectors.toList());

// ✅ GOOD - use Supplier for reusable stream
Supplier<Stream<String>> streamSupplier = () -> names.stream();
long count = streamSupplier.get().count();
List<String> list = streamSupplier.get().collect(Collectors.toList());
```

---

**Când folosești Stream vs Loop:**

| Scenario | Prefer |
|----------|--------|
| Transformări complexe | **Stream** |
| Filtering + mapping + collecting | **Stream** |
| Grouping, partitioning, reducing | **Stream** |
| Operații simple pe liste mici (<100) | **Loop** |
| Need break/continue | **Loop** |
| Performance critical (hot path) | **Loop** (benchmark first!) |
| Procesare paralelă CPU-intensive | **parallelStream** |
| Side effects needed | **Loop** |
| Early exit important | **Loop** sau `findFirst` |
| Debugging complex logic | **Loop** |
| Infinite sequences | **Stream** |

---

**Best Practices Streams:**

**✅ 1. Preferă method references**

```java
// ❌ Verbose lambda
list.stream()
    .map(s -> s.toUpperCase())
    .collect(Collectors.toList());

// ✅ Method reference
list.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());
```

**✅ 2. Folosește primitive streams când e posibil**

```java
// ❌ Boxing overhead
Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5);
int sum = stream.mapToInt(Integer::intValue).sum();

// ✅ Direct primitive stream
int sum = IntStream.of(1, 2, 3, 4, 5).sum();
int sum = IntStream.rangeClosed(1, 5).sum();
```

**✅ 3. Evită collecting to stream again**

```java
// ❌ BAD - collecting to stream again
users.stream()
    .collect(Collectors.toList())
    .stream()  // Unnecessary!
    .filter(User::isActive)
    .collect(Collectors.toList());

// ✅ GOOD - one stream
users.stream()
    .filter(User::isActive)
    .collect(Collectors.toList());
```

**✅ 4. Folosește collectors specializați**

```java
// Joining strings
String result = names.stream()
    .collect(Collectors.joining(", ", "[", "]"));
// [Alice, Bob, Charlie]

// ToMap
Map<Long, User> map = users.stream()
    .collect(Collectors.toMap(User::getId, Function.identity()));

// GroupingBy cu downstream collector
Map<String, Long> countByRole = users.stream()
    .collect(Collectors.groupingBy(
        User::getRole,
        Collectors.counting()
    ));
```

**✅ 5. Short-circuit operations când e posibil**

```java
// anyMatch - stops at first match
boolean hasAdmin = users.stream()
    .anyMatch(User::isAdmin);

// findFirst - stops at first
Optional<User> first = users.stream()
    .filter(User::isActive)
    .findFirst();

// limit - processes only N elements
List<User> top10 = users.stream()
    .sorted(Comparator.comparing(User::getScore).reversed())
    .limit(10)
    .collect(Collectors.toList());
```

**✅ 6. Evită side effects**

```java
// ❌ BAD
List<String> results = new ArrayList<>();
stream.forEach(results::add);

// ✅ GOOD
List<String> results = stream.collect(Collectors.toList());
```

**✅ 7. Parallel streams cu grijă**

```java
// ✅ Good for parallel - CPU-intensive, independent operations
List<BigInteger> primes = numbers.parallelStream()
    .filter(this::isPrime)  // CPU-intensive
    .collect(Collectors.toList());

// ❌ Bad for parallel - I/O-bound, order matters
users.parallelStream()
    .forEach(this::saveToDatabase);  // I/O, unpredictable order
```

**✅ 8. Custom collectors când e nevoie**

```java
// Custom collector pentru immutable list
public static <T> Collector<T, ?, List<T>> toImmutableList() {
    return Collectors.collectingAndThen(
        Collectors.toList(),
        Collections::unmodifiableList
    );
}

List<String> immutable = stream.collect(toImmutableList());
```

---

**Operații Stream utile:**

**flatMap - flatten nested collections**
```java
List<List<String>> nested = List.of(
    List.of("A", "B"),
    List.of("C", "D"),
    List.of("E")
);

List<String> flat = nested.stream()
    .flatMap(Collection::stream)
    .collect(Collectors.toList());
// [A, B, C, D, E]

// Multiple lists
List<String> combined = Stream.of(list1, list2, list3)
    .flatMap(Collection::stream)
    .collect(Collectors.toList());
```

**distinct - remove duplicates**
```java
List<Integer> unique = numbers.stream()
    .distinct()
    .collect(Collectors.toList());

// Distinct by property (Java 9+)
List<User> uniqueByEmail = users.stream()
    .collect(Collectors.toMap(
        User::getEmail,
        Function.identity(),
        (u1, u2) -> u1  // Keep first
    ))
    .values()
    .stream()
    .collect(Collectors.toList());
```

**sorted - ordering**
```java
// Natural order
List<String> sorted = names.stream()
    .sorted()
    .collect(Collectors.toList());

// Custom comparator
List<User> sorted = users.stream()
    .sorted(Comparator.comparing(User::getName))
    .collect(Collectors.toList());

// Multiple criteria
List<User> sorted = users.stream()
    .sorted(Comparator
        .comparing(User::getRole)
        .thenComparing(User::getName))
    .collect(Collectors.toList());

// Reversed
List<User> sorted = users.stream()
    .sorted(Comparator.comparing(User::getAge).reversed())
    .collect(Collectors.toList());
```

**reduce - custom aggregation**
```java
// Sum
int sum = numbers.stream()
    .reduce(0, Integer::sum);

// Product
int product = numbers.stream()
    .reduce(1, (a, b) -> a * b);

// Concatenation
String concat = strings.stream()
    .reduce("", (a, b) -> a + b);

// Complex reduction
Optional<User> oldest = users.stream()
    .reduce((u1, u2) -> u1.getAge() > u2.getAge() ? u1 : u2);
```

**takeWhile / dropWhile (Java 9+)**
```java
// takeWhile - take elements while predicate true
List<Integer> lessThan5 = Stream.of(1, 2, 3, 6, 4, 5)
    .takeWhile(n -> n < 5)
    .collect(Collectors.toList());
// [1, 2, 3] - stops at 6

// dropWhile - skip elements while predicate true
List<Integer> afterFirst5 = Stream.of(1, 2, 3, 6, 4, 5)
    .dropWhile(n -> n < 5)
    .collect(Collectors.toList());
// [6, 4, 5]
```

---

**Concluzie:**

Streams sunt excelente pentru:
- Cod declarativ, lizibil
- Operații complexe pe colecții
- Procesare paralelă
- Pipeline-uri de transformări

Loop-urile clasice sunt mai bune pentru:
- Performance critical code
- Liste mici
- Logic complex cu break/continue
- Debugging

**Regula de aur:** Folosește Streams pentru **expresivitate**, Loop-uri pentru **performance**. Măsoară când contează!
Continuare fișier...

### Optional

#### Ai lucrat cu Optional? Cum previne NullPointerException și când NU ar trebui folosit?

**Răspuns:**

Da, `Optional<T>` este un container care **poate sau nu** să conțină o valoare non-null. Este un wrapper functional care forțează tratarea explicită a absenței valorii, introdus în Java 8.

**Problema fundamentală cu null:**

```java
// The Billion Dollar Mistake - Tony Hoare
User user = userRepository.findById(id);  // Poate returna null
String email = user.getEmail();  // NullPointerException!

// Defensive programming - null checks everywhere
if (user != null) {
    Address address = user.getAddress();
    if (address != null) {
        String city = address.getCity();
        if (city != null) {
            return city.toUpperCase();
        }
    }
}
return "UNKNOWN";
```

**Cum previne Optional NPE:**

**1. Forțează awareness-ul că valoarea poate lipsi**

```java
// ❌ Signature unclear - poate returna null?
User findById(Long id);  // Trebuie să citești docs/implementation

// ✅ Signature clear - poate să nu existe
Optional<User> findById(Long id);  // Evident că poate lipsi
```

**2. API functional pentru handling**

```java
// În loc de null checks, folosești combinatori
return userRepository.findById(id)
    .map(User::getAddress)
    .map(Address::getCity)
    .map(String::toUpperCase)
    .orElse("UNKNOWN");
```

---

**Crearea Optional:**

```java
// 1. Empty Optional
Optional<String> empty = Optional.empty();
empty.isPresent();  // false

// 2. Optional cu valoare non-null (throws NPE if null)
Optional<String> opt = Optional.of("value");
Optional.of(null);  // ❌ NullPointerException

// 3. Optional care POATE fi null (cel mai folosit)
String value = getPossiblyNullValue();
Optional<String> opt = Optional.ofNullable(value);
```

---

**Consuming Optional - Pattern-uri:**

**❌ Anti-patterns (defeats the purpose):**

```java
Optional<User> optUser = userRepository.findById(id);

// ❌ BAD - ca un null check
if (optUser.isPresent()) {
    User user = optUser.get();
    // Use user
}

// ❌ BAD - get() without check
User user = optUser.get();  // NoSuchElementException dacă empty!

// ❌ BAD - orElse(null)
User user = optUser.orElse(null);  // De ce Optional atunci?
```

**✅ Patterns corecte:**

**1. ifPresent - execute dacă există**

```java
optUser.ifPresent(user -> {
    System.out.println("Found: " + user.getName());
    sendEmail(user.getEmail());
});

// Method reference
optUser.ifPresent(this::sendWelcomeEmail);
```

**2. orElse - default value**

```java
User user = optUser.orElse(DEFAULT_USER);

String email = optUser
    .map(User::getEmail)
    .orElse("no-email@example.com");
```

**3. orElseGet - lazy default (Supplier)**

```java
// ❌ BAD - eager evaluation
User user = optUser.orElse(createDefaultUser());  // Always called!

// ✅ GOOD - lazy evaluation
User user = optUser.orElseGet(() -> createDefaultUser());  // Only if empty
User user = optUser.orElseGet(this::createDefaultUser);

// Important când crearea e costisitoare
User user = optUser.orElseGet(() -> {
    log.info("Creating default user");
    return userService.createGuest();  // DB call doar dacă lipsește
});
```

**4. orElseThrow - exception dacă lipsește**

```java
// Custom exception
User user = optUser.orElseThrow(() -> 
    new UserNotFoundException("User " + id + " not found"));

// Java 10+ - NoSuchElementException
User user = optUser.orElseThrow();
```

**5. ifPresentOrElse - ambele branch-uri (Java 9+)**

```java
optUser.ifPresentOrElse(
    user -> System.out.println("Found: " + user.getName()),
    () -> System.out.println("Not found")
);

// Practical example
optUser.ifPresentOrElse(
    this::updateUser,
    this::createNewUser
);
```

---

**Transforming Optional:**

**1. map - transform value**

```java
Optional<User> optUser = findUser(id);
Optional<String> optEmail = optUser.map(User::getEmail);
Optional<Integer> optAge = optUser.map(User::getAge);

// Chaining
Optional<String> optUpperEmail = optUser
    .map(User::getEmail)
    .map(String::toUpperCase);
```

**2. flatMap - avoid Optional<Optional<T>>**

```java
// ❌ Problem cu map - nested Optional
Optional<User> optUser = findUser(id);
Optional<Optional<Address>> nested = optUser.map(User::getAddress);  // getAddress() returns Optional

// ✅ Solution cu flatMap - flatten
Optional<Address> optAddress = optUser.flatMap(User::getAddress);

// Chaining multiple Optionals
Optional<String> city = userRepository.findById(id)
    .flatMap(User::getAddress)      // Optional<Address>
    .flatMap(Address::getCity)      // Optional<String>
    .map(String::toUpperCase);      // Optional<String>

// Equivalent fără Optional
String city = null;
User user = userRepository.findById(id);
if (user != null) {
    Address address = user.getAddress();
    if (address != null) {
        String c = address.getCity();
        if (c != null) {
            city = c.toUpperCase();
        }
    }
}
```

**3. filter - conditional**

```java
Optional<User> optAdmin = optUser
    .filter(User::isAdmin);

Optional<User> optActiveAdmin = optUser
    .filter(User::isActive)
    .filter(User::isAdmin);

// Complex filter
Optional<User> eligible = optUser
    .filter(user -> user.getAge() >= 18)
    .filter(user -> user.hasValidEmail())
    .filter(user -> user.isVerified());
```

**4. or - alternative Optional (Java 9+)**

```java
// Try first repository, fallback to second
Optional<User> user = userRepository.findById(id)
    .or(() -> userRepository.findByEmail(email))
    .or(() -> userRepository.findByUsername(username));

// Lazy evaluation - doar dacă primele sunt empty
Optional<Config> config = findInCache(key)
    .or(() -> findInDatabase(key))
    .or(() -> loadDefaultConfig());
```

**5. stream - convert to Stream (Java 9+)**

```java
// Filter empty Optionals from list
List<Optional<String>> optionals = List.of(
    Optional.of("A"),
    Optional.empty(),
    Optional.of("B"),
    Optional.empty(),
    Optional.of("C")
);

List<String> values = optionals.stream()
    .flatMap(Optional::stream)  // Filters out empty
    .collect(Collectors.toList());
// [A, B, C]

// Practical: get emails from users, skip those without email
List<String> emails = users.stream()
    .map(User::getEmail)        // Stream<Optional<String>>
    .flatMap(Optional::stream)  // Stream<String> - only present
    .collect(Collectors.toList());
```

---

**Când NU ar trebui folosit Optional:**

**❌ 1. Ca field în clasă**

```java
// ❌ BAD - Optional is not Serializable
public class User {
    private Optional<String> middleName;  // NO!
    private Optional<Address> address;    // NO!
}

// Probleme:
// - Nu e Serializable (Jackson, JPA)
// - Memory overhead (extra object wrapper)
// - Semantic confusion (field vs return value)

// ✅ GOOD - use null, expose Optional in getter
public class User {
    private String middleName;  // Can be null
    private Address address;    // Can be null
    
    public Optional<String> getMiddleName() {
        return Optional.ofNullable(middleName);
    }
    
    public Optional<Address> getAddress() {
        return Optional.ofNullable(address);
    }
}
```

**❌ 2. Ca parametru de metodă**

```java
// ❌ BAD - confusing, forces caller to wrap
public void setEmail(Optional<String> email) {
    email.ifPresent(this::updateEmail);
}

// Caller forced to wrap
service.setEmail(Optional.of("test@example.com"));  // Awkward!
service.setEmail(Optional.empty());  // Why not just null?

// ✅ GOOD - use overloading sau @Nullable
public void setEmail(String email) {
    if (email != null) {
        this.email = email;
    }
}

public void clearEmail() {
    this.email = null;
}

// Sau cu annotation
public void setEmail(@Nullable String email) {
    this.email = email;
}
```

**❌ 3. În colecții**

```java
// ❌ BAD - adds complexity
List<Optional<User>> users = new ArrayList<>();
Map<String, Optional<User>> userMap = new HashMap<>();

// Problems:
// - Confusing: empty list vs list with empty Optionals?
// - What does null mean in this context?
// - Extra wrapping/unwrapping

users.add(Optional.of(user));
users.add(Optional.empty());
users.add(null);  // ??? What does this mean?

// ✅ GOOD - empty collection or missing key
List<User> users = new ArrayList<>();  // Empty if no users
Map<String, User> userMap = new HashMap<>();  // Key absent if no user

// If you need to distinguish "not present" vs "present but null" (rare):
Map<String, User> map = new HashMap<>();
map.put("key1", user);  // Present
map.put("key2", null);  // Present but null
// "key3" - not in map (absent)

boolean hasKey = map.containsKey("key1");
```

**❌ 4. Pentru primitive types**

```java
// ❌ BAD - boxing overhead
Optional<Integer> count = Optional.of(42);
Optional<Long> id = Optional.of(100L);
Optional<Double> price = Optional.of(99.99);

// ✅ GOOD - use specialized OptionalInt, OptionalLong, OptionalDouble
OptionalInt count = OptionalInt.of(42);
OptionalLong id = OptionalLong.of(100L);
OptionalDouble price = OptionalDouble.of(99.99);

// API slightly different
OptionalInt opt = OptionalInt.of(42);
opt.isPresent();
opt.getAsInt();  // Not get()
opt.orElse(0);
opt.ifPresent(System.out::println);
```

**❌ 5. Când vrei să returnezi colecție**

```java
// ❌ BAD - confusing
public Optional<List<User>> getUsers() {
    List<User> users = userRepository.findAll();
    return Optional.ofNullable(users);
}

// Confusing: Optional.empty() vs Optional.of(emptyList())?

// ✅ GOOD - return empty collection
public List<User> getUsers() {
    List<User> users = userRepository.findAll();
    return users != null ? users : Collections.emptyList();
}

// Or simply (most repositories don't return null anyway)
public List<User> getUsers() {
    return userRepository.findAll();  // Never null
}
```

**❌ 6. În constructor parameters**

```java
// ❌ BAD
public class UserService {
    private final UserRepository repository;
    
    public UserService(Optional<UserRepository> repository) {
        this.repository = repository.orElseThrow();  // Why Optional?
    }
}

// ✅ GOOD - fail fast if null
public class UserService {
    private final UserRepository repository;
    
    public UserService(UserRepository repository) {
        this.repository = Objects.requireNonNull(repository, "repository cannot be null");
    }
}
```

---

**Best Practices:**

**✅ 1. Return Optional from methods that might not have result**

```java
// Repository
public interface UserRepository {
    Optional<User> findById(Long id);
    Optional<User> findByEmail(String email);
    List<User> findAll();  // Never Optional<List>!
}

// Service
public Optional<String> getUserEmail(Long userId) {
    return userRepository.findById(userId)
        .map(User::getEmail);
}
```

**✅ 2. Chain operations instead of nested ifs**

```java
// ❌ BAD - defeats purpose
Optional<User> optUser = findUser(id);
if (optUser.isPresent()) {
    User user = optUser.get();
    Optional<Address> optAddr = user.getAddress();
    if (optAddr.isPresent()) {
        Address addr = optAddr.get();
        // ...
    }
}

// ✅ GOOD - functional chaining
String city = findUser(id)
    .flatMap(User::getAddress)
    .flatMap(Address::getCity)
    .map(String::toUpperCase)
    .orElse("UNKNOWN");
```

**✅ 3. Use orElseThrow for required values**

```java
// Make it explicit that value MUST exist
User user = userRepository.findById(id)
    .orElseThrow(() -> new UserNotFoundException(id));

// Controller example
@GetMapping("/users/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    return userRepository.findById(id)
        .map(ResponseEntity::ok)
        .orElse(ResponseEntity.notFound().build());
}
```

**✅ 4. Combine with Streams**

```java
// Get all valid emails
List<String> emails = users.stream()
    .map(User::getEmail)
    .flatMap(Optional::stream)  // Java 9+
    .collect(Collectors.toList());

// Or Java 8
List<String> emails = users.stream()
    .map(User::getEmail)
    .filter(Optional::isPresent)
    .map(Optional::get)
    .collect(Collectors.toList());
```

---

**Exemple practice:**

**1. Service Layer**

```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    public User getUserOrThrow(Long id) {
        return userRepository.findById(id)
            .orElseThrow(() -> new UserNotFoundException("User " + id + " not found"));
    }
    
    public Optional<String> getUserEmail(Long id) {
        return userRepository.findById(id)
            .map(User::getEmail);
    }
    
    public String getDisplayName(Long id) {
        return userRepository.findById(id)
            .filter(User::hasCompletedProfile)
            .map(user -> user.getFirstName() + " " + user.getLastName())
            .orElse("Anonymous User");
    }
}
```

**2. Controller**

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        return userService.findById(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }
    
    @GetMapping("/{id}/email")
    public ResponseEntity<String> getUserEmail(@PathVariable Long id) {
        return userService.findById(id)
            .flatMap(User::getEmail)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }
}
```

**3. Complex business logic**

```java
public BigDecimal calculateDiscount(Long userId, Long productId) {
    return userRepository.findById(userId)
        .filter(User::isPremiumMember)
        .flatMap(user -> productRepository.findById(productId))
        .filter(product -> product.isEligibleForDiscount())
        .map(product -> product.getPrice().multiply(new BigDecimal("0.20")))
        .orElse(BigDecimal.ZERO);
}
```

**4. Fallback chain**

```java
public Config getConfiguration(String key) {
    return findInCache(key)
        .or(() -> findInDatabase(key))
        .or(() -> findInConfigFile(key))
        .or(() -> getDefaultConfig(key))
        .orElseThrow(() -> new ConfigNotFoundException(key));
}
```

---

**Concluzie Optional:**

**Folosește Optional pentru:**
- Return values care pot lipsi logic
- Indicare clară în API că valoarea e opțională
- Chaining funcțional de transformări

**NU folosi Optional pentru:**
- Fields în clasă
- Method parameters
- Collections
- Primitive types (folosește OptionalInt/Long/Double)
- Performance-critical code (overhead-ul wrapper-ului)

**Regula de aur:** Optional este pentru **return types**, nu pentru **internal state** sau **method parameters**.

---

### Thread vs ExecutorService

#### Care e diferența între Thread și ExecutorService?

**Răspuns:**

**Thread** = unitate de bază de execuție concurentă (low-level)  
**ExecutorService** = framework de nivel înalt pentru management thread pool-uri (high-level)

---

**Direct Thread Usage - Low Level:**

```java
// ❌ Manual thread management
Thread thread = new Thread(() -> {
    System.out.println("Task running in: " + Thread.currentThread().getName());
    // Business logic
});

thread.start();  // Start execution
thread.join();   // Wait for completion

// Probleme:
// 1. Overhead: fiecare task = un thread nou (1MB stack + OS resources)
// 2. No reuse: thread moare după task
// 3. Manual lifecycle: trebuie să gestionezi start/join/interrupt
// 4. No task queue: dacă ai 1000 tasks = 1000 threads (resource exhaustion)
// 5. No return value: Runnable nu returnează nimic
// 6. Exception handling dificil: UncaughtExceptionHandler
```

**ExecutorService - High Level:**

```java
// ✅ Managed thread pool
ExecutorService executor = Executors.newFixedThreadPool(10);

// Submit task
Future<String> future = executor.submit(() -> {
    Thread.sleep(1000);
    return "Task completed";
});

// Get result
String result = future.get();  // Blocks until done

// Shutdown
executor.shutdown();
executor.awaitTermination(60, TimeUnit.SECONDS);
```

---

**Diferențe fundamentale:**

| Aspect | Thread | ExecutorService |
|--------|--------|-----------------|
| **Abstraction** | Low-level | High-level |
| **Creation** | Manual `new Thread()` | Thread pool reuse |
| **Lifecycle** | Manual start/join/interrupt | Managed submit/shutdown |
| **Resource** | 1 thread = 1 task | N threads handle M tasks |
| **Queue** | Nu există | Built-in task queue |
| **Return value** | Nu (Runnable) | Da (Callable + Future) |
| **Exception** | UncaughtExceptionHandler | Future.get() throws |
| **Cancellation** | thread.interrupt() | future.cancel() |
| **Completion** | thread.join() | future.get() / awaitTermination |
| **Scalability** | Poor (thread per task) | Good (thread pool) |
| **Cost** | High (OS thread) | Low (reuse) |

---

**Thread Lifecycle:**

```
NEW → RUNNABLE → RUNNING → TERMINATED
        ↓           ↓
      BLOCKED    WAITING
                 TIMED_WAITING

Thread States:
- NEW: Created but not started
- RUNNABLE: Ready to run or running
- BLOCKED: Waiting for monitor lock
- WAITING: Waiting indefinitely (wait(), join())
- TIMED_WAITING: Waiting with timeout (sleep(), wait(timeout))
- TERMINATED: Execution completed
```

```java
Thread thread = new Thread(() -> {
    System.out.println("Running");
});

System.out.println(thread.getState());  // NEW
thread.start();
System.out.println(thread.getState());  // RUNNABLE
thread.join();
System.out.println(thread.getState());  // TERMINATED
```

---

**ExecutorService - Thread Pool Types:**

**1. Fixed Thread Pool**

```java
ExecutorService executor = Executors.newFixedThreadPool(10);

// Caracteristici:
// - 10 threads create la început
// - Task queue nelimitată (LinkedBlockingQueue)
// - Dacă toate thread-urile sunt busy, task-urile așteaptă în queue
// - Thread-urile nu mor (reused)

// Use case:
// - Known workload
// - Bounded concurrency
// - Long-running service
```

**2. Cached Thread Pool**

```java
ExecutorService executor = Executors.newCachedThreadPool();

// Caracteristici:
// - 0 threads inițial
// - Creează thread-uri on-demand
// - Reuse idle threads (60s timeout)
// - Poate crește nelimitat (risc OutOfMemoryError!)

// Use case:
// - Many short-lived async tasks
// - Variable workload
// - NOT for long-running tasks
```

**3. Single Thread Executor**

```java
ExecutorService executor = Executors.newSingleThreadExecutor();

// Caracteristici:
// - Exact 1 thread
// - Task-urile execută secvențial (FIFO)
// - Garantează ordering
// - Dacă thread-ul moare, se creează altul

// Use case:
// - Sequential processing with async
// - Event loop
// - Order matters
```

**4. Scheduled Thread Pool**

```java
ScheduledExecutorService scheduled = Executors.newScheduledThreadPool(5);

// Schedule cu delay
scheduled.schedule(() -> {
    System.out.println("Executed after 10 seconds");
}, 10, TimeUnit.SECONDS);

// Schedule periodic (fixed rate)
scheduled.scheduleAtFixedRate(() -> {
    System.out.println("Every 5 seconds (from start)");
}, 0, 5, TimeUnit.SECONDS);

// Schedule periodic (fixed delay)
scheduled.scheduleWithFixedDelay(() -> {
    System.out.println("5 seconds after previous completion");
}, 0, 5, TimeUnit.SECONDS);

// Use case:
// - Cron-like tasks
// - Polling
// - Periodic cleanup
```

**5. Work Stealing Pool (Java 8+)**

```java
ExecutorService executor = Executors.newWorkStealingPool();

// Caracteristici:
// - ForkJoinPool underneath
// - Parallelism = CPU cores
// - Work stealing: idle threads steal work from busy threads
// - Best for CPU-intensive tasks

// Use case:
// - Parallel computations
// - Divide and conquer algorithms
// - CPU-bound tasks
```

---

**Runnable vs Callable:**

**Runnable:**
```java
// No return value, no checked exception
Runnable runnable = () -> {
    System.out.println("Running");
    // void, can't return anything
    // Can't throw checked exceptions
};

executor.execute(runnable);  // Fire and forget
```

**Callable:**
```java
// Returns value, can throw exception
Callable<String> callable = () -> {
    Thread.sleep(1000);
    if (someCondition) {
        throw new Exception("Error occurred");
    }
    return "Result";
};

Future<String> future = executor.submit(callable);
try {
    String result = future.get();  // Blocks until done
    System.out.println(result);
} catch (ExecutionException e) {
    // Exception from callable
    Throwable cause = e.getCause();
    cause.printStackTrace();
} catch (InterruptedException e) {
    // Thread interrupted while waiting
    Thread.currentThread().interrupt();
}
```

---

**Future API:**

```java
Future<String> future = executor.submit(() -> {
    Thread.sleep(5000);
    return "Done";
});

// Check if done (non-blocking)
boolean done = future.isDone();

// Check if cancelled
boolean cancelled = future.isCancelled();

// Cancel task
boolean cancelled = future.cancel(true);  // mayInterruptIfRunning
// true: interrupt thread
// false: don't interrupt, just prevent from starting

// Get result (blocking)
String result = future.get();

// Get with timeout
try {
    String result = future.get(2, TimeUnit.SECONDS);
} catch (TimeoutException e) {
    System.out.println("Timeout! Task still running");
    future.cancel(true);
}
```

---

**CompletableFuture - Async Programming (Java 8+):**

Much better than Future - non-blocking, composable.

```java
// 1. Supply async
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    // Runs in ForkJoinPool.commonPool() by default
    return "Hello";
}, executor);  // Or provide custom executor

// 2. Run async (no return)
CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
    System.out.println("Task");
}, executor);

// 3. Chaining - thenApply
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "hello")
    .thenApply(String::toUpperCase)  // Transform result
    .thenApply(s -> s + " WORLD");

future.get();  // "HELLO WORLD"

// 4. Chaining - thenAccept (consume, no return)
CompletableFuture.supplyAsync(() -> "hello")
    .thenAccept(result -> System.out.println(result));

// 5. Chaining - thenRun (no input, no output)
CompletableFuture.supplyAsync(() -> "hello")
    .thenRun(() -> System.out.println("Done"));

// 6. Exception handling
CompletableFuture.supplyAsync(() -> {
    if (true) throw new RuntimeException("Error");
    return "Result";
})
.exceptionally(ex -> {
    System.err.println("Error: " + ex.getMessage());
    return "Default";
})
.thenAccept(System.out::println);  // Prints: Default

// 7. Combining futures
CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> "Hello");
CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> "World");

CompletableFuture<String> combined = future1.thenCombine(future2, 
    (s1, s2) -> s1 + " " + s2);

combined.get();  // "Hello World"

// 8. Compose - chaining dependent async ops
CompletableFuture<User> userFuture = CompletableFuture.supplyAsync(() -> findUser(id));

CompletableFuture<Order> orderFuture = userFuture.thenCompose(user ->
    CompletableFuture.supplyAsync(() -> findOrder(user.getOrderId()))
);

// 9. All of - wait for all
CompletableFuture<String> f1 = CompletableFuture.supplyAsync(() -> "A");
CompletableFuture<String> f2 = CompletableFuture.supplyAsync(() -> "B");
CompletableFuture<String> f3 = CompletableFuture.supplyAsync(() -> "C");

CompletableFuture<Void> allOf = CompletableFuture.allOf(f1, f2, f3);
allOf.join();  // Waits for all

// 10. Any of - first to complete
CompletableFuture<Object> anyOf = CompletableFuture.anyOf(f1, f2, f3);
Object first = anyOf.get();  // Returns first completed
```

---

**Proper Shutdown:**

```java
ExecutorService executor = Executors.newFixedThreadPool(10);

try {
    // Submit tasks
    executor.submit(() -> doWork());
    
} finally {
    // Graceful shutdown
    executor.shutdown();  // No new tasks accepted
    
    try {
        // Wait for termination
        if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
            // Timeout - force shutdown
            List<Runnable> notExecuted = executor.shutdownNow();
            System.out.println("Tasks not executed: " + notExecuted.size());
            
            // Wait again
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                System.err.println("Executor did not terminate");
            }
        }
    } catch (InterruptedException e) {
        // Interrupted while waiting
        executor.shutdownNow();
        Thread.currentThread().interrupt();
    }
}
```

---

**Thread Safety - Shared State:**

**❌ Problem: Race Condition**

```java
class Counter {
    private int count = 0;
    
    public void increment() {
        count++;  // NOT atomic! Read-Modify-Write
        // 1. Read count (0)
        // 2. Add 1 (1)
        // 3. Write count (1)
        // If 2 threads do this simultaneously → lost update!
    }
    
    public int getCount() {
        return count;
    }
}

// Test
Counter counter = new Counter();
ExecutorService executor = Executors.newFixedThreadPool(10);

for (int i = 0; i < 1000; i++) {
    executor.submit(counter::increment);
}

executor.shutdown();
executor.awaitTermination(10, TimeUnit.SECONDS);

System.out.println(counter.getCount());  // Expected: 1000, Actual: 987? 992?
```

**✅ Solution 1: Synchronized**

```java
class Counter {
    private int count = 0;
    
    public synchronized void increment() {
        count++;
    }
    
    public synchronized int getCount() {
        return count;
    }
}

// Now thread-safe, but:
// - Contention if many threads
// - Only one thread at a time
```

**✅ Solution 2: ReentrantLock**

```java
class Counter {
    private int count = 0;
    private final Lock lock = new ReentrantLock();
    
    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();  // Always unlock in finally!
        }
    }
    
    public int getCount() {
        lock.lock();
        try {
            return count;
        } finally {
            lock.unlock();
        }
    }
}

// More flexible than synchronized:
// - tryLock() with timeout
// - lockInterruptibly()
// - Multiple condition variables
```

**✅ Solution 3: AtomicInteger (Best for counters)**

```java
class Counter {
    private final AtomicInteger count = new AtomicInteger(0);
    
    public void increment() {
        count.incrementAndGet();  // Atomic operation
    }
    
    public int getCount() {
        return count.get();
    }
}

// Lock-free, high performance
// Uses CAS (Compare-And-Swap) CPU instruction
```

**✅ Solution 4: LongAdder (Best for high contention)**

```java
class Counter {
    private final LongAdder count = new LongAdder();
    
    public void increment() {
        count.increment();  // Very fast, even with contention
    }
    
    public long getCount() {
        return count.sum();
    }
}

// Better than AtomicLong for high contention
// Internally maintains multiple counters, sums on read
```

---

**Controlling Thread Count per Resource:**

**Semaphore - limit concurrent access:**

```java
// Max 3 threads can access resource simultaneously
Semaphore semaphore = new Semaphore(3);

ExecutorService executor = Executors.newFixedThreadPool(10);

for (int i = 0; i < 20; i++) {
    executor.submit(() -> {
        try {
            semaphore.acquire();  // Wait if 3 threads already using
            try {
                // Access shared resource
                System.out.println("Accessing resource: " + 
                    Thread.currentThread().getName());
                Thread.sleep(1000);
            } finally {
                semaphore.release();  // Release permit
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    });
}

// Use case:
// - Database connection pool
// - Rate limiting
// - Resource quotas
```

**Custom ThreadPoolExecutor - fine-grained control:**

```java
ThreadPoolExecutor executor = new ThreadPoolExecutor(
    5,              // corePoolSize - min threads
    10,             // maximumPoolSize - max threads
    60,             // keepAliveTime
    TimeUnit.SECONDS,
    new LinkedBlockingQueue<>(100),  // Task queue (max 100)
    new ThreadFactory() {
        private final AtomicInteger counter = new AtomicInteger(0);
        
        @Override
        public Thread newThread(Runnable r) {
            Thread t = new Thread(r, "Worker-" + counter.incrementAndGet());
            t.setDaemon(false);
            t.setPriority(Thread.NORM_PRIORITY);
            return t;
        }
    },
    new ThreadPoolExecutor.CallerRunsPolicy()  // Rejection policy
);

// Rejection policies:
// - AbortPolicy: Throw RejectedExecutionException (default)
// - CallerRunsPolicy: Caller thread executes
// - DiscardPolicy: Silently discard
// - DiscardOldestPolicy: Discard oldest queued task

// Monitoring
int activeCount = executor.getActiveCount();
long completedTasks = executor.getCompletedTaskCount();
int poolSize = executor.getPoolSize();
int queueSize = executor.getQueue().size();
```

---

**Best Practices:**

**✅ 1. Always shutdown executors**

```java
// Use try-with-resources (Java 19+)
try (ExecutorService executor = Executors.newFixedThreadPool(10)) {
    executor.submit(() -> doWork());
}  // Auto shutdown

// Or manual
executor.shutdown();
executor.awaitTermination(60, TimeUnit.SECONDS);
```

**✅ 2. Handle InterruptedException properly**

```java
Future<String> future = executor.submit(() -> {
    try {
        Thread.sleep(10000);
        return "Done";
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();  // Restore interrupted status
        throw e;  // Or handle gracefully
    }
});
```

**✅ 3. Name your threads**

```java
ThreadFactory namedFactory = new ThreadFactoryBuilder()
    .setNameFormat("worker-%d")
    .build();

ExecutorService executor = Executors.newFixedThreadPool(10, namedFactory);

// Easier debugging, thread dumps
```

**✅ 4. Set appropriate pool sizes**

```java
// CPU-intensive: cores + 1
int cpuBound = Runtime.getRuntime().availableProcessors() + 1;

// I/O-intensive: cores * 2 (or more)
int ioBound = Runtime.getRuntime().availableProcessors() * 2;

// Formula: threads = cores * (1 + wait_time / compute_time)
```

**✅ 5. Use CompletableFuture for async chains**

```java
// Much cleaner than nested callbacks
CompletableFuture.supplyAsync(() -> fetchUser(id))
    .thenApplyAsync(user -> fetchOrders(user.getId()))
    .thenApplyAsync(orders -> calculateTotal(orders))
    .thenAccept(total -> System.out.println("Total: " + total))
    .exceptionally(ex -> {
        log.error("Error", ex);
        return null;
    });
```

---

**Concluzie Thread vs ExecutorService:**

| Use Case | Recommendation |
|----------|----------------|
| Single task | Thread.ofVirtual() (Java 21) or Thread |
| Multiple tasks | ExecutorService |
| Fixed workload | newFixedThreadPool |
| Variable short tasks | newCachedThreadPool |
| Sequential processing | newSingleThreadExecutor |
| Scheduled tasks | newScheduledThreadPool |
| CPU-intensive parallel | newWorkStealingPool |
| Async pipelines | CompletableFuture |
| High concurrency I/O | Virtual Threads (Java 21) |

**Modern recommendation:** Use ExecutorService (especially CompletableFuture) instead of raw Threads, unless you have very specific needs. For Java 21+, Virtual Threads are game-changing for high-concurrency I/O.


### ObjectMapper

#### Ce este ObjectMapper în Java și cum îl folosești?

**Răspuns:**

**ObjectMapper** este clasa principală din biblioteca **Jackson** pentru **serializare/deserializare JSON** ↔ Java Objects. Este de facto standard-ul pentru JSON în ecosistemul Java.

**Dependență Maven:**
```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.16.0</version>
</dependency>
```

---

**Basic Usage:**

**Serializare (Java → JSON):**
```java
ObjectMapper mapper = new ObjectMapper();

User user = new User("John", "john@example.com", 30);

// 1. To String
String json = mapper.writeValueAsString(user);
// {"name":"John","email":"john@example.com","age":30}

// 2. To File
mapper.writeValue(new File("user.json"), user);

// 3. To OutputStream
mapper.writeValue(outputStream, user);

// 4. To byte array
byte[] jsonBytes = mapper.writeValueAsBytes(user);
```

**Deserializare (JSON → Java):**
```java
// 1. From String
String json = "{\"name\":\"John\",\"email\":\"john@example.com\",\"age\":30}";
User user = mapper.readValue(json, User.class);

// 2. From File
User user = mapper.readValue(new File("user.json"), User.class);

// 3. From URL
User user = mapper.readValue(new URL("https://api.example.com/user"), User.class);

// 4. From InputStream
User user = mapper.readValue(inputStream, User.class);

// 5. From byte array
User user = mapper.readValue(jsonBytes, User.class);
```

---

**Pretty Print:**

```java
User user = new User("John", "john@example.com", 30);

// Compact (default)
String json = mapper.writeValueAsString(user);
// {"name":"John","email":"john@example.com","age":30}

// Pretty print
String prettyJson = mapper
    .writerWithDefaultPrettyPrinter()
    .writeValueAsString(user);
/*
{
  "name" : "John",
  "email" : "john@example.com",
  "age" : 30
}
*/
```

---

**Configuration:**

```java
ObjectMapper mapper = new ObjectMapper();

// 1. Ignore unknown properties (don't fail on extra fields in JSON)
mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);

// 2. Don't include null values in JSON
mapper.setSerializationInclusion(JsonInclude.Include.NON_NULL);

// 3. Don't include empty values
mapper.setSerializationInclusion(JsonInclude.Include.NON_EMPTY);

// 4. Pretty print by default
mapper.enable(SerializationFeature.INDENT_OUTPUT);

// 5. Fail on null for primitive types
mapper.configure(DeserializationFeature.FAIL_ON_NULL_FOR_PRIMITIVES, true);

// 6. Accept single value as array
mapper.configure(DeserializationFeature.ACCEPT_SINGLE_VALUE_AS_ARRAY, true);

// 7. Allow comments in JSON (non-standard)
mapper.configure(JsonParser.Feature.ALLOW_COMMENTS, true);

// 8. Order properties alphabetically
mapper.configure(MapperFeature.SORT_PROPERTIES_ALPHABETICALLY, true);
```

**Java 8 Date/Time Support:**

```java
// Register JavaTimeModule for LocalDate, LocalDateTime, etc.
ObjectMapper mapper = new ObjectMapper();
mapper.registerModule(new JavaTimeModule());

// Don't write dates as timestamps
mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);

// Now works with Java 8 date/time
class Event {
    private LocalDateTime timestamp;
    private LocalDate date;
    // ...
}

Event event = new Event(LocalDateTime.now(), LocalDate.now());
String json = mapper.writeValueAsString(event);
// {"timestamp":"2025-01-10T15:30:45.123","date":"2025-01-10"}
```

**Naming Strategy:**

```java
// Snake case (user_name instead of userName)
mapper.setPropertyNamingStrategy(PropertyNamingStrategies.SNAKE_CASE);

class User {
    private String firstName;  // → "first_name" in JSON
    private String lastName;   // → "last_name" in JSON
}

// Kebab case (user-name)
mapper.setPropertyNamingStrategy(PropertyNamingStrategies.KEBAB_CASE);

// Lower camel case (default)
mapper.setPropertyNamingStrategy(PropertyNamingStrategies.LOWER_CAMEL_CASE);

// Upper camel case (PascalCase)
mapper.setPropertyNamingStrategy(PropertyNamingStrategies.UPPER_CAMEL_CASE);
```

---

**Annotations:**

**@JsonProperty - custom field name:**
```java
public class User {
    @JsonProperty("full_name")
    private String name;  // JSON: "full_name", Java: "name"
    
    @JsonProperty(access = JsonProperty.Access.WRITE_ONLY)
    private String password;  // Only deserialize, never serialize
    
    @JsonProperty(access = JsonProperty.Access.READ_ONLY)
    private Long id;  // Only serialize, never deserialize
}
```

**@JsonIgnore - exclude field:**
```java
public class User {
    private String name;
    
    @JsonIgnore
    private String password;  // Never in JSON
    
    @JsonIgnore
    public String getPassword() {
        return password;
    }
}
```

**@JsonIgnoreProperties - class level:**
```java
@JsonIgnoreProperties({"password", "internalId"})
public class User {
    private String name;
    private String password;  // Ignored
    private String internalId;  // Ignored
}

// Ignore unknown properties (per class)
@JsonIgnoreProperties(ignoreUnknown = true)
public class User {
    // Extra fields in JSON won't fail deserialization
}
```

**@JsonFormat - date/number formatting:**
```java
public class Event {
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    private LocalDateTime timestamp;
    
    @JsonFormat(pattern = "yyyy-MM-dd")
    private LocalDate date;
    
    @JsonFormat(shape = JsonFormat.Shape.STRING)
    private BigDecimal amount;  // "123.45" instead of 123.45
}
```

**@JsonInclude - control inclusion:**
```java
public class User {
    private String name;
    
    @JsonInclude(JsonInclude.Include.NON_NULL)
    private String middleName;  // Only if not null
    
    @JsonInclude(JsonInclude.Include.NON_EMPTY)
    private List<String> tags;  // Only if not empty
}
```

**@JsonAlias - multiple names for same field:**
```java
public class User {
    @JsonAlias({"email", "e-mail", "emailAddress", "mail"})
    private String email;  // Accepts any of these names in JSON
}
```

**@JsonCreator + @JsonProperty - constructor deserialization:**
```java
public class User {
    private final String name;
    private final String email;
    
    @JsonCreator
    public User(
        @JsonProperty("name") String name,
        @JsonProperty("email") String email
    ) {
        this.name = name;
        this.email = email;
    }
}
```

**@JsonValue - serialize as single value:**
```java
public enum Status {
    ACTIVE("A"),
    INACTIVE("I");
    
    private String code;
    
    Status(String code) {
        this.code = code;
    }
    
    @JsonValue  // Serialize using code
    public String getCode() {
        return code;
    }
}

// JSON: "A" instead of "ACTIVE"
```

**@JsonSerialize / @JsonDeserialize - custom serializer:**
```java
public class MoneySerializer extends JsonSerializer<BigDecimal> {
    @Override
    public void serialize(BigDecimal value, JsonGenerator gen, SerializerProvider provider) 
        throws IOException {
        gen.writeString("$" + value.toString());
    }
}

public class Product {
    @JsonSerialize(using = MoneySerializer.class)
    private BigDecimal price;  // JSON: "$99.99"
}
```

---

**Collections:**

**List:**
```java
List<User> users = Arrays.asList(user1, user2, user3);

// Serialize
String json = mapper.writeValueAsString(users);

// Deserialize - need TypeReference
List<User> parsedUsers = mapper.readValue(json, 
    new TypeReference<List<User>>() {});

// Or using JavaType
JavaType type = mapper.getTypeFactory()
    .constructCollectionType(List.class, User.class);
List<User> parsedUsers = mapper.readValue(json, type);
```

**Map:**
```java
Map<String, User> userMap = new HashMap<>();
userMap.put("john", new User("John", "john@example.com"));

// Serialize
String json = mapper.writeValueAsString(userMap);

// Deserialize
Map<String, User> parsed = mapper.readValue(json, 
    new TypeReference<Map<String, User>>() {});
```

**Generic Types:**
```java
public class Response<T> {
    private T data;
    private String message;
    // getters/setters
}

// Deserialize generic type
String json = "{\"data\":{\"name\":\"John\"},\"message\":\"Success\"}";

JavaType type = mapper.getTypeFactory()
    .constructParametricType(Response.class, User.class);
Response<User> response = mapper.readValue(json, type);
```

---

**JSON to Map (dynamic):**

```java
// When you don't have a class
String json = "{\"name\":\"John\",\"age\":30,\"active\":true}";

// Parse to Map
Map<String, Object> map = mapper.readValue(json, Map.class);

String name = (String) map.get("name");
Integer age = (Integer) map.get("age");
Boolean active = (Boolean) map.get("active");

// Or with TypeReference for type safety
Map<String, Object> map = mapper.readValue(json, 
    new TypeReference<Map<String, Object>>() {});
```

**Object to Map:**

```java
User user = new User("John", "john@example.com", 30);

// Convert object to Map
Map<String, Object> map = mapper.convertValue(user, Map.class);
// {name=John, email=john@example.com, age=30}

// Or with TypeReference
Map<String, Object> map = mapper.convertValue(user, 
    new TypeReference<Map<String, Object>>() {});
```

**Map to Object:**

```java
Map<String, Object> map = new HashMap<>();
map.put("name", "John");
map.put("email", "john@example.com");
map.put("age", 30);

User user = mapper.convertValue(map, User.class);
```

---

**JsonNode - Tree Model:**

```java
String json = "{\"name\":\"John\",\"age\":30,\"address\":{\"city\":\"NYC\"}}";

// Parse to tree
JsonNode root = mapper.readTree(json);

// Navigate tree
String name = root.get("name").asText();  // "John"
int age = root.get("age").asInt();  // 30
String city = root.at("/address/city").asText();  // "NYC" (JsonPointer)

// Check existence
if (root.has("email")) {
    String email = root.get("email").asText();
}

// Iterate array
JsonNode array = mapper.readTree("[1,2,3]");
for (JsonNode node : array) {
    System.out.println(node.asInt());
}

// Modify tree
ObjectNode obj = mapper.createObjectNode();
obj.put("name", "John");
obj.put("age", 30);
obj.putObject("address").put("city", "NYC");

String json = mapper.writeValueAsString(obj);
```

---

**Error Handling:**

```java
try {
    User user = mapper.readValue(json, User.class);
} catch (JsonProcessingException e) {
    // Parsing error or mapping error
    System.err.println("Invalid JSON: " + e.getMessage());
    
    // Get location
    JsonLocation location = e.getLocation();
    System.err.println("Error at line " + location.getLineNr() + 
                       ", column " + location.getColumnNr());
}
```

---

**În Spring Boot:**

**Auto-configuration:**

Spring Boot auto-configures ObjectMapper cu:
- JavaTimeModule registered
- Sensible defaults
- Can be customized via properties

```properties
# application.properties
spring.jackson.serialization.indent-output=true
spring.jackson.serialization.write-dates-as-timestamps=false
spring.jackson.deserialization.fail-on-unknown-properties=false
spring.jackson.default-property-inclusion=non_null
spring.jackson.property-naming-strategy=SNAKE_CASE
```

**Inject ObjectMapper:**

```java
@RestController
public class UserController {
    
    @Autowired
    private ObjectMapper objectMapper;  // Spring-configured instance
    
    @PostMapping("/users")
    public User createUser(@RequestBody String json) throws JsonProcessingException {
        User user = objectMapper.readValue(json, User.class);
        return userService.save(user);
    }
}
```

**Customize ObjectMapper:**

```java
@Configuration
public class JacksonConfig {
    
    @Bean
    public ObjectMapper objectMapper() {
        ObjectMapper mapper = new ObjectMapper();
        
        // Register modules
        mapper.registerModule(new JavaTimeModule());
        mapper.registerModule(new Jdk8Module());
        
        // Configuration
        mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
        mapper.setSerializationInclusion(JsonInclude.Include.NON_NULL);
        mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
        
        // Naming strategy
        mapper.setPropertyNamingStrategy(PropertyNamingStrategies.SNAKE_CASE);
        
        return mapper;
    }
}
```

**@RequestBody and @ResponseBody:**

```java
@RestController
public class UserController {
    
    // Spring uses ObjectMapper automatically
    
    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        // JSON → User (automatic deserialization)
        return userService.save(user);
        // User → JSON (automatic serialization)
    }
    
    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        return userService.findById(id)
            .map(ResponseEntity::ok)  // User → JSON
            .orElse(ResponseEntity.notFound().build());
    }
}
```

---

**Best Practices:**

**✅ 1. Reuse ObjectMapper (thread-safe)**

```java
// ❌ BAD - creates new instance every time (expensive)
public String toJson(Object obj) throws JsonProcessingException {
    return new ObjectMapper().writeValueAsString(obj);
}

// ✅ GOOD - reuse instance (thread-safe)
private static final ObjectMapper MAPPER = new ObjectMapper();

public String toJson(Object obj) throws JsonProcessingException {
    return MAPPER.writeValueAsString(obj);
}
```

**✅ 2. Handle exceptions properly**

```java
try {
    User user = mapper.readValue(json, User.class);
    return user;
} catch (JsonProcessingException e) {
    log.error("Failed to parse JSON", e);
    throw new BadRequestException("Invalid JSON format");
}
```

**✅ 3. Use @JsonInclude wisely**

```java
// Class level
@JsonInclude(JsonInclude.Include.NON_NULL)
public class User {
    // All null fields excluded
}

// Or globally
mapper.setSerializationInclusion(JsonInclude.Include.NON_NULL);
```

**✅ 4. Validate after deserialization**

```java
@PostMapping("/users")
public User createUser(@RequestBody @Valid User user) {
    // @Valid triggers Bean Validation
    return userService.save(user);
}

public class User {
    @NotBlank
    private String name;
    
    @Email
    private String email;
    
    @Min(18)
    private int age;
}
```

**✅ 5. Use TypeReference for generics**

```java
// ✅ Correct
List<User> users = mapper.readValue(json, new TypeReference<List<User>>() {});

// ❌ Wrong - returns List<LinkedHashMap>
List<User> users = mapper.readValue(json, List.class);
```

---

**Concluzie ObjectMapper:**

Jackson ObjectMapper este:
- **De facto standard** pentru JSON în Java
- **Thread-safe** după configurare
- **Performant** și **flexibil**
- **Bine integrat** cu Spring Boot
- **Extensibil** prin annotations și custom serializers

**Key points:**
- Reuse instance (thread-safe)
- Configure once, use everywhere
- Use annotations pentru control fin
- Handle exceptions
- Prefer TypeReference pentru generics


---

## Rezumat Rapid - Topice Suplimentare

### Interfaces & Default Methods

**Ce poate conține o interfață în Java 8+?**
- Abstract methods (implicit public abstract)
- Default methods (implementare în interfață)
- Static methods
- Private methods (Java 9+) - helper methods
- Constants (public static final)

```java
public interface MyInterface {
    void abstractMethod();  // Abstract
    
    default void defaultMethod() {  // Default
        System.out.println("Default implementation");
    }
    
    static void staticMethod() {  // Static
        System.out.println("Static utility");
    }
    
    private void helperMethod() {  // Private (Java 9+)
        System.out.println("Helper");
    }
}
```

---

## Spring Framework - Rezumat

### @Controller vs @RestController

- **@Controller:** MVC views (HTML), needs `@ResponseBody` pentru JSON
- **@RestController:** `@Controller` + `@ResponseBody`, returnează JSON direct

### Dependency Injection

**Proces:**
1. Component scan (@ComponentScan)
2. Bean definition creation
3. Dependency resolution
4. Bean instantiation (în ordinea dependențelor)
5. Dependency injection
6. Bean ready

**Tipuri:**
- Constructor injection (RECOMMENDED) - immutable
- Setter injection - optional dependencies
- Field injection (AVOID) - hard to test

### Bean Scopes

1. **singleton** (default) - 1 per container
2. **prototype** - nou la fiecare request
3. **request** - 1 per HTTP request
4. **session** - 1 per HTTP session
5. **application** - 1 per ServletContext
6. **websocket** - 1 per WebSocket session

### API Definition

**Metode HTTP:**
- **GET** - retrieve (idempotent, safe)
- **POST** - create (not idempotent)
- **PUT** - full update (idempotent)
- **PATCH** - partial update
- **DELETE** - delete (idempotent)

**PUT vs PATCH:**
- PUT: replace entire resource (toate câmpurile)
- PATCH: update specific fields (doar ce trimiti)

### Spring Batch

**Componente:**
- **Job** - container for steps
- **Step** - phase în job
- **ItemReader** - read data (CSV, DB, XML)
- **ItemProcessor** - transform data
- **ItemWriter** - write data (DB, file)

**Use cases:** CSV → DB, data migrations, ETL, report generation

---

## Microservices - Rezumat

### Microservices vs Monolith

**Avantaje Microservices:**
- Independent deployment
- Technology diversity
- Better scalability
- Fault isolation

**Dezavantaje:**
- Complexity
- Network latency
- Distributed transactions
- Debugging difficulty

### Communication

**Synchronous:** REST, gRPC
**Asynchronous:** RabbitMQ, Kafka

**RabbitMQ vs Kafka:**
- RabbitMQ: message broker, task queues, AMQP
- Kafka: event streaming, high throughput, log retention

### Monitoring

**Stack tipic:**
- Logs: ELK (Elasticsearch, Logstash, Kibana)
- Metrics: Prometheus + Grafana
- Traces: Jaeger, Zipkin
- APM: New Relic, Datadog

### Thread Control

**Semaphore** - limit concurrent access:
```java
Semaphore semaphore = new Semaphore(3);  // Max 3 threads
semaphore.acquire();  // Wait if full
// Use resource
semaphore.release();
```

---

## Databases - Rezumat

### Relational vs Non-Relational

| Feature | Relational | Non-Relational |
|---------|------------|----------------|
| Schema | Fixed | Flexible |
| Scaling | Vertical | Horizontal |
| Transactions | ACID | BASE |
| Relationships | Foreign keys, JOINs | Embedded, denormalized |

### PostgreSQL vs Couchbase

| Aspect | PostgreSQL | Couchbase |
|--------|------------|-----------|
| Type | SQL | NoSQL (Document) |
| Data | Tables | JSON documents |
| Query | SQL | N1QL |
| Joins | Efficient | Limited |
| Use Case | Complex queries | High throughput, caching |

---

## DevOps - Rezumat

### Docker

**Ce este:** Platform pentru containerizare

**Concepte:**
- **Image** - template
- **Container** - runtime instance
- **Dockerfile** - build instructions

```dockerfile
FROM openjdk:17
COPY target/app.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Gradle

**Build automation tool** (alternative to Maven)
- Groovy/Kotlin DSL
- Faster builds
- More flexible

### Java SE vs Java EE

- **Java SE:** Core Java (JDK, JRE, core libraries)
- **Java EE (Jakarta EE):** Enterprise (Servlets, JPA, EJB, JMS)
- **Spring Boot:** Modern alternative to Java EE

---

## System Design - URL Shortener

**Design:**

1. **Generate short code:**
```java
long id = counter.incrementAndGet();
String shortCode = Base62.encode(id);  // "aBc123"
```

2. **Store mapping:**
```sql
CREATE TABLE urls (
    short_code VARCHAR(10) PRIMARY KEY,
    long_url TEXT NOT NULL,
    created_at TIMESTAMP,
    clicks INT DEFAULT 0
);
```

3. **Redirect:**
```java
@GetMapping("/{shortCode}")
public RedirectView redirect(@PathVariable String shortCode) {
    String longUrl = urlRepository.findByShortCode(shortCode);
    return new RedirectView(longUrl);
}
```

**Considerations:**
- Collision handling
- Expiration
- Analytics
- Rate limiting
- Caching (Redis)

---

## Concluzie Generală

Acest document acoperă:
✅ Metodologii (Agile, Kanban, Observability)
✅ Java Evolution (11, 17, 21 - features majore)
✅ Java Core (Streams, Optional, Threads, ObjectMapper)
✅ Spring Framework (DI, Scopes, REST APIs, Batch)
✅ Microservices (arhitectură, comunicare, monitoring)
✅ Databases (SQL vs NoSQL, PostgreSQL vs Couchbase)
✅ DevOps (Docker, Gradle)
✅ System Design (URL Shortener)

**Resurse recomandate:**
- "Effective Java" - Joshua Bloch
- "Spring in Action" - Craig Walls
- "Designing Data-Intensive Applications" - Martin Kleppmann
- "Building Microservices" - Sam Newman
- Spring.io documentation
- Baeldung.com tutorials

**Pentru interviuri:**
- Înțelege CÂND și DE CE folosești fiecare pattern/tool
- Dă exemple concrete din experiență
- Discută trade-offs și alternative
- Arată că gândești la scale, performance, maintainability

**Mult succes la interviuri! 🚀**

