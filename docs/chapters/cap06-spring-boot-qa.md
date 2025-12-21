# Capitolul 6 – Spring Boot (VERSIUNEA FINALĂ)
## Q421–Q500 — Nivel Senior

> Limbă: Română (UTF-8, diacritice corecte)  
> Scop: Interviuri Senior / Lead / Staff  
> Tip: Teorie aprofundată

---

## Q421 [Spring][Boot][Senior]
### Ce este Spring Boot și ce problemă rezolvă față de Spring clasic?

**Definiție:**  
Spring Boot este un proiect Spring care simplifică crearea aplicațiilor Spring prin convenții, auto-configurare și startup rapid.

**Probleme rezolvate:**  
- configurare excesivă  
- boilerplate XML/JavaConfig  
- pornire greoaie a aplicațiilor

---

## Q422 [Spring][Boot][Senior]
### Ce înseamnă „convention over configuration” în Spring Boot?

**Răspuns:**  
Spring Boot aplică valori implicite inteligente bazate pe classpath și context, reducând configurația explicită.

---

## Q423 [Spring][Boot][Senior]
### Ce este auto-configurarea în Spring Boot?

**Răspuns:**  
Mecanism prin care Spring Boot configurează automat bean-uri pe baza:
- classpath-ului  
- proprietăților  
- condițiilor runtime

---

## Q424 [Spring][Boot][Senior]
### Cum funcționează intern auto-configurarea?

**Răspuns:**  
Prin clase `@Configuration` condiționale activate cu `@ConditionalOn...`.

---

## Q425 [Spring][Boot][Senior]
### Rolul anotării `@SpringBootApplication`

**Răspuns:**  
Este o meta-anotație ce combină:
- `@Configuration`  
- `@EnableAutoConfiguration`  
- `@ComponentScan`

---

## Q426 [Spring][Boot][Senior]
### Ce este `spring.factories` / `AutoConfiguration.imports`?

**Răspuns:**  
Mecanisme pentru înregistrarea auto-configurărilor.

---

## Q427 [Spring][Boot][Senior]
### Diferența dintre Spring Boot 2 și 3 (conceptual)

**Răspuns:**  
- Java 17+  
- Jakarta EE  
- Observability îmbunătățită

---

## Q428 [Spring][Boot][Senior]
### Ce este starter-ul în Spring Boot?

**Răspuns:**  
Un set de dependențe preconfigurate pentru un scop specific.

---

## Q429 [Spring][Boot][Senior]
### De ce starter-ele sunt importante?

**Răspuns:**  
Asigură compatibilitate și reduc conflictele de versiuni.

---

## Q430 [Spring][Boot][Senior]
### Ce este embedded server?

**Răspuns:**  
Server web inclus în aplicație (Tomcat, Jetty, Undertow).

---

## Q431 [Spring][Boot][Senior]
### Avantajele serverelor embedded

**Răspuns:**  
- deployment simplu  
- aplicație self-contained  
- consistență între medii

---

## Q432 [Spring][Boot][Senior]
### Ce este `ApplicationRunner` vs `CommandLineRunner`?

**Răspuns:**  
- ApplicationRunner: argumente tipizate  
- CommandLineRunner: array de String

---

## Q433 [Spring][Boot][Senior]
### Ce este lifecycle-ul unei aplicații Spring Boot?

**Răspuns:**  
Bootstrap → Context init → Auto-config → Ready → Shutdown

---

## Q434 [Spring][Boot][Senior]
### Ce este `SpringApplication`?

**Răspuns:**  
Clasa care pornește aplicația Spring Boot.

---

## Q435 [Spring][Boot][Senior]
### Ce este banner-ul Spring Boot?

**Răspuns:**  
Text afișat la startup, configurabil sau dezactivabil.

---

## Q436 [Spring][Boot][Senior]
### Ce este `application.properties` vs `application.yml`?

**Răspuns:**  
Două formate pentru configurare; YAML este mai expresiv.

---

## Q437 [Spring][Boot][Senior]
### Ordinea de încărcare a configurațiilor

**Răspuns:**  
CLI → env vars → config files → defaults

---

## Q438 [Spring][Boot][Senior]
### Ce sunt profilele Spring Boot?

**Răspuns:**  
Mecanism pentru configurare specifică mediului.

---

## Q439 [Spring][Boot][Senior]
### Cum se activează profilele?

**Răspuns:**  
- `spring.profiles.active`  
- variabile de mediu  
- CLI

---

## Q440 [Spring][Boot][Senior]
### Ce este `@ConfigurationProperties` (recap)?

**Răspuns:**  
Mapează proprietăți pe obiecte type-safe.

---

## Q441 [Spring][Boot][Senior]
### Ce este Actuator?

**Răspuns:**  
Modul pentru monitorizare și management aplicație.

