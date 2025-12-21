# Capitolul 7 – Spring MVC & REST (VERSIUNEA FINALĂ)
## Q501–Q580 — Nivel Senior

> Limbă: Română (UTF-8, diacritice corecte)  
> Scop: Interviuri Senior / Lead / Staff  
> Tip: Teorie aprofundată

---

## Q501 [Spring][MVC][REST][Senior]
### Ce este Spring MVC și ce rol are în arhitectura unei aplicații?

**Definiție:**  
Spring MVC este framework-ul web din ecosistemul Spring care implementează pattern-ul Model–View–Controller.

**Rol:**  
- gestionează request/response HTTP  
- mapează URL-uri la controllere  
- separă responsabilitățile

---

## Q502 [Spring][MVC][REST][Senior]
### Ce este DispatcherServlet?

**Răspuns:**  
Front Controller-ul central al Spring MVC care orchestrează procesarea cererilor.

---

## Q503 [Spring][MVC][REST][Senior]
### Fluxul unei cereri HTTP în Spring MVC

**Răspuns:**  
Client → DispatcherServlet → HandlerMapping → Controller → ViewResolver → Response

---

## Q504 [Spring][MVC][REST][Senior]
### Ce este HandlerMapping?

**Răspuns:**  
Componenta care decide ce controller gestionează request-ul.

---

## Q505 [Spring][MVC][REST][Senior]
### Ce este HandlerAdapter?

**Răspuns:**  
Permite DispatcherServlet-ului să invoce controller-ul.

---

## Q506 [Spring][MVC][REST][Senior]
### Ce este un Controller în Spring MVC?

**Răspuns:**  
Componentă care procesează cereri web și returnează răspunsuri.

---

## Q507 [Spring][MVC][REST][Senior]
### Diferența dintre `@Controller` și `@RestController`

**Răspuns:**  
- `@Controller`: returnează view-uri  
- `@RestController`: returnează body JSON/XML

---

## Q508 [Spring][MVC][REST][Senior]
### Ce este `@RequestMapping`?

**Răspuns:**  
Mapează cereri HTTP la metodele controller.

---

## Q509 [Spring][MVC][REST][Senior]
### Diferența dintre `@GetMapping`, `@PostMapping`, etc.

**Răspuns:**  
Specializări semantice pentru metode HTTP.

---

## Q510 [Spring][MVC][REST][Senior]
### Ce este content negotiation?

**Răspuns:**  
Selectarea formatului de răspuns pe baza headerelor.

---

## Q511 [Spring][MVC][REST][Senior]
### Ce rol are header-ul `Accept`?

**Răspuns:**  
Indică formatele acceptate de client.

---

## Q512 [Spring][MVC][REST][Senior]
### Ce este `HttpMessageConverter`?

**Răspuns:**  
Convertește obiecte Java în payload HTTP și invers.

---

## Q513 [Spring][MVC][REST][Senior]
### Ce este Jackson și ce rol are?

**Răspuns:**  
Bibliotecă pentru serializare/deserializare JSON.

---

## Q514 [Spring][MVC][REST][Senior]
### Ce este `@RequestBody`?

**Răspuns:**  
Mapează body-ul cererii pe un obiect Java.

---

## Q515 [Spring][MVC][REST][Senior]
### Ce este `@ResponseBody`?

**Răspuns:**  
Scrie direct obiectul în body-ul răspunsului.

---

## Q516 [Spring][MVC][REST][Senior]
### Ce este `@PathVariable`?

**Răspuns:**  
Extrage valori din URL.

---

## Q517 [Spring][MVC][REST][Senior]
### Ce este `@RequestParam`?

**Răspuns:**  
Extrage parametri din query string.

---

## Q518 [Spring][MVC][REST][Senior]
### Diferența `@PathVariable` vs `@RequestParam`

**Răspuns:**  
- Path: parte din resursă  
- Param: filtru/opțiune

---

## Q519 [Spring][MVC][REST][Senior]
### Ce este `@RequestHeader`?

**Răspuns:**  
Accesează headere HTTP.

---

## Q520 [Spring][MVC][REST][Senior]
### Ce este `@CookieValue`?

**Răspuns:**  
Accesează cookie-uri.

---

## Q521 [Spring][MVC][REST][Senior]
### Ce este `@ModelAttribute`?

**Răspuns:**  
Leagă datele request-ului la obiecte.

---

## Q522 [Spring][MVC][REST][Senior]
### Ce este data binding?

**Răspuns:**  
Maparea automată a inputului HTTP la obiecte Java.

---

## Q523 [Spring][MVC][REST][Senior]
### Ce este validarea în Spring MVC?

