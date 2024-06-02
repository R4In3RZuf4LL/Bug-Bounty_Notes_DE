
## Einleitung

[[CORS]] ([[Cross-Origin Resource Sharing]]) ist ein Mechanismus, der es Webservern ermöglicht, kontrollierten Zugriff auf Ressourcen von einer anderen Domain als der Ursprungsdomain zu gewähren. Dies ist notwendig, da moderne Browser aus Sicherheitsgründen standardmäßig Cross-Origin-Anfragen blockieren. [[CORS]] definiert eine Methode, um Webanwendungen die Kommunikation mit anderen Domains zu erlauben, während gleichzeitig Sicherheitsrichtlinien durchgesetzt werden. In dieser Note werden die technischen Aspekte und verschiedenen Arten von [[CORS]]-Richtlinien untersucht, wobei besonderer Augenmerk auf ihre Bedeutung im Kontext von Bug-Bounty-Programmen gelegt wird.

## Grundlagen von [[CORS]]

### Funktionsweise

[[CORS]] funktioniert durch Hinzufügen spezieller [[HTTP-Header]] zu Anfragen und Antworten, um den Zugriff auf Ressourcen zu steuern. Ein typischer [[CORS]]-Flow besteht aus zwei Hauptphasen: dem Preflight-Request und dem eigentlichen Request.

### Preflight-Request

Bevor bestimmte Arten von Anfragen (wie solche, die nicht als "simple" eingestuft werden) gesendet werden, führt der Browser einen Preflight-Request durch, um zu überprüfen, ob die [[CORS]]-Richtlinien des Servers die geplante Anfrage zulassen. Dies geschieht mit einer OPTIONS-Anfrage.

- **Beispiel für einen Preflight-Request**:
  ```
  OPTIONS /api/data HTTP/1.1
  Host: example.com
  Origin: http://anotherdomain.com
  Access-Control-Request-Method: POST
  Access-Control-Request-Headers: Content-Type
  ```

- **Antwort auf einen Preflight-Request**:
  ```
  HTTP/1.1 200 OK
  Access-Control-Allow-Origin: http://anotherdomain.com
  Access-Control-Allow-Methods: POST, GET, OPTIONS
  Access-Control-Allow-Headers: Content-Type
  ```

### Hauptanfrage

Wenn der Preflight-Request erfolgreich ist, sendet der Browser die eigentliche Anfrage mit entsprechenden [[CORS]]-Headern.

- **Beispiel für eine Hauptanfrage**:
  ```
  POST /api/data HTTP/1.1
  Host: example.com
  Origin: http://anotherdomain.com
  Content-Type: application/json

  { "key": "value" }
  ```

- **Antwort auf eine Hauptanfrage**:
  ```
  HTTP/1.1 200 OK
  Access-Control-Allow-Origin: http://anotherdomain.com
  Content-Type: application/json

  { "response": "data" }
  ```

## Arten von [[CORS]]-Richtlinien

### Access-Control-Allow-Origin

Dieser Header gibt an, welche Ursprünge (Domains) auf die Ressource zugreifen dürfen. Es kann ein spezifischer Ursprung oder das Wildcard-Zeichen `*` sein, das alle Ursprünge erlaubt.

- **Beispiel**:
  ```
  Access-Control-Allow-Origin: http://anotherdomain.com
  ```

### Access-Control-Allow-Methods

Dieser Header listet die [[HTTP]]-Methoden auf, die für den Zugriff auf die Ressource erlaubt sind.

- **Beispiel**:
  ```
  Access-Control-Allow-Methods: GET, POST, PUT, DELETE
  ```

### Access-Control-Allow-Headers

Dieser Header spezifiziert die erlaubten Header, die in der Anfrage enthalten sein dürfen.

- **Beispiel**:
  ```
  Access-Control-Allow-Headers: Content-Type, Authorization
  ```

### Access-Control-Expose-Headers

Dieser Header gibt die Header an, die der Browser für die Ursprungsdomäne sichtbar machen soll.

- **Beispiel**:
  ```
  Access-Control-Expose-Headers: Content-Length, X-Kuma-Revision
  ```

### Access-Control-Allow-Credentials

Dieser Header gibt an, ob Anmeldeinformationen wie [[Cookies]], [[HTTP]]-[[Authentifizierung]] oder Client-Zertifikate in der Anfrage enthalten sein dürfen.

- **Beispiel**:
  ```
  Access-Control-Allow-Credentials: true
  ```

### Access-Control-Max-Age

Dieser Header gibt die Dauer (in Sekunden) an, wie lange die Ergebnisse eines Preflight-Requests gecacht werden dürfen.

- **Beispiel**:
  ```
  Access-Control-Max-Age: 86400
  ```

## [[CORS]]-Richtlinien in Bezug auf Bug Bounty

### Sicherheitsrisiken

[[CORS]]-Richtlinien können, wenn sie nicht korrekt konfiguriert sind, schwerwiegende Sicherheitslücken verursachen. Hier sind einige der häufigsten Risiken:

- **Wildcard-Missbrauch (`*`)**: Das Zulassen aller Ursprünge mit `*` kann riskant sein, insbesondere wenn die Ressource sensible Daten enthält.
- **Offene Ursprünge**: Das Erlauben spezifischer, aber nicht vertrauenswürdiger Ursprünge kann zu Datenexfiltration führen.
- **Fehlkonfigurierte Allow-Credentials**: Das Setzen von `Access-Control-Allow-Credentials` auf `true` ohne eine restriktive `Access-Control-Allow-Origin`-Richtlinie kann zu Cross-Site Request Forgery (CSRF) führen.

### Sicherheitsmaßnahmen

Um [[CORS]]-Richtlinien sicher zu gestalten, sollten folgende Maßnahmen ergriffen werden:

- **Beschränkung der Ursprünge**: Erlauben Sie nur spezifische, vertrauenswürdige Ursprünge.
- **Minimierung der Methoden**: Beschränken Sie die erlaubten [[HTTP]]-Methoden auf das notwendige Minimum.
- **Sichere Header-Konfiguration**: Erlauben Sie nur die notwendigen Header.
- **Sicheres Credentials-Handling**: Vermeiden Sie `Access-Control-Allow-Credentials: true`, es sei denn, es ist unbedingt erforderlich und sicher konfiguriert.

## Schlussfolgerung

[[CORS]]-Richtlinien sind ein entscheidender Mechanismus, um die Sicherheit und Integrität von Webanwendungen zu gewährleisten, die mit Ressourcen über verschiedene Ursprünge hinweg arbeiten. Eine korrekte Implementierung und Konfiguration von [[CORS]] sind entscheidend, um Sicherheitslücken zu vermeiden, die von Angreifern ausgenutzt werden könnten. Im Kontext von Bug-Bounty-Programmen sind [[CORS]]-Richtlinien ein wichtiger Prüfbereich, da Fehlkonfigurationen zu erheblichen Sicherheitsrisiken führen können. Durch das Verständnis der verschiedenen Arten von [[CORS]]-Richtlinien und die Implementierung robuster Sicherheitsmaßnahmen können Entwickler und Sicherheitsforscher dazu beitragen, die Sicherheit von Webanwendungen zu gewährleisten.