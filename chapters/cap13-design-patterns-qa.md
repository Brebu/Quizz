# Design Patterns – Întrebări & Răspunsuri

## 📋 Cuprins
- [Introducere](#introducere)
- [Creational Patterns](#creational-patterns)
  - [Singleton](#singleton)
  - [Factory Method](#factory-method)
  - [Abstract Factory](#abstract-factory)
  - [Builder](#builder)
  - [Prototype](#prototype)
- [Structural Patterns](#structural-patterns)
  - [Adapter](#adapter)
  - [Decorator](#decorator)
  - [Proxy](#proxy)
  - [Facade](#facade)
  - [Composite](#composite)
- [Behavioral Patterns](#behavioral-patterns)
  - [Strategy](#strategy)
  - [Observer](#observer)
  - [Command](#command)
  - [Template Method](#template-method)
  - [Chain of Responsibility](#chain-of-responsibility)
  - [State](#state)
  - [Iterator](#iterator)
- [Patterns în Spring Framework](#patterns-în-spring-framework)

---

## Introducere

### Ce sunt Design Patterns?

**Răspuns:**
Design patterns sunt soluții reutilizabile la probleme comune în design-ul software. Ele reprezintă best practices evoluate în timp, documentate de Gang of Four (GoF) în 1994.

**Nu sunt:**
- Cod gata făcut care poate fi copiat
- Soluții universale pentru orice problemă
- Frameworks sau librării

**Sunt:**
- Template-uri pentru rezolvarea problemelor
- Vocabular comun între dezvoltatori
- Soluții testate în timp pentru probleme recurente

---

### Care sunt categoriile principale de patterns?

**Răspuns:**

1. **Creational Patterns** (5)
   - Se ocupă cu crearea obiectelor
   - Abstractizează procesul de instanțiere
   - Exemple: Singleton, Factory, Builder

2. **Structural Patterns** (7)
   - Se ocupă cu compoziția claselor și obiectelor
   - Simplifică designul prin identificarea relațiilor
   - Exemple: Adapter, Decorator, Proxy

3. **Behavioral Patterns** (11)
   - Se ocupă cu interacțiunea și responsabilitățile între obiecte
   - Definesc comunicarea între obiecte
   - Exemple: Strategy, Observer, Command

---

### Când folosești un design pattern și când nu?

**Răspuns:**

**Folosește când:**
- Problema este recurentă și bine definită
- Pattern-ul reduce complexitatea
- Echipa înțelege pattern-ul
- Beneficiile depășesc costurile

**NU folosi când:**
- Problema e simplă și nu justifică overhead-ul
- Over-engineering - "pattern pentru că da"
- Echipa nu e familiarizată (introduce confuzie)
- Pattern-ul nu se potrivește contextului

**Regula de aur:** KISS (Keep It Simple, Stupid) > Pattern-uri complicate

---

## Creational Patterns

### Singleton

#### Ce este Singleton și când îl folosești?

**Răspuns:**

**Definiție:**
Asigură că o clasă are **o singură instanță** și oferă un **punct global de acces** la aceasta.

**Când îl folosești:**
- Logger-e, connection pools, cache-uri
- Configuration managers
- Resurse partajate (thread pools, database connections)
- Device drivers

**Implementare clasică (thread-safe):**
```java
public class DatabaseConnection {
    private static volatile DatabaseConnection instance;
    private Connection connection;
    
    private DatabaseConnection() {
        // constructor privat
        this.connection = createConnection();
    }
    
    public static DatabaseConnection getInstance() {
        if (instance == null) {
            synchronized (DatabaseConnection.class) {
                if (instance == null) {
                    instance = new DatabaseConnection();
                }
            }
        }
        return instance;
    }
}
```

---

#### Care sunt problemele cu Singleton?

**Răspuns:**

**1. Testabilitate redusă**
- Greu de mock-uit
- Creează dependențe ascunse
- State global = side effects

**2. Încalcă Single Responsibility Principle**
- Gestionează și crearea și logica de business

**3. Probleme în medii concurente**
- Necesită sincronizare → overhead
- Double-checked locking poate avea race conditions

**4. Probleme în medii distribuite**
- Nu garantează unicitate cross-JVM
- În Kubernetes/microservices - mai multe instanțe ale pod-ului

**Alternativa modernă: Dependency Injection**
```java
@Configuration
public class AppConfig {
    @Bean
    @Scope("singleton") // implicit în Spring
    public DatabaseConnection databaseConnection() {
        return new DatabaseConnection();
    }
}
```

---

#### Cum implementezi Singleton thread-safe în Java?

**Răspuns:**

**1. Eager initialization (simplă, thread-safe)**
```java
public class Singleton {
    private static final Singleton INSTANCE = new Singleton();
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```
✅ Thread-safe  
✅ Simplă  
❌ Nu e lazy (se creează la class loading)

**2. Lazy initialization cu synchronized (thread-safe dar lent)**
```java
public class Singleton {
    private static Singleton instance;
    
    private Singleton() {}
    
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
✅ Lazy loading  
✅ Thread-safe  
❌ Synchronized la fiecare apel (overhead)

**3. Double-checked locking (optimizat)**
```java
public class Singleton {
    private static volatile Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
✅ Lazy loading  
✅ Thread-safe  
✅ Performance bun  
⚠️ Necesită `volatile` (Java 5+)

**4. Bill Pugh Singleton (cea mai bună)**
```java
public class Singleton {
    private Singleton() {}
    
    private static class SingletonHelper {
        private static final Singleton INSTANCE = new Singleton();
    }
    
    public static Singleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```
✅ Lazy loading (inner class se încarcă la primul acces)  
✅ Thread-safe (class loading e thread-safe în JVM)  
✅ Fără sincronizare explicită  
✅ **Best practice**

**5. Enum Singleton (Joshua Bloch)**
```java
public enum Singleton {
    INSTANCE;
    
    public void doSomething() {
        // business logic
    }
}
```
✅ Thread-safe  
✅ Protecție împotriva serialization  
✅ Protecție împotriva reflection attacks  
✅ Cel mai sigur, dar mai puțin folosit

---

### Factory Method

#### Ce este Factory Method și cum diferă de simple factory?

**Răspuns:**

**Factory Method Pattern:**
Definește o **interfață pentru crearea obiectelor**, dar lasă **subclasele să decidă** ce clasă să instanțieze.

**Simple Factory (nu e GoF pattern):**
```java
// Simple Factory - o singură clasă factory
public class VehicleFactory {
    public static Vehicle createVehicle(String type) {
        switch (type) {
            case "car": return new Car();
            case "bike": return new Bike();
            default: throw new IllegalArgumentException();
        }
    }
}
```

**Factory Method (GoF pattern):**
```java
// Creator abstract
public abstract class VehicleFactory {
    // Factory method
    public abstract Vehicle createVehicle();
    
    // Template method care folosește factory method
    public void deliverVehicle() {
        Vehicle vehicle = createVehicle();
        vehicle.prepare();
        vehicle.deliver();
    }
}

// Concrete creators
public class CarFactory extends VehicleFactory {
    @Override
    public Vehicle createVehicle() {
        return new Car();
    }
}

public class BikeFactory extends VehicleFactory {
    @Override
    public Vehicle createVehicle() {
        return new Bike();
    }
}
```

**Diferențe cheie:**
- **Simple Factory:** O clasă, o metodă, switch/if
- **Factory Method:** Ierarhie de clase, polimorfism, extensibilitate

**Când folosești Factory Method:**
- Framework-uri unde nu știi exact ce obiecte vor fi create
- Când vrei să delegi crearea către subclase
- Când ai logică complexă de creație care variază

---

#### Unde vezi Factory Method în Spring?

**Răspuns:**

**1. FactoryBean interface**
```java
public interface FactoryBean<T> {
    T getObject() throws Exception;  // factory method
    Class<?> getObjectType();
    default boolean isSingleton() { return true; }
}
```

Exemplu:
```java
@Component
public class ToolFactory implements FactoryBean<Tool> {
    @Override
    public Tool getObject() throws Exception {
        // logică complexă de creare
        return new ToolImpl();
    }
    
    @Override
    public Class<?> getObjectType() {
        return Tool.class;
    }
}
```

**2. BeanFactory hierarchy**
- `BeanFactory` → `ApplicationContext`
- `getBean()` este un factory method
- Subclasele (`AnnotationConfigApplicationContext`, `XmlApplicationContext`) implementează crearea

**3. Repository interfaces în Spring Data**
```java
public interface UserRepository extends JpaRepository<User, Long> {
    // Spring Data generează implementarea (factory method)
    List<User> findByLastName(String lastName);
}
```

---

### Abstract Factory

#### Ce este Abstract Factory și când îl folosești?

**Răspuns:**

**Definiție:**
Furnizează o **interfață pentru crearea de familii de obiecte înrudite** fără a specifica clasele lor concrete.

**Diferența față de Factory Method:**
- **Factory Method:** Creează un singur tip de obiect
- **Abstract Factory:** Creează familii întregi de obiecte

**Exemplu: UI Components pentru diferite OS-uri**
```java
// Abstract factory
public interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
    Menu createMenu();
}

// Concrete factory pentru Windows
public class WindowsFactory implements GUIFactory {
    public Button createButton() { return new WindowsButton(); }
    public Checkbox createCheckbox() { return new WindowsCheckbox(); }
    public Menu createMenu() { return new WindowsMenu(); }
}

// Concrete factory pentru MacOS
public class MacOSFactory implements GUIFactory {
    public Button createButton() { return new MacOSButton(); }
    public Checkbox createCheckbox() { return new MacOSCheckbox(); }
    public Menu createMenu() { return new MacOSMenu(); }
}

// Client code
public class Application {
    private Button button;
    private Checkbox checkbox;
    
    public Application(GUIFactory factory) {
        button = factory.createButton();
        checkbox = factory.createCheckbox();
    }
}
```

**Când îl folosești:**
- Sistem trebuie să fie independent de cum sunt create produsele
- Ai familii de produse care trebuie folosite împreună
- Vrei să enforces-zi consistența între produse
- Exemple: UI toolkits, database drivers, cloud providers

---

### Builder

#### De ce ai nevoie de Builder Pattern?

**Răspuns:**

**Problema: Telescoping Constructor Anti-pattern**
```java
public class Pizza {
    private String size;
    private boolean cheese;
    private boolean pepperoni;
    private boolean bacon;
    
    public Pizza(String size) { ... }
    public Pizza(String size, boolean cheese) { ... }
    public Pizza(String size, boolean cheese, boolean pepperoni) { ... }
    public Pizza(String size, boolean cheese, boolean pepperoni, boolean bacon) { ... }
    // 💀 Nightmare la 10+ parametri
}
```

**Probleme:**
- Prea mulți constructori
- Ordinea parametrilor confuză
- Greu de citit: `new Pizza("large", true, false, true)`
- Nu poți avea parametri opționali cu același tip

**Soluția: Builder Pattern**
```java
public class Pizza {
    private final String size;          // required
    private final boolean cheese;       // optional
    private final boolean pepperoni;    // optional
    private final boolean bacon;        // optional
    
    private Pizza(Builder builder) {
        this.size = builder.size;
        this.cheese = builder.cheese;
        this.pepperoni = builder.pepperoni;
        this.bacon = builder.bacon;
    }
    
    public static class Builder {
        // Required parameters
        private final String size;
        
        // Optional parameters - cu valori default
        private boolean cheese = false;
        private boolean pepperoni = false;
        private boolean bacon = false;
        
        public Builder(String size) {
            this.size = size;
        }
        
        public Builder cheese(boolean value) {
            cheese = value;
            return this;
        }
        
        public Builder pepperoni(boolean value) {
            pepperoni = value;
            return this;
        }
        
        public Builder bacon(boolean value) {
            bacon = value;
            return this;
        }
        
        public Pizza build() {
            return new Pizza(this);
        }
    }
}

// Usage - fluent și lizibil
Pizza pizza = new Pizza.Builder("large")
    .cheese(true)
    .pepperoni(true)
    .build();
```

**Beneficii:**
✅ Cod lizibil  
✅ Imutabilitate (final fields)  
✅ Parametri opționali  
✅ Validare în `build()`  
✅ Method chaining

---

#### Unde vezi Builder în ecosistemul Java/Spring?

**Răspuns:**

**1. StringBuilder/StringBuffer**
```java
String result = new StringBuilder()
    .append("Hello")
    .append(" ")
    .append("World")
    .toString();
```

**2. Stream API**
```java
List<String> result = Stream.of("a", "b", "c")
    .filter(s -> s.length() > 0)
    .map(String::toUpperCase)
    .collect(Collectors.toList());
```

**3. Spring - RestTemplate / WebClient**
```java
RestTemplate restTemplate = new RestTemplateBuilder()
    .setConnectTimeout(Duration.ofSeconds(5))
    .setReadTimeout(Duration.ofSeconds(5))
    .build();

WebClient client = WebClient.builder()
    .baseUrl("https://api.example.com")
    .defaultHeader("Authorization", "Bearer token")
    .build();
```

**4. Spring Security**
```java
http
    .authorizeRequests()
        .antMatchers("/public/**").permitAll()
        .anyRequest().authenticated()
    .and()
    .formLogin()
        .loginPage("/login")
        .permitAll()
    .and()
    .logout()
        .permitAll();
```

**5. Lombok @Builder**
```java
@Builder
@Data
public class User {
    private String firstName;
    private String lastName;
    private String email;
    private int age;
}

// Usage
User user = User.builder()
    .firstName("John")
    .lastName("Doe")
    .email("john@example.com")
    .age(30)
    .build();
```

---

### Prototype

#### Ce este Prototype Pattern și când îl folosești?

**Răspuns:**

**Definiție:**
Creează obiecte noi prin **clonarea unui prototip existent** în loc de instanțiere clasică cu `new`.

**Când îl folosești:**
- Crearea obiectului e costisitoare (DB query, network call, computație)
- Vrei să eviți subclase multe pentru factory
- Runtime configuration - obiectul se configurează dinamic

**Implementare în Java:**
```java
public class Document implements Cloneable {
    private String title;
    private String content;
    private List<String> images;
    
    @Override
    public Document clone() {
        try {
            Document cloned = (Document) super.clone();
            // Deep copy pentru obiecte mutabile
            cloned.images = new ArrayList<>(this.images);
            return cloned;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}

// Usage
Document original = new Document("Report", "Content...");
Document copy = original.clone();
copy.setTitle("Report Copy");
```

**Shallow vs Deep Copy:**
```java
// ❌ Shallow copy - partajează referințele
Document shallow = (Document) super.clone();

// ✅ Deep copy - clone și nested objects
Document deep = (Document) super.clone();
deep.images = new ArrayList<>(this.images);
deep.metadata = metadata.clone();
```

**Alternativă modernă: Copy constructors**
```java
public class Document {
    private String title;
    private List<String> images;
    
    // Copy constructor
    public Document(Document other) {
        this.title = other.title;
        this.images = new ArrayList<>(other.images);
    }
}

Document copy = new Document(original);
```

**În Spring:**
```java
@Scope("prototype")  // Nu e Prototype pattern, dar similar
@Component
public class PrototypeBean {
    // O nouă instanță la fiecare getBean()
}
```

---

## Structural Patterns

### Adapter

#### Ce este Adapter Pattern și când îl folosești?

**Răspuns:**

**Definiție:**
Convertește interfața unei clase în altă interfață așteptată de clienți. Permite colaborarea între clase cu interfețe incompatibile.

**Analogie:** Adaptor de priză EU → US

**Când îl folosești:**
- Integrezi librării third-party cu interfețe diferite
- Legacy code cu interfețe vechi
- Wrapper peste API-uri externe

**Implementare:**
```java
// Target interface (ce vrea clientul)
public interface MediaPlayer {
    void play(String filename);
}

// Adaptee (clasa existentă incompatibilă)
public class VLCPlayer {
    public void playVLC(String filename) {
        System.out.println("Playing VLC: " + filename);
    }
}

// Adapter
public class VLCAdapter implements MediaPlayer {
    private VLCPlayer vlcPlayer;
    
    public VLCAdapter(VLCPlayer vlcPlayer) {
        this.vlcPlayer = vlcPlayer;
    }
    
    @Override
    public void play(String filename) {
        vlcPlayer.playVLC(filename);  // adaptare
    }
}

// Client
MediaPlayer player = new VLCAdapter(new VLCPlayer());
player.play("movie.vlc");
```

**În Spring:**
```java
// HandlerAdapter în Spring MVC
public interface HandlerAdapter {
    boolean supports(Object handler);
    ModelAndView handle(HttpServletRequest request, 
                       HttpServletResponse response, 
                       Object handler);
}

// Adaptează diferite tipuri de controller-e
// (@Controller, Controller interface, HttpRequestHandler)
```

---

### Decorator

#### Ce este Decorator Pattern și cum diferă de moștenire?

**Răspuns:**

**Definiție:**
Atașează **responsabilități noi unui obiect dinamic**, fără a modifica clasa. Alternativă flexibilă la subclassing.

**Moștenire vs Decorator:**
- **Moștenire:** Compile-time, statică, explozie de subclase
- **Decorator:** Runtime, dinamică, compunere

**Problema cu moștenirea:**
```java
// Ai BeverageWithMilk, BeverageWithSugar, BeverageWithMilkAndSugar...
// Cu 5 add-ons = 2^5 = 32 clase! 💀
```

**Soluția cu Decorator:**
```java
// Component
public interface Beverage {
    String getDescription();
    double getCost();
}

// Concrete component
public class Coffee implements Beverage {
    public String getDescription() { return "Coffee"; }
    public double getCost() { return 2.0; }
}

// Decorator base
public abstract class BeverageDecorator implements Beverage {
    protected Beverage beverage;
    
    public BeverageDecorator(Beverage beverage) {
        this.beverage = beverage;
    }
}

// Concrete decorators
public class Milk extends BeverageDecorator {
    public Milk(Beverage beverage) {
        super(beverage);
    }
    
    public String getDescription() {
        return beverage.getDescription() + ", Milk";
    }
    
    public double getCost() {
        return beverage.getCost() + 0.5;
    }
}

public class Sugar extends BeverageDecorator {
    public Sugar(Beverage beverage) {
        super(beverage);
    }
    
    public String getDescription() {
        return beverage.getDescription() + ", Sugar";
    }
    
    public double getCost() {
        return beverage.getCost() + 0.2;
    }
}

// Usage - compunere runtime
Beverage beverage = new Coffee();
beverage = new Milk(beverage);
beverage = new Sugar(beverage);
// "Coffee, Milk, Sugar" - $2.70
```

**În Java I/O:**
```java
InputStream input = new FileInputStream("file.txt");
input = new BufferedInputStream(input);      // decorator
input = new DataInputStream(input);          // decorator
```

**În Spring:**
```java
// BeanPostProcessor - decorează bean-urile
@Component
public class LoggingBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) {
        // Înfășoară bean-ul într-un proxy (decorator)
        return Proxy.newProxyInstance(...);
    }
}
```

---

### Proxy

#### Ce este Proxy Pattern și ce tipuri de proxy există?

**Răspuns:**

**Definiție:**
Oferă un **surogat sau placeholder** pentru alt obiect, controlând accesul la acesta.

**Tipuri de Proxy:**

**1. Virtual Proxy** - lazy loading
```java
public class ImageProxy implements Image {
    private RealImage realImage;
    private String filename;
    
    public ImageProxy(String filename) {
        this.filename = filename;
    }
    
    @Override
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename);  // lazy load
        }
        realImage.display();
    }
}
```

**2. Protection Proxy** - access control
```java
public class DocumentProxy implements Document {
    private RealDocument realDocument;
    private User currentUser;
    
    @Override
    public void edit() {
        if (!currentUser.hasPermission("EDIT")) {
            throw new AccessDeniedException();
        }
        realDocument.edit();
    }
}
```

**3. Remote Proxy** - reprezentare locală pentru obiect remote
```java
// RMI, REST clients - proxy local pentru server remote
UserService userService = restTemplate.getForObject(...);
```

**4. Caching Proxy**
```java
public class CachingProxy implements DataService {
    private RealDataService realService;
    private Map<String, Data> cache = new HashMap<>();
    
    @Override
    public Data getData(String id) {
        if (cache.containsKey(id)) {
            return cache.get(id);
        }
        Data data = realService.getData(id);
        cache.put(id, data);
        return data;
    }
}
```

**În Spring:**
```java
// AOP Proxies
@Transactional  // Spring creează proxy care adaugă tranzacție
public void saveUser(User user) {
    userRepository.save(user);
}

// @Cacheable - caching proxy
@Cacheable("users")
public User getUser(Long id) {
    return userRepository.findById(id);
}

// @Async - async proxy
@Async
public Future<String> asyncMethod() {
    return new AsyncResult<>("Result");
}
```

**Proxy vs Decorator:**
- **Proxy:** Controlează accesul (same interface)
- **Decorator:** Adaugă funcționalitate (extends interface)

---

### Facade

#### Ce este Facade Pattern și când îl folosești?

**Răspuns:**

**Definiție:**
Oferă o **interfață simplificată** la un subsistem complex.

**Analogie:** Remote control (facade) pentru home theater (subsistem complex: TV, DVD, Amplifier, Lights...)

**Când îl folosești:**
- Ai subsisteme complexe cu multe clase
- Vrei să reduci cuplarea între subsisteme
- Vrei să ascunzi complexitatea implementării

**Exemplu:**
```java
// Subsistem complex
class CPU {
    public void freeze() { }
    public void jump(long position) { }
    public void execute() { }
}

class Memory {
    public void load(long position, byte[] data) { }
}

class HardDrive {
    public byte[] read(long lba, int size) { }
}

// Facade
public class ComputerFacade {
    private CPU cpu;
    private Memory memory;
    private HardDrive hardDrive;
    
    public ComputerFacade() {
        this.cpu = new CPU();
        this.memory = new Memory();
        this.hardDrive = new HardDrive();
    }
    
    public void start() {
        cpu.freeze();
        memory.load(BOOT_ADDRESS, hardDrive.read(BOOT_SECTOR, SECTOR_SIZE));
        cpu.jump(BOOT_ADDRESS);
        cpu.execute();
    }
}

// Client - interfață simplă
ComputerFacade computer = new ComputerFacade();
computer.start();  // Ascunde toată complexitatea
```

**În Spring:**
```java
// RestTemplate - facade peste HTTP client libraries
RestTemplate restTemplate = new RestTemplate();
User user = restTemplate.getForObject(url, User.class);
// Ascunde HttpURLConnection, headers, parsing JSON, etc.

// JdbcTemplate - facade peste JDBC
JdbcTemplate jdbc = new JdbcTemplate(dataSource);
List<User> users = jdbc.query("SELECT * FROM users", userRowMapper);
// Ascunde Connection, Statement, ResultSet, exception handling
```

---

### Composite

#### Ce este Composite Pattern și când îl folosești?

**Răspuns:**

**Definiție:**
Compune obiecte în **structuri de tip arbore** pentru a reprezenta ierarhii parte-întreg. Permite clienților să trateze **uniform** obiecte individuale și compoziții.

**Când îl folosești:**
- Structuri ierarhice (arbori)
- File systems, UI components, org charts
- Când vrei să tratezi uniform leaf-uri și compozite

**Exemplu: File System**
```java
// Component
public interface FileSystemComponent {
    void print(String indent);
    int getSize();
}

// Leaf
public class File implements FileSystemComponent {
    private String name;
    private int size;
    
    public File(String name, int size) {
        this.name = name;
        this.size = size;
    }
    
    @Override
    public void print(String indent) {
        System.out.println(indent + "File: " + name + " (" + size + " KB)");
    }
    
    @Override
    public int getSize() {
        return size;
    }
}

// Composite
public class Directory implements FileSystemComponent {
    private String name;
    private List<FileSystemComponent> children = new ArrayList<>();
    
    public Directory(String name) {
        this.name = name;
    }
    
    public void add(FileSystemComponent component) {
        children.add(component);
    }
    
    public void remove(FileSystemComponent component) {
        children.remove(component);
    }
    
    @Override
    public void print(String indent) {
        System.out.println(indent + "Directory: " + name);
        for (FileSystemComponent child : children) {
            child.print(indent + "  ");
        }
    }
    
    @Override
    public int getSize() {
        return children.stream()
            .mapToInt(FileSystemComponent::getSize)
            .sum();
    }
}

// Usage
Directory root = new Directory("root");
Directory home = new Directory("home");
Directory user = new Directory("user");

File file1 = new File("document.txt", 100);
File file2 = new File("image.jpg", 500);

user.add(file1);
user.add(file2);
home.add(user);
root.add(home);

root.print("");
// Output:
// Directory: root
//   Directory: home
//     Directory: user
//       File: document.txt (100 KB)
//       File: image.jpg (500 KB)

System.out.println("Total size: " + root.getSize() + " KB");
```

**În UI Frameworks:**
```java
// Swing - Component hierarchy
JPanel panel = new JPanel();          // Composite
panel.add(new JButton("Click me"));   // Leaf
panel.add(new JLabel("Text"));        // Leaf

JPanel nested = new JPanel();         // Composite
nested.add(new JTextField());         // Leaf
panel.add(nested);                    // Composite în composite
```

---

## Behavioral Patterns

### Strategy

#### Ce este Strategy Pattern și când îl folosești?

**Răspuns:**

**Definiție:**
Definește o **familie de algoritmi**, îi **encapsulează** pe fiecare și îi face **interschimbabili**. Strategy permite algoritmului să varieze independent de clienții care îl folosesc.

**Problema:** Comportament care variază → if/switch statements
```java
// ❌ Anti-pattern
public class PaymentProcessor {
    public void processPayment(String type, double amount) {
        if (type.equals("credit_card")) {
            // credit card logic
        } else if (type.equals("paypal")) {
            // paypal logic
        } else if (type.equals("bitcoin")) {
            // bitcoin logic
        }
        // Nou tip = modificare clasă (Open-Closed violation)
    }
}
```

**Soluția cu Strategy:**
```java
// Strategy interface
public interface PaymentStrategy {
    void pay(double amount);
}

// Concrete strategies
public class CreditCardStrategy implements PaymentStrategy {
    private String cardNumber;
    
    public CreditCardStrategy(String cardNumber) {
        this.cardNumber = cardNumber;
    }
    
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " with credit card " + cardNumber);
    }
}

public class PayPalStrategy implements PaymentStrategy {
    private String email;
    
    public PayPalStrategy(String email) {
        this.email = email;
    }
    
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " via PayPal account " + email);
    }
}

// Context
public class ShoppingCart {
    private PaymentStrategy paymentStrategy;
    
    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.paymentStrategy = strategy;
    }
    
    public void checkout(double amount) {
        paymentStrategy.pay(amount);
    }
}

// Usage
ShoppingCart cart = new ShoppingCart();
cart.setPaymentStrategy(new CreditCardStrategy("1234-5678"));
cart.checkout(100.0);

cart.setPaymentStrategy(new PayPalStrategy("user@example.com"));
cart.checkout(50.0);
```

**În Spring:**
```java
// ApplicationContext selection strategy
ApplicationContext context;
if (useAnnotations) {
    context = new AnnotationConfigApplicationContext(Config.class);
} else {
    context = new ClassPathXmlApplicationContext("beans.xml");
}

// Transaction propagation strategies
@Transactional(propagation = Propagation.REQUIRED)
@Transactional(propagation = Propagation.REQUIRES_NEW)
```

**În Java:**
```java
// Comparator - strategy pentru sorting
Collections.sort(list, new Comparator<String>() {
    public int compare(String s1, String s2) {
        return s1.length() - s2.length();
    }
});

// Lambda - strategy inline
Collections.sort(list, (s1, s2) -> s1.length() - s2.length());
```

---

### Observer

#### Ce este Observer Pattern și când îl folosești?

**Răspuns:**

**Definiție:**
Definește o **dependență one-to-many** între obiecte, astfel încât când un obiect își schimbă starea, toți dependenții săi sunt **notificați și actualizați automat**.

**Analogie:** Newsletter subscription - când publisher-ul publică, toți subscribers sunt notificați

**Componente:**
- **Subject (Observable):** Obiectul observat
- **Observer:** Obiectul care observă
- **Notify:** Mecanismul de notificare

**Implementare:**
```java
// Observer interface
public interface Observer {
    void update(String message);
}

// Subject
public class NewsAgency {
    private List<Observer> observers = new ArrayList<>();
    private String news;
    
    public void addObserver(Observer observer) {
        observers.add(observer);
    }
    
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }
    
    public void setNews(String news) {
        this.news = news;
        notifyObservers();
    }
    
    private void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(news);
        }
    }
}

// Concrete Observer
public class NewsChannel implements Observer {
    private String name;
    private String news;
    
    public NewsChannel(String name) {
        this.name = name;
    }
    
    @Override
    public void update(String news) {
        this.news = news;
        System.out.println(name + " received news: " + news);
    }
}

// Usage
NewsAgency agency = new NewsAgency();
NewsChannel cnn = new NewsChannel("CNN");
NewsChannel bbc = new NewsChannel("BBC");

agency.addObserver(cnn);
agency.addObserver(bbc);

agency.setNews("Breaking: Design patterns are awesome!");
// Output:
// CNN received news: Breaking: Design patterns are awesome!
// BBC received news: Breaking: Design patterns are awesome!
```

**În Java:**
```java
// java.util.Observer (deprecated în Java 9)
// PropertyChangeListener
PropertyChangeSupport support = new PropertyChangeSupport(this);
support.addPropertyChangeListener(listener);
support.firePropertyChange("property", oldValue, newValue);
```

**În Spring:**
```java
// ApplicationEvent & ApplicationListener
@Component
public class UserCreatedEvent extends ApplicationEvent {
    private User user;
    
    public UserCreatedEvent(Object source, User user) {
        super(source);
        this.user = user;
    }
}

@Component
public class EmailNotificationListener implements ApplicationListener<UserCreatedEvent> {
    @Override
    public void onApplicationEvent(UserCreatedEvent event) {
        // Send email to new user
        sendWelcomeEmail(event.getUser());
    }
}

@Component
public class AnalyticsListener implements ApplicationListener<UserCreatedEvent> {
    @Override
    public void onApplicationEvent(UserCreatedEvent event) {
        // Track user creation in analytics
        trackUserCreation(event.getUser());
    }
}

// Publisher
@Service
public class UserService {
    @Autowired
    private ApplicationEventPublisher eventPublisher;
    
    public User createUser(User user) {
        User saved = userRepository.save(user);
        eventPublisher.publishEvent(new UserCreatedEvent(this, saved));
        return saved;
    }
}

// Sau cu @EventListener (mai modern)
@Component
public class NotificationService {
    @EventListener
    public void handleUserCreated(UserCreatedEvent event) {
        sendWelcomeEmail(event.getUser());
    }
    
    @Async
    @EventListener
    public void handleUserCreatedAsync(UserCreatedEvent event) {
        // Async processing
    }
}
```

**Push vs Pull Model:**
```java
// Push - subject trimite toate datele
void update(String news);  // Observer primește datele

// Pull - observer trage datele
void update(NewsAgency agency);  // Observer poate cere ce vrea
```

---

### Command

#### Ce este Command Pattern și când îl folosești?

**Răspuns:**

**Definiție:**
**Encapsulează o cerere ca obiect**, permițând parametrizarea clienților cu diferite cereri, queue-uirea sau logging-ul cererilor și suport pentru undo operations.

**Când îl folosești:**
- Undo/Redo operations
- Transaction systems
- Job queues, task scheduling
- Macro recording
- Request logging

**Componente:**
- **Command:** Interfață cu execute()
- **ConcreteCommand:** Implementare care invocă receiver
- **Receiver:** Obiectul care știe să execute cererea
- **Invoker:** Cere execuția comenzii
- **Client:** Creează comenzile

**Exemplu: Text Editor**
```java
// Command interface
public interface Command {
    void execute();
    void undo();
}

// Receiver
public class TextEditor {
    private StringBuilder text = new StringBuilder();
    
    public void write(String text) {
        this.text.append(text);
    }
    
    public void deleteLast(int length) {
        int start = text.length() - length;
        text.delete(start, text.length());
    }
    
    public String getText() {
        return text.toString();
    }
}

// Concrete Commands
public class WriteCommand implements Command {
    private TextEditor editor;
    private String textToWrite;
    
    public WriteCommand(TextEditor editor, String text) {
        this.editor = editor;
        this.textToWrite = text;
    }
    
    @Override
    public void execute() {
        editor.write(textToWrite);
    }
    
    @Override
    public void undo() {
        editor.deleteLast(textToWrite.length());
    }
}

// Invoker
public class TextEditorInvoker {
    private Stack<Command> commandHistory = new Stack<>();
    
    public void execute(Command command) {
        command.execute();
        commandHistory.push(command);
    }
    
    public void undo() {
        if (!commandHistory.isEmpty()) {
            Command command = commandHistory.pop();
            command.undo();
        }
    }
}

// Usage
TextEditor editor = new TextEditor();
TextEditorInvoker invoker = new TextEditorInvoker();

invoker.execute(new WriteCommand(editor, "Hello "));
invoker.execute(new WriteCommand(editor, "World"));
System.out.println(editor.getText());  // "Hello World"

invoker.undo();
System.out.println(editor.getText());  // "Hello "
```

**În Spring:**
```java
// JdbcTemplate - command objects
jdbcTemplate.execute("CREATE TABLE users (...)");  // Command encapsulat

// RestTemplate - HTTP commands
restTemplate.execute(url, HttpMethod.GET, requestCallback, responseExtractor);
```

**În frameworks:**
```java
// Runnable/Callable - command pattern
ExecutorService executor = Executors.newFixedThreadPool(10);
executor.execute(() -> {  // Command ca lambda
    System.out.println("Task executed");
});

// Swing ActionListener
button.addActionListener(e -> {  // Command
    performAction();
});
```

---

### Template Method

#### Ce este Template Method Pattern și când îl folosești?

**Răspuns:**

**Definiție:**
Definește **scheletul unui algoritm** într-o metodă, delegate-ind pași specifici către subclase. Subclasele pot **redefini pași** fără a schimba structura algoritmului.

**Când îl folosești:**
- Algoritm comun cu variații în pași specifici
- Framework design - lasă hook-uri pentru extensie
- Evită duplicarea codului în clase similare

**Exemplu:**
```java
// Abstract class cu template method
public abstract class DataProcessor {
    
    // Template method - final pentru a nu fi override-uit
    public final void process() {
        readData();
        processData();
        writeData();
    }
    
    // Pași abstracti - subclasele trebuie să implementeze
    protected abstract void readData();
    protected abstract void processData();
    
    // Pas cu implementare default (hook method)
    protected void writeData() {
        System.out.println("Writing data to default location");
    }
}

// Concrete implementations
public class CSVDataProcessor extends DataProcessor {
    @Override
    protected void readData() {
        System.out.println("Reading CSV file");
    }
    
    @Override
    protected void processData() {
        System.out.println("Processing CSV data");
    }
}

public class XMLDataProcessor extends DataProcessor {
    @Override
    protected void readData() {
        System.out.println("Reading XML file");
    }
    
    @Override
    protected void processData() {
        System.out.println("Processing XML data");
    }
    
    @Override
    protected void writeData() {
        System.out.println("Writing XML to custom location");
    }
}

// Usage
DataProcessor processor = new CSVDataProcessor();
processor.process();
// Output:
// Reading CSV file
// Processing CSV data
// Writing data to default location
```

**Hollywood Principle:** "Don't call us, we'll call you"
- Framework-ul apelează metodele tale, nu tu pe ale framework-ului

**În Spring:**
```java
// JdbcTemplate - template method pattern
public <T> T query(String sql, ResultSetExtractor<T> rse) {
    Connection con = getConnection();      // template controlled
    PreparedStatement ps = con.prepareStatement(sql);
    ResultSet rs = ps.executeQuery();
    T result = rse.extractData(rs);        // hook - you implement
    closeResources(con, ps, rs);           // template controlled
    return result;
}

// Usage
List<User> users = jdbcTemplate.query(
    "SELECT * FROM users",
    rs -> {  // Hook implementation
        List<User> list = new ArrayList<>();
        while (rs.next()) {
            list.add(mapUser(rs));
        }
        return list;
    }
);
```

**În servlet API:**
```java
public abstract class HttpServlet {
    // Template method
    public void service(HttpServletRequest req, HttpServletResponse res) {
        String method = req.getMethod();
        if (method.equals("GET")) {
            doGet(req, res);    // hook
        } else if (method.equals("POST")) {
            doPost(req, res);   // hook
        }
        // ...
    }
    
    protected void doGet(HttpServletRequest req, HttpServletResponse res) {
        // Default implementation
    }
}

// Your servlet
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse res) {
        // Your implementation
    }
}
```

---

### Chain of Responsibility

#### Ce este Chain of Responsibility și când îl folosești?

**Răspuns:**

**Definiție:**
Evită cuplarea sender-ului la receiver prin oferirea posibilității **mai multor obiecte să proceseze cererea**. Chain-uiește obiectele receiver și **pasează cererea de-a lungul lanțului** până când un obiect o procesează.

**Când îl folosești:**
- Processing pipeline (filters, validators)
- Event handling hierarchies
- Logging frameworks (different log levels)
- Request handling în web frameworks

**Exemplu: Support Ticket System**
```java
// Handler interface
public abstract class SupportHandler {
    protected SupportHandler nextHandler;
    
    public void setNextHandler(SupportHandler next) {
        this.nextHandler = next;
    }
    
    public abstract void handleRequest(SupportTicket ticket);
}

// Concrete handlers
public class Level1Support extends SupportHandler {
    @Override
    public void handleRequest(SupportTicket ticket) {
        if (ticket.getPriority() <= 1) {
            System.out.println("Level 1: Handling ticket " + ticket.getId());
        } else if (nextHandler != null) {
            nextHandler.handleRequest(ticket);
        }
    }
}

public class Level2Support extends SupportHandler {
    @Override
    public void handleRequest(SupportTicket ticket) {
        if (ticket.getPriority() <= 2) {
            System.out.println("Level 2: Handling ticket " + ticket.getId());
        } else if (nextHandler != null) {
            nextHandler.handleRequest(ticket);
        }
    }
}

public class Level3Support extends SupportHandler {
    @Override
    public void handleRequest(SupportTicket ticket) {
        System.out.println("Level 3: Handling critical ticket " + ticket.getId());
    }
}

// Usage
SupportHandler level1 = new Level1Support();
SupportHandler level2 = new Level2Support();
SupportHandler level3 = new Level3Support();

level1.setNextHandler(level2);
level2.setNextHandler(level3);

// Send requests
level1.handleRequest(new SupportTicket(1, 1));  // Handled by Level 1
level1.handleRequest(new SupportTicket(2, 2));  // Handled by Level 2
level1.handleRequest(new SupportTicket(3, 3));  // Handled by Level 3
```

**În Spring Security:**
```java
// FilterChain - chain of responsibility
public class SecurityFilterChain {
    private List<Filter> filters;
    
    public void doFilter(ServletRequest request, ServletResponse response) {
        // Each filter decides to process or pass to next
        // SecurityContextPersistenceFilter
        // → UsernamePasswordAuthenticationFilter
        // → BasicAuthenticationFilter
        // → FilterSecurityInterceptor
    }
}
```

**În Servlet API:**
```java
// Filter chain
public void doFilter(ServletRequest request, 
                     ServletResponse response,
                     FilterChain chain) {
    // Pre-processing
    System.out.println("Before filter");
    
    // Pass to next filter
    chain.doFilter(request, response);
    
    // Post-processing
    System.out.println("After filter");
}
```

**În logging:**
```java
// Logger hierarchy - chain of responsibility
Logger logger = LoggerFactory.getLogger(MyClass.class);
logger.debug("Debug message");  // Poate fi handled de parent logger
```

---

### State

#### Ce este State Pattern și când îl folosești?

**Răspuns:**

**Definiție:**
Permite unui obiect să își **schimbe comportamentul când starea internă se schimbă**. Obiectul pare să își schimbe clasa.

**Problema: State management cu if/switch**
```java
// ❌ Anti-pattern
public class TCPConnection {
    private String state = "CLOSED";
    
    public void open() {
        if (state.equals("CLOSED")) {
            // logic for opening
            state = "ESTABLISHED";
        } else if (state.equals("LISTENING")) {
            // different logic
        }
        // Complex, error-prone
    }
}
```

**Soluția cu State Pattern:**
```java
// State interface
public interface TCPState {
    void open(TCPConnection connection);
    void close(TCPConnection connection);
    void acknowledge(TCPConnection connection);
}

// Concrete states
public class ClosedState implements TCPState {
    @Override
    public void open(TCPConnection connection) {
        System.out.println("Opening connection...");
        connection.setState(new EstablishedState());
    }
    
    @Override
    public void close(TCPConnection connection) {
        System.out.println("Connection already closed");
    }
    
    @Override
    public void acknowledge(TCPConnection connection) {
        System.out.println("Cannot acknowledge, connection is closed");
    }
}

public class EstablishedState implements TCPState {
    @Override
    public void open(TCPConnection connection) {
        System.out.println("Connection already established");
    }
    
    @Override
    public void close(TCPConnection connection) {
        System.out.println("Closing connection...");
        connection.setState(new ClosedState());
    }
    
    @Override
    public void acknowledge(TCPConnection connection) {
        System.out.println("Data acknowledged");
    }
}

// Context
public class TCPConnection {
    private TCPState state;
    
    public TCPConnection() {
        this.state = new ClosedState();
    }
    
    public void setState(TCPState state) {
        this.state = state;
    }
    
    public void open() {
        state.open(this);
    }
    
    public void close() {
        state.close(this);
    }
    
    public void acknowledge() {
        state.acknowledge(this);
    }
}

// Usage
TCPConnection connection = new TCPConnection();
connection.open();        // Opening connection...
connection.acknowledge(); // Data acknowledged
connection.close();       // Closing connection...
connection.acknowledge(); // Cannot acknowledge, connection is closed
```

**Când îl folosești:**
- Comportament variază în funcție de stare
- Mulți if/switch pe baza stării
- State machine implementation
- Workflow engines

**În Spring State Machine:**
```java
@Configuration
@EnableStateMachine
public class StateMachineConfig extends StateMachineConfigurerAdapter<States, Events> {
    @Override
    public void configure(StateMachineStateConfigurer<States, Events> states) {
        states
            .withStates()
            .initial(States.DRAFT)
            .state(States.REVIEW)
            .state(States.PUBLISHED);
    }
    
    @Override
    public void configure(StateMachineTransitionConfigurer<States, Events> transitions) {
        transitions
            .withExternal()
            .source(States.DRAFT).target(States.REVIEW).event(Events.SUBMIT)
            .and()
            .withExternal()
            .source(States.REVIEW).target(States.PUBLISHED).event(Events.APPROVE);
    }
}
```

---

### Iterator

#### Ce este Iterator Pattern și cum funcționează în Java?

**Răspuns:**

**Definiție:**
Oferă o modalitate de **acces secvențial la elementele** unei colecții **fără a expune reprezentarea internă**.

**Beneficii:**
- Uniform access la diferite colecții
- Multiple iterații simultane
- Decuplare: colecția != iterația

**În Java Collections:**
```java
// Iterator interface
public interface Iterator<E> {
    boolean hasNext();
    E next();
    default void remove() {
        throw new UnsupportedOperationException();
    }
}

// Usage
List<String> list = Arrays.asList("A", "B", "C");

// 1. Manual iterator
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    System.out.println(it.next());
}

// 2. Enhanced for (uses iterator internally)
for (String item : list) {
    System.out.println(item);
}

// 3. forEach (Java 8)
list.forEach(System.out::println);

// 4. Stream API
list.stream().forEach(System.out::println);
```

**Custom Iterator:**
```java
public class BookCollection implements Iterable<Book> {
    private List<Book> books = new ArrayList<>();
    
    public void addBook(Book book) {
        books.add(book);
    }
    
    @Override
    public Iterator<Book> iterator() {
        return books.iterator();  // Delegate la ArrayList iterator
    }
    
    // Sau custom iterator
    public Iterator<Book> reverseIterator() {
        return new Iterator<Book>() {
            private int index = books.size() - 1;
            
            @Override
            public boolean hasNext() {
                return index >= 0;
            }
            
            @Override
            public Book next() {
                return books.get(index--);
            }
        };
    }
}

// Usage
BookCollection library = new BookCollection();
library.addBook(new Book("Book 1"));
library.addBook(new Book("Book 2"));

// Forward iteration
for (Book book : library) {
    System.out.println(book);
}

// Reverse iteration
Iterator<Book> reverse = library.reverseIterator();
while (reverse.hasNext()) {
    System.out.println(reverse.next());
}
```

**Fail-fast vs Fail-safe:**
```java
// Fail-fast (ArrayList, HashMap)
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
for (String item : list) {
    list.remove(item);  // ConcurrentModificationException
}

// Correct way
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    it.next();
    it.remove();  // ✅ Correct
}

// Fail-safe (CopyOnWriteArrayList, ConcurrentHashMap)
List<String> safe = new CopyOnWriteArrayList<>(Arrays.asList("A", "B", "C"));
for (String item : safe) {
    safe.remove(item);  // ✅ No exception, works on snapshot
}
```

---

## Patterns în Spring Framework

### Ce design patterns folosește Spring Framework?

**Răspuns:**

Spring este construit pe o mulțime de design patterns. Iată cele mai importante:

**1. Dependency Injection (IoC Container)**
- **Pattern:** Dependency Injection
- **Loc:** Core-ul Spring
```java
@Service
public class UserService {
    private final UserRepository repository;
    
    // Constructor injection - best practice
    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

**2. Singleton**
- **Scop default:** `@Bean`, `@Component`
```java
@Bean
// @Scope("singleton") - implicit
public DataSource dataSource() {
    return new HikariDataSource();
}
```

**3. Prototype**
- **Scop:** `@Scope("prototype")`
```java
@Component
@Scope("prototype")
public class ShoppingCart {
    // Nouă instanță la fiecare request
}
```

**4. Factory Method**
- **FactoryBean** interface
```java
public class MyBeanFactory implements FactoryBean<MyBean> {
    @Override
    public MyBean getObject() throws Exception {
        return new MyBean();  // Factory method
    }
}
```

**5. Proxy**
- **AOP, @Transactional, @Cacheable, @Async**
```java
@Service
public class UserService {
    @Transactional  // Spring creează proxy cu tranzacție
    public void saveUser(User user) {
        userRepository.save(user);
    }
}
```
**Cum funcționează:**
```
Client → Proxy (tranzacție) → Target (UserService)
         ↓
    beginTransaction()
    try {
        target.saveUser(user)
        commit()
    } catch {
        rollback()
    }
```

**6. Template Method**
- **JdbcTemplate, RestTemplate, JmsTemplate**
```java
public <T> T query(String sql, ResultSetExtractor<T> rse) {
    // Template controlează workflow-ul
    Connection con = getConnection();
    PreparedStatement ps = prepareStatement(sql);
    ResultSet rs = ps.executeQuery();
    T result = rse.extractData(rs);  // Hook - tu implementezi
    closeResources();
    return result;
}
```

**7. Strategy**
- **Transaction propagation, Caching strategies**
```java
@Transactional(propagation = Propagation.REQUIRED)
@Transactional(propagation = Propagation.REQUIRES_NEW)
@Transactional(propagation = Propagation.NESTED)
```

**8. Observer**
- **ApplicationEvent & @EventListener**
```java
@Component
public class UserService {
    @Autowired
    private ApplicationEventPublisher publisher;
    
    public User createUser(User user) {
        User saved = repository.save(user);
        publisher.publishEvent(new UserCreatedEvent(saved));
        return saved;
    }
}

@Component
public class EmailService {
    @EventListener
    public void onUserCreated(UserCreatedEvent event) {
        sendWelcomeEmail(event.getUser());
    }
}
```

**9. Adapter**
- **HandlerAdapter** în Spring MVC
```java
// Adaptează diferite tipuri de handlers
public interface HandlerAdapter {
    boolean supports(Object handler);
    ModelAndView handle(HttpServletRequest request, 
                       HttpServletResponse response,
                       Object handler);
}

// SimpleControllerHandlerAdapter, HttpRequestHandlerAdapter, etc.
```

**10. Decorator**
- **BeanPostProcessor**
```java
@Component
public class LoggingBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) {
        // Wrap bean în logging proxy (decorator)
        return createLoggingProxy(bean);
    }
}
```

**11. Front Controller**
- **DispatcherServlet**
```java
// Single entry point pentru toate request-urile
Client → DispatcherServlet → HandlerMapping → Controller
                           ↓
                      ViewResolver → View
```

**12. Chain of Responsibility**
- **Filter chain, Interceptor chain**
```java
// Spring Security FilterChain
SecurityContextPersistenceFilter
  → UsernamePasswordAuthenticationFilter
    → BasicAuthenticationFilter
      → FilterSecurityInterceptor

// Spring MVC Interceptors
@Component
public class LoggingInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, 
                            HttpServletResponse response,
                            Object handler) {
        // Process și decide dacă continuă chain-ul
        return true;
    }
}
```

**13. Builder**
- **RestTemplate, WebClient, SecurityFilterChain**
```java
RestTemplate restTemplate = new RestTemplateBuilder()
    .setConnectTimeout(Duration.ofSeconds(5))
    .setReadTimeout(Duration.ofSeconds(5))
    .build();

WebClient client = WebClient.builder()
    .baseUrl("https://api.example.com")
    .defaultHeader("Authorization", "Bearer token")
    .build();
```

**14. Facade**
- **Simplified interfaces** peste subsisteme complexe
```java
// RestTemplate - facade peste HTTP complexities
User user = restTemplate.getForObject(url, User.class);

// JdbcTemplate - facade peste JDBC
List<User> users = jdbcTemplate.query(sql, rowMapper);
```

---

### Cum combină Spring mai multe patterns?

**Răspuns:**

**Exemplu: @Transactional**

Combină:
1. **Proxy** - pentru transaction management
2. **Template Method** - transaction workflow
3. **Strategy** - propagation strategies
4. **Decorator** - adaugă behavior

```java
@Service
public class UserService {
    
    @Transactional(propagation = Propagation.REQUIRED)  // Strategy
    public void saveUser(User user) {
        userRepository.save(user);
    }
}

// Ce se întâmplă în spate:
// 1. Spring creează PROXY (Proxy pattern)
// 2. Proxy folosește TransactionTemplate (Template Method)
// 3. TransactionTemplate aplică Propagation strategy
// 4. Proxy DECOREAZĂ metoda cu transaction behavior
```

**Exemplu: Spring MVC Request Processing**

```
Request → DispatcherServlet (Front Controller)
            ↓
         Filter Chain (Chain of Responsibility)
            ↓
         HandlerMapping (Strategy - diferite mapping strategies)
            ↓
         HandlerAdapter (Adapter - adaptează la controller)
            ↓
         Controller (actual handler)
            ↓
         ViewResolver (Strategy - diferite view resolvers)
            ↓
         View (Template - view rendering)
```

---

## Best Practices

### Când să folosești și când să NU folosești patterns?

**Răspuns:**

**✅ FOLOSEȘTE patterns când:**

1. **Problema e recurentă și bine definită**
```java
// ✅ Strategy pentru algoritmi interschimbabili
PaymentStrategy strategy = getPaymentStrategy(type);
strategy.pay(amount);
```

2. **Pattern-ul reduce complexitatea**
```java
// ❌ Fără Decorator - explozie de clase
BeverageWithMilk, BeverageWithSugar, BeverageWithMilkAndSugar...

// ✅ Cu Decorator
Beverage beverage = new Milk(new Sugar(new Coffee()));
```

3. **Echipa înțelege pattern-ul**
- Vocabular comun
- Review-uri mai ușoare
- Onboarding mai rapid

4. **Beneficiile > Costuri**
- Mai multă flexibilitate
- Mai ușor de testat
- Mai ușor de extins

**❌ NU folosi patterns când:**

1. **Over-engineering**
```java
// ❌ Overkill pentru 2 clase simple
interface Animal { void makeSound(); }
class AnimalFactory {
    public static Animal createAnimal(String type) { ... }
}

// ✅ Keep it simple
Dog dog = new Dog();
dog.bark();
```

2. **Problema e simplă**
```java
// ❌ Strategy pentru o singură implementare
interface Calculator { int add(int a, int b); }

// ✅ Just do it
public int add(int a, int b) { return a + b; }
```

3. **Pattern forțat**
```java
// ❌ "Trebuie să folosesc 10 patterns în proiect"
// Patterns nu sunt scop în sine!
```

4. **Echipa nu e familiarizată**
- Introduce confuzie
- Greșeli de implementare
- Maintenance nightmare

**Regula de aur:**
> **YAGNI** (You Aren't Gonna Need It) + **KISS** (Keep It Simple, Stupid)
> 
> Adaugă pattern când devine necesar, nu preventiv!

---

### Ce greșeli se fac frecvent cu patterns?

**Răspuns:**

**1. Pattern Obsession**
```java
// ❌ Folosesc patterns doar să le folosesc
public class SimpleCalculatorFactorySingletonBuilderProxy {
    // WTF? Pentru 2+2?
}

// ✅ Simplitate
public class Calculator {
    public int add(int a, int b) { return a + b; }
}
```

**2. Singleton Abuse**
```java
// ❌ Tot e singleton
@Component  // singleton by default
public class EverythingService {
    // God object anti-pattern
}

// ✅ Singleton doar când e necesar
@Component
@Scope("prototype")  // Nou la fiecare request
public class ShoppingCart { }
```

**3. Over-abstraction**
```java
// ❌ 5 nivele de abstractizare pentru o operație simplă
interface PaymentProcessor {
    interface PaymentProcessorFactory {
        interface PaymentProcessorFactoryBuilder {
            // 😵
        }
    }
}

// ✅ Atât cât e necesar
interface PaymentProcessor {
    void processPayment(Payment payment);
}
```

**4. Ignoring SOLID**
```java
// ❌ Singleton care face totul
public class GodSingleton {
    public void doEverything() { }
    // Încalcă Single Responsibility
}

// ✅ Mai multe clase, fiecare cu responsabilitate clară
@Service
public class UserService { }

@Service
public class EmailService { }
```

**5. Copy-Paste fără înțelegere**
```java
// ❌ Copiază cod de pe Stack Overflow fără să înțeleagă
public class MyPattern {
    // Ce face asta? Nu știu, dar funcționează!
}

// ✅ Înțelege CÂND și DE CE
// Pattern-ul rezolvă problema X în contextul Y
```

---

## Concluzie

**Ce să reținem despre Design Patterns:**

1. **Nu sunt silver bullet** - nu rezolvă toate problemele
2. **Sunt vocabular comun** - facilitează comunicarea
3. **Evoluează cu limbajul** - unele patterns sunt built-in în limbaje moderne
4. **Context matters** - același pattern poate fi implementat diferit
5. **KISS > Complexity** - simplitatea învinge complexitatea

**Patterns în interviuri:**
- **Cunoaște-le pe cele comune** (Singleton, Factory, Strategy, Observer)
- **Înțelege CÂND se folosesc**, nu doar HOW
- **Dă exemple din experiență** (Spring, Java ecosystem)
- **Recunoaște trade-offs** - fiecare pattern are costuri

**Următorii pași:**
- Identifică patterns în cod existent (Spring, Java libraries)
- Refactorizează cod existent către patterns
- Scrie cod nou având în minte patterns, dar fără să forțezi
- Citește "Design Patterns" de GoF pentru înțelegere profundă

---

**🎯 Resurse recomandate:**
- "Design Patterns: Elements of Reusable Object-Oriented Software" (GoF)
- "Head First Design Patterns" (vizual, ușor de înțeles)
- "Effective Java" de Joshua Bloch (best practices + patterns)
- Refactoring.guru (explicații vizuale excelente)
