# Capitolul 3 – Concurrency & Multithreading (VERSIUNEA FINALĂ)
## Q181–Q260 — Nivel Senior

> Limbă: Română (UTF-8, diacritice corecte)  
> Scop: Interviuri Senior / Lead / Staff  
> Tip: Teorie aprofundată

---

## Q181 [Java][Concurrency][Senior]
### Ce este concurența și de ce este dificilă?

**Definiție:**  
Concurența reprezintă execuția mai multor fluxuri de control în același timp sau aparent simultan.

**De ce este dificilă:**  
- interleaving imprevizibil  
- bug-uri greu de reprodus  
- dependență de arhitectura hardware

---

## Q182 [Java][Concurrency][Senior]
### Diferența dintre concurență și paralelism

**Răspuns:**  
- Concurență: structură logică  
- Paralelism: execuție efectiv simultană

---

## Q183 [Java][Concurrency][Senior]
### Ce este un thread în Java?

**Răspuns:**  
Un thread este cea mai mică unitate de execuție gestionată de JVM.

---

## Q184 [Java][Concurrency][Senior]
### Lifecycle-ul unui thread

**Răspuns:**  
New → Runnable → Running → Blocked/Waiting → Terminated

---

## Q185 [Java][Concurrency][Senior]
### De ce NU este recomandat `new Thread()`?

**Răspuns:**  
- cost mare de creare  
- lipsă control  
- scalabilitate slabă

---

## Q186 [Java][Concurrency][Senior]
### Ce este `Runnable` vs `Callable`?

**Răspuns:**  
- Runnable: fără rezultat  
- Callable: returnează rezultat și aruncă excepții

---

## Q187 [Java][Concurrency][Senior]
### Ce este `ExecutorService`?

**Răspuns:**  
Abstracție pentru managementul thread-urilor.

---

## Q188 [Java][Concurrency][Senior]
### Tipuri de thread pools în Java

**Răspuns:**  
- Fixed  
- Cached  
- Single  
- Scheduled

---

## Q189 [Java][Concurrency][Senior]
### Ce este work-stealing pool?

**Răspuns:**  
Pool unde thread-urile „fură” task-uri pentru load balancing.

---

## Q190 [Java][Concurrency][Senior]
### Ce este `ForkJoinPool`?

**Răspuns:**  
Implementare pentru task-uri recursive și paralele.

---

## Q191 [Java][Concurrency][Senior]
### Ce este race condition?

**Răspuns:**  
Acces concurent necontrolat asupra stării partajate.

---

## Q192 [Java][Concurrency][Senior]
### Ce este critical section?

**Răspuns:**  
Cod care accesează resurse partajate.

---

## Q193 [Java][Concurrency][Senior]
### Ce este `synchronized`?

**Răspuns:**  
Mecanism de excludere mutuală și barieră de memorie.

---

## Q194 [Java][Concurrency][Senior]
### Diferența dintre synchronized method și block

**Răspuns:**  
- method: lock pe `this`  
- block: lock explicit

---

## Q195 [Java][Concurrency][Senior]
### Ce este monitor lock?

**Răspuns:**  
Lock asociat fiecărui obiect Java.

---

## Q196 [Java][Concurrency][Senior]
### Ce este deadlock?

**Răspuns:**  
Blocare circulară între thread-uri.

---

## Q197 [Java][Concurrency][Senior]
### Condiții necesare pentru deadlock

**Răspuns:**  
- mutual exclusion  
- hold and wait  
- no preemption  
- circular wait

---

## Q198 [Java][Concurrency][Senior]
### Ce este livelock?

**Răspuns:**  
Thread-uri active fără progres.

---

## Q199 [Java][Concurrency][Senior]
### Ce este starvation?

**Răspuns:**  
Un thread nu primește resurse.

---

## Q200 [Java][Concurrency][Senior]
### Ce este fairness în locking?

**Răspuns:**  
Distribuirea echitabilă a lock-urilor.

---

## Q201 [Java][Concurrency][Senior]
### Ce este `volatile`?

**Răspuns:**  
Asigură vizibilitate și ordonare, nu atomicitate.

---

## Q202 [Java][Concurrency][Senior]
### Ce este Java Memory Model (JMM)?

**Răspuns:**  
Definește regulile de vizibilitate între thread-uri.

---

## Q203 [Java][Concurrency][Senior]
### Ce este happens-before?

**Răspuns:**  
Relație de vizibilitate garantată.

---

