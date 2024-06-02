
## Einleitung

[[HTTP]] (Hypertext Transfer Protocol) ist das grundlegende Protokoll für die Datenübertragung im Web. [[HTTP]]-Anfragen ermöglichen die Kommunikation zwischen Clients und Servern und sind die Basis für die Interaktion mit Webanwendungen und APIs. Es gibt verschiedene Arten von [[HTTP]]-Anfragen, die jeweils unterschiedliche Funktionen und Einsatzbereiche haben. In dieser Note werden die technischen Aspekte und verschiedenen Arten von [[HTTP]]-Anfragen untersucht, wobei besonderer Augenmerk auf ihre Bedeutung im Kontext von Bug-Bounty-Programmen gelegt wird.

## Arten von [[HTTP]]-Anfragen

### GET

Die [[GET-Methode]] wird verwendet, um Daten von einem Server abzurufen. Sie ist die am häufigsten verwendete [[HTTP]]-Methode und dient hauptsächlich dazu, Ressourcen wie Webseiten, Bilder oder Dateien anzufordern.

- **Eigenschaften**:
  - Anfragen werden im [[URL-Query-String]] kodiert.
  - Daten sind in der [[URL (Uniform Resource Locator)]] sichtbar und können in Browser-Cache und Logs gespeichert werden.
  - GET-Anfragen sollten keine Änderungen am Serverzustand bewirken (idempotent).

- **Beispiel**:
  ```
  GET /api/v1/users?id=123 HTTP/1.1
  Host: example.com
  ```

### POST

Die [[POST-Methode]] wird verwendet, um Daten an den Server zu senden, um neue Ressourcen zu erstellen oder bestehende zu aktualisieren. Diese Methode wird häufig für Formulare und Datei-Uploads verwendet.

- **Eigenschaften**:
  - Anfragen senden Daten im Body der [[HTTP]]-Nachricht.
  - POST-Anfragen sind nicht idempotent; wiederholte Anfragen können unterschiedliche Ergebnisse erzeugen.
  - Häufig für das Erstellen von Ressourcen verwendet.

- **Beispiel**:
  ```
  POST /api/v1/users HTTP/1.1
  Host: example.com
  Content-Type: application/json

  {
    "name": "John Doe",
    "email": "john@example.com"
  }
  ```

### PUT

Die PUT-Methode wird verwendet, um eine Ressource vollständig zu aktualisieren oder eine neue Ressource zu erstellen, wenn sie noch nicht existiert. Im Gegensatz zu POST ist PUT idempotent.

- **Eigenschaften**:
  - Anfragen senden Daten im Body der [[HTTP]]-Nachricht.
  - PUT-Anfragen sind idempotent; wiederholte Anfragen erzeugen das gleiche Ergebnis.
  - Häufig für das vollständige Update einer Ressource verwendet.

- **Beispiel**:
  ```
  PUT /api/v1/users/123 HTTP/1.1
  Host: example.com
  Content-Type: application/json

  {
    "name": "John Doe",
    "email": "john@example.com"
  }
  ```

### DELETE

Die DELETE-Methode wird verwendet, um eine Ressource vom Server zu löschen. Sie ist ebenfalls idempotent.

- **Eigenschaften**:
  - Anfragen haben normalerweise keinen Body.
  - DELETE-Anfragen sind idempotent; wiederholte Anfragen führen zur Löschung der Ressource oder bestätigen, dass sie bereits gelöscht ist.

- **Beispiel**:
  ```
  DELETE /api/v1/users/123 HTTP/1.1
  Host: example.com
  ```

### HEAD

Die HEAD-Methode ist ähnlich wie GET, ruft jedoch nur die Header-Informationen der Ressource ab, ohne den Body. Sie wird verwendet, um Metadaten über die Ressource zu erhalten.

- **Eigenschaften**:
  - Anfragen senden keine Daten im Body.
  - HEAD-Anfragen sind nützlich, um Informationen wie Größe, Typ und letzte Änderung der Ressource zu prüfen.

