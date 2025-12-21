# Capitolul 8 – Spring Data JPA & Hibernate (VERSIUNEA FINALĂ)
## Q581–Q660 — Nivel Senior

> Limbă: Română (UTF-8, diacritice corecte)  
> Scop: Interviuri Senior / Lead / Staff  
> Tip: Teorie aprofundată

---

## Q581 [Spring][Data][JPA][Senior]
### Ce este JPA și ce problemă rezolvă?

**Definiție:**  
JPA (Java Persistence API) este o specificație pentru maparea obiect–relațională (ORM).

**Probleme rezolvate:**  
- impedance mismatch între OOP și baze relaționale  
- boilerplate JDBC  
- gestionarea tranzacțiilor și a relațiilor

---

## Q582 [Spring][Data][JPA][Senior]
### Ce este Hibernate?

**Răspuns:**  
Hibernate este cea mai utilizată implementare JPA.

---

## Q583 [Spring][Data][JPA][Senior]
### Diferența JPA vs Hibernate

**Răspuns:**  
- JPA: specificație  
- Hibernate: implementare + extensii

---

## Q584 [Spring][Data][JPA][Senior]
### Ce este ORM?

**Răspuns:**  
Maparea automată între obiecte Java și tabele SQL.

---

## Q585 [Spring][Data][JPA][Senior]
### Ce este EntityManager?

**Răspuns:**  
API-ul principal pentru interacțiunea cu persistence context.

---

## Q586 [Spring][Data][JPA][Senior]
### Ce este persistence context?

**Răspuns:**  
Contextul care gestionează entitățile atașate.

---

## Q587 [Spring][Data][JPA][Senior]
### Stările unei entități JPA

**Răspuns:**  
- transient  
- managed  
- detached  
- removed

---

## Q588 [Spring][Data][JPA][Senior]
### Ce este dirty checking?

**Răspuns:**  
Mecanism prin care Hibernate detectează modificările entităților.

---

## Q589 [Spring][Data][JPA][Senior]
### Când se execută SQL-ul real?

**Răspuns:**  
La flush sau commit.

---

## Q590 [Spring][Data][JPA][Senior]
### Ce este flush?

**Răspuns:**  
Sincronizarea persistence context cu baza de date.

---

## Q591 [Spring][Data][JPA][Senior]
### Diferența flush vs commit

**Răspuns:**  
- flush: trimite SQL  
- commit: finalizează tranzacția

---

## Q592 [Spring][Data][JPA][Senior]
### Ce este `@Entity`?

**Răspuns:**  
Marchează o clasă ca entitate persistentă.

---

## Q593 [Spring][Data][JPA][Senior]
### Ce este `@Table`?

**Răspuns:**  
Mapează entitatea la un tabel.

---

## Q594 [Spring][Data][JPA][Senior]
### Ce este `@Id` și strategii de generare

**Răspuns:**  
- AUTO  
- IDENTITY  
- SEQUENCE  
- TABLE

---

## Q595 [Spring][Data][JPA][Senior]
### Diferența IDENTITY vs SEQUENCE

**Răspuns:**  
IDENTITY: DB generează ID  
SEQUENCE: Hibernate poate batch-ui

---

## Q596 [Spring][Data][JPA][Senior]
### Ce este `@Column`?

**Răspuns:**  
Configurează coloana SQL.

---

## Q597 [Spring][Data][JPA][Senior]
### Ce este `@Transient`?

**Răspuns:**  
Câmp nepersistent.

---

## Q598 [Spring][Data][JPA][Senior]
### Ce este `@Version`?

**Răspuns:**  
Activează optimistic locking.

---

## Q599 [Spring][Data][JPA][Senior]
### Ce este optimistic locking?

**Răspuns:**  
Control concurență bazat pe versiune.

---

## Q600 [Spring][Data][JPA][Senior]
### Ce este pessimistic locking?

**Răspuns:**  
Blocare explicită la nivel DB.

---

## Q601 [Spring][Data][JPA][Senior]
### Diferența optimistic vs pessimistic locking

**Răspuns:**  
Optimistic: mai scalabil  
Pessimistic: mai sigur

---

## Q602 [Spring][Data][JPA][Senior]
### Ce este `@OneToOne`?

**Răspuns:**  
Relație 1–1.

---

## Q603 [Spring][Data][JPA][Senior]
### Ce este `@OneToMany`?

**Răspuns:**  
Relație 1–N.

---