## Q204 [Java][Concurrency][Senior]
### Ce este atomicitatea?

**Răspuns:**  
Operație indivizibilă din punct de vedere concurent.

---

## Q205 [Java][Concurrency][Senior]
### Ce sunt clasele Atomic?

**Răspuns:**  
Clase lock-free pentru operații atomice.

---

## Q206 [Java][Concurrency][Senior]
### Diferența dintre lock-free și wait-free

**Răspuns:**  
- lock-free: progres garantat global  
- wait-free: progres garantat per thread

---

## Q207 [Java][Concurrency][Senior]
### Ce este `ReentrantLock`?

**Răspuns:**  
Lock explicit cu funcționalități avansate.

---

## Q208 [Java][Concurrency][Senior]
### ReentrantLock vs synchronized

**Răspuns:**  
- control explicit  
- timeout  
- fairness

---

## Q209 [Java][Concurrency][Senior]
### Ce este `ReadWriteLock`?

**Răspuns:**  
Permite citiri concurente.

---

## Q210 [Java][Concurrency][Senior]
### Ce este `StampedLock`?

**Răspuns:**  
Lock performant cu optimist read.

---

## Q211 [Java][Concurrency][Senior]
### Ce este `CountDownLatch`?

**Răspuns:**  
Sincronizare unidirecțională.

---

## Q212 [Java][Concurrency][Senior]
### Ce este `CyclicBarrier`?

**Răspuns:**  
Sincronizare reutilizabilă.

---

## Q213 [Java][Concurrency][Senior]
### Ce este `Semaphore`?

**Răspuns:**  
Controlează accesul la resurse limitate.

---

## Q214 [Java][Concurrency][Senior]
### Ce este `CompletableFuture`?

**Răspuns:**  
API asincron bazat pe futures compozabile.

---

## Q215 [Java][Concurrency][Senior]
### Avantajele CompletableFuture

**Răspuns:**  
- compoziție  
- non-blocking  
- pipeline async

---

## Q216 [Java][Concurrency][Senior]
### Ce este blocking vs non-blocking?

**Răspuns:**  
- blocking: thread-ul așteaptă  
- non-blocking: continuă execuția

---

## Q217 [Java][Concurrency][Senior]
### Ce este busy waiting?

**Răspuns:**  
Așteptare activă, consumatoare de CPU.

---

## Q218 [Java][Concurrency][Senior]
### Ce este thread-safety?

**Răspuns:**  
Corectitudine sub acces concurent.

---

## Q219 [Java][Concurrency][Senior]
### Ce este immutabilitatea în concurență?

**Răspuns:**  
Strategie principală pentru thread-safety.

---

## Q220 [Java][Concurrency][Senior]
### Ce este safe publication?

**Răspuns:**  
Publicarea corectă a obiectelor.

---

## Q221 [Java][Concurrency][Senior]
### Ce este ThreadLocal?

**Răspuns:**  
Stocare izolată per thread.

---

## Q222 [Java][Concurrency][Senior]
### Riscurile ThreadLocal

**Răspuns:**  
- memory leaks  
- probleme în thread pools

---

## Q223 [Java][Concurrency][Senior]
### Ce este false sharing?

**Răspuns:**  
Contenție la nivel de cache line.

---

## Q224 [Java][Concurrency][Senior]
### Ce este cache coherence?

**Răspuns:**  
Menținerea consistenței cache-urilor CPU.

---

## Q225 [Java][Concurrency][Senior]
### Ce este context switching?

**Răspuns:**  
Schimbarea thread-ului activ.

---

## Q226 [Java][Concurrency][Senior]
### Costul context switching

**Răspuns:**  
Pierdere de timp CPU și cache.

---

## Q227 [Java][Concurrency][Senior]
### Ce este parallel stream?

**Răspuns:**  
Stream procesat în paralel.

---

## Q228 [Java][Concurrency][Senior]
### Când NU folosești parallel streams?

**Răspuns:**  
- task-uri mici  
- I/O  
- ThreadLocal

---

## Q229 [Java][Concurrency][Senior]
### Ce este back-pressure?

**Răspuns:**  
Controlul fluxului pentru a evita supraîncărcarea.

---

## Q230 [Java][Concurrency][Senior]
### Ce este actor model?

**Răspuns:**  
Model concurent bazat pe mesaje.

---

## Q231 [Java][Concurrency][Senior]
### Ce este reactive programming?

**Răspuns:**  
Programare asincronă orientată pe fluxuri.

---

