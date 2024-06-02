
## Einleitung

Der [[URL (Uniform Resource Locator)]]-Query-String ist ein wesentlicher Bestandteil der [[URL (Uniform Resource Locator)]] (Uniform Resource Locator) und wird verwendet, um zusätzliche Daten an den Server zu übermitteln. Diese Daten werden häufig zur Filterung, Sortierung oder für die Paginierung von Inhalten verwendet. Der Query-String folgt auf das Fragezeichen (?) in der [[URL (Uniform Resource Locator)]] und besteht aus Schlüssel-Wert-Paaren, die durch das Kaufmännische Und (&) getrennt sind. In dieser Note werden die technischen Aspekte und verschiedenen Arten von [[URL (Uniform Resource Locator)]]-Query-Strings erläutert, wobei besonderer Augenmerk auf ihre Bedeutung im Kontext von APIs und Bug-Bounty-Programmen gelegt wird.

## Aufbau und Syntax des [[URL (Uniform Resource Locator)]]-Query-Strings

Ein [[URL (Uniform Resource Locator)]]-Query-String besteht aus einem oder mehreren Schlüssel-Wert-Paaren, die durch das Gleichheitszeichen (=) verbunden sind. Mehrere Paare werden durch das Kaufmännische Und (&) getrennt. Die allgemeine Syntax ist wie folgt:

```
http://example.com/page?key1=value1&key2=value2
```

### Beispiele

1. **Einzelner Parameter**:
   ```
   http://example.com/search?q=API
   ```
   - `q` ist der Schlüssel
   - `API` ist der Wert

2. **Mehrere Parameter**:
   ```
   http://example.com/search?q=API&type=document&page=2
   ```
   - `q` ist der Schlüssel mit dem Wert `API`
   - `type` ist der Schlüssel mit dem Wert `document`
   - `page` ist der Schlüssel mit dem Wert `2`

## Arten von [[URL (Uniform Resource Locator)]]-Query-Strings

### Einzelne Parameter

Diese Art von Query-String enthält nur ein einzelnes Schlüssel-Wert-Paar. Sie wird oft für einfache Suchanfragen oder Filter verwendet.

### Mehrere Parameter

Mehrere Schlüssel-Wert-Paare können in einem Query-String kombiniert werden. Dies ist typisch für komplexere Abfragen, die mehrere Kriterien umfassen, wie Sortierung, Paginierung und Filterung.

### Verschachtelte Parameter

Verschachtelte Parameter werden verwendet, um hierarchische Datenstrukturen darzustellen. Sie können durch spezielle Zeichen wie Punkte oder Klammern getrennt werden.

- **Beispiel mit Punkten**:
  ```
  http://example.com/search?filter.name=John&filter.age=30
  ```

- **Beispiel mit Klammern**:
  ```
  http://example.com/search?filter[name]=John&filter[age]=30
  ```

### Arrays in Query-Strings

Arrays können im Query-String durch mehrfache Schlüsselverwendung oder durch Klammern dargestellt werden.

- **Mehrfache Schlüsselverwendung**:
  ```
  http://example.com/search?tag=python&tag=javascript&tag=html
  ```

- **Verwendung von Klammern**:
  ```
  http://example.com/search?tag[]=python&tag[]=javascript&tag[]=html
  ```

### Kodierung von Query-Strings

Spezielle Zeichen in Query-Strings müssen [[URL (Uniform Resource Locator)]]-kodiert werden, um sicherzustellen, dass sie korrekt übertragen und interpretiert werden. Dies betrifft insbesondere Zeichen wie Leerzeichen, &, ?, und =.

- **Leerzeichen**: wird zu `%20` oder `+`
- **&**: wird zu `%26`
- **?**: wird zu `%3F`
- **=**: wird zu `%3D`

## URL-Query-Strings in Bezug auf Bug Bounty

[[URL (Uniform Resource Locator)]]-Query-Strings sind häufig ein Ziel für Angriffe, da sie Benutzereingaben direkt in die [[URL (Uniform Resource Locator)]] einbetten. Einige der gängigsten Angriffstechniken umfassen:

### SQL-Injection

Angreifer können versuchen, SQL-Befehle über unsachgemäß behandelte Query-Strings in die Datenbank einzuschleusen.

- **Beispiel**:
  ```
  http://example.com/search?query=' OR '1'='1
  ```

### Cross-Site Scripting (XSS)

Schadcode kann in Query-Strings eingeschleust und in der Webanwendung ausgeführt werden, wenn die Eingaben nicht korrekt validiert und gefiltert werden.

- **Beispiel**:
  ```
  http://example.com/search?query=<script>alert('XSS')</script>
  ```

### Cross-Site Request Forgery (CSRF)

Angreifer können bösartige URLs erstellen, die beim Anklicken unerwünschte Aktionen auf einer anderen Seite ausführen, auf der der Benutzer authentifiziert ist.

- **Beispiel**:
  ```
  http://example.com/delete-account?token=abcdef
  ```

### Parameter Tampering

Manipulation von Query-String-Parametern kann dazu führen, dass unautorisierte Aktionen ausgeführt werden, wie das Abrufen oder Ändern sensibler Daten.

- **Beispiel**:
  ```
  http://example.com/update?user_id=123&role=admin
  ```

## Sicherheitsmaßnahmen

Um die Sicherheit von [[URL (Uniform Resource Locator)]]-Query-Strings zu gewährleisten, sollten folgende Maßnahmen ergriffen werden:

### Eingabevalidierung

Stellen Sie sicher, dass alle Eingaben im Query-String ordnungsgemäß validiert und nur erwartete Daten akzeptiert werden.

### Prepared Statements

Verwenden Sie vorbereitete Anweisungen (Prepared Statements) für Datenbankabfragen, um SQL-Injection zu verhindern.

### Encoding

Codieren Sie alle Ausgaben, die von Query-Strings stammen, um XSS-Angriffe zu verhindern.

### Zugriffskontrollen

Implementieren Sie strikte Zugriffskontrollen und überprüfen Sie die Autorisierung bei jeder Anfrage, um sicherzustellen, dass Benutzer nur auf die Ressourcen zugreifen können, für die sie berechtigt sind.

### CSRF-Schutz

Verwenden Sie CSRF-[[Token]], um sicherzustellen, dass Anfragen authentisch sind und von berechtigten Benutzern stammen.

## Schlussfolgerung

[[URL (Uniform Resource Locator)]]-Query-Strings sind ein essenzieller Bestandteil der Kommunikation zwischen Client und Server im Web. Sie bieten eine flexible Methode zur Übertragung von Daten, bergen jedoch auch erhebliche Sicherheitsrisiken. Im Kontext von Bug-Bounty-Programmen sind [[URL (Uniform Resource Locator)]]-Query-Strings ein kritischer Prüfbereich, da sie oft anfällig für verschiedene Arten von Angriffen sind. Durch das Verständnis der verschiedenen Arten von Query-Strings und die Implementierung robuster Sicherheitsmaßnahmen können Entwickler und Sicherheitsforscher dazu beitragen, die Sicherheit und Integrität von Webanwendungen zu gewährleisten.