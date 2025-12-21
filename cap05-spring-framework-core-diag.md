# Capitolul 5 – Spring Framework Core
## Q341–Q420 — Nivel Senior

> 📚 Scop: Interviuri Senior / Lead / Staff
> 💾 Encoding: UTF-8

---

## 🎯 HARTA MENTALĂ

```mermaid
mindmap
  root((Spring Core))
    IoC Container
      BeanFactory
      ApplicationContext
    Dependency Injection
      Constructor
      Setter
      Field
    Bean Lifecycle
      Instantiation
      Initialization
      Destruction
    Scopes
      Singleton
      Prototype
    Configuration
      Annotations
      JavaConfig
      Profiles
    AOP
      Aspects
      Pointcuts
      Advice
```

---

# 📦 SECȚIUNEA 1: IoC & DEPENDENCY INJECTION

## Q341-342: Inversion of Control

```mermaid
graph TB
    subgraph "❌ Fără IoC"
        A1[ServiceA] -->|"new ServiceB()"| B1[ServiceB]
        B1 -->|"new Repository()"| C1[Repository]
    end
    
    subgraph "✅ Cu IoC - Spring"
        CONT[Spring Container]
        CONT -->|injectează| A2[ServiceA]
        CONT -->|injectează| B2[ServiceB]
        CONT -->|injectează| C2[Repository]
        A2 -.->|folosește| B2
        B2 -.->|folosește| C2
    end
```

**IoC** = Inversiunea Controlului. În loc ca obiectele să-și creeze singure dependențele, un container extern (Spring) le creează și le furnizează.

**Beneficii:**
- Decuplare între clase
- Testare ușoară (mock-uri)
- Configurare flexibilă
- Schimbare implementări fără modificare cod

---

## Q343-344: Dependency Injection - Cele 3 Tipuri

```mermaid
graph TB
    subgraph "✅ Constructor Injection - RECOMANDAT"
        CI[Service]
        CI --> CIF["private final Repo repo"]
        CI --> CIC["Service(Repo repo)"]
    end
    
    subgraph "⚠️ Setter Injection"
        SI[Service]
        SI --> SIF["private Repo repo"]
        SI --> SIM["@Autowired setRepo()"]
    end
    
    subgraph "❌ Field Injection - EVITĂ"
        FI[Service]
        FI --> FIF["@Autowired private Repo repo"]
    end
```

```java
// ✅ CONSTRUCTOR INJECTION - Best Practice
@Service
public class OrderService {
    
    private final OrderRepository repository;
    private final PaymentService paymentService;
    
    // @Autowired implicit din Spring 4.3+
    public OrderService(OrderRepository repository, 
                       PaymentService paymentService) {
        this.repository = repository;
        this.paymentService = paymentService;
    }
}

// Avantaje Constructor Injection:
// ✅ Imutabilitate (final)
// ✅ Dependențe explicite
// ✅ Nu poate exista obiect invalid
// ✅ Testare ușoară
// ✅ Detectează circular dependencies

// ❌ FIELD INJECTION - De evitat
@Service
public class BadService {
    @Autowired private Repository repo; // Ascuns, nu e final
}
```

---

## Q345-346: Spring Container

### BeanFactory vs ApplicationContext

```mermaid
graph TB
    subgraph "BeanFactory"
        BF[BeanFactory]
        BF --> BF1["Lazy init"]
        BF --> BF2["Basic DI"]
        BF --> BF3["Minimal"]
    end
    
    subgraph "ApplicationContext ✅"
        AC[ApplicationContext]
        AC --> AC1["Eager init"]
        AC --> AC2["Full DI + AOP"]
        AC --> AC3["Events"]
        AC --> AC4["i18n"]
        AC --> AC5["Web support"]
    end
    
    BF -.->|extends| AC
```

**ApplicationContext** este interfața preferată - include tot ce oferă BeanFactory plus funcționalități enterprise.

```java
// Creare context
ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);

// Obținere bean
UserService service = ctx.getBean(UserService.class);
```

---

## Q347-350: Stereotypes și Component Scanning

```mermaid
graph TB
    COMP["@Component<br/>Generic bean"]
    COMP --> SERV["@Service<br/>Business logic"]
    COMP --> REPO["@Repository<br/>Data access + exception translation"]
    COMP --> CTRL["@Controller<br/>Web MVC"]
    COMP --> REST["@RestController<br/>REST API"]
    COMP --> CONF["@Configuration<br/>Bean definitions"]
```