## Q232 [Java][Concurrency][Senior]
### Diferența dintre reactive și imperative

**Răspuns:**  
- imperative: control direct  
- reactive: fluxuri declarative

---

## Q233 [Java][Concurrency][Senior]
### Ce este event loop?

**Răspuns:**  
Procesare evenimente într-un singur thread.

---

## Q234 [Java][Concurrency][Senior]
### Ce este thread-per-request model?

**Răspuns:**  
Model clasic, limitat în scalabilitate.

---

## Q235 [Java][Concurrency][Senior]
### Ce este async I/O?

**Răspuns:**  
Operații I/O non-blocante.

---

## Q236 [Java][Concurrency][Senior]
### Ce este scalability?

**Răspuns:**  
Capacitatea de a gestiona creșterea sarcinii.

---

## Q237 [Java][Concurrency][Senior]
### Ce este throughput?

**Răspuns:**  
Număr de operații pe unitate de timp.

---

## Q238 [Java][Concurrency][Senior]
### Ce este latency?

**Răspuns:**  
Timpul de răspuns.

---

## Q239 [Java][Concurrency][Senior]
### Ce este tail latency?

**Răspuns:**  
Latență în cazurile extreme.

---

## Q240 [Java][Concurrency][Senior]
### Ce este Amdahl’s Law?

**Răspuns:**  
Limita teoretică a paralelizării.

---

## Q241 [Java][Concurrency][Senior]
### Ce este Little’s Law?

**Răspuns:**  
Relația dintre throughput, latență și load.

---

## Q242 [Java][Concurrency][Senior]
### Ce este contention?

**Răspuns:**  
Competiție pentru resurse.

---

## Q243 [Java][Concurrency][Senior]
### Ce este lock contention?

**Răspuns:**  
Blocare frecventă pe lock-uri.

---

## Q244 [Java][Concurrency][Senior]
### Ce este priority inversion?

**Răspuns:**  
Thread cu prioritate mare blocat de unul mic.

---

## Q245 [Java][Concurrency][Senior]
### Ce este cooperative multitasking?

**Răspuns:**  
Thread-urile cedează voluntar execuția.

---

## Q246 [Java][Concurrency][Senior]
### Ce este preemptive multitasking?

**Răspuns:**  
Scheduler-ul decide schimbarea.

---

## Q247 [Java][Concurrency][Senior]
### Ce este spinlock?

**Răspuns:**  
Lock bazat pe busy waiting.

---

## Q248 [Java][Concurrency][Senior]
### Când este util un spinlock?

**Răspuns:**  
Pentru secțiuni critice foarte scurte.

---

## Q249 [Java][Concurrency][Senior]
### Ce este memory visibility bug?

**Răspuns:**  
Thread-urile văd valori stale.

---

## Q250 [Java][Concurrency][Senior]
### Ce este instruction reordering?

**Răspuns:**  
Rearanjarea operațiilor pentru optimizare.

---

## Q251 [Java][Concurrency][Senior]
### Ce este happens-before violation?

**Răspuns:**  
Lipsa garantării vizibilității.

---

## Q252 [Java][Concurrency][Senior]
### Ce este double-checked locking?

**Răspuns:**  
Pattern pentru inițializare lazy.

---

## Q253 [Java][Concurrency][Senior]
### De ce double-checked locking era problematic?

**Răspuns:**  
Probleme JMM înainte de Java 5.

---

## Q254 [Java][Concurrency][Senior]
### Ce este initialization-on-demand holder idiom?

**Răspuns:**  
Pattern sigur pentru singleton.

---

## Q255 [Java][Concurrency][Senior]
### Ce este thread dump?

**Răspuns:**  
Snapshot al stării thread-urilor.

---

## Q256 [Java][Concurrency][Senior]
### Ce este deadlock detection?

**Răspuns:**  
Identificarea blocajelor.

---

## Q257 [Java][Concurrency][Senior]
### Ce este starvation detection?

**Răspuns:**  
Identificarea thread-urilor blocate pe termen lung.

---

## Q258 [Java][Concurrency][Senior]
### Ce este concurrency testing?

**Răspuns:**  
Testarea comportamentului sub concurență.

---

## Q259 [Java][Concurrency][Senior]
### Ce este stress testing?

**Răspuns:**  
Testarea sub încărcare extremă.

---

## Q260 [Java][Concurrency][Senior]
### Care este regula de aur în concurență?

**Răspuns:**  
Evită starea partajată dacă este posibil.

---
