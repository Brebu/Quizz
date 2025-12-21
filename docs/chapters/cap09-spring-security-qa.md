# Capitolul 9 – Spring Security (VERSIUNEA FINALĂ)
## Q661–Q740 — Nivel Senior

> Limbă: Română (UTF-8, diacritice corecte)  
> Scop: Interviuri Senior / Lead / Staff  
> Tip: Teorie aprofundată

---

## Q661 [Spring][Security][Senior]
### Ce este Spring Security și ce problemă rezolvă?

**Definiție:**  
Spring Security este framework-ul standard pentru autentificare și autorizare în aplicații Spring.

**Probleme rezolvate:**  
- controlul accesului  
- protecție împotriva atacurilor comune  
- integrare cu standarde moderne de securitate

---

## Q662 [Spring][Security][Senior]
### Diferența dintre autentificare și autorizare

**Răspuns:**  
- Autentificare: cine ești  
- Autorizare: ce ai voie să faci

---

## Q663 [Spring][Security][Senior]
### Ce este Security Filter Chain?

**Răspuns:**  
Lanț de filtre care interceptează fiecare cerere HTTP.

---

## Q664 [Spring][Security][Senior]
### Rolul `DelegatingFilterProxy`

**Răspuns:**  
Leagă containerul web de Spring Security.

---

## Q665 [Spring][Security][Senior]
### Ce este `FilterChainProxy`?

**Răspuns:**  
Dispatcher-ul intern al filtrelor de securitate.

---

## Q666 [Spring][Security][Senior]
### Ce este `SecurityContext`?

**Răspuns:**  
Stochează informațiile de securitate curente.

---

## Q667 [Spring][Security][Senior]
### Ce este `SecurityContextHolder`?

**Răspuns:**  
Acces static la contextul de securitate.

---

## Q668 [Spring][Security][Senior]
### Strategii de stocare pentru SecurityContext

**Răspuns:**  
- ThreadLocal (default)  
- InheritableThreadLocal  
- Global

---

## Q669 [Spring][Security][Senior]
### Ce este `Authentication`?

**Răspuns:**  
Reprezintă identitatea autentificată.

---

## Q670 [Spring][Security][Senior]
### Ce este `Principal`?

**Răspuns:**  
Identitatea utilizatorului.

---

## Q671 [Spring][Security][Senior]
### Ce este `GrantedAuthority`?

**Răspuns:**  
Permisiune acordată utilizatorului.

---

## Q672 [Spring][Security][Senior]
### Diferența ROLE vs AUTHORITY

**Răspuns:**  
ROLE este un AUTHORITY cu prefix `ROLE_`.

---

## Q673 [Spring][Security][Senior]
### Ce este `UserDetails`?

**Răspuns:**  
Modelul intern al utilizatorului.

---

## Q674 [Spring][Security][Senior]
### Ce este `UserDetailsService`?

**Răspuns:**  
Încarcă utilizatorii dintr-o sursă.

---

## Q675 [Spring][Security][Senior]
### Ce este `AuthenticationManager`?

**Răspuns:**  
Coordonează procesul de autentificare.

---

## Q676 [Spring][Security][Senior]
### Ce este `AuthenticationProvider`?

**Răspuns:**  
Validează credențialele.

---

## Q677 [Spring][Security][Senior]
### Fluxul autentificării în Spring Security

**Răspuns:**  
Request → Filter → AuthenticationManager → Provider → SecurityContext

---

## Q678 [Spring][Security][Senior]
### Ce este form login?

**Răspuns:**  
Autentificare bazată pe formular HTML.

---

## Q679 [Spring][Security][Senior]
### Ce este HTTP Basic Auth?

**Răspuns:**  
Autentificare simplă prin header Authorization.

---

## Q680 [Spring][Security][Senior]
### Riscurile HTTP Basic

**Răspuns:**  
Credențiale transmise la fiecare request.

---

## Q681 [Spring][Security][Senior]
### Ce este stateless authentication?

**Răspuns:**  
Serverul nu păstrează sesiuni.

---

## Q682 [Spring][Security][Senior]
### Ce este session-based auth?

**Răspuns:**  
Autentificare bazată pe sesiuni server-side.

---

## Q683 [Spring][Security][Senior]
### Ce este JWT?