```java
@Repository
public class JpaUserRepository implements UserRepository {
    @PersistenceContext
    private EntityManager em;
    
    public Optional<User> findById(Long id) {
        return Optional.ofNullable(em.find(User.class, id));
    }
}

@Service
public class UserService {
    private final UserRepository repository;
    
    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}

@RestController
@RequestMapping("/api/users")
public class UserController {
    private final UserService userService;
    
    public UserController(UserService userService) {
        this.userService = userService;
    }
}
```

---

## Q352-353: @Qualifier și @Primary

```mermaid
graph TB
    INT[PaymentProcessor interface]
    INT --> IMPL1["@Primary<br/>CreditCardProcessor"]
    INT --> IMPL2["@Qualifier 'paypal'<br/>PayPalProcessor"]
    INT --> IMPL3["@Qualifier 'bank'<br/>BankProcessor"]
```

```java
@Service
@Primary  // Default când nu se specifică
public class CreditCardProcessor implements PaymentProcessor { }

@Service("paypal")
public class PayPalProcessor implements PaymentProcessor { }

@Service
public class CheckoutService {
    
    private final PaymentProcessor defaultProcessor;
    private final PaymentProcessor paypalProcessor;
    
    public CheckoutService(
            PaymentProcessor defaultProcessor,  // Primește @Primary
            @Qualifier("paypal") PaymentProcessor paypalProcessor) {
        this.defaultProcessor = defaultProcessor;
        this.paypalProcessor = paypalProcessor;
    }
}
```

---

# 📦 SECȚIUNEA 2: BEAN SCOPES & LIFECYCLE

## Q354-356: Bean Scopes

```mermaid
graph TB
    subgraph "Singleton DEFAULT"
        SING["O instanță per container"]
    end
    
    subgraph "Prototype"
        PROT["Instanță nouă la fiecare request"]
    end
    
    subgraph "Web Scopes"
        REQ["request - per HTTP request"]
        SESS["session - per HTTP session"]
    end
```

```java
@Component
@Scope("prototype")
public class PrototypeBean {
    private final UUID id = UUID.randomUUID();
}

// ⚠️ PROBLEMĂ: Prototype în Singleton
@Service
public class SingletonService {
    @Autowired
    private PrototypeBean proto; // ❌ Creat O SINGURĂ DATĂ!
}

// ✅ SOLUȚIE: ObjectProvider
@Service
public class SingletonService {
    @Autowired
    private ObjectProvider<PrototypeBean> protoProvider;
    
    public void doWork() {
        PrototypeBean fresh = protoProvider.getObject(); // Nou!
    }
}
```

---

## Q357-361: Bean Lifecycle

```mermaid
sequenceDiagram
    participant Container
    participant BPP as BeanPostProcessor
    participant Bean
    
    Container->>Bean: 1. Constructor
    Container->>Bean: 2. Dependency Injection
    Container->>Bean: 3. Aware interfaces
    Container->>BPP: 4. postProcessBefore
    Container->>Bean: 5. @PostConstruct
    Container->>Bean: 6. afterPropertiesSet()
    Container->>BPP: 7. postProcessAfter
    Note over Bean: BEAN READY
    Note over Container: ... app runs ...
    Container->>Bean: 8. @PreDestroy
    Container->>Bean: 9. destroy()
```

```java
@Component
public class MyBean implements InitializingBean, DisposableBean {
    
    @Autowired
    private SomeDependency dep;
    
    // 1. Constructor - dep e NULL aici!
    public MyBean() {
        System.out.println("1. Constructor");
    }
    
    // 5. @PostConstruct - RECOMANDAT pentru init
    @PostConstruct
    public void init() {
        System.out.println("5. @PostConstruct - dep available: " + dep);
    }
    
    // 6. InitializingBean
    @Override
    public void afterPropertiesSet() {
        System.out.println("6. afterPropertiesSet");
    }
    
    // 8. @PreDestroy - RECOMANDAT pentru cleanup
    @PreDestroy
    public void cleanup() {
        System.out.println("8. @PreDestroy");
    }
    
    // 9. DisposableBean
    @Override
    public void destroy() {
        System.out.println("9. destroy");
    }
}
```

---

# 📦 SECȚIUNEA 3: CONFIGURATION

## Q362-365: @Configuration și @Bean

```mermaid
graph TB
    subgraph "@Configuration"
        CONF[AppConfig]
        CONF --> B1["@Bean dataSource()"]
        CONF --> B2["@Bean jdbcTemplate()"]
        CONF --> B3["@Bean txManager()"]
    end
```

