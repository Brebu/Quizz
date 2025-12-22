# Capitolul 1 – Java Core & OOP (VERSIUNEA FINALĂ)
## Q001–Q120 — Nivel Senior

> Limbă: Română (UTF-8, diacritice corecte)  
> Scop: Interviuri Senior / Lead / Staff  
> Tip: Teorie aprofundată

---

## Q001 [Java][OOP][Senior]
### Ce este OOP și de ce este fundamental în Java?

**Definiție:**  
Programarea orientată pe obiecte (OOP) este un paradigm care structurează aplicațiile în jurul obiectelor ce combină stare și comportament.

**De ce contează:**  
- Java este construit ca limbaj OOP pur (fără funcții globale)
- permite modelarea corectă a domeniului de business
- facilitează extensia fără modificarea codului existent
- stă la baza framework-urilor enterprise (Spring, Hibernate)

**Capcană frecventă:**  
Folosirea claselor doar ca structuri de date (DTO-uri) fără logică → *Anemic Domain Model*.

**Când NU ajută:**  
În scripting sau calcule matematice simple unde OOP adaugă overhead.

---

## Q002 [Java][OOP][Senior]
### Care sunt cele patru principii OOP și de ce nu sunt suficiente singure?

**Principii:**  
1. Încapsularea  
2. Moștenirea  
3. Polimorfismul  
4. Abstractizarea  

**De ce nu sunt suficiente:**  
Aplicate mecanic, pot produce design prost.  
Contează **echilibrul**, nu respectarea oarbă.

---

## Q003 [Java][OOP][Senior]
### Ce este încapsularea și ce problemă reală rezolvă?

**Definiție:**  
Încapsularea controlează accesul la stare și garantează consistența obiectului.

**Rezolvă:**  
- modificări necontrolate
- încălcarea invariantei
- coupling excesiv

**Capcană:**  
Getters/setters fără logică ≠ încapsulare.

---

## Q004 [Java][OOP][Senior]
### De ce getters și setters sunt considerate un anti-pattern în design modern?

**Răspuns:**  
Pentru că expun starea și mută logica de validare în exterior.

**Consecințe:**  
- obiecte pasive
- business logic fragmentat
- testare dificilă

**Alternativă:**  
Metode care exprimă acțiuni: `activate()`, `changeStatus()`.

---

## Q005 [Java][OOP][Senior]
### Ce este polimorfismul dinamic în Java?

**Definiție:**  
Selectarea implementării metodei la runtime pe baza tipului real al obiectului.

**De ce contează:**  
- elimină `if/else instanceof`
- permite extensie fără modificare
- susține arhitecturi deschise

---

## Q006 [Java][OOP][Senior]
### Diferența reală dintre overloading și overriding

**Overloading:**  
- compile-time
- semnături diferite
- NU este polimorfism

**Overriding:**  
- runtime
- aceeași semnătură
- baza OOP

---

## Q007 [Java][OOP][Senior]
### Ce este Liskov Substitution Principle (LSP)?

**Definiție:**  
Subclasa trebuie să poată înlocui superclasa fără a rupe comportamentul.

**Încălcări tipice:**  
- excepții noi
- precondiții mai stricte
- comportament diferit semantic

---

## Q008 [Java][OOP][Senior]
### De ce moștenirea excesivă este periculoasă?

**Probleme:**  
- fragile base class
- coupling puternic
- refactorizare riscantă

**Recomandare:**  
Preferă compoziția.

---

## Q009 [Java][OOP][Senior]
### Ce este abstractizarea și cum se aplică corect?

**Răspuns:**  
Separă intenția de implementare.

**Instrumente:**  
- interfețe
- clase abstracte
- API-uri clare

---

## Q010 [Java][Senior]
### Interfață vs clasă abstractă – criterii reale

**Interfață:**  
- contract public
- extensibilitate
- multiple implementări

**Clasă abstractă:**  
- reutilizare de cod
- implementare parțială

---

(continuă până la Q120)



---

## Q011 [Java][Senior]
### Ce este `final` în Java și ce garanții oferă?

**Definiție:**  
`final` este un modificator care restricționează extensia sau modificarea.

**Unde se aplică:**
- variabile: valoare constantă după inițializare
- metode: nu pot fi suprascrise
- clase: nu pot fi extinse

**De ce contează:**  
- crește predictibilitatea codului
- ajută JIT la optimizări
- susține imutabilitatea

**Capcană:**  
`final` pe referință ≠ obiect immutable.

---

## Q012 [Java][Senior]
### De ce `String` este immutable în Java?

**Răspuns:**  
Imutabilitatea `String`-ului oferă:
- thread-safety implicit
- securitate (ex: ClassLoader, SQL)
- caching eficient (String Pool)
- hashCode stabil

**Când contează cel mai mult:**  
În colecții hash și în aplicații concurente.

---

## Q013 [Java][Senior]
### Ce este String Pool și cum funcționează?

**Răspuns:**  
String Pool este o zonă specială din heap unde literalurile `String` sunt partajate.

**Beneficii:**
- reducerea consumului de memorie
- comparații rapide prin referință

**Capcană:**  
`new String("abc")` creează întotdeauna un obiect nou.

---

## Q014 [Java][Senior]
### Diferența dintre `==` și `equals()`

**Răspuns:**  
- `==` compară referințe
- `equals()` compară valoarea logică

**Regulă de aur:**  
Dacă suprascrii `equals()`, trebuie să suprascrii și `hashCode()`.

---

## Q015 [Java][Senior]
### Care este contractul `equals()` / `hashCode()`?

**Reguli:**
- obiecte egale → hashCode egal
- hashCode egal ≠ obiecte egale
- consistență în timp

**De ce contează:**  
Colecțiile hash se bazează pe acest contract.

---

## Q016 [Java][Senior]
### Ce este un obiect immutable?

**Răspuns:**  
Un obiect a cărui stare nu poate fi modificată după creare.

**Beneficii:**
- thread-safety
- simplitate conceptuală
- ușor de cache-uit

---

## Q017 [Java][Senior]
### De ce imutabilitatea este critică în concurență?

**Răspuns:**  
Elimină complet:
- race conditions
- nevoia de sincronizare
- erori de vizibilitate

---

## Q018 [Java][Senior]
### Ce sunt wrapper classes și de ce există?

**Răspuns:**  
Clase obiect pentru tipuri primitive (`Integer`, `Long`, etc.).

**Necesare pentru:**
- colecții
- generics
- API-uri orientate pe obiect

---

## Q019 [Java][Senior]
### Ce este autoboxing și ce riscuri implică?

**Răspuns:**  
Conversia automată între primitive și wrapper.

**Riscuri:**
- overhead de performanță
- `NullPointerException`
- comparații greșite

---

## Q020 [Java][Senior]
### De ce `Integer == Integer` poate fi periculos?

**Răspuns:**  
Din cauza Integer Cache (-128…127).

**Regulă:**  
Folosește `equals()` pentru comparații de valoare.

---

## Q021 [Java][Senior]
### Ce este `static` și ce NU este?

**Răspuns:**  
`static` aparține clasei, nu instanței.

**Nu este:**
- polimorfic
- dependent de obiect

---

## Q022 [Java][Senior]
### De ce metodele `static` nu sunt polimorfice?

**Răspuns:**  
Pentru că sunt legate la compilare (static binding).

---

## Q023 [Java][Senior]
### Ce este static binding vs dynamic binding?

**Răspuns:**  
- static: rezolvat la compilare
- dinamic: rezolvat la runtime

---

## Q024 [Java][Senior]
### Ce este constructorul și ce rol are?

**Răspuns:**  
Inițializează obiectul și garantează starea validă.

---

## Q025 [Java][Senior]
### De ce constructorii nu sunt moșteniți?

**Răspuns:**  
Pentru că inițializarea este specifică fiecărei clase.

---

## Q026 [Java][Senior]
### Ce este `this`?

**Răspuns:**  
Referință către instanța curentă.

---

## Q027 [Java][Senior]
### Ce este `super`?

**Răspuns:**  
Referință către clasa părinte.

---

