# Capitolul 4 – JVM, Garbage Collection & Performance (VERSIUNEA FINALĂ)
## Q261–Q340 — Nivel Senior

> Limbă: Română (UTF-8, diacritice corecte)  
> Scop: Interviuri Senior / Lead / Staff  
> Tip: Teorie aprofundată

---

## Q261 [Java][JVM][Senior]
### Ce este JVM și care este rolul ei?

**Definiție:**  
Java Virtual Machine (JVM) este motorul de execuție care rulează bytecode-ul Java.

**Roluri principale:**  
- abstractizează hardware-ul  
- gestionează memoria  
- execută bytecode  
- oferă securitate și portabilitate

---

## Q262 [Java][JVM][Senior]
### De ce Java este considerată platform-independent?

**Răspuns:**  
Pentru că bytecode-ul este executat de JVM, nu direct de OS.

---

## Q263 [Java][JVM][Senior]
### Ce este bytecode-ul?

**Răspuns:**  
Reprezentare intermediară a codului Java compilat.

---

## Q264 [Java][JVM][Senior]
### Ce este ClassLoader și ce rol are?

**Răspuns:**  
Încarcă clasele în JVM.

**Tipuri:**  
- Bootstrap  
- Platform  
- Application

---

## Q265 [Java][JVM][Senior]
### Ce este delegation model la ClassLoader?

**Răspuns:**  
Caută clasele întâi în părinte, apoi în copil.

---

## Q266 [Java][JVM][Senior]
### Ce este Metaspace?

**Răspuns:**  
Zona de memorie pentru metadata claselor (Java 8+).

---

## Q267 [Java][JVM][Senior]
### De ce PermGen a fost eliminat?

**Răspuns:**  
Limitări rigide și OutOfMemoryError frecvente.

---

## Q268 [Java][JVM][Senior]
### Structura memoriei JVM

**Răspuns:**  
- Heap  
- Stack  
- Metaspace  
- Native Memory

---

## Q269 [Java][JVM][Senior]
### Ce este Heap-ul?

**Răspuns:**  
Zona unde sunt alocate obiectele.

---

## Q270 [Java][JVM][Senior]
### Ce este Stack-ul?

**Răspuns:**  
Zona pentru frame-uri de metode și variabile locale.

---

## Q271 [Java][JVM][Senior]
### Diferența dintre Heap și Stack

**Răspuns:**  
- Heap: obiecte, GC  
- Stack: frame-uri, thread-local

---

## Q272 [Java][JVM][Senior]
### Ce este Garbage Collection?

**Răspuns:**  
Procesul de eliberare automată a memoriei neutilizate.

---

## Q273 [Java][JVM][Senior]
### De ce este GC necesar?

**Răspuns:**  
Previne memory leaks și erori de memorie.

---

## Q274 [Java][JVM][Senior]
### Ce este reachability?

**Răspuns:**  
Un obiect este „live” dacă este accesibil din GC roots.

---

## Q275 [Java][JVM][Senior]
### Ce sunt GC Roots?

**Răspuns:**  
Referințe de pornire pentru GC.

---

## Q276 [Java][JVM][Senior]
### Tipuri de referințe în Java

**Răspuns:**  
- Strong  
- Soft  
- Weak  
- Phantom

---

## Q277 [Java][JVM][Senior]
### Diferența dintre Soft și Weak Reference

**Răspuns:**  
Soft: cache memory-sensitive  
Weak: GC agresiv

---

## Q278 [Java][JVM][Senior]
### Ce este generational GC?

**Răspuns:**  
Ipoteza că obiectele tinere mor repede.

---

## Q279 [Java][JVM][Senior]
### Ce este Young Generation?

**Răspuns:**  
Zona pentru obiecte noi.

---

## Q280 [Java][JVM][Senior]
### Ce este Old Generation?

**Răspuns:**  
Zona pentru obiecte longevive.

---

## Q281 [Java][JVM][Senior]
### Ce este Minor GC?

**Răspuns:**  
GC în Young Generation.

---

## Q282 [Java][JVM][Senior]
### Ce este Major GC / Full GC?

**Răspuns:**  
GC pe întreg heap-ul.

---

## Q283 [Java][JVM][Senior]
### De ce Full GC este periculos?

**Răspuns:**  
Produce pauze mari (stop-the-world).

---

## Q284 [Java][JVM][Senior]
### Ce este Stop-The-World?

