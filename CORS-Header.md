
## Einleitung

[[CORS]] ([[Cross-Origin Resource Sharing]]) ist ein Mechanismus, der es Webanwendungen ermöglicht, Ressourcen von einer anderen Domain als der eigenen sicher anzufordern. Da moderne Webbrowser standardmäßig Cross-Origin-Anfragen blockieren, um Sicherheitsrisiken wie Cross-Site Scripting (XSS) und Cross-Site Request Forgery (CSRF) zu minimieren, spielt [[CORS]] eine entscheidende Rolle bei der sicheren Freigabe von Ressourcen über verschiedene Domains hinweg. Diese Note erläutert die technischen Details der verschiedenen [[CORS]]-Header und ihre Bedeutung im Kontext von APIs und Bug-Bounty-Programmen.

## Überblick über CORS

[[CORS]] erlaubt es Servern, anzugeben, welche Ursprünge (Domains) auf ihre Ressourcen zugreifen dürfen. Dies geschieht durch das Hinzufügen spezifischer [[HTTP-Header]] zu den Antworten auf Cross-Origin-Anfragen. [[CORS]] kann als eine Art "Whitelist" für Cross-Origin-Anfragen betrachtet werden.

## Arten von [[CORS]]-Headern

### `Access-Control-Allow-Origin`

Dieser Header gibt an, welche Ursprünge auf die Ressource zugreifen dürfen. Es ist der wichtigste [[CORS]]-Header.

- **Werte**:
  - Eine spezifische Ursprungs-Domain (z.B., `https://example.com`)
  - Ein Sternchen (`*`), um alle Ursprünge zu erlauben (nicht empfohlen für Produktionsumgebungen)

- **Beispiel**:
  ```
  Access-Control-Allow-Origin: https://example.com
  ```

### `Access-Control-Allow-Methods`

Dieser Header gibt an, welche [[HTTP-Methoden]] der Server für Cross-Origin-Anfragen zulässt.

- **Werte**: Eine Liste von [[HTTP-Methoden]] (z.B., `GET`, `POST`, `PUT`, `DELETE`)

- **Beispiel**:
  ```
  Access-Control-Allow-Methods: GET, POST, PUT, DELETE
  ```

### `Access-Control-Allow-Headers`

Dieser Header gibt an, welche [[HTTP-Header]] in der Anfrage vom Client zugelassen sind.

- **Werte**: Eine Liste von Header-Feldern (z.B., `Content-Type`, `Authorization`)

- **Beispiel**:
  ```
  Access-Control-Allow-Headers: Content-Type, Authorization
  ```

### `Access-Control-Allow-Credentials`

Dieser Header gibt an, ob [[Cookies]] und andere Anmeldeinformationen bei Cross-Origin-Anfragen gesendet werden dürfen.

- **Werte**:
  - `true` (Anmeldeinformationen erlaubt)
  - `false` (Anmeldeinformationen nicht erlaubt, standardmäßig)

- **Beispiel**:
  ```
  Access-Control-Allow-Credentials: true
  ```

### `Access-Control-Expose-Headers`

Dieser Header gibt an, welche Header dem Client zugänglich gemacht werden dürfen.

- **Werte**: Eine Liste von Header-Feldern

- **Beispiel**:
  ```
  Access-Control-Expose-Headers: Content-Length, X-Kuma-Revision
  ```

### `Access-Control-Max-Age`

Dieser Header gibt an, wie lange die Ergebnisse des Preflight-Checks gecacht werden dürfen.

- **Werte**: Eine Zeitspanne in Sekunden

- **Beispiel**:
  ```
  Access-Control-Max-Age: 86400
  ```

### `Access-Control-Request-Method` und `Access-Control-Request-Headers`

Diese Header werden von Browsern in Preflight-Anfragen gesendet, um die vom Server unterstützten Methoden und Header zu ermitteln.

- **Beispiel einer Preflight-Anfrage**:
  ```
  OPTIONS /api/v1/resource HTTP/1.1
  Host: example.com
  Access-Control-Request-Method: POST
  Access-Control-Request-Headers: Content-Type
  ```

## [[CORS]] im Kontext von Bug Bounty

### Sicherheitsrisiken

[[CORS-Fehlkonfigurationen]] können zu erheblichen Sicherheitslücken führen, die von Angreifern ausgenutzt werden können. Einige der häufigsten Risiken umfassen:

- **Offene Freigaben (`Access-Control-Allow-Origin: *`)**: Dies erlaubt allen Ursprüngen, auf die Ressourcen zuzugreifen, was zu Datenexfiltration und anderen Sicherheitsproblemen führen kann.
- **Anmeldeinformationen ohne strenge Ursprungskontrolle**: Das Zulassen von Anmeldeinformationen (`Access-Control-Allow-Credentials: true`) in Verbindung mit einer nicht spezifischen Ursprungskontrolle kann Angreifern ermöglichen, authentifizierte Anfragen im Namen eines Benutzers zu senden.

### Sicherheitsmaßnahmen

Um die Sicherheit von [[CORS]]-Konfigurationen zu gewährleisten, sollten folgende Maßnahmen ergriffen werden:

- **Spezifische Ursprungskontrolle**: Verwenden Sie spezifische Ursprünge anstelle von `*`, um nur vertrauenswürdige Domains zuzulassen.
- **Strikte Header- und Methodenkontrolle**: Geben Sie nur die notwendigen [[HTTP-Methoden]] und Header frei, die tatsächlich benötigt werden.
- **Anmeldeinformationen sorgfältig handhaben**: Verwenden Sie `Access-Control-Allow-Credentials: true` nur in Verbindung mit spezifischen Ursprüngen und wenn unbedingt erforderlich.
- **Regelmäßige Überprüfungen**: Überprüfen Sie regelmäßig die [[CORS]]-Konfigurationen auf Fehlkonfigurationen und potenzielle Sicherheitslücken.

## Schlussfolgerung

[[CORS]]-Header sind ein wichtiger Mechanismus zur sicheren Freigabe von Ressourcen über verschiedene Ursprünge hinweg. Ihre korrekte Implementierung und Konfiguration sind entscheidend, um Sicherheitsrisiken zu minimieren und die Integrität der Daten zu gewährleisten. Im Kontext von Bug-Bounty-Programmen sind [[CORS]]-Header ein kritischer Prüfbereich, da Fehlkonfigurationen zu erheblichen Sicherheitslücken führen können. Durch das Verständnis der verschiedenen Arten von [[CORS]]-Headern und die Implementierung robuster Sicherheitsmaßnahmen können Entwickler und Sicherheitsforscher dazu beitragen, die Sicherheit von Webanwendungen zu gewährleisten.