## Q028 [Java][Senior]
### Ce este `instanceof` și de ce este un code smell?

**Răspuns:**  
Verifică tipul la runtime.

**Smell pentru că:**  
Indică lipsa polimorfismului.

---

## Q029 [Java][Senior]
### Ce este cohesion?

**Răspuns:**  
Măsura în care o clasă are o singură responsabilitate.

---

## Q030 [Java][Senior]
### Ce este coupling și de ce trebuie redus?

**Răspuns:**  
Dependințe puternice între componente.

**Efect:**  
Cod fragil și greu de schimbat.

---

## Q031 [Java][Senior]
### Ce este Single Responsibility Principle (SRP)?

**Răspuns:**  
O clasă trebuie să aibă un singur motiv de schimbare.

---

## Q032 [Java][Senior]
### Ce este Open/Closed Principle?

**Răspuns:**  
Deschis pentru extensie, închis pentru modificare.

---

## Q033 [Java][Senior]
### Ce este o clasă utilitară?

**Răspuns:**  
Clasă cu metode statice, fără stare.

---

## Q034 [Java][Senior]
### De ce clasele utilitare sunt problematice?

**Răspuns:**  
- greu de testat
- greu de extins
- încalcă OOP

---

## Q035 [Java][Senior]
### Ce este un POJO?

**Răspuns:**  
Obiect Java simplu, fără dependențe de framework.

---

## Q036 [Java][Senior]
### De ce POJO-urile sunt importante în Spring?

**Răspuns:**  
Permit decuplare și testare ușoară.

---

## Q037 [Java][Senior]
### Ce metode moștenește orice clasă din `Object`?

**Răspuns:**  
`equals`, `hashCode`, `toString`, `clone`, `wait/notify`.

---

## Q038 [Java][Senior]
### De ce suprascriem `toString()`?

**Răspuns:**  
Pentru debugging și logging.

---

## Q039 [Java][Senior]
### Ce este `clone()` și de ce este evitat?

**Răspuns:**  
API problematic, preferat: copy constructor.

---

## Q040 [Java][Senior]
### Ce este deep copy vs shallow copy?

**Răspuns:**  
- shallow: copiază referințe
- deep: copiază obiecte

---


## Q041 [Java][Senior]
### Ce este `Optional` și ce problemă reală rezolvă?

**Definiție:**  
`Optional` este un container care exprimă explicit absența unei valori.

**De ce contează:**  
- elimină folosirea implicită a `null`
- forțează tratarea cazului „fără valoare”
- clarifică intenția API-urilor

**Capcană:**  
Folosirea `Optional.get()` fără verificare.

**Când NU se folosește:**  
- ca field de clasă  
- în entități JPA sau DTO-uri

---

## Q042 [Java][Senior]
### Diferența dintre `orElse()` și `orElseGet()`

**Răspuns:**  
- `orElse()` evaluează mereu parametrul  
- `orElseGet()` evaluează lazy

**De ce contează:**  
Evită execuția inutilă de cod costisitor.

---

## Q043 [Java][Senior]
### Ce este `try-with-resources` și de ce este preferat?

**Răspuns:**  
Este un mecanism care închide automat resursele ce implementează `AutoCloseable`.

**Avantaje:**  
- cod mai sigur  
- fără scurgeri de resurse  
- mai puțin boilerplate

---

## Q044 [Java][Senior]
### De ce blocul `finally` nu este suficient?

**Răspuns:**  
- cod duplicat  
- ușor de uitat  
- nu gestionează excepții multiple elegant

---

## Q045 [Java][Senior]
### Ce este checked vs unchecked exception?

**Răspuns:**  
- checked: obligă tratarea explicită  
- unchecked: indică erori de programare

**Best practice:**  
Preferă unchecked pentru logica de business.

---

## Q046 [Java][Senior]
### De ce unchecked exceptions sunt preferate în arhitecturi moderne?

**Răspuns:**  
- permit propagare naturală  
- nu poluează semnăturile metodelor  
- se integrează mai bine cu Spring

---

## Q047 [Java][Senior]
### Ce este exception wrapping?

