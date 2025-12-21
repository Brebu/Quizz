# Capitolul 5 – Spring Framework Core (VERSIUNEA FINALĂ)
## Q341–Q420 — Nivel Senior

> Limbă: Română (UTF-8, diacritice corecte)  
> Scop: Interviuri Senior / Lead / Staff  
> Tip: Teorie aprofundată

---

## Q341 [Spring][Core][Senior]
### Ce este Spring Framework și ce problemă fundamentală rezolvă?

**Definiție:**  
Spring Framework este un framework enterprise care facilitează dezvoltarea aplicațiilor Java prin **inversiunea controlului** și **injecția de dependențe**.

**Problema rezolvată:**  
- coupling strâns între componente  
- configurare rigidă  
- testare dificilă  

---

## Q342 [Spring][Core][Senior]
### Ce este Inversion of Control (IoC)?

**Răspuns:**  
IoC mută responsabilitatea creării și gestionării obiectelor din codul aplicației într-un container.

**Beneficiu major:**  
Cod decuplat și testabil.

---

## Q343 [Spring][Core][Senior]
### Ce este Dependency Injection (DI)?

**Răspuns:**  
DI este mecanismul prin care dependențele sunt furnizate din exterior.

**Tipuri:**  
- constructor injection (recomandat)  
- setter injection  
- field injection (de evitat)

---

## Q344 [Spring][Core][Senior]
### De ce constructor injection este recomandat?

**Răspuns:**  
- imutabilitate  
- dependențe clare  
- testare ușoară  
- evită bean-uri invalide

---

## Q345 [Spring][Core][Senior]
### Ce este Spring Container?

**Răspuns:**  
Componenta care creează, configurează și gestionează ciclul de viață al bean-urilor.

---

## Q346 [Spring][Core][Senior]
### Diferența dintre BeanFactory și ApplicationContext

**Răspuns:**  
- BeanFactory: lazy, minimal  
- ApplicationContext: eager, enterprise-ready

---

## Q347 [Spring][Core][Senior]
### Ce este un Spring Bean?

**Răspuns:**  
Obiect gestionat de Spring Container.

---

## Q348 [Spring][Core][Senior]
### Cum este identificat un bean în Spring?

**Răspuns:**  
Prin:
- tip  
- nume  
- qualifiers

---

## Q349 [Spring][Core][Senior]
### Ce este component scanning?

**Răspuns:**  
Mecanism automat de descoperire a bean-urilor.

---

## Q350 [Spring][Core][Senior]
### Rolul anotărilor `@Component`, `@Service`, `@Repository`

**Răspuns:**  
Specializări semantice pentru claritate și tooling.

---

## Q351 [Spring][Core][Senior]
### Diferența dintre `@Autowired` și constructor injection

**Răspuns:**  
`@Autowired` pe constructor este implicit din Spring 4.3+.

---

## Q352 [Spring][Core][Senior]
### Ce este `@Qualifier`?

**Răspuns:**  
Specifică ce bean să fie injectat când există ambiguități.

---

## Q353 [Spring][Core][Senior]
### Ce este `@Primary`?

**Răspuns:**  
Marchează bean-ul implicit.

---

## Q354 [Spring][Core][Senior]
### Ce este scope-ul unui bean?

**Răspuns:**  
Definește durata de viață și vizibilitatea bean-ului.

---

## Q355 [Spring][Core][Senior]
### Scope-uri standard în Spring

**Răspuns:**  
- singleton (default)  
- prototype  
- request  
- session  
- application

---

## Q356 [Spring][Core][Senior]
### Diferența dintre singleton Spring și singleton clasic

**Răspuns:**  
Singleton Spring este per-container, nu per-JVM.

---

## Q357 [Spring][Core][Senior]
### Ce este lifecycle-ul unui bean?

**Răspuns:**  
Creare → injectare → inițializare → utilizare → distrugere.

---

## Q358 [Spring][Core][Senior]
### Rolul `@PostConstruct` și `@PreDestroy`

**Răspuns:**  
Hook-uri pentru inițializare și cleanup.

---

## Q359 [Spring][Core][Senior]
### Ce este `BeanPostProcessor`?

**Răspuns:**  
Interceptează bean-urile înainte și după inițializare.

---

## Q360 [Spring][Core][Senior]
### Ce este `BeanFactoryPostProcessor`?

**Răspuns:**  
Modifică definițiile bean-urilor înainte de instanțiere.

---

## Q361 [Spring][Core][Senior]
### Diferența dintre cele două post-processors

**Răspuns:**  
- BeanFactoryPostProcessor: metadata  
- BeanPostProcessor: instanțe