**Răspuns:**  
Verificarea datelor de intrare.

---

## Q524 [Spring][MVC][REST][Senior]
### Ce este Bean Validation (JSR 380)?

**Răspuns:**  
Standard pentru validarea datelor.

---

## Q525 [Spring][MVC][REST][Senior]
### Rolul anotărilor `@Valid` și `@Validated`

**Răspuns:**  
Declanșează validarea obiectelor.

---

## Q526 [Spring][MVC][REST][Senior]
### Ce este `BindingResult`?

**Răspuns:**  
Conține erori de validare.

---

## Q527 [Spring][MVC][REST][Senior]
### Ce se întâmplă dacă lipsește `BindingResult`?

**Răspuns:**  
Se aruncă excepție.

---

## Q528 [Spring][MVC][REST][Senior]
### Ce este `@ExceptionHandler`?

**Răspuns:**  
Gestionează excepții la nivel de controller.

---

## Q529 [Spring][MVC][REST][Senior]
### Ce este `@ControllerAdvice`?

**Răspuns:**  
Gestionează excepții global.

---

## Q530 [Spring][MVC][REST][Senior]
### Diferența `@ExceptionHandler` vs `@ControllerAdvice`

**Răspuns:**  
Local vs global.

---

## Q531 [Spring][MVC][REST][Senior]
### Ce este `ResponseEntity`?

**Răspuns:**  
Wrapper complet pentru răspuns HTTP.

---

## Q532 [Spring][MVC][REST][Senior]
### De ce este util `ResponseEntity`?

**Răspuns:**  
Control complet asupra statusului și headerelor.

---

## Q533 [Spring][MVC][REST][Senior]
### Ce este HTTP status code?

**Răspuns:**  
Cod numeric care descrie rezultatul cererii.

---

## Q534 [Spring][MVC][REST][Senior]
### Categorii de status codes

**Răspuns:**  
- 2xx: succes  
- 4xx: client error  
- 5xx: server error

---

## Q535 [Spring][MVC][REST][Senior]
### Ce este idempotency?

**Răspuns:**  
Operație repetabilă fără efecte secundare.

---

## Q536 [Spring][MVC][REST][Senior]
### Ce metode HTTP sunt idempotente?

**Răspuns:**  
GET, PUT, DELETE.

---

## Q537 [Spring][MVC][REST][Senior]
### Ce este REST?

**Răspuns:**  
Stil arhitectural bazat pe resurse.

---

## Q538 [Spring][MVC][REST][Senior]
### Principiile REST

**Răspuns:**  
- stateless  
- cacheable  
- uniform interface

---

## Q539 [Spring][MVC][REST][Senior]
### Ce este o resursă REST?

**Răspuns:**  
Entitate identificată prin URI.

---

## Q540 [Spring][MVC][REST][Senior]
### Ce este HATEOAS?

**Răspuns:**  
Navigare prin link-uri hypermedia.

---

## Q541 [Spring][MVC][REST][Senior]
### Ce este pagination?

**Răspuns:**  
Împărțirea rezultatelor în pagini.

---

## Q542 [Spring][MVC][REST][Senior]
### Ce este sorting și filtering?

**Răspuns:**  
Mecanisme de control al datelor returnate.

---

## Q543 [Spring][MVC][REST][Senior]
### Ce este versionarea API-urilor?

**Răspuns:**  
Gestionarea schimbărilor incompatibile.

---

## Q544 [Spring][MVC][REST][Senior]
### Metode de versionare API

**Răspuns:**  
- URI  
- header  
- parametru

---

## Q545 [Spring][MVC][REST][Senior]
### Ce este backward compatibility în API-uri?

**Răspuns:**  
Menținerea suportului pentru clienți vechi.

---

## Q546 [Spring][MVC][REST][Senior]
### Ce este contract-first vs code-first?

**Răspuns:**  
- contract-first: OpenAPI  
- code-first: controller-driven

---

## Q547 [Spring][MVC][REST][Senior]
### Ce este OpenAPI / Swagger?

**Răspuns:**  
Specificație pentru documentare API.

---

## Q548 [Spring][MVC][REST][Senior]
### De ce documentarea API este critică?

**Răspuns:**  
Asigură adoptare și mentenabilitate.

---

## Q549 [Spring][MVC][REST][Senior]
### Ce este DTO și de ce este necesar?

**Răspuns:**  
Obiect de transfer pentru decuplare.

---

## Q550 [Spring][MVC][REST][Senior]
### Ce este mapping între entități și DTO-uri?

**Răspuns:**  
Transformarea modelelor interne.

---

## Q551 [Spring][MVC][REST][Senior]
### Ce este MapStruct?