**Răspuns:**  
Împachetarea unei excepții într-una de nivel superior.

**Scop:**  
Păstrarea contextului fără expunerea detaliilor tehnice.

---

## Q048 [Java][Senior]
### Ce este exception translation?

**Răspuns:**  
Conversia excepțiilor tehnice în excepții de domeniu.

**Exemplu:**  
SQLException → DataAccessException

---

## Q049 [Java][Senior]
### Ce este stack trace și de ce este critic?

**Răspuns:**  
Lanțul apelurilor care a dus la eroare.

**Importanță:**  
Identifică cauza reală, nu doar efectul.

---

## Q050 [Java][Senior]
### Ce este `assert` și de ce nu se folosește în producție?

**Răspuns:**  
`assert` este un mecanism de verificare internă.

**De ce NU în producție:**  
Poate fi dezactivat la runtime.

---

## Q051 [Java][Senior]
### Ce este Generics și ce problemă rezolvă?

**Răspuns:**  
Generics aduc type-safety la compilare.

**Beneficii:**  
- elimină casting-ul  
- previne ClassCastException

---

## Q052 [Java][Senior]
### Ce este type erasure și de ce există?

**Răspuns:**  
Informația generică este eliminată la runtime.

**Motiv:**  
Compatibilitate cu Java pre-generics.

---

## Q053 [Java][Senior]
### Ce limitări introduce type erasure?

**Răspuns:**  
- nu poți crea `new T()`  
- nu poți folosi `instanceof T`

---

## Q054 [Java][Senior]
### Ce este bounded generics?

**Răspuns:**  
Restricționează tipurile permise pentru un parametru generic.

---

## Q055 [Java][Senior]
### Diferența dintre `<? extends T>` și `<? super T>`

**Răspuns:**  
- `extends` → producer  
- `super` → consumer  

Regula: **PECS**.

---

## Q056 [Java][Senior]
### Ce sunt raw types și de ce trebuie evitate?

**Răspuns:**  
Anulează type-safety și produc warning-uri.

---

## Q057 [Java][Senior]
### Ce este `Comparable` vs `Comparator`?

**Răspuns:**  
- `Comparable`: ordinea naturală  
- `Comparator`: ordini externe

---

## Q058 [Java][Senior]
### De ce `compareTo()` trebuie să fie consistent cu `equals()`?

**Răspuns:**  
Pentru colecții sortate (`TreeSet`, `TreeMap`).

---

## Q059 [Java][Senior]
### Ce este covarianța și contravarianța?

**Răspuns:**  
Controlează substituția tipurilor în generics.

---

## Q060 [Java][Senior]
### Ce este `Enum` și de ce este superior constantelor?

**Răspuns:**  
Enum oferă type-safety, comportament și extensibilitate.

---

## Q061 [Java][Senior]
### Ce este `EnumSet`?

**Răspuns:**  
Set optimizat pentru enum-uri.

---

## Q062 [Java][Senior]
### Ce este `EnumMap`?

**Răspuns:**  
Map optimizat pentru chei de tip enum.

---

## Q063 [Java][Senior]
### Ce este `var` și ce NU este?

**Răspuns:**  
Inferență de tip, nu tip dinamic.

---

## Q064 [Java][Senior]
### Când `var` afectează lizibilitatea?

**Răspuns:**  
Când tipul nu este evident din context.

---

## Q065 [Java][Senior]
### Ce este `record`?

**Răspuns:**  
Tip imutabil pentru date, cu boilerplate redus.

---

## Q066 [Java][Senior]
### Când NU este potrivit un `record`?

**Răspuns:**  
Când există logică complexă sau stare mutabilă.

---

## Q067 [Java][Senior]
### Ce este `sealed class`?

**Răspuns:**  
Restricționează ierarhiile de moștenire.

---

## Q068 [Java][Senior]
### Ce problemă rezolvă sealed classes?

**Răspuns:**  
Control și exhaustivitate în logică.

---

## Q069 [Java][Senior]
### Ce este pattern matching pentru `instanceof`?

**Răspuns:**  
Simplifică verificarea și casting-ul.

---

