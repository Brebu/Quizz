# Capitolul 11 – Messaging, Kafka & Event Streaming (VERSIUNEA FINALĂ)
## Q821–Q900 — Nivel Senior

> Limbă: Română (UTF-8, diacritice corecte)  
> Scop: Interviuri Senior / Lead / Staff  
> Tip: Teorie aprofundată

---

## Q821 [Messaging][Kafka][Senior]
### Ce este messaging-ul și ce problemă rezolvă?

**Definiție:**  
Messaging-ul permite comunicarea asincronă între componente prin schimb de mesaje.

**Probleme rezolvate:**  
- decuplare  
- scalabilitate  
- toleranță la erori

---

## Q822 [Messaging][Kafka][Senior]
### Ce este event streaming?

**Răspuns:**  
Procesarea continuă a fluxurilor de evenimente.

---

## Q823 [Messaging][Kafka][Senior]
### Diferența messaging clasic vs event streaming

**Răspuns:**  
- messaging: comandă / task  
- streaming: eveniment imuabil

---

## Q824 [Messaging][Kafka][Senior]
### Ce este Apache Kafka?

**Răspuns:**  
Platformă distribuită de event streaming.

---

## Q825 [Messaging][Kafka][Senior]
### Ce probleme rezolvă Kafka?

**Răspuns:**  
- throughput mare  
- persistență  
- scalare orizontală

---

## Q826 [Messaging][Kafka][Senior]
### Ce este un broker Kafka?

**Răspuns:**  
Nod care stochează și servește mesaje.

---

## Q827 [Messaging][Kafka][Senior]
### Ce este un topic Kafka?

**Răspuns:**  
Categorie logică de evenimente.

---

## Q828 [Messaging][Kafka][Senior]
### Ce este o partiție?

**Răspuns:**  
Unitate de paralelism și ordonare.

---

## Q829 [Messaging][Kafka][Senior]
### De ce partițiile sunt critice?

**Răspuns:**  
Determină paralelismul și ordering-ul.

---

## Q830 [Messaging][Kafka][Senior]
### Ce este offset-ul?

**Răspuns:**  
Poziția unui mesaj într-o partiție.

---

## Q831 [Messaging][Kafka][Senior]
### Ce este retention policy?

**Răspuns:**  
Regula de păstrare a mesajelor.

---

## Q832 [Messaging][Kafka][Senior]
### Tipuri de retention

**Răspuns:**  
- time-based  
- size-based  
- log compaction

---

## Q833 [Messaging][Kafka][Senior]
### Ce este log compaction?

**Răspuns:**  
Păstrează ultimul mesaj per cheie.

---

## Q834 [Messaging][Kafka][Senior]
### Ce este producer-ul Kafka?

**Răspuns:**  
Componenta care publică mesaje.

---

## Q835 [Messaging][Kafka][Senior]
### Ce este consumer-ul Kafka?

**Răspuns:**  
Componenta care citește mesaje.

---

## Q836 [Messaging][Kafka][Senior]
### Ce este consumer group?

**Răspuns:**  
Grup de consumatori care împart load-ul.

---

## Q837 [Messaging][Kafka][Senior]
### Kafka garantează ordering global?

**Răspuns:**  
Nu, doar per partiție.

---

## Q838 [Messaging][Kafka][Senior]
### Ce este rebalance?

**Răspuns:**  
Redistribuirea partițiilor între consumatori.

---

## Q839 [Messaging][Kafka][Senior]
### Probleme cauzate de rebalance

**Răspuns:**  
- pauze  
- duplicate processing

---

## Q840 [Messaging][Kafka][Senior]
### Ce este consumer offset management?

**Răspuns:**  
Gestionarea poziției de citire.

---

## Q841 [Messaging][Kafka][Senior]
### Auto commit vs manual commit

**Răspuns:**  
Auto: simplu, riscant  
Manual: control fin

---

## Q842 [Messaging][Kafka][Senior]
### Ce este at-least-once delivery?

**Răspuns:**  
Mesajele pot fi procesate de mai multe ori.

---

## Q843 [Messaging][Kafka][Senior]
### Ce este at-most-once delivery?

**Răspuns:**  
Mesajele pot fi pierdute.

---

## Q844 [Messaging][Kafka][Senior]
### Ce este exactly-once semantics (EOS)?

**Răspuns:**  
Procesare fără duplicate (la nivel Kafka).

---

