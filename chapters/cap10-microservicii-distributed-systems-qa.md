# Capitolul 10 – Microservicii & Distributed Systems (VERSIUNEA FINALĂ)
## Q741–Q820 — Nivel Senior

> Limbă: Română (UTF-8, diacritice corecte)  
> Scop: Interviuri Senior / Lead / Staff  
> Tip: Teorie aprofundată

---

## Q741 [Architecture][Microservices][Senior]
### Ce sunt microserviciile și ce problemă rezolvă?

**Definiție:**  
Microserviciile sunt un stil arhitectural în care o aplicație este compusă din servicii mici, independente, care comunică prin rețea.

**Probleme rezolvate:**  
- monoliți greu de scalat  
- deploy-uri riscante  
- cuplare excesivă

---

## Q742 [Architecture][Microservices][Senior]
### Diferența dintre monolit și microservicii

**Răspuns:**  
- Monolit: o singură unitate de deploy  
- Microservicii: deploy independent

---

## Q743 [Architecture][Microservices][Senior]
### Ce este bounded context?

**Răspuns:**  
Limită clară a unui model de domeniu (DDD).

---

## Q744 [Architecture][Microservices][Senior]
### De ce DDD este important în microservicii?

**Răspuns:**  
Previne servicii prost definite și cuplare logică.

---

## Q745 [Architecture][Microservices][Senior]
### Ce este loose coupling?

**Răspuns:**  
Serviciile pot evolua independent.

---

## Q746 [Architecture][Microservices][Senior]
### Ce este high cohesion?

**Răspuns:**  
Responsabilități strâns legate într-un serviciu.

---

## Q747 [Architecture][Microservices][Senior]
### Ce este service discovery?

**Răspuns:**  
Mecanism de descoperire dinamică a serviciilor.

---

## Q748 [Architecture][Microservices][Senior]
### Client-side vs server-side discovery

**Răspuns:**  
Client-side: clientul decide  
Server-side: gateway/proxy

---

## Q749 [Architecture][Microservices][Senior]
### Ce este API Gateway?

**Răspuns:**  
Punct unic de intrare pentru clienți.

---

## Q750 [Architecture][Microservices][Senior]
### Beneficiile API Gateway

**Răspuns:**  
- securitate centralizată  
- rate limiting  
- routing

---

## Q751 [Architecture][Microservices][Senior]
### Ce este circuit breaker?

**Răspuns:**  
Protecție împotriva dependențelor instabile.

---

## Q752 [Architecture][Microservices][Senior]
### Ce este bulkhead pattern?

**Răspuns:**  
Izolarea resurselor pentru a preveni cascading failure.

---

## Q753 [Architecture][Microservices][Senior]
### Ce este retry pattern?

**Răspuns:**  
Reîncercarea cererilor eșuate.

---

## Q754 [Architecture][Microservices][Senior]
### Riscurile retry-ului

**Răspuns:**  
Amplificarea traficului și a erorilor.

---

## Q755 [Architecture][Microservices][Senior]
### Ce este timeout management?

**Răspuns:**  
Limitarea duratei apelurilor.

---

## Q756 [Architecture][Microservices][Senior]
### Ce este cascading failure?

**Răspuns:**  
Eșec în lanț al serviciilor.

---

## Q757 [Architecture][Microservices][Senior]
### Ce este eventual consistency?

**Răspuns:**  
Consistență atinsă în timp.

---

## Q758 [Architecture][Microservices][Senior]
### Ce este CAP Theorem?

**Răspuns:**  
Consistency, Availability, Partition tolerance.

---

## Q759 [Architecture][Microservices][Senior]
### Ce trade-off face CAP?

**Răspuns:**  
Nu poți avea toate cele trei simultan.

---

## Q760 [Architecture][Microservices][Senior]
### Ce este strong consistency?

**Răspuns:**  
Datele sunt sincronizate imediat.

---

## Q761 [Architecture][Microservices][Senior]
### Ce este eventual consistency (recap)?

**Răspuns:**  
Sistemul converge în timp.

---

## Q762 [Architecture][Microservices][Senior]
### Ce este distributed transaction?

**Răspuns:**  
Tranzacție peste mai multe servicii.

---

## Q763 [Architecture][Microservices][Senior]
### De ce 2PC este problematic?

**Răspuns:**  
Blocant și greu de scalat.

---

## Q764 [Architecture][Microservices][Senior]
### Ce este Saga pattern?