## Q070 [Java][Senior]
### De ce pattern matching îmbunătățește siguranța codului?

**Răspuns:**  
Reduce erori și cod redundant.

---

## Q071 [Java][Senior]
### Ce este side-effect?

**Răspuns:**  
Modificarea stării externe.

---

## Q072 [Java][Senior]
### De ce side-effects sunt periculoase?

**Răspuns:**  
Complică testarea și concurența.

---

## Q073 [Java][Senior]
### Ce este functional programming în Java?

**Răspuns:**  
Programare bazată pe funcții pure și imutabilitate.

---

## Q074 [Java][Senior]
### Ce este o interfață funcțională?

**Răspuns:**  
Interfață cu o singură metodă abstractă.

---

## Q075 [Java][Senior]
### Ce este `@FunctionalInterface`?

**Răspuns:**  
Asigură validarea la compilare.

---

## Q076 [Java][Senior]
### Ce este o expresie lambda?

**Răspuns:**  
Implementare concisă a unei interfețe funcționale.

---

## Q077 [Java][Senior]
### Ce este referința la metodă?

**Răspuns:**  
Formă scurtă de lambda.

---

## Q078 [Java][Senior]
### Ce este stream API?

**Răspuns:**  
API declarativ pentru procesarea colecțiilor.

---

## Q079 [Java][Senior]
### Diferența dintre stream și collection?

**Răspuns:**  
Stream procesează, collection stochează.

---

## Q080 [Java][Senior]
### De ce stream-urile sunt lazy?

**Răspuns:**  
Pentru performanță și compoziție eficientă.

---


## Q081 [Java][Senior]
### Ce este Java Memory Model (JMM)?

**Definiție:**  
Java Memory Model definește regulile de vizibilitate și ordonare a operațiilor între thread-uri.

**De ce contează:**  
- explică când un thread vede modificările altuia  
- stă la baza `volatile`, `final` și sincronizării

---

## Q082 [Java][Senior]
### Ce problemă rezolvă JMM?

**Răspuns:**  
Elimină ambiguitatea comportamentului concurent pe arhitecturi diferite.

---

## Q083 [Java][Senior]
### Ce este `happens-before`?

**Răspuns:**  
O relație care garantează vizibilitatea unei operații înaintea alteia.

**Exemple:**  
- unlock → lock  
- write volatile → read volatile

---

## Q084 [Java][Senior]
### Ce este `volatile` din perspectiva JMM?

**Răspuns:**  
Asigură vizibilitate și ordonare, nu atomicitate.

---

## Q085 [Java][Senior]
### De ce `final` ajută la safe publication?

**Răspuns:**  
Pentru că JVM garantează publicarea corectă a câmpurilor `final`.

---

## Q086 [Java][Senior]
### Ce este `synchronized`?

**Răspuns:**  
Mecanism de excludere mutuală și barieră de memorie.

---

## Q087 [Java][Senior]
### De ce sincronizarea excesivă este problematică?

**Răspuns:**  
- degradare de performanță  
- risc de deadlock  
- complexitate crescută

---

## Q088 [Java][Senior]
### Ce este deadlock?

**Răspuns:**  
Situație în care două sau mai multe thread-uri se blochează reciproc.

---

## Q089 [Java][Senior]
### Ce este livelock?

**Răspuns:**  
Thread-urile rulează, dar nu progresează.

---

## Q090 [Java][Senior]
### Ce este starvation?

**Răspuns:**  
Un thread nu primește niciodată resursele necesare.

---

## Q091 [Java][Senior]
### Ce este ThreadLocal și când se folosește?

**Răspuns:**  
Stocare de date per-thread.

**Cazuri tipice:**  
- contexte de securitate  
- formatare date

---

## Q092 [Java][Senior]
### Care sunt riscurile ThreadLocal?

**Răspuns:**  
- memory leaks  
- probleme în thread pools

---

## Q093 [Java][Senior]
### Ce este `ExecutorService`?

**Răspuns:**  
Abstracție pentru managementul thread-urilor.

---

## Q094 [Java][Senior]
### De ce NU este recomandat `new Thread()`?