**Răspuns:**  
Bibliotecă pentru mapping compile-time.

---

## Q552 [Spring][MVC][REST][Senior]
### Ce este lazy loading în serializare?

**Răspuns:**  
Încărcare tardivă a relațiilor.

---

## Q553 [Spring][MVC][REST][Senior]
### Probleme cu Jackson și lazy loading

**Răspuns:**  
LazyInitializationException.

---

## Q554 [Spring][MVC][REST][Senior]
### Ce este `@JsonIgnore`?

**Răspuns:**  
Exclude câmpuri din serializare.

---

## Q555 [Spring][MVC][REST][Senior]
### Ce este `@JsonView`?

**Răspuns:**  
Controlează vizibilitatea câmpurilor.

---

## Q556 [Spring][MVC][REST][Senior]
### Ce este CORS?

**Răspuns:**  
Politică de securitate cross-origin.

---

## Q557 [Spring][MVC][REST][Senior]
### Cum configurezi CORS în Spring?

**Răspuns:**  
Prin `@CrossOrigin` sau config global.

---

## Q558 [Spring][MVC][REST][Senior]
### Ce este CSRF?

**Răspuns:**  
Atac prin cereri falsificate.

---

## Q559 [Spring][MVC][REST][Senior]
### CSRF și REST APIs

**Răspuns:**  
Dezactivat de obicei pentru stateless APIs.

---

## Q560 [Spring][MVC][REST][Senior]
### Ce este statelessness în REST?

**Răspuns:**  
Serverul nu păstrează stare client.

---

## Q561 [Spring][MVC][REST][Senior]
### Ce este caching HTTP?

**Răspuns:**  
Stocarea răspunsurilor pentru performanță.

---

## Q562 [Spring][MVC][REST][Senior]
### Header-e de caching importante

**Răspuns:**  
Cache-Control, ETag.

---

## Q563 [Spring][MVC][REST][Senior]
### Ce este ETag?

**Răspuns:**  
Identificator de versiune a resursei.

---

## Q564 [Spring][MVC][REST][Senior]
### Ce este conditional GET?

**Răspuns:**  
GET bazat pe ETag/If-None-Match.

---

## Q565 [Spring][MVC][REST][Senior]
### Ce este rate limiting?

**Răspuns:**  
Limitarea cererilor.

---

## Q566 [Spring][MVC][REST][Senior]
### Ce este API Gateway pattern?

**Răspuns:**  
Punct unic de intrare.

---

## Q567 [Spring][MVC][REST][Senior]
### Ce este backend-for-frontend (BFF)?

**Răspuns:**  
Backend dedicat unui client.

---

## Q568 [Spring][MVC][REST][Senior]
### Ce este contract testing?

**Răspuns:**  
Testarea contractului API.

---

## Q569 [Spring][MVC][REST][Senior]
### Ce este consumer-driven contract?

**Răspuns:**  
Contract definit de consumator.

---

## Q570 [Spring][MVC][REST][Senior]
### Ce este Pact?

**Răspuns:**  
Framework pentru contract testing.

---

## Q571 [Spring][MVC][REST][Senior]
### Ce este error handling consistent?

**Răspuns:**  
Format uniform al erorilor.

---

## Q572 [Spring][MVC][REST][Senior]
### Ce este RFC 7807?

**Răspuns:**  
Standard pentru erori HTTP (Problem Details).

---

## Q573 [Spring][MVC][REST][Senior]
### Ce este `ProblemDetail` în Spring 6?

**Răspuns:**  
Implementare RFC 7807.

---

## Q574 [Spring][MVC][REST][Senior]
### Ce este input sanitization?

**Răspuns:**  
Curățarea datelor de intrare.

---

## Q575 [Spring][MVC][REST][Senior]
### Ce este output encoding?

**Răspuns:**  
Protecție împotriva XSS.

---

## Q576 [Spring][MVC][REST][Senior]
### Ce este API throttling?

**Răspuns:**  
Limitare adaptivă a traficului.

---

## Q577 [Spring][MVC][REST][Senior]
### Ce este observability pentru API-uri?

**Răspuns:**  
Metrici, log-uri, trace-uri.

---

## Q578 [Spring][MVC][REST][Senior]
### Ce este distributed tracing?

**Răspuns:**  
Urmărirea cererilor între servicii.

---

## Q579 [Spring][MVC][REST][Senior]
### Ce este API anti-pattern?

**Răspuns:**  
Design defectuos al API-ului.

---

## Q580 [Spring][MVC][REST][Senior]
### Care este regula de aur pentru REST APIs?

**Răspuns:**  
API-ul este un contract, nu o implementare.

---