**Răspuns:**  
Coordonare de tranzacții prin pași compensați.

---

## Q765 [Architecture][Microservices][Senior]
### Tipuri de Saga

**Răspuns:**  
- orchestration  
- choreography

---

## Q766 [Architecture][Microservices][Senior]
### Ce este idempotency în microservicii?

**Răspuns:**  
Operații repetabile fără efecte adverse.

---

## Q767 [Architecture][Microservices][Senior]
### Ce este message broker?

**Răspuns:**  
Intermediar pentru mesaje async.

---

## Q768 [Architecture][Microservices][Senior]
### Exemple de message brokers

**Răspuns:**  
Kafka, RabbitMQ, ActiveMQ.

---

## Q769 [Architecture][Microservices][Senior]
### Ce este event-driven architecture?

**Răspuns:**  
Comunicare bazată pe evenimente.

---

## Q770 [Architecture][Microservices][Senior]
### Avantajele event-driven

**Răspuns:**  
- decuplare  
- scalabilitate  
- reziliență

---

## Q771 [Architecture][Microservices][Senior]
### Ce este message ordering?

**Răspuns:**  
Menținerea ordinii mesajelor.

---

## Q772 [Architecture][Microservices][Senior]
### Ce este at-least-once delivery?

**Răspuns:**  
Mesajele pot fi livrate de mai multe ori.

---

## Q773 [Architecture][Microservices][Senior]
### Ce este exactly-once semantics?

**Răspuns:**  
Procesare garantată o singură dată (conceptual).

---

## Q774 [Architecture][Microservices][Senior]
### Ce este back-pressure?

**Răspuns:**  
Controlul fluxului de date.

---

## Q775 [Architecture][Microservices][Senior]
### Ce este schema evolution?

**Răspuns:**  
Evoluția formatului mesajelor.

---

## Q776 [Architecture][Microservices][Senior]
### Ce este contract testing (recap)?

**Răspuns:**  
Testarea contractelor între servicii.

---

## Q777 [Architecture][Microservices][Senior]
### Ce este consumer-driven contract testing?

**Răspuns:**  
Contract definit de consumator.

---

## Q778 [Architecture][Microservices][Senior]
### Ce este observability în sisteme distribuite?

**Răspuns:**  
Metrici, log-uri și trace-uri corelate.

---

## Q779 [Architecture][Microservices][Senior]
### Ce este distributed tracing?

**Răspuns:**  
Urmărirea unei cereri prin mai multe servicii.

---

## Q780 [Architecture][Microservices][Senior]
### Ce este correlation ID?

**Răspuns:**  
Identificator comun pentru request-uri.

---

## Q781 [Architecture][Microservices][Senior]
### Ce este centralized logging?

**Răspuns:**  
Colectarea log-urilor într-un loc central.

---

## Q782 [Architecture][Microservices][Senior]
### Ce este health check?

**Răspuns:**  
Verificarea stării serviciului.

---

## Q783 [Architecture][Microservices][Senior]
### Ce este readiness vs liveness (recap)?

**Răspuns:**  
- readiness: poate primi trafic  
- liveness: rulează

---

## Q784 [Architecture][Microservices][Senior]
### Ce este autoscaling?

**Răspuns:**  
Scalare automată în funcție de load.

---

## Q785 [Architecture][Microservices][Senior]
### Ce este horizontal vs vertical scaling?

**Răspuns:**  
- horizontal: mai multe instanțe  
- vertical: mai multe resurse

---

## Q786 [Architecture][Microservices][Senior]
### Ce este stateless service?

**Răspuns:**  
Nu păstrează stare locală.

---

## Q787 [Architecture][Microservices][Senior]
### Ce este stateful service?

**Răspuns:**  
Păstrează stare locală.

---

## Q788 [Architecture][Microservices][Senior]
### De ce stateless este preferat?

**Răspuns:**  
Scalare și reziliență mai bune.

---

## Q789 [Architecture][Microservices][Senior]
### Ce este data ownership?

**Răspuns:**  
Fiecare serviciu își deține datele.

---

## Q790 [Architecture][Microservices][Senior]
### De ce shared database este un anti-pattern?

**Răspuns:**  
Crește cuplarea și riscul de schimbări.

---

## Q791 [Architecture][Microservices][Senior]
### Ce este database per service?

**Răspuns:**  
Fiecare serviciu are DB proprie.

---