## Q845 [Messaging][Kafka][Senior]
### Cum obține Kafka EOS?

**Răspuns:**  
Prin idempotent producer și tranzacții.

---

## Q846 [Messaging][Kafka][Senior]
### Ce este idempotent producer?

**Răspuns:**  
Producer care evită duplicatele.

---

## Q847 [Messaging][Kafka][Senior]
### Ce sunt Kafka transactions?

**Răspuns:**  
Gruparea producer + consumer offset într-o tranzacție.

---

## Q848 [Messaging][Kafka][Senior]
### Ce este ISR (In-Sync Replicas)?

**Răspuns:**  
Replici sincronizate ale unei partiții.

---

## Q849 [Messaging][Kafka][Senior]
### Ce este replication factor?

**Răspuns:**  
Numărul de copii ale unei partiții.

---

## Q850 [Messaging][Kafka][Senior]
### Ce se întâmplă dacă leader-ul cade?

**Răspuns:**  
Un follower din ISR devine leader.

---

## Q851 [Messaging][Kafka][Senior]
### Ce este leader election?

**Răspuns:**  
Alegerea brokerului activ.

---

## Q852 [Messaging][Kafka][Senior]
### Ce este ZooKeeper și rolul lui?

**Răspuns:**  
Coordonare cluster (legacy).

---

## Q853 [Messaging][Kafka][Senior]
### Ce este KRaft?

**Răspuns:**  
Noua arhitectură Kafka fără ZooKeeper.

---

## Q854 [Messaging][Kafka][Senior]
### Ce este producer acknowledgment (acks)?

**Răspuns:**  
Confirmarea scrierii mesajelor.

---

## Q855 [Messaging][Kafka][Senior]
### Tipuri de acks

**Răspuns:**  
- 0  
- 1  
- all

---

## Q856 [Messaging][Kafka][Senior]
### Trade-off-uri acks=all

**Răspuns:**  
Siguranță mare, latență mai mare.

---

## Q857 [Messaging][Kafka][Senior]
### Ce este message key și de ce contează?

**Răspuns:**  
Determină partiția și ordering-ul.

---

## Q858 [Messaging][Kafka][Senior]
### Ce este schema registry?

**Răspuns:**  
Gestionarea schemelor de mesaje.

---

## Q859 [Messaging][Kafka][Senior]
### De ce Avro/Protobuf sunt preferate?

**Răspuns:**  
Compacte, schema evolution.

---

## Q860 [Messaging][Kafka][Senior]
### Ce este schema evolution?

**Răspuns:**  
Schimbarea compatibilă a schemelor.

---

## Q861 [Messaging][Kafka][Senior]
### Tipuri de compatibilitate

**Răspuns:**  
- backward  
- forward  
- full

---

## Q862 [Messaging][Kafka][Senior]
### Ce este dead letter topic (DLT)?

**Răspuns:**  
Topic pentru mesaje eșuate.

---

## Q863 [Messaging][Kafka][Senior]
### Ce este retry topic?

**Răspuns:**  
Topic pentru reîncercări controlate.

---

## Q864 [Messaging][Kafka][Senior]
### Ce este back-pressure în Kafka?

**Răspuns:**  
Controlul ratei de producere/consum.

---

## Q865 [Messaging][Kafka][Senior]
### Ce este lag-ul consumer-ului?

**Răspuns:**  
Diferența dintre offset-ul curent și ultimul.

---

## Q866 [Messaging][Kafka][Senior]
### De ce lag-ul mare este periculos?

**Răspuns:**  
Crește latența și riscul de timeouts.

---

## Q867 [Messaging][Kafka][Senior]
### Ce este Kafka Streams?

**Răspuns:**  
API pentru stream processing.

---

## Q868 [Messaging][Kafka][Senior]
### Kafka Streams vs Spark/Flink

**Răspuns:**  
Kafka Streams: lightweight, embedded.

---

## Q869 [Messaging][Kafka][Senior]
### Ce este state store?

**Răspuns:**  
Stare locală pentru stream processing.

---

## Q870 [Messaging][Kafka][Senior]
### Ce este windowing?

**Răspuns:**  
Procesarea evenimentelor pe ferestre de timp.

---

## Q871 [Messaging][Kafka][Senior]
### Ce este out-of-order events?

**Răspuns:**  
Evenimente sosite cu întârziere.

---

## Q872 [Messaging][Kafka][Senior]
### Ce este exactly-once în Kafka Streams?