**Răspuns:**  
Pauză în care toate thread-urile aplicației sunt oprite.

---

## Q285 [Java][JVM][Senior]
### Ce este throughput vs latency în GC?

**Răspuns:**  
- throughput: timp de execuție  
- latency: pauze

---

## Q286 [Java][JVM][Senior]
### Ce este Serial GC?

**Răspuns:**  
GC simplu, single-threaded.

---

## Q287 [Java][JVM][Senior]
### Ce este Parallel GC?

**Răspuns:**  
GC orientat pe throughput.

---

## Q288 [Java][JVM][Senior]
### Ce este CMS GC?

**Răspuns:**  
GC concurrent, orientat pe latență (deprecated).

---

## Q289 [Java][JVM][Senior]
### Ce este G1 GC?

**Răspuns:**  
GC regional, echilibru throughput/latency.

---

## Q290 [Java][JVM][Senior]
### De ce G1 este default în Java modern?

**Răspuns:**  
Oferă pauze predictibile și scalabilitate.

---

## Q291 [Java][JVM][Senior]
### Ce este ZGC?

**Răspuns:**  
GC cu pauze foarte mici (<10ms).

---

## Q292 [Java][JVM][Senior]
### Ce este Shenandoah GC?

**Răspuns:**  
GC concurrent cu relocare.

---

## Q293 [Java][JVM][Senior]
### Ce este GC tuning?

**Răspuns:**  
Ajustarea parametrilor GC.

---

## Q294 [Java][JVM][Senior]
### Ce este memory leak în Java?

**Răspuns:**  
Obiecte referențiate inutil, necolectate.

---

## Q295 [Java][JVM][Senior]
### Cauze comune de memory leak

**Răspuns:**  
- colecții statice  
- listeners neeliminați  
- cache-uri necontrolate

---

## Q296 [Java][JVM][Senior]
### Ce este OutOfMemoryError?

**Răspuns:**  
Epuizarea memoriei disponibile.

---

## Q297 [Java][JVM][Senior]
### Tipuri comune de OutOfMemoryError

**Răspuns:**  
- Java heap space  
- Metaspace  
- GC overhead limit exceeded

---

## Q298 [Java][JVM][Senior]
### Ce este StackOverflowError?

**Răspuns:**  
Depășirea stack-ului unui thread.

---

## Q299 [Java][JVM][Senior]
### Ce este JIT Compiler?

**Răspuns:**  
Compilator care optimizează codul la runtime.

---

## Q300 [Java][JVM][Senior]
### Ce este HotSpot?

**Răspuns:**  
Implementarea JVM de referință.

---

## Q301 [Java][JVM][Senior]
### Ce este tiered compilation?

**Răspuns:**  
Combină compilare interpretată și optimizată.

---

## Q302 [Java][JVM][Senior]
### Ce este escape analysis?

**Răspuns:**  
Determină dacă obiectele pot fi alocate pe stack.

---

## Q303 [Java][JVM][Senior]
### Ce este lock elision?

**Răspuns:**  
Eliminarea lock-urilor inutile.

---

## Q304 [Java][JVM][Senior]
### Ce este biased locking?

**Răspuns:**  
Optimizare pentru lock-uri necontestate.

---

## Q305 [Java][JVM][Senior]
### De ce biased locking a fost dezactivat?

**Răspuns:**  
Complexitate și beneficii reduse pe hardware modern.

---

## Q306 [Java][JVM][Senior]
### Ce este profiling?

**Răspuns:**  
Analiza performanței aplicației.

---

## Q307 [Java][JVM][Senior]
### Ce este sampling vs instrumentation?

**Răspuns:**  
- sampling: periodic  
- instrumentation: detaliat

---

## Q308 [Java][JVM][Senior]
### Ce este heap dump?

**Răspuns:**  
Snapshot al heap-ului.

---

## Q309 [Java][JVM][Senior]
### Ce este thread dump?

**Răspuns:**  
Snapshot al thread-urilor.

---

## Q310 [Java][JVM][Senior]
### Ce este JFR (Java Flight Recorder)?

**Răspuns:**  
Instrument de monitorizare JVM.

---

## Q311 [Java][JVM][Senior]
### Ce este JMC (Java Mission Control)?

**Răspuns:**  
UI pentru analiză JFR.

---

## Q312 [Java][JVM][Senior]
### Ce este warm-up time?