## Q604 [Spring][Data][JPA][Senior]
### Ce este `@ManyToOne`?

**Răspuns:**  
Relație N–1.

---

## Q605 [Spring][Data][JPA][Senior]
### Ce este `@ManyToMany`?

**Răspuns:**  
Relație N–N.

---

## Q606 [Spring][Data][JPA][Senior]
### Ce este owning side vs inverse side?

**Răspuns:**  
Owning side controlează FK.

---

## Q607 [Spring][Data][JPA][Senior]
### Ce este `mappedBy`?

**Răspuns:**  
Definește partea inversă a relației.

---

## Q608 [Spring][Data][JPA][Senior]
### Ce este fetch type?

**Răspuns:**  
Strategia de încărcare a relațiilor.

---

## Q609 [Spring][Data][JPA][Senior]
### FetchType.EAGER vs LAZY

**Răspuns:**  
EAGER: imediat  
LAZY: la acces

---

## Q610 [Spring][Data][JPA][Senior]
### De ce EAGER este periculos?

**Răspuns:**  
Explozie de query-uri și memorie.

---

## Q611 [Spring][Data][JPA][Senior]
### Ce este N+1 query problem?

**Răspuns:**  
Executarea excesivă de query-uri.

---

## Q612 [Spring][Data][JPA][Senior]
### Cum rezolvi N+1?

**Răspuns:**  
- fetch join  
- EntityGraph  
- batch fetching

---

## Q613 [Spring][Data][JPA][Senior]
### Ce este JPQL?

**Răspuns:**  
Query language orientat pe entități.

---

## Q614 [Spring][Data][JPA][Senior]
### Diferența JPQL vs SQL

**Răspuns:**  
JPQL operează pe entități, nu tabele.

---

## Q615 [Spring][Data][JPA][Senior]
### Ce este Criteria API?

**Răspuns:**  
API type-safe pentru query-uri dinamice.

---

## Q616 [Spring][Data][JPA][Senior]
### Ce este Specification?

**Răspuns:**  
Abstracție Spring pentru Criteria API.

---

## Q617 [Spring][Data][JPA][Senior]
### Ce este Spring Data JPA?

**Răspuns:**  
Abstracție peste JPA pentru repository-uri.

---

## Q618 [Spring][Data][JPA][Senior]
### Ce este `JpaRepository`?

**Răspuns:**  
Repository complet CRUD + paging.

---

## Q619 [Spring][Data][JPA][Senior]
### Ce este query derivation?

**Răspuns:**  
Generarea query-urilor din nume de metode.

---

## Q620 [Spring][Data][JPA][Senior]
### Limite ale query derivation

**Răspuns:**  
Nume greu de citit, query-uri complexe.

---

## Q621 [Spring][Data][JPA][Senior]
### Ce este `@Query`?

**Răspuns:**  
Definește query-uri custom JPQL/SQL.

---

## Q622 [Spring][Data][JPA][Senior]
### Ce este native query?

**Răspuns:**  
Query SQL direct.

---

## Q623 [Spring][Data][JPA][Senior]
### Riscuri native queries

**Răspuns:**  
Portabilitate redusă.

---

## Q624 [Spring][Data][JPA][Senior]
### Ce este pagination în JPA?

**Răspuns:**  
Împărțirea rezultatelor în pagini.

---

## Q625 [Spring][Data][JPA][Senior]
### Ce este sorting în repository-uri?

**Răspuns:**  
Ordonarea rezultatelor.

---

## Q626 [Spring][Data][JPA][Senior]
### Ce este `Page` vs `Slice`

**Răspuns:**  
Page: count query  
Slice: fără count

---

## Q627 [Spring][Data][JPA][Senior]
### Ce este `@Transactional`?

**Răspuns:**  
Delimitează tranzacții.

---

## Q628 [Spring][Data][JPA][Senior]
### Propagation types

**Răspuns:**  
REQUIRED, REQUIRES_NEW, SUPPORTS etc.

---

## Q629 [Spring][Data][JPA][Senior]
### Isolation levels

**Răspuns:**  
READ_COMMITTED, REPEATABLE_READ etc.

---

## Q630 [Spring][Data][JPA][Senior]
### Ce este read-only transaction?

**Răspuns:**  
Optimizează performanța.

---

## Q631 [Spring][Data][JPA][Senior]
### Ce este Open Session in View (OSIV)?

**Răspuns:**  
Menține sesiunea deschisă până la view.

---