```java
@Configuration
public class DatabaseConfig {
    
    @Bean
    public DataSource dataSource() {
        HikariDataSource ds = new HikariDataSource();
        ds.setJdbcUrl("jdbc:postgresql://localhost/db");
        ds.setUsername("user");
        return ds;
    }
    
    @Bean
    public JdbcTemplate jdbcTemplate(DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }
}

// ⚠️ De ce @Configuration e PROXIAT?
@Configuration
public class ServiceConfig {
    
    @Bean
    public ServiceA serviceA() {
        return new ServiceA(serviceB()); // Apel intern
    }
    
    @Bean
    public ServiceB serviceB() {
        return new ServiceB();
    }
}
// Spring CGLIB proxy interceptează serviceB()
// și returnează ACELAȘI bean (singleton)
// Fără proxy: ar crea instanțe noi!
```

---

## Q381-386: Profiles și Properties

### Profiles

```mermaid
graph LR
    DEV["@Profile 'dev'"] --> H2[H2 Database]
    PROD["@Profile 'prod'"] --> PG[PostgreSQL]
```

```java
@Configuration
@Profile("dev")
public class DevConfig {
    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.H2)
            .build();
    }
}

@Configuration
@Profile("prod")
public class ProdConfig {
    @Bean
    public DataSource dataSource() {
        HikariDataSource ds = new HikariDataSource();
        ds.setJdbcUrl("jdbc:postgresql://prod-server/db");
        return ds;
    }
}

// Activare: spring.profiles.active=dev
```

### @ConfigurationProperties

```yaml
# application.yml
app:
  mail:
    host: smtp.gmail.com
    port: 587
```

```java
@ConfigurationProperties(prefix = "app.mail")
@Validated
public class MailProperties {
    @NotBlank
    private String host;
    
    @Min(1) @Max(65535)
    private int port = 587;
    
    // getters + setters
}

@Configuration
@EnableConfigurationProperties(MailProperties.class)
public class MailConfig {
    
    @Bean
    public JavaMailSender mailSender(MailProperties props) {
        JavaMailSenderImpl sender = new JavaMailSenderImpl();
        sender.setHost(props.getHost());
        sender.setPort(props.getPort());
        return sender;
    }
}
```

---

# 📦 SECȚIUNEA 4: AOP

## Q367-374: Aspect-Oriented Programming

### Concepte AOP

```mermaid
graph TB
    ASPECT["ASPECT - Clasa cu cross-cutting logic"]
    JP["JOIN POINT - Punct de execuție (method)"]
    PC["POINTCUT - Selectează join points"]
    ADV["ADVICE - Codul executat"]
```

### Advice Types

```mermaid
graph LR
    BEFORE["@Before"] --> TARGET[Method]
    TARGET --> AFR["@AfterReturning"]
    TARGET --> AFT["@AfterThrowing"]
    AFR --> AFTER["@After"]
    AFT --> AFTER
    AROUND["@Around"] -.->|"wrap"| TARGET
```

```java
@Aspect
@Component
@Slf4j
public class LoggingAspect {
    
    // Pointcut - toate metodele din service
    @Pointcut("execution(* com.example.service.*.*(..))")
    public void serviceLayer() {}
    
    // Before
    @Before("serviceLayer()")
    public void logBefore(JoinPoint jp) {
        log.info("Calling: {}", jp.getSignature().getName());
    }
    
    // Around - cel mai puternic
    @Around("serviceLayer()")
    public Object logAround(ProceedingJoinPoint pjp) throws Throwable {
        long start = System.currentTimeMillis();
        
        try {
            Object result = pjp.proceed(); // Execută metoda
            log.info("{} took {}ms", pjp.getSignature().getName(),
                System.currentTimeMillis() - start);
            return result;
        } catch (Exception e) {
            log.error("Exception in {}: {}", 
                pjp.getSignature().getName(), e.getMessage());
            throw e;
        }
    }
    
    // AfterReturning - acces la rezultat
    @AfterReturning(pointcut = "serviceLayer()", returning = "result")
    public void logResult(JoinPoint jp, Object result) {
        log.info("{} returned: {}", jp.getSignature().getName(), result);
    }
    
    // AfterThrowing - acces la excepție
    @AfterThrowing(pointcut = "serviceLayer()", throwing = "ex")
    public void logException(JoinPoint jp, Exception ex) {
        log.error("{} threw: {}", jp.getSignature().getName(), ex.getMessage());
    }
}
```

### Pointcut Expressions

```java
// Toate metodele din package
@Pointcut("execution(* com.example.service.*.*(..))")

// Metode cu anumită adnotare
@Pointcut("@annotation(Transactional)")

// Clase cu anumită adnotare
@Pointcut("@within(Service)")

// Bean după nume
@Pointcut("bean(*Service)")

// Combinații
@Pointcut("serviceLayer() && !@annotation(NoLog)")
```

### Spring AOP Proxies

