# Capitolul 2 – Collections Framework (VERSIUNEA FINALĂ)
## Q121–Q200 — Nivel Senior

> Limbă: Română (UTF-8, diacritice corecte)  
> Scop: Interviuri Senior / Lead / Staff  
> Tip: Teorie aprofundată

---

## Q121 [Java][Collections][Senior]
### Ce este Java Collections Framework și ce problemă rezolvă?

**Definiție:**  
Java Collections Framework (JCF) este un set de interfețe și implementări standard pentru gestionarea colecțiilor de obiecte.

**De ce contează:**  
- oferă API unificat și consistent  
- separă contractul de implementare  
- permite schimbarea implementării fără impact major  
- este extrem de optimizat și testat

**Capcană:**  
Folosirea colecțiilor fără a înțelege complexitatea operațiilor.

---

## Q122 [Java][Collections][Senior]
### Care sunt principalele interfețe din Collections Framework?

**Răspuns:**  
- `Collection`  
- `List`  
- `Set`  
- `Queue` / `Deque`  
- `Map` (nu extinde `Collection`)

**De ce `Map` este separată:**  
Pentru că gestionează perechi cheie–valoare, nu elemente simple.

---

## Q123 [Java][Collections][Senior]
### Diferența fundamentală dintre `List`, `Set` și `Map`

**Răspuns:**  
- `List`: elemente ordonate, duplicate permise  
- `Set`: elemente unice  
- `Map`: chei unice → valori

---

## Q124 [Java][Collections][Senior]
### Ce este `ArrayList` și când este potrivit?

**Răspuns:**  
Implementare bazată pe array dinamic.

**Potrivit când:**  
- acces frecvent prin index  
- iterație intensivă  
- inserări rare în mijloc

---

## Q125 [Java][Collections][Senior]
### Care sunt costurile operațiilor în `ArrayList`?

**Răspuns:**  
- get/set: O(1)  
- add la final: amortizat O(1)  
- insert/remove la mijloc: O(n)

---

## Q126 [Java][Collections][Senior]
### Ce este `LinkedList` și când este util?

**Răspuns:**  
Listă dublu înlănțuită.

**Util când:**  
- inserări/ștergeri frecvente  
- operații de tip deque

**Capcană:**  
Accesul prin index este O(n).

---

## Q127 [Java][Collections][Senior]
### De ce `LinkedList` este rar alegerea corectă?

**Răspuns:**  
- consum mare de memorie  
- cache locality slab  
- performanță mai slabă în practică decât `ArrayList`

---

## Q128 [Java][Collections][Senior]
### Ce este `Vector` și de ce este evitat?

**Răspuns:**  
List sincronizată implicit.

**Probleme:**  
- sincronizare grosieră  
- performanță slabă  
- API legacy

---

## Q129 [Java][Collections][Senior]
### Ce este `CopyOnWriteArrayList`?

**Răspuns:**  
List thread-safe care copiază array-ul la fiecare modificare.

**Potrivit când:**  
- multe citiri  
- puține scrieri

---

## Q130 [Java][Collections][Senior]
### Ce este `Set` și ce garantează?

**Răspuns:**  
Colecție fără duplicate.

**Unicitatea este definită de:**  
`equals()` și `hashCode()`.

---

## Q131 [Java][Collections][Senior]
### Ce este `HashSet`?

**Răspuns:**  
Set bazat pe `HashMap`.

**Caracteristici:**  
- ordine negarantată  
- performanță O(1) amortizat

---

## Q132 [Java][Collections][Senior]
### Ce este `LinkedHashSet`?

**Răspuns:**  
HashSet cu ordine de inserare păstrată.

---

## Q133 [Java][Collections][Senior]
### Ce este `TreeSet`?

**Răspuns:**  
Set sortat bazat pe arbore roșu-negru.

**Cerință:**  
Elementele trebuie să fie comparabile.

---

## Q134 [Java][Collections][Senior]
### Diferența dintre `HashSet` și `TreeSet`

**Răspuns:**  
- `HashSet`: rapid, nesortat  
- `TreeSet`: sortat, mai lent (O(log n))

---

## Q135 [Java][Collections][Senior]
### Ce este `Map` și ce garantează?