- **Beispiel**:
  ```
  HEAD /api/v1/users/123 HTTP/1.1
  Host: example.com
  ```

### OPTIONS

Die OPTIONS-Methode wird verwendet, um die vom Server unterstützten HTTP-Methoden für eine bestimmte Ressource zu ermitteln. Sie ist nützlich für die Implementierung von CORS (Cross-Origin Resource Sharing).

- **Eigenschaften**:
  - Anfragen haben normalerweise keinen Body.
  - OPTIONS-Anfragen geben die unterstützten Methoden und andere Optionen zurück.

- **Beispiel**:
  ```
  OPTIONS /api/v1/users HTTP/1.1
  Host: example.com
  ```

### PATCH

Die PATCH-Methode wird verwendet, um eine Ressource partiell zu aktualisieren. Sie ist im Gegensatz zu PUT nicht idempotent und wird für inkrementelle Updates verwendet.

- **Eigenschaften**:
  - Anfragen senden Daten im Body der HTTP-Nachricht.
  - PATCH-Anfragen sind nicht idempotent; wiederholte Anfragen können unterschiedliche Ergebnisse erzeugen.

- **Beispiel**:
  ```
  PATCH /api/v1/users/123 HTTP/1.1
  Host: example.com
  Content-Type: application/json

  {
    "email": "john_new@example.com"
  }
  ```

## HTTP-Anfragen in Bezug auf Bug Bounty

### Sicherheitsrisiken

HTTP-Anfragen sind häufig Ziele von Angriffen, da sie die Interaktion zwischen Client und Server steuern. Einige der wichtigsten Sicherheitsrisiken umfassen:

- **SQL-Injection**: Angreifer können schädlichen SQL-Code in Parameter von HTTP-Anfragen einschleusen, um Datenbanken zu manipulieren.
- **Cross-Site Scripting (XSS)**: Über GET- und POST-Anfragen können bösartige Skripte eingeschleust werden, die im Browser des Opfers ausgeführt werden.
- **Cross-Site Request Forgery (CSRF)**: Angreifer können Opfer dazu bringen, ungewollte Aktionen auf einer Webseite durchzuführen, auf der sie authentifiziert sind.
- **Unzureichende Validierung und Sanitierung**: Mangelnde Eingabevalidierung kann zu verschiedenen Angriffen führen, darunter Injektionen und Overflows.

### Sicherheitsmaßnahmen

Um HTTP-Anfragen sicher zu gestalten, sollten folgende Maßnahmen ergriffen werden:

- **Eingabevalidierung**: Alle Eingaben sollten streng validiert und gefiltert werden.
- **Prepared Statements**: Verwenden Sie vorbereitete Anweisungen für Datenbankabfragen, um SQL-Injection zu verhindern.
- **Content Security Policy (CSP)**: Implementieren Sie CSP, um XSS-Angriffe zu verhindern.
- **CSRF-Tokens**: Nutzen Sie CSRF-Tokens, um sicherzustellen, dass Anfragen von berechtigten Quellen stammen.
- **Rate Limiting**: Implementieren Sie Mechanismen zur Begrenzung der Anzahl von Anfragen pro Zeiteinheit, um DoS-Angriffe zu verhindern.

## Schlussfolgerung

HTTP-Anfragen sind der Kern der Kommunikation im Web und spielen eine entscheidende Rolle in der Funktionalität von Webanwendungen und APIs. Verschiedene Arten von HTTP-Anfragen erfüllen unterschiedliche Zwecke und haben spezifische Eigenschaften und Einsatzgebiete. Im Kontext von Bug-Bounty-Programmen sind HTTP-Anfragen ein kritischer Prüfbereich, da sie potenzielle Schwachstellen bieten, die von Angreifern ausgenutzt werden können. Durch das Verständnis der verschiedenen Arten von HTTP-Anfragen und die Implementierung robuster Sicherheitsmaßnahmen können Entwickler und Sicherheitsforscher dazu beitragen, die Sicherheit und Integrität von Webanwendungen zu gewährleisten.