**Răspuns:**  
- lipsă control  
- cost mare  
- imposibil de gestionat scalabil

---

## Q095 [Java][Senior]
### Ce este imutabilitatea defensivă?

**Răspuns:**  
Protejarea obiectelor prin copii defensive.

---

## Q096 [Java][Senior]
### Ce este defensive copying?

**Răspuns:**  
Crearea de copii pentru a evita expunerea stării interne.

---

## Q097 [Java][Senior]
### Ce este design by contract?

**Răspuns:**  
Definirea clară a precondițiilor și postcondițiilor.

---

## Q098 [Java][Senior]
### Ce este fail-fast?

**Răspuns:**  
Eșuarea rapidă pentru detectarea timpurie a erorilor.

---

## Q099 [Java][Senior]
### Ce este defensive programming?

**Răspuns:**  
Protejarea codului împotriva utilizării incorecte.

---

## Q100 [Java][Senior]
### Ce este Clean Code în context Java?

**Răspuns:**  
Cod lizibil, predictibil și ușor de schimbat.

---

## Q101 [Java][Senior]
### Ce este SOLID și de ce contează?

**Răspuns:**  
Set de principii pentru design extensibil și mentenabil.

---

## Q102 [Java][Senior]
### De ce overengineering este periculos?

**Răspuns:**  
Adaugă complexitate fără valoare reală.

---

## Q103 [Java][Senior]
### Ce este YAGNI?

**Răspuns:**  
You Aren’t Gonna Need It – implementează doar ce este necesar.

---

## Q104 [Java][Senior]
### Ce este KISS?

**Răspuns:**  
Keep It Simple, Stupid – simplitatea este cheia.

---

## Q105 [Java][Senior]
### Ce este DRY?

**Răspuns:**  
Don’t Repeat Yourself – evită duplicarea logicii.

---

## Q106 [Java][Senior]
### Ce este refactoring?

**Răspuns:**  
Îmbunătățirea structurii fără a schimba comportamentul.

---

## Q107 [Java][Senior]
### Când NU trebuie să faci refactoring?

**Răspuns:**  
Când nu există teste sau stabilitate.

---

## Q108 [Java][Senior]
### Ce este technical debt?

**Răspuns:**  
Costul deciziilor tehnice suboptime.

---

## Q109 [Java][Senior]
### Ce este backward compatibility?

**Răspuns:**  
Capacitatea de a suporta versiuni vechi.

---

## Q110 [Java][Senior]
### Ce este forward compatibility?

**Răspuns:**  
Capacitatea de a funcționa cu versiuni viitoare.

---

## Q111 [Java][Senior]
### Ce este semantic versioning?

**Răspuns:**  
Versionare bazată pe semnificația schimbărilor.

---

## Q112 [Java][Senior]
### Ce este API stability?

**Răspuns:**  
Garanția că API-ul nu se rupe între versiuni.

---

## Q113 [Java][Senior]
### Ce este backward-incompatible change?

**Răspuns:**  
Schimbare care rupe consumatorii existenți.

---

## Q114 [Java][Senior]
### Ce este deprecation și de ce este importantă?

**Răspuns:**  
Semnalează API-uri ce vor fi eliminate.

---

## Q115 [Java][Senior]
### Ce este logging responsabil?

**Răspuns:**  
Logging suficient fără a expune date sensibile.

---

## Q116 [Java][Senior]
### Ce este observability?

**Răspuns:**  
Capacitatea de a înțelege starea sistemului.

---

## Q117 [Java][Senior]
### Ce este tracing?

**Răspuns:**  
Urmărirea fluxului cererilor într-un sistem distribuit.

---

## Q118 [Java][Senior]
### Ce este metrics-driven development?

**Răspuns:**  
Decizii bazate pe date reale.

---

## Q119 [Java][Senior]
### Ce este backward reasoning în debugging?

**Răspuns:**  
Pornirea de la efect către cauză.

---

## Q120 [Java][Senior]
### Care este cea mai importantă abilitate a unui Senior Java Engineer?

**Răspuns:**  
Judecata tehnică: a ști **ce să NU faci**.

---