**Răspuns:**  
Asociază chei unice cu valori.

**Cheile sunt unice conform:**  
`equals()` + `hashCode()`.

---

## Q136 [Java][Collections][Senior]
### Ce este `HashMap`?

**Răspuns:**  
Implementare de `Map` bazată pe hashing.

**Performanță:**  
O(1) amortizat pentru get/put.

---

## Q137 [Java][Collections][Senior]
### Cum funcționează intern `HashMap`?

**Răspuns:**  
- hashCode → bucket  
- coliziuni rezolvate prin chaining  
- din Java 8: tree bins pentru coliziuni mari

---

## Q138 [Java][Collections][Senior]
### Ce este hash collision și cum o gestionează HashMap?

**Răspuns:**  
Coliziune când două chei au același hash.

**Gestionare:**  
- listă în bucket  
- arbore dacă depășește pragul

---

## Q139 [Java][Collections][Senior]
### Ce este load factor?

**Răspuns:**  
Raportul dintre dimensiune și capacitate.

**Implicit:**  
0.75 – compromis bun între memorie și performanță.

---

## Q140 [Java][Collections][Senior]
### Ce este rehashing?

**Răspuns:**  
Redimensionarea și redistribuirea bucket-urilor.

**Cost:**  
Operație scumpă (O(n)).

---

## Q141 [Java][Collections][Senior]
### De ce `HashMap` permite o cheie `null`?

**Răspuns:**  
Din considerente istorice și de compatibilitate.

---

## Q142 [Java][Collections][Senior]
### Ce este `Hashtable` și de ce nu se mai folosește?

**Răspuns:**  
Map sincronizat legacy.

**Probleme:**  
- sincronizare globală  
- performanță slabă

---

## Q143 [Java][Collections][Senior]
### Ce este `ConcurrentHashMap`?

**Răspuns:**  
Map thread-safe cu blocare granulară.

**Avantaj:**  
Scalabilitate mult mai bună decât `Hashtable`.

---

## Q144 [Java][Collections][Senior]
### De ce `ConcurrentHashMap` nu permite `null`?

**Răspuns:**  
Pentru a evita ambiguitatea între „absent” și „valoare null”.

---

## Q145 [Java][Collections][Senior]
### Ce este `WeakHashMap`?

**Răspuns:**  
Map cu chei slabe (garbage collected).

**Util când:**  
Cache-uri temporare.

---

## Q146 [Java][Collections][Senior]
### Ce este `IdentityHashMap`?

**Răspuns:**  
Compară cheile prin `==`, nu `equals()`.

**Capcană:**  
Comportament surprinzător.

---

## Q147 [Java][Collections][Senior]
### Ce este `EnumMap`?

**Răspuns:**  
Map optimizat pentru chei enum.

---

## Q148 [Java][Collections][Senior]
### Ce este `Queue` și ce problemă rezolvă?

**Răspuns:**  
Colecție pentru procesare ordonată (FIFO).

---

## Q149 [Java][Collections][Senior]
### Ce este `Deque`?

**Răspuns:**  
Coada dublu terminată (double-ended queue).

---

## Q150 [Java][Collections][Senior]
### Ce este `ArrayDeque` și de ce este preferat?

**Răspuns:**  
Deque performant, fără overhead de sincronizare.

---

## Q151 [Java][Collections][Senior]
### Ce este `PriorityQueue`?

**Răspuns:**  
Coada bazată pe heap.

**Nu garantează ordine totală la iterație.**

---

## Q152 [Java][Collections][Senior]
### Ce este fail-fast iterator?

**Răspuns:**  
Iterator care aruncă excepție la modificări structurale.

---

## Q153 [Java][Collections][Senior]
### Ce este fail-safe iterator?

**Răspuns:**  
Iterator care lucrează pe o copie.

---

## Q154 [Java][Collections][Senior]
### Diferența dintre fail-fast și fail-safe

**Răspuns:**  
- fail-fast: detectează rapid erori  
- fail-safe: cost mai mare de memorie

---

## Q155 [Java][Collections][Senior]
### Ce este `Collections.synchronizedList()`?

**Răspuns:**  
Wrapper sincronizat.

**Capcană:**  
Iterarea trebuie sincronizată manual.

---

## Q156 [Java][Collections][Senior]
### Ce este `Collections.unmodifiableList()`?

**Răspuns:**  
Wrapper read-only.

---

## Q157 [Java][Collections][Senior]
### De ce unmodifiable ≠ immutable?

**Răspuns:**  
Starea internă poate fi modificată din exterior.

---

## Q158 [Java][Collections][Senior]
### Ce sunt colecțiile imutabile (Java 9+)?

**Răspuns:**  
Colecții create prin `List.of()`, `Set.of()`.

---

## Q159 [Java][Collections][Senior]
### Avantajele colecțiilor imutabile

**Răspuns:**  
- thread-safety  
- simplitate  
- performanță predictibilă

---

## Q160 [Java][Collections][Senior]
### Ce este `Spliterator`?

**Răspuns:**  
Iterator avansat pentru procesare paralelă.

---

## Q161 [Java][Collections][Senior]
### Ce este `Iterable` vs `Iterator`?

**Răspuns:**  
- Iterable: furnizează iterator  
- Iterator: parcurge elementele

---

## Q162 [Java][Collections][Senior]
### Ce este `Comparable` vs `Comparator` (recap)?

**Răspuns:**  
- natural vs extern  
- coupling vs decoupling

---

## Q163 [Java][Collections][Senior]
### Ce este stable sort?

**Răspuns:**  
Sortare care păstrează ordinea relativă.

---

## Q164 [Java][Collections][Senior]
### Ce algoritm de sortare folosește Java?

**Răspuns:**  
- TimSort pentru obiecte  
- Dual-Pivot Quicksort pentru primitive

---

## Q165 [Java][Collections][Senior]
### Ce este Big-O și de ce contează în colecții?

**Răspuns:**  
Descrie complexitatea operațiilor.

---

## Q166 [Java][Collections][Senior]
### De ce `List.contains()` este O(n)?

**Răspuns:**  
Pentru că trebuie să parcurgă lista.

---

## Q167 [Java][Collections][Senior]
### De ce `Set.contains()` este O(1) amortizat?

**Răspuns:**  
Pentru că folosește hashing.

---

## Q168 [Java][Collections][Senior]
### Ce este memory footprint?

**Răspuns:**  
Cantitatea de memorie consumată.

---

## Q169 [Java][Collections][Senior]
### Cum alegi colecția corectă?

**Răspuns:**  
Pe baza:
- operațiilor dominante  
- volumului de date  
- concurenței

---

## Q170 [Java][Collections][Senior]
### Ce este anti-pattern-ul „premature optimization”?

**Răspuns:**  
Optimizare fără date reale.

---

## Q171 [Java][Collections][Senior]
### Ce este auto-resizing în colecții?

**Răspuns:**  
Creșterea capacității interne.

---

## Q172 [Java][Collections][Senior]
### De ce este bine să setezi capacitatea inițială?

**Răspuns:**  
Evită rehashing-ul costisitor.

---

## Q173 [Java][Collections][Senior]
### Ce este `Collections.emptyList()`?

**Răspuns:**  
Listă imutabilă goală.

---

## Q174 [Java][Collections][Senior]
### Ce este `Collections.singletonList()`?

**Răspuns:**  
Listă imutabilă cu un element.

---

## Q175 [Java][Collections][Senior]
### Ce este boxing în colecții?

**Răspuns:**  
Conversia primitivelor în obiecte.

---

## Q176 [Java][Collections][Senior]
### Impactul boxing-ului asupra performanței

**Răspuns:**  
- overhead CPU  
- consum de memorie

---

## Q177 [Java][Collections][Senior]
### Ce este escape analysis?

**Răspuns:**  
Optimizare JVM pentru alocări.

---

## Q178 [Java][Collections][Senior]
### Ce este locality of reference?

**Răspuns:**  
Accesarea datelor apropiate în memorie.

---

## Q179 [Java][Collections][Senior]
### De ce `ArrayList` profită de locality?

**Răspuns:**  
Datele sunt contigue în memorie.

---

## Q180 [Java][Collections][Senior]
### Care este cea mai frecventă greșeală la colecții?

**Răspuns:**  
Alegerea colecției fără a înțelege pattern-ul de acces.

---