**Răspuns:**  
Procesare tranzacțională end-to-end.

---

## Q873 [Messaging][Kafka][Senior]
### Ce este KSQL / ksqlDB?

**Răspuns:**  
SQL pentru stream-uri Kafka.

---

## Q874 [Messaging][Kafka][Senior]
### Ce este Connect API?

**Răspuns:**  
Integrare Kafka cu sisteme externe.

---

## Q875 [Messaging][Kafka][Senior]
### Ce este source vs sink connector?

**Răspuns:**  
- source: importă date  
- sink: exportă date

---

## Q876 [Messaging][Kafka][Senior]
### Ce este offset reset policy?

**Răspuns:**  
Ce face consumer-ul fără offset.

---

## Q877 [Messaging][Kafka][Senior]
### Tipuri de offset reset

**Răspuns:**  
earliest, latest.

---

## Q878 [Messaging][Kafka][Senior]
### Ce este message ordering guarantee?

**Răspuns:**  
Ordine garantată doar per partiție.

---

## Q879 [Messaging][Kafka][Senior]
### Ce este compression în Kafka?

**Răspuns:**  
Reducerea dimensiunii mesajelor.

---

## Q880 [Messaging][Kafka][Senior]
### Tipuri de compression

**Răspuns:**  
gzip, snappy, lz4, zstd.

---

## Q881 [Messaging][Kafka][Senior]
### Ce este throttling?

**Răspuns:**  
Limitarea intenționată a traficului.

---

## Q882 [Messaging][Kafka][Senior]
### Ce este quota management?

**Răspuns:**  
Limitarea resurselor per client.

---

## Q883 [Messaging][Kafka][Senior]
### Ce este multi-cluster Kafka?

**Răspuns:**  
Mai multe clustere Kafka.

---

## Q884 [Messaging][Kafka][Senior]
### Ce este MirrorMaker?

**Răspuns:**  
Replicare între clustere.

---

## Q885 [Messaging][Kafka][Senior]
### Ce este exactly-once vs effectively-once?

**Răspuns:**  
Effectively-once este mai realist.

---

## Q886 [Messaging][Kafka][Senior]
### Ce este message duplication?

**Răspuns:**  
Procesarea multiplă a aceluiași mesaj.

---

## Q887 [Messaging][Kafka][Senior]
### Strategii anti-duplicate

**Răspuns:**  
- idempotency  
- deduplication  
- transactional writes

---

## Q888 [Messaging][Kafka][Senior]
### Ce este poison message?

**Răspuns:**  
Mesaj care eșuează constant.

---

## Q889 [Messaging][Kafka][Senior]
### Ce este schema incompatibility?

**Răspuns:**  
Mesaje care nu mai pot fi deserializate.

---

## Q890 [Messaging][Kafka][Senior]
### Ce este consumer-driven lag?

**Răspuns:**  
Lag cauzat de logică lentă.

---

## Q891 [Messaging][Kafka][Senior]
### Ce este producer back-pressure?

**Răspuns:**  
Încetinirea producer-ului.

---

## Q892 [Messaging][Kafka][Senior]
### Ce este exactly-once delivery mit?

**Răspuns:**  
Perfect once este aproape imposibil.

---

## Q893 [Messaging][Kafka][Senior]
### Ce este replayability?

**Răspuns:**  
Capacitatea de a reconsuma mesaje.

---

## Q894 [Messaging][Kafka][Senior]
### Ce este data reprocessing?

**Răspuns:**  
Reprocesarea istorică a datelor.

---

## Q895 [Messaging][Kafka][Senior]
### Ce este observability în Kafka?

**Răspuns:**  
Metrici, lag, throughput.

---

## Q896 [Messaging][Kafka][Senior]
### Ce este consumer group scaling?

**Răspuns:**  
Scalare prin adăugare de consumatori.

---

## Q897 [Messaging][Kafka][Senior]
### Ce este partition skew?

**Răspuns:**  
Distribuție inegală a mesajelor.

---

## Q898 [Messaging][Kafka][Senior]
### Ce este hot partition?

**Răspuns:**  
Partiție supraîncărcată.

---

## Q899 [Messaging][Kafka][Senior]
### Ce este exactly-once processing în business?

**Răspuns:**  
Idempotency + deduplication.

---

## Q900 [Messaging][Kafka][Senior]
### Care este regula de aur în Kafka?

**Răspuns:**  
Design-ul consumer-ului este mai important decât producer-ul.

---