## Q792 [Architecture][Microservices][Senior]
### Ce este API composition?

**Răspuns:**  
Agregarea datelor din mai multe servicii.

---

## Q793 [Architecture][Microservices][Senior]
### Ce este CQRS?

**Răspuns:**  
Separarea citirii de scriere.

---

## Q794 [Architecture][Microservices][Senior]
### Ce este Event Sourcing?

**Răspuns:**  
Persistarea evenimentelor, nu a stării.

---

## Q795 [Architecture][Microservices][Senior]
### Când NU folosești Event Sourcing?

**Răspuns:**  
Pentru domenii simple sau fără audit.

---

## Q796 [Architecture][Microservices][Senior]
### Ce este sidecar pattern?

**Răspuns:**  
Container auxiliar per serviciu.

---

## Q797 [Architecture][Microservices][Senior]
### Ce este service mesh?

**Răspuns:**  
Layer de infrastructură pentru trafic.

---

## Q798 [Architecture][Microservices][Senior]
### Beneficii service mesh

**Răspuns:**  
- observability  
- securitate  
- retry/circuit breaker

---

## Q799 [Architecture][Microservices][Senior]
### Ce este zero-downtime deployment?

**Răspuns:**  
Deploy fără întreruperi.

---

## Q800 [Architecture][Microservices][Senior]
### Tehnici de zero-downtime deployment

**Răspuns:**  
- blue/green  
- canary  
- rolling

---

## Q801 [Architecture][Microservices][Senior]
### Ce este backward compatibility în microservicii?

**Răspuns:**  
Menținerea suportului pentru versiuni vechi.

---

## Q802 [Architecture][Microservices][Senior]
### Ce este versionarea contractelor?

**Răspuns:**  
Gestionarea schimbărilor API.

---

## Q803 [Architecture][Microservices][Senior]
### Ce este resilience engineering?

**Răspuns:**  
Proiectarea pentru eșec.

---

## Q804 [Architecture][Microservices][Senior]
### Ce este chaos engineering?

**Răspuns:**  
Testarea intenționată a eșecurilor.

---

## Q805 [Architecture][Microservices][Senior]
### Ce este network latency?

**Răspuns:**  
Întârziere de rețea.

---

## Q806 [Architecture][Microservices][Senior]
### Ce este partial failure?

**Răspuns:**  
Eșec parțial într-un sistem distribuit.

---

## Q807 [Architecture][Microservices][Senior]
### Ce este split brain?

**Răspuns:**  
Partiționare logică a clusterului.

---

## Q808 [Architecture][Microservices][Senior]
### Ce este leader election?

**Răspuns:**  
Alegerea unui nod coordonator.

---

## Q809 [Architecture][Microservices][Senior]
### Ce este consensus algorithm?

**Răspuns:**  
Algoritm pentru acord între noduri.

---

## Q810 [Architecture][Microservices][Senior]
### Exemple de algoritmi de consens

**Răspuns:**  
Raft, Paxos.

---

## Q811 [Architecture][Microservices][Senior]
### Ce este eventual leader consistency?

**Răspuns:**  
Consistență cu lider stabil în timp.

---

## Q812 [Architecture][Microservices][Senior]
### Ce este sharding?

**Răspuns:**  
Partiționarea datelor.

---

## Q813 [Architecture][Microservices][Senior]
### Ce este replication?

**Răspuns:**  
Duplicarea datelor.

---

## Q814 [Architecture][Microservices][Senior]
### Ce este quorum?

**Răspuns:**  
Majoritate necesară pentru decizii.

---

## Q815 [Architecture][Microservices][Senior]
### Ce este hot partition?

**Răspuns:**  
Partiție supraîncărcată.

---

## Q816 [Architecture][Microservices][Senior]
### Ce este load balancing?

**Răspuns:**  
Distribuirea traficului.

---

## Q817 [Architecture][Microservices][Senior]
### Tipuri de load balancing

**Răspuns:**  
- round-robin  
- least connections  
- weighted

---

## Q818 [Architecture][Microservices][Senior]
### Ce este service-to-service authentication?

**Răspuns:**  
Autentificarea între servicii.

---

## Q819 [Architecture][Microservices][Senior]
### Ce este mTLS?

**Răspuns:**  
Autentificare mutuală TLS.

---

## Q820 [Architecture][Microservices][Senior]
### Care este regula de aur în sisteme distribuite?

**Răspuns:**  
Rețeaua NU este fiabilă.

---