---

## Q442 [Spring][Boot][Senior]
### Endpoint-uri Actuator importante

**Răspuns:**  
- `/health`  
- `/metrics`  
- `/info`  
- `/env`

---

## Q443 [Spring][Boot][Senior]
### Ce este health indicator?

**Răspuns:**  
Componentă care raportează starea aplicației.

---

## Q444 [Spring][Boot][Senior]
### Ce este readiness vs liveness?

**Răspuns:**  
- readiness: poate primi trafic  
- liveness: este vie

---

## Q445 [Spring][Boot][Senior]
### Ce este Micrometer?

**Răspuns:**  
Facade pentru metrici (Prometheus, Datadog etc.).

---

## Q446 [Spring][Boot][Senior]
### Ce este observability în Spring Boot 3?

**Răspuns:**  
Integrare nativă pentru metrics, traces și logs.

---

## Q447 [Spring][Boot][Senior]
### Ce este auto-configuration report?

**Răspuns:**  
Raport care arată ce auto-configurări au fost aplicate.

---

## Q448 [Spring][Boot][Senior]
### Cum dezactivezi o auto-configurare?

**Răspuns:**  
- `exclude` în `@SpringBootApplication`  
- `spring.autoconfigure.exclude`

---

## Q449 [Spring][Boot][Senior]
### Ce este layering în Spring Boot JAR?

**Răspuns:**  
Separarea straturilor pentru build eficient Docker.

---

## Q450 [Spring][Boot][Senior]
### Ce este fat JAR?

**Răspuns:**  
JAR ce conține toate dependențele.

---

## Q451 [Spring][Boot][Senior]
### Avantaje și dezavantaje fat JAR

**Răspuns:**  
+ deployment simplu  
− dimensiune mare

---

## Q452 [Spring][Boot][Senior]
### Ce este devtools?

**Răspuns:**  
Tooling pentru reload rapid în development.

---

## Q453 [Spring][Boot][Senior]
### Ce este externalized configuration?

**Răspuns:**  
Configurare din afara aplicației.

---

## Q454 [Spring][Boot][Senior]
### Ce este `@ConditionalOnProperty`?

**Răspuns:**  
Activează bean-uri pe baza proprietăților.

---

## Q455 [Spring][Boot][Senior]
### Ce este `@ConditionalOnClass`?

**Răspuns:**  
Activează bean-uri dacă există clase pe classpath.

---

## Q456 [Spring][Boot][Senior]
### Ce este `@ConditionalOnMissingBean`?

**Răspuns:**  
Permite override de către utilizator.

---

## Q457 [Spring][Boot][Senior]
### Ce este `@AutoConfigureAfter` / `@AutoConfigureBefore`?

**Răspuns:**  
Controlează ordinea auto-configurărilor.

---

## Q458 [Spring][Boot][Senior]
### Ce este bootstrap context?

**Răspuns:**  
Context timpuriu pentru configurare (Spring Cloud).

---

## Q459 [Spring][Boot][Senior]
### Ce este `SpringApplicationBuilder`?

**Răspuns:**  
API fluent pentru pornirea aplicației.

---

## Q460 [Spring][Boot][Senior]
### Ce este graceful shutdown?

**Răspuns:**  
Oprire controlată a aplicației.

---

## Q461 [Spring][Boot][Senior]
### Cum implementezi graceful shutdown?

**Răspuns:**  
Prin Actuator și configurare server.

---

## Q462 [Spring][Boot][Senior]
### Ce este `ExitCodeGenerator`?

**Răspuns:**  
Controlează codul de ieșire al aplicației.

---

## Q463 [Spring][Boot][Senior]
### Ce este layering pentru Docker images?

**Răspuns:**  
Optimizarea cache-ului Docker.

---

## Q464 [Spring][Boot][Senior]
### Ce este native image (GraalVM)?

**Răspuns:**  
Compilare ahead-of-time într-un binar nativ.

---

## Q465 [Spring][Boot][Senior]
### Avantaje și dezavantaje native image

**Răspuns:**  
+ startup rapid  
− build complex

---

## Q466 [Spring][Boot][Senior]
### Ce este `spring.main.lazy-initialization`?

**Răspuns:**  
Amână inițializarea bean-urilor.

---

## Q467 [Spring][Boot][Senior]
### Când este util lazy initialization?

**Răspuns:**  
Startup rapid, consum redus inițial.

---

## Q468 [Spring][Boot][Senior]
### Ce este `ApplicationContextRunner`?

**Răspuns:**  
Testare light pentru auto-configurări.

---

## Q469 [Spring][Boot][Senior]
### Ce este `@SpringBootTest`?

**Răspuns:**  
Pornește context complet pentru teste.

---

## Q470 [Spring][Boot][Senior]
### Ce este slicing în teste?