---

## Q362 [Spring][Core][Senior]
### Ce este `@Configuration`?

**Răspuns:**  
Clasă care definește bean-uri.

---

## Q363 [Spring][Core][Senior]
### Ce este `@Bean`?

**Răspuns:**  
Metodă factory pentru bean-uri.

---

## Q364 [Spring][Core][Senior]
### De ce clasele `@Configuration` sunt proxiate?

**Răspuns:**  
Pentru a garanta singleton semantics.

---

## Q365 [Spring][Core][Senior]
### Ce este proxy-ul în Spring?

**Răspuns:**  
Obiect intermediar pentru cross-cutting concerns.

---

## Q366 [Spring][Core][Senior]
### Diferența dintre JDK proxy și CGLIB

**Răspuns:**  
- JDK: pe interfețe  
- CGLIB: pe clase

---

## Q367 [Spring][Core][Senior]
### Ce este AOP?

**Răspuns:**  
Aspect-Oriented Programming pentru logică transversală.

---

## Q368 [Spring][Core][Senior]
### Ce este un aspect?

**Răspuns:**  
Modularizare a cross-cutting logic.

---

## Q369 [Spring][Core][Senior]
### Ce este join point?

**Răspuns:**  
Punct de execuție interceptabil.

---

## Q370 [Spring][Core][Senior]
### Ce este advice?

**Răspuns:**  
Cod executat la un join point.

---

## Q371 [Spring][Core][Senior]
### Tipuri de advice

**Răspuns:**  
- Before  
- After  
- AfterReturning  
- AfterThrowing  
- Around

---

## Q372 [Spring][Core][Senior]
### Ce este pointcut?

**Răspuns:**  
Expresie care selectează join points.

---

## Q373 [Spring][Core][Senior]
### Diferența dintre proxy-based AOP și weaving

**Răspuns:**  
Spring AOP este proxy-based.

---

## Q374 [Spring][Core][Senior]
### Limitări Spring AOP

**Răspuns:**  
- doar metode publice  
- self-invocation nu este interceptată

---

## Q375 [Spring][Core][Senior]
### Ce este circular dependency?

**Răspuns:**  
Două sau mai multe bean-uri care depind reciproc.

---

## Q376 [Spring][Core][Senior]
### Cum gestionează Spring circular dependencies?

**Răspuns:**  
Prin early references (doar pentru field/setter injection).

---

## Q377 [Spring][Core][Senior]
### De ce constructor injection nu suportă circular dependencies?

**Răspuns:**  
Pentru că bean-urile trebuie create complet.

---

## Q378 [Spring][Core][Senior]
### Ce este `@Lazy`?

**Răspuns:**  
Amână inițializarea bean-ului.

---

## Q379 [Spring][Core][Senior]
### Când este util `@Lazy`?

**Răspuns:**  
- startup rapid  
- evitarea circular dependencies

---

## Q380 [Spring][Core][Senior]
### Ce este `@DependsOn`?

**Răspuns:**  
Controlează ordinea de inițializare.

---

## Q381 [Spring][Core][Senior]
### Ce este `@Profile`?

**Răspuns:**  
Activează bean-uri condiționat pe mediu.

---

## Q382 [Spring][Core][Senior]
### Ce este `Environment` în Spring?

**Răspuns:**  
Acces la proprietăți și profile.

---

## Q383 [Spring][Core][Senior]
### Ce este `PropertySource`?

**Răspuns:**  
Sursă de configurare.

---

## Q384 [Spring][Core][Senior]
### Ordinea de rezolvare a proprietăților

**Răspuns:**  
CLI → env vars → application.properties → defaults

---

## Q385 [Spring][Core][Senior]
### Ce este `@Value` și ce limitări are?

**Răspuns:**  
Injectează valori primitive, dar nu este type-safe.

---

## Q386 [Spring][Core][Senior]
### De ce `@ConfigurationProperties` este preferat?

**Răspuns:**  
- type-safe  
- validabil  
- organizat

---

## Q387 [Spring][Core][Senior]
### Ce este `@EnableAutoConfiguration`?

**Răspuns:**  
Activează configurarea automată (Spring Boot).

---

## Q388 [Spring][Core][Senior]
### Diferența Spring vs Spring Boot (core)

**Răspuns:**  
Spring Boot reduce configurarea manuală.

---

## Q389 [Spring][Core][Senior]
### Ce este `ApplicationContextInitializer`?

**Răspuns:**  
Customizează contextul la startup.

---

## Q390 [Spring][Core][Senior]
### Ce este `SmartLifecycle`?

**Răspuns:**  
Control fin al startup/shutdown.