```mermaid
graph TB
    subgraph "JDK Dynamic Proxy"
        JDK["Target implements Interface"]
        JDK --> JDKP["Proxy implements Interface"]
    end
    
    subgraph "CGLIB Proxy"
        CG["Target class (no interface)"]
        CG --> CGP["Proxy extends Target"]
    end
```

```java
// ⚠️ LIMITARE: Self-invocation NU trece prin proxy!
@Service
public class OrderService {
    
    @Transactional
    public void processOrder(Order order) {
        save(order);
        sendEmail(order); // ❌ this.sendEmail() - NO PROXY!
    }
    
    @Async
    public void sendEmail(Order order) {
        // Nu va fi async! E apel direct.
    }
}

// ✅ SOLUȚIE: Extrage în altă clasă
@Service
public class OrderService {
    private final EmailService emailService;
    
    @Transactional
    public void processOrder(Order order) {
        save(order);
        emailService.sendEmail(order); // ✅ Prin proxy!
    }
}
```

---

# 📦 SECȚIUNEA 5: ADVANCED

## Q375-377: Circular Dependencies

```mermaid
graph LR
    A[ServiceA] -->|needs| B[ServiceB]
    B -->|needs| A
    ERR["❌ BeanCurrentlyInCreationException"]
```

```java
// ❌ Circular dependency
@Service
public class ServiceA {
    public ServiceA(ServiceB b) { }
}

@Service
public class ServiceB {
    public ServiceB(ServiceA a) { }
}

// ✅ SOLUȚIE 1: Redesign (BEST)

// ✅ SOLUȚIE 2: @Lazy
@Service
public class ServiceA {
    public ServiceA(@Lazy ServiceB b) { }
}

// ✅ SOLUȚIE 3: ObjectProvider
@Service
public class ServiceA {
    private final ObjectProvider<ServiceB> bProvider;
    
    public ServiceA(ObjectProvider<ServiceB> bProvider) {
        this.bProvider = bProvider;
    }
}
```

## Q391-392: Application Events

```java
// Event
public class OrderCreatedEvent {
    private final Order order;
    public OrderCreatedEvent(Order order) { this.order = order; }
    public Order getOrder() { return order; }
}

// Publisher
@Service
public class OrderService {
    @Autowired
    private ApplicationEventPublisher publisher;
    
    public void createOrder(Order order) {
        orderRepo.save(order);
        publisher.publishEvent(new OrderCreatedEvent(order));
    }
}

// Listener
@Component
public class OrderListener {
    
    @EventListener
    public void onOrderCreated(OrderCreatedEvent event) {
        log.info("Order created: {}", event.getOrder().getId());
    }
    
    @Async
    @EventListener
    public void sendEmailAsync(OrderCreatedEvent event) {
        emailService.send(event.getOrder());
    }
    
    @TransactionalEventListener(phase = AFTER_COMMIT)
    public void afterCommit(OrderCreatedEvent event) {
        // După commit tranzacție
    }
}
```

## Q393-394: Conditional Beans

```java
@Bean
@ConditionalOnProperty(name = "cache.enabled", havingValue = "true")
public CacheManager cacheManager() {
    return new ConcurrentMapCacheManager();
}

@Bean
@ConditionalOnClass(DataSource.class)
public JdbcTemplate jdbcTemplate() { }

@Bean
@ConditionalOnMissingBean
public ObjectMapper objectMapper() {
    return new ObjectMapper(); // Default, poate fi suprascris
}
```

---

# 🎯 CHEAT SHEET SPRING CORE

## Injection Best Practices

| Tip | Când | Exemplu |
|-----|------|---------|
| **Constructor** ✅ | Întotdeauna (default) | `public Service(Repo repo)` |
| **Setter** | Dependențe opționale | `@Autowired(required=false)` |
| **Field** ❌ | Niciodată în producție | Doar pentru teste rapide |

## Bean Scopes

| Scope | Instanțe | Când |
|-------|----------|------|
| **singleton** | 1 per container | Default, stateless |
| **prototype** | N per request | Stateful, short-lived |
| **request** | 1 per HTTP request | Web, request data |
| **session** | 1 per HTTP session | Web, user data |

## AOP Quick Reference

| Advice | Execuție | Use Case |
|--------|----------|----------|
| `@Before` | Înainte | Logging, security |
| `@After` | După (always) | Cleanup |
| `@AfterReturning` | După succes | Audit result |
| `@AfterThrowing` | După excepție | Error logging |
| `@Around` | Wrap | Timing, transactions |

---

> 💡 **Regula de Aur Spring:**  
> *"Preferă Constructor Injection, evită Field Injection, și ține minte că self-invocation nu trece prin proxy!"*