## Q632 [Spring][Data][JPA][Senior]
### De ce OSIV este controversat?

**Răspuns:**  
Ascunde probleme de design.

---

## Q633 [Spring][Data][JPA][Senior]
### Ce este second-level cache?

**Răspuns:**  
Cache între sesiuni.

---

## Q634 [Spring][Data][JPA][Senior]
### Ce este query cache?

**Răspuns:**  
Cache pentru rezultate query.

---

## Q635 [Spring][Data][JPA][Senior]
### Ce este first-level cache?

**Răspuns:**  
Cache-ul persistence context.

---

## Q636 [Spring][Data][JPA][Senior]
### Ce este batch processing?

**Răspuns:**  
Procesarea în grup a operațiilor.

---

## Q637 [Spring][Data][JPA][Senior]
### Ce este JDBC batching?

**Răspuns:**  
Executarea SQL în batch.

---

## Q638 [Spring][Data][JPA][Senior]
### Ce este entity graph?

**Răspuns:**  
Definirea explicită a fetch plan-ului.

---

## Q639 [Spring][Data][JPA][Senior]
### Ce este `@EntityGraph`?

**Răspuns:**  
Activează entity graphs.

---

## Q640 [Spring][Data][JPA][Senior]
### Ce este cascade?

**Răspuns:**  
Propagarea operațiilor către relații.

---

## Q641 [Spring][Data][JPA][Senior]
### Tipuri de cascade

**Răspuns:**  
PERSIST, MERGE, REMOVE etc.

---

## Q642 [Spring][Data][JPA][Senior]
### Ce este orphanRemoval?

**Răspuns:**  
Șterge entități orfane.

---

## Q643 [Spring][Data][JPA][Senior]
### Riscuri cu cascade REMOVE

**Răspuns:**  
Ștergeri neintenționate.

---

## Q644 [Spring][Data][JPA][Senior]
### Ce este `equals` și `hashCode` pentru entități?

**Răspuns:**  
Trebuie implementate cu grijă.

---

## Q645 [Spring][Data][JPA][Senior]
### De ce ID-ul nu e bun pentru equals?

**Răspuns:**  
ID-ul nu există în transient.

---

## Q646 [Spring][Data][JPA][Senior]
### Ce este DTO projection?

**Răspuns:**  
Selectarea directă în DTO.

---

## Q647 [Spring][Data][JPA][Senior]
### Ce este interface-based projection?

**Răspuns:**  
Projection bazată pe interfețe.

---

## Q648 [Spring][Data][JPA][Senior]
### Ce este fetch join?

**Răspuns:**  
Join care încarcă relații.

---

## Q649 [Spring][Data][JPA][Senior]
### Riscuri cu fetch join

**Răspuns:**  
Duplicate și paging problematic.

---

## Q650 [Spring][Data][JPA][Senior]
### Ce este subselect fetching?

**Răspuns:**  
Optimizare pentru colecții.

---

## Q651 [Spring][Data][JPA][Senior]
### Ce este multi-tenancy?

**Răspuns:**  
Suport pentru mai mulți clienți.

---

## Q652 [Spring][Data][JPA][Senior]
### Tipuri de multi-tenancy

**Răspuns:**  
- schema  
- database  
- discriminator

---

## Q653 [Spring][Data][JPA][Senior]
### Ce este schema migration?

**Răspuns:**  
Evoluția bazei de date.

---

## Q654 [Spring][Data][JPA][Senior]
### Rolul Flyway / Liquibase

**Răspuns:**  
Gestionarea migrărilor.

---

## Q655 [Spring][Data][JPA][Senior]
### Ce este DDL auto?

**Răspuns:**  
Generarea automată a schemelor.

---

## Q656 [Spring][Data][JPA][Senior]
### De ce DDL auto e periculos în prod?

**Răspuns:**  
Poate distruge date.

---

## Q657 [Spring][Data][JPA][Senior]
### Ce este database locking?

**Răspuns:**  
Blocarea rândurilor/tabelelor.

---

## Q658 [Spring][Data][JPA][Senior]
### Ce este phantom read?

**Răspuns:**  
Citire inconsistentă.

---

## Q659 [Spring][Data][JPA][Senior]
### Ce este repeatable read?

**Răspuns:**  
Nivel de izolare.

---

## Q660 [Spring][Data][JPA][Senior]
### Care este regula de aur în JPA?

**Răspuns:**  
Controlează explicit ce și când încarci.

---