---

## Q391 [Spring][Core][Senior]
### Ce este `@EventListener`?

**Răspuns:**  
Ascultă evenimente din context.

---

## Q392 [Spring][Core][Senior]
### Ce este `ApplicationEvent`?

**Răspuns:**  
Eveniment publicat în context.

---

## Q393 [Spring][Core][Senior]
### Ce este `@Conditional`?

**Răspuns:**  
Activează bean-uri condițional.

---

## Q394 [Spring][Core][Senior]
### Exemple de `@Conditional`

**Răspuns:**  
- `@ConditionalOnProperty`  
- `@ConditionalOnClass`

---

## Q395 [Spring][Core][Senior]
### Ce este `FactoryBean`?

**Răspuns:**  
Bean care produce alte bean-uri.

---

## Q396 [Spring][Core][Senior]
### Diferența `BeanFactory` vs `FactoryBean`

**Răspuns:**  
FactoryBean produce obiecte.

---

## Q397 [Spring][Core][Senior]
### Ce este `@Import`?

**Răspuns:**  
Include configurații suplimentare.

---

## Q398 [Spring][Core][Senior]
### Ce este modularizarea configurației Spring?

**Răspuns:**  
Separarea logică a configurațiilor.

---

## Q399 [Spring][Core][Senior]
### Ce este context hierarchy?

**Răspuns:**  
Ierarhie de ApplicationContext.

---

## Q400 [Spring][Core][Senior]
### Care este regula de aur în Spring Core?

**Răspuns:**  
Preferă simplitatea și constructor injection.

---

## Q401 [Spring][Core][Senior]
### Ce este auto-wiring by type vs by name?

**Răspuns:**  
- by type: implicit  
- by name: explicit

---

## Q402 [Spring][Core][Senior]
### Ce este `ResolvableType`?

**Răspuns:**  
Rezolvă tipuri generice la runtime.

---

## Q403 [Spring][Core][Senior]
### Ce este `ObjectProvider`?

**Răspuns:**  
Acces lazy și opțional la bean-uri.

---

## Q404 [Spring][Core][Senior]
### Ce este `@Lookup`?

**Răspuns:**  
Injectează bean-uri prototype în singleton.

---

## Q405 [Spring][Core][Senior]
### Ce este method injection?

**Răspuns:**  
Injectare dinamică de dependențe.

---

## Q406 [Spring][Core][Senior]
### Ce este `ScopedProxyMode`?

**Răspuns:**  
Proxy pentru scope-uri non-singleton.

---

## Q407 [Spring][Core][Senior]
### Ce este context refresh?

**Răspuns:**  
Reinițializarea ApplicationContext.

---

## Q408 [Spring][Core][Senior]
### Ce este `ContextClosedEvent`?

**Răspuns:**  
Eveniment la shutdown.

---

## Q409 [Spring][Core][Senior]
### Ce este `@Order`?

**Răspuns:**  
Definește ordinea de execuție.

---

## Q410 [Spring][Core][Senior]
### Ce este `Ordered` interface?

**Răspuns:**  
Contract pentru ordonare.

---

## Q411 [Spring][Core][Senior]
### Ce este `@DependsOn` (recap)?

**Răspuns:**  
Controlează dependențele de inițializare.

---

## Q412 [Spring][Core][Senior]
### Ce este bean overriding?

**Răspuns:**  
Înlocuirea definiției unui bean.

---

## Q413 [Spring][Core][Senior]
### De ce bean overriding este periculos?

**Răspuns:**  
Poate masca erori de configurare.

---

## Q414 [Spring][Core][Senior]
### Ce este `@Role`?

**Răspuns:**  
Clasifică bean-uri pentru tooling.

---

## Q415 [Spring][Core][Senior]
### Ce este `@Description`?

**Răspuns:**  
Documentează bean-uri.

---

## Q416 [Spring][Core][Senior]
### Ce este `@AliasFor`?

**Răspuns:**  
Leagă atribute de anotări.

---

## Q417 [Spring][Core][Senior]
### Ce este meta-annotation?

**Răspuns:**  
Anotație folosită pe alte anotări.

---

## Q418 [Spring][Core][Senior]
### Ce este stereotype annotation?

**Răspuns:**  
Anotare cu rol semantic (`@Service`).

---

## Q419 [Spring][Core][Senior]
### Ce este context caching în teste?

**Răspuns:**  
Reutilizarea contextului Spring.

---

## Q420 [Spring][Core][Senior]
### Care este cea mai mare greșeală în Spring Core?

**Răspuns:**  
Abuzul de magie în loc de design clar.

---