**Răspuns:**  
Testarea parțială a contextului.

---

## Q471 [Spring][Boot][Senior]
### Exemple de test slicing

**Răspuns:**  
- `@WebMvcTest`  
- `@DataJpaTest`

---

## Q472 [Spring][Boot][Senior]
### Ce este `@MockBean`?

**Răspuns:**  
Înlocuiește bean-uri reale cu mock-uri.

---

## Q473 [Spring][Boot][Senior]
### Ce este `@SpyBean`?

**Răspuns:**  
Spionează bean-uri reale.

---

## Q474 [Spring][Boot][Senior]
### Ce este auto-configuration ordering issue?

**Răspuns:**  
Conflicte între auto-configurări.

---

## Q475 [Spring][Boot][Senior]
### Ce este `spring.config.import`?

**Răspuns:**  
Import de configurații externe.

---

## Q476 [Spring][Boot][Senior]
### Ce este config data API?

**Răspuns:**  
Nou mecanism de încărcare config (Boot 2.4+).

---

## Q477 [Spring][Boot][Senior]
### Ce este profile-specific config?

**Răspuns:**  
Config specific unui profil.

---

## Q478 [Spring][Boot][Senior]
### Ce este `spring.application.name`?

**Răspuns:**  
Identificatorul aplicației.

---

## Q479 [Spring][Boot][Senior]
### Ce este `spring.main.web-application-type`?

**Răspuns:**  
Definește tipul aplicației (SERVLET, REACTIVE).

---

## Q480 [Spring][Boot][Senior]
### Ce este `SpringBootServletInitializer`?

**Răspuns:**  
Integrare cu server extern.

---

## Q481 [Spring][Boot][Senior]
### Când NU este nevoie de server extern?

**Răspuns:**  
Când folosești embedded server.

---

## Q482 [Spring][Boot][Senior]
### Ce este layered context?

**Răspuns:**  
Separarea logică a contextelor.

---

## Q483 [Spring][Boot][Senior]
### Ce este bootstrap failure?

**Răspuns:**  
Eșec la pornire din cauza config.

---

## Q484 [Spring][Boot][Senior]
### Ce este auto-restart?

**Răspuns:**  
Restart automat în dev.

---

## Q485 [Spring][Boot][Senior]
### Ce este `spring.main.allow-bean-definition-overriding`?

**Răspuns:**  
Permite override de bean-uri.

---

## Q486 [Spring][Boot][Senior]
### Riscurile bean overriding

**Răspuns:**  
Poate ascunde erori.

---

## Q487 [Spring][Boot][Senior]
### Ce este dependency management în Boot?

**Răspuns:**  
Control centralizat al versiunilor.

---

## Q488 [Spring][Boot][Senior]
### Ce este BOM (Bill of Materials)?

**Răspuns:**  
Listă de versiuni compatibile.

---

## Q489 [Spring][Boot][Senior]
### Ce este starter parent?

**Răspuns:**  
POM părinte pentru proiecte Boot.

---

## Q490 [Spring][Boot][Senior]
### Ce este `spring-boot-maven-plugin`?

**Răspuns:**  
Build și packaging Boot.

---

## Q491 [Spring][Boot][Senior]
### Ce este layered JAR?

**Răspuns:**  
JAR cu layere pentru containere.

---

## Q492 [Spring][Boot][Senior]
### Ce este buildpacks?

**Răspuns:**  
Construire imagini Docker fără Dockerfile.

---

## Q493 [Spring][Boot][Senior]
### Ce este `bootBuildImage`?

**Răspuns:**  
Task pentru build image.

---

## Q494 [Spring][Boot][Senior]
### Ce este actuator security?

**Răspuns:**  
Protejarea endpoint-urilor Actuator.

---

## Q495 [Spring][Boot][Senior]
### Ce este management port?

**Răspuns:**  
Port separat pentru Actuator.

---

## Q496 [Spring][Boot][Senior]
### Ce este `spring.lifecycle.timeout-per-shutdown-phase`?

**Răspuns:**  
Timeout pentru shutdown.

---

## Q497 [Spring][Boot][Senior]
### Ce este `spring.main.banner-mode`?

**Răspuns:**  
Controlează afișarea banner-ului.

---

## Q498 [Spring][Boot][Senior]
### Ce este `spring.main.register-shutdown-hook`?

**Răspuns:**  
Înregistrează shutdown hook JVM.

---

## Q499 [Spring][Boot][Senior]
### Ce este `ApplicationAvailability`?

**Răspuns:**  
Starea aplicației (liveness/readiness).

---

## Q500 [Spring][Boot][Senior]
### Care este regula de aur în Spring Boot?

**Răspuns:**  
Înțelege ce face auto-configurarea înainte să o suprascrii.

---