**Răspuns:**  
Timpul până la optimizare maximă.

---

## Q313 [Java][JVM][Senior]
### De ce benchmark-urile naive sunt înșelătoare?

**Răspuns:**  
Nu țin cont de JIT și GC.

---

## Q314 [Java][JVM][Senior]
### Ce este JMH?

**Răspuns:**  
Framework de benchmark corect.

---

## Q315 [Java][JVM][Senior]
### Ce este false sharing (recap)?

**Răspuns:**  
Contenție la nivel de cache line.

---

## Q316 [Java][JVM][Senior]
### Ce este NUMA?

**Răspuns:**  
Arhitectură cu acces neuniform la memorie.

---

## Q317 [Java][JVM][Senior]
### Impactul NUMA asupra JVM

**Răspuns:**  
Afectează latența și throughput-ul.

---

## Q318 [Java][JVM][Senior]
### Ce este compressed oops?

**Răspuns:**  
Reducerea dimensiunii referințelor.

---

## Q319 [Java][JVM][Senior]
### Ce este TLAB?

**Răspuns:**  
Thread Local Allocation Buffer.

---

## Q320 [Java][JVM][Senior]
### De ce TLAB îmbunătățește performanța?

**Răspuns:**  
Reduce sincronizarea la alocare.

---

## Q321 [Java][JVM][Senior]
### Ce este GC log?

**Răspuns:**  
Log detaliat al activității GC.

---

## Q322 [Java][JVM][Senior]
### Ce informații importante oferă GC log-ul?

**Răspuns:**  
- pauze  
- frecvență  
- reclaim rate

---

## Q323 [Java][JVM][Senior]
### Ce este memory pressure?

**Răspuns:**  
Stres ridicat pe sistemul de memorie.

---

## Q324 [Java][JVM][Senior]
### Ce este allocation rate?

**Răspuns:**  
Rata de alocare a obiectelor.

---

## Q325 [Java][JVM][Senior]
### De ce allocation rate mare este periculoasă?

**Răspuns:**  
Crește frecvența GC.

---

## Q326 [Java][JVM][Senior]
### Ce este object pooling și de ce e riscant?

**Răspuns:**  
Reutilizarea obiectelor – adesea anti-pattern.

---

## Q327 [Java][JVM][Senior]
### Ce este escape to heap?

**Răspuns:**  
Obiectele nu mai pot fi alocate pe stack.

---

## Q328 [Java][JVM][Senior]
### Ce este GC thrashing?

**Răspuns:**  
GC rulează excesiv fără progres.

---

## Q329 [Java][JVM][Senior]
### Ce este latency SLA?

**Răspuns:**  
Garanție contractuală de latență.

---

## Q330 [Java][JVM][Senior]
### Cum alegi GC-ul potrivit?

**Răspuns:**  
Pe baza SLA, workload și memorie.

---

## Q331 [Java][JVM][Senior]
### Ce este heap sizing?

**Răspuns:**  
Configurarea dimensiunii heap-ului.

---

## Q332 [Java][JVM][Senior]
### De ce Xms ≈ Xmx este recomandat?

**Răspuns:**  
Stabilitate și predictibilitate.

---

## Q333 [Java][JVM][Senior]
### Ce este metaspace leak?

**Răspuns:**  
Clase încărcate dar neeliberate.

---

## Q334 [Java][JVM][Senior]
### Cauze comune de metaspace leak

**Răspuns:**  
- classloadere custom  
- framework-uri dinamice

---

## Q335 [Java][JVM][Senior]
### Ce este permgen-style leak?

**Răspuns:**  
Leak de metadata (istoric).

---

## Q336 [Java][JVM][Senior]
### Ce este full GC trigger?

**Răspuns:**  
Eveniment care declanșează Full GC.

---

## Q337 [Java][JVM][Senior]
### Ce este safepoint?

**Răspuns:**  
Punct sigur pentru operații JVM.

---

## Q338 [Java][JVM][Senior]
### De ce safepoint-urile contează?

**Răspuns:**  
Pot introduce latență neașteptată.

---

## Q339 [Java][JVM][Senior]
### Ce este deoptimization?

**Răspuns:**  
Revenirea de la cod optimizat.

---

## Q340 [Java][JVM][Senior]
### Care este regula de aur pentru performanță JVM?

**Răspuns:**  
Măsoară înainte să optimizezi.

---