**Răspuns:**  
Token semnat care conține claims.

---

## Q684 [Spring][Security][Senior]
### Structura unui JWT

**Răspuns:**  
Header + Payload + Signature.

---

## Q685 [Spring][Security][Senior]
### Avantaje JWT

**Răspuns:**  
- stateless  
- scalabil  
- interoperabil

---

## Q686 [Spring][Security][Senior]
### Riscuri JWT

**Răspuns:**  
- revocare dificilă  
- token leakage

---

## Q687 [Spring][Security][Senior]
### Ce este OAuth 2.0?

**Răspuns:**  
Framework de autorizare delegată.

---

## Q688 [Spring][Security][Senior]
### Roluri în OAuth 2.0

**Răspuns:**  
- Resource Owner  
- Client  
- Authorization Server  
- Resource Server

---

## Q689 [Spring][Security][Senior]
### Ce este OpenID Connect (OIDC)?

**Răspuns:**  
Layer de autentificare peste OAuth2.

---

## Q690 [Spring][Security][Senior]
### Diferența OAuth2 vs OIDC

**Răspuns:**  
OAuth2: autorizare  
OIDC: autentificare

---

## Q691 [Spring][Security][Senior]
### Ce este Authorization Code Flow?

**Răspuns:**  
Flow securizat pentru aplicații server-side.

---

## Q692 [Spring][Security][Senior]
### Ce este PKCE?

**Răspuns:**  
Protecție împotriva authorization code interception.

---

## Q693 [Spring][Security][Senior]
### Ce este Client Credentials Flow?

**Răspuns:**  
Autentificare service-to-service.

---

## Q694 [Spring][Security][Senior]
### Ce este Resource Server?

**Răspuns:**  
Serviciu care validează token-uri.

---

## Q695 [Spring][Security][Senior]
### Ce este Authorization Server?

**Răspuns:**  
Emite token-uri.

---

## Q696 [Spring][Security][Senior]
### Ce este `@EnableMethodSecurity`?

**Răspuns:**  
Activează securitate la nivel de metodă.

---

## Q697 [Spring][Security][Senior]
### Ce este `@PreAuthorize`?

**Răspuns:**  
Controlează accesul cu expresii SpEL.

---

## Q698 [Spring][Security][Senior]
### Ce este SpEL în Spring Security?

**Răspuns:**  
Language pentru expresii de securitate.

---

## Q699 [Spring][Security][Senior]
### Ce este `@PostAuthorize`?

**Răspuns:**  
Verifică accesul după execuție.

---

## Q700 [Spring][Security][Senior]
### Ce este method-level security?

**Răspuns:**  
Autorizare la nivel de business logic.

---

## Q701 [Spring][Security][Senior]
### Ce este CSRF?

**Răspuns:**  
Atac bazat pe cereri falsificate.

---

## Q702 [Spring][Security][Senior]
### Când se dezactivează CSRF?

**Răspuns:**  
Pentru API-uri stateless.

---

## Q703 [Spring][Security][Senior]
### Ce este CORS?

**Răspuns:**  
Politică de securitate cross-origin.

---

## Q704 [Spring][Security][Senior]
### Cum gestionează Spring Security CORS?

**Răspuns:**  
Prin filtre dedicate și config explicit.

---

## Q705 [Spring][Security][Senior]
### Ce este SameSite cookie?

**Răspuns:**  
Protecție împotriva CSRF.

---

## Q706 [Spring][Security][Senior]
### Ce este password encoding?

**Răspuns:**  
Hashing securizat al parolelor.

---

## Q707 [Spring][Security][Senior]
### Ce este `PasswordEncoder`?

**Răspuns:**  
Interfață pentru hashing parole.

---

## Q708 [Spring][Security][Senior]
### De ce BCrypt este recomandat?

**Răspuns:**  
Salt + cost adaptiv.

---

## Q709 [Spring][Security][Senior]
### Ce este security misconfiguration?

**Răspuns:**  
Configurare incorectă a securității.

---

## Q710 [Spring][Security][Senior]
### Ce este principle of least privilege?

**Răspuns:**  
Acordă minimul necesar de permisiuni.

---

## Q711 [Spring][Security][Senior]
### Ce este defense in depth?

**Răspuns:**  
Straturi multiple de securitate.

---

## Q712 [Spring][Security][Senior]
### Ce este `OncePerRequestFilter`?

**Răspuns:**  
Filtru executat o singură dată per request.

---

## Q713 [Spring][Security][Senior]
### Ce este custom authentication filter?

**Răspuns:**  
Filtru personalizat pentru token-uri.

---

## Q714 [Spring][Security][Senior]
### Ce este `SecurityFilterChain` bean?

**Răspuns:**  
Configurația declarativă a filtrelor.

---

## Q715 [Spring][Security][Senior]
### Ce este `HttpSecurity`?

**Răspuns:**  
DSL pentru configurare securitate web.

---

## Q716 [Spring][Security][Senior]
### Ce este `WebSecurityCustomizer`?

**Răspuns:**  
Exclude rute din securitate.

---

## Q717 [Spring][Security][Senior]
### Ce este `@AuthenticationPrincipal`?

**Răspuns:**  
Injectează utilizatorul autentificat.

---

## Q718 [Spring][Security][Senior]
### Ce este logout?

**Răspuns:**  
Invalidarea autentificării.

---

## Q719 [Spring][Security][Senior]
### Ce este token revocation?

**Răspuns:**  
Invalidarea token-urilor.

---

## Q720 [Spring][Security][Senior]
### Ce este refresh token?

**Răspuns:**  
Token pentru obținerea unuia nou.

---

## Q721 [Spring][Security][Senior]
### Ce este silent re-authentication?

**Răspuns:**  
Reautentificare fără interacțiune.

---

## Q722 [Spring][Security][Senior]
### Ce este impersonation?

**Răspuns:**  
Acționarea în numele altui utilizator.

---

## Q723 [Spring][Security][Senior]
### Ce este security context propagation?

**Răspuns:**  
Transmiterea contextului între thread-uri.

---

## Q724 [Spring][Security][Senior]
### Probleme de securitate cu async

**Răspuns:**  
Pierderea contextului.

---

## Q725 [Spring][Security][Senior]
### Ce este `@Async` și securitatea?

**Răspuns:**  
Necesită propagare explicită.

---

## Q726 [Spring][Security][Senior]
### Ce este method security vs URL security?

**Răspuns:**  
Business logic vs infrastructură.

---

## Q727 [Spring][Security][Senior]
### Ce este security testing?

**Răspuns:**  
Testarea regulilor de acces.

---

## Q728 [Spring][Security][Senior]
### Ce este `@WithMockUser`?

**Răspuns:**  
Mock de utilizator în teste.

---

## Q729 [Spring][Security][Senior]
### Ce este penetration testing?

**Răspuns:**  
Testare activă a vulnerabilităților.

---

## Q730 [Spring][Security][Senior]
### Ce este OWASP Top 10?

**Răspuns:**  
Lista celor mai comune vulnerabilități.

---

## Q731 [Spring][Security][Senior]
### Ce este Broken Access Control?

**Răspuns:**  
Acces neautorizat.

---

## Q732 [Spring][Security][Senior]
### Ce este Insecure Direct Object Reference (IDOR)?

**Răspuns:**  
Acces direct la resurse.

---

## Q733 [Spring][Security][Senior]
### Ce este XSS?

**Răspuns:**  
Cross-Site Scripting.

---

## Q734 [Spring][Security][Senior]
### Ce este SQL Injection?

**Răspuns:**  
Injectarea de cod SQL.

---

## Q735 [Spring][Security][Senior]
### Ce este security headers?

**Răspuns:**  
Headere pentru protecție browser.

---

## Q736 [Spring][Security][Senior]
### Exemple de security headers

**Răspuns:**  
CSP, HSTS, X-Frame-Options.

---

## Q737 [Spring][Security][Senior]
### Ce este audit logging?

**Răspuns:**  
Logarea evenimentelor de securitate.

---

## Q738 [Spring][Security][Senior]
### Ce este compliance?

**Răspuns:**  
Respectarea standardelor legale.

---

## Q739 [Spring][Security][Senior]
### Ce este zero trust?

**Răspuns:**  
Nicio entitate nu este implicit de încredere.

---

## Q740 [Spring][Security][Senior]
### Care este regula de aur în Spring Security?

**Răspuns:**  
Explicit > implicit în securitate.

---
