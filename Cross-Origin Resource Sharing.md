
# Ausführliche Analyse zu Cross-Origin Resource Sharing und seine Rolle in [[Bug-Bounty-Programme]]n

## Einführung

Cross-Origin Resource Sharing ([[CORS]]) ist ein Sicherheitskonzept im Web, das es Webseiten erlaubt, auf Ressourcen einer anderen Domain zuzugreifen. [[CORS]] ist eine Erweiterung der Same-Origin-Policy (SOP), die standardmäßig verhindert, dass Skripte auf Ressourcen zugreifen, die nicht von derselben Domain stammen, von der das Skript geladen wurde. Dieser Leitfaden erläutert die Funktionsweise von [[CORS]], seine verschiedenen Modi und die Bedeutung im Kontext von Bug Bounty und Cybersecurity.

## Grundlagen von [[CORS]]

[[CORS]] ermöglicht es sicheren Webseiten, Anfragen an andere Domains zu stellen und dabei bestimmte Bedingungen einzuhalten. Die Entscheidung, ob eine [[CORS-Anfrage]] erlaubt wird, liegt beim Server, der die angeforderten Ressourcen bereitstellt. Dies geschieht durch das Senden spezieller [[CORS-Header]] in der [[HTTP-Antwort]].

### Wesentliche [[CORS-Header]]

- **Access-Control-Allow-Origin**: Gibt an, welche Ursprungsdomänen Zugriff auf die Ressourcen haben dürfen.
- **Access-Control-Allow-Methods**: Legt fest, welche [[HTTP-Methoden]] erlaubt sind.
- **Access-Control-Allow-Headers**: Bestimmt, welche [[HTTP-Header]] in der Anfrage verwendet werden dürfen.
- **Access-Control-Allow-Credentials**: Erlaubt oder verbietet das Senden von Credentials wie [[Cookies]] und [[HTTP-Authentifizierungsinformationen]].

## Arten von [[CORS-Anfragen]]

### Einfache Anfragen

Diese Anfragen verwenden Methoden wie GET oder POST mit bestimmten sicheren Headern. Einfache Anfragen lösen keine Preflight-Anfrage aus, weil sie als sicher genug angesehen werden, um keine zusätzliche Prüfung zu benötigen.

### Preflight-Anfragen

Preflight-Anfragen sind vorbereitende Anfragen, die mit der OPTIONS-Methode gesendet werden, um zu überprüfen, ob die nachfolgende tatsächliche Anfrage sicher ist. Dies geschieht vor allem, wenn Anfragen Methoden wie PUT oder DELETE verwenden oder benutzerdefinierte Header enthalten.

### Anfragen mit Credentials

Diese Art von Anfragen sendet Credentials wie [[Cookies]] oder [[HTTP-Authentifizierungsinformationen]] mit Anfragen an andere Ursprünge. Für solche Anfragen muss der `Access-Control-Allow-Credentials`-Header auf `true` gesetzt sein.

## [[CORS]] im Kontext von Bug Bounty und Cybersecurity

[[CORS]] ist besonders relevant in der Welt der Cybersecurity, da eine falsche oder fehlerhafte [[CORS-Konfiguration]] die Tür für verschiedene Sicherheitsrisiken öffnen kann.

### Sicherheitsrisiken und Angriffsvektoren

- **Data Leakage**: Wenn der `Access-Control-Allow-Origin`-Header zu liberal konfiguriert ist (z.B. `*`), können potenziell schädliche Websites Zugriff auf sensible Daten erhalten.
- **CSRF-Angriffe**: Eine fehlerhafte [[CORS-Politik]] kann Cross-Site Request Forgery erleichtern, insbesondere wenn Anfragen mit Credentials erlaubt sind.

### Beispiele

- **Zu offene Einstellung**:
    
    httpCopy code
    
    `Access-Control-Allow-Origin: * Access-Control-Allow-Methods: GET, POST, PUT`
    
    Diese Einstellung erlaubt es jeder Website, Daten von diesem Server abzurufen und zu manipulieren, was zu Datenlecks führen kann.
    
- **Sichere [[CORS]]-Einstellung**:
    
    httpCopy code
    
    `Access-Control-Allow-Origin: https://trusteddomain.com Access-Control-Allow-Methods: GET, POST Access-Control-Allow-Headers: X-Custom-Header`
    
    Diese Einstellung beschränkt den Zugriff auf vertrauenswürdige Domänen und spezifische Aktionen, wodurch das Risiko von Missbrauch verringert wird.
    

## Best Practices

- **Eingeschränkte Ursprünge**: Setzen Sie `Access-Control-Allow-Origin` immer so restriktiv wie möglich.
- **Vermeidung von Wildcards**: Vermeiden Sie die Verwendung von `*` in sensiblen Einstellungen.
- **Sichere Verwendung von Credentials**: Credentials sollten nur erlaubt sein, wenn absolut notwendig und nur über sichere Kanäle.

## Fazit

[[CORS]] ist ein kritischer Bestandteil der modernen Webarchitektur, der es ermöglicht, die Grenzen der Same-Origin-Policy sicher zu erweitern. Für Bug-Bounty-Jäger und Sicherheitsexperten ist das Verständnis von [[CORS]] unerlässlich, um Schwachstellen zu erkennen, die durch schlechte [[CORS-Konfigurationen]] entstehen können. Ein gründliches Verständnis und die korrekte Implementierung von [[CORS]] tragen dazu bei, die Sicherheit von Webanwendungen zu erhöhen und die Privatsphäre der Benutzer zu schützen.

## Quellen

- Offizielle Dokumentation zu [[CORS]] von W3C und anderen relevanten technischen Gremien.
- Fachliteratur und Ressourcen zur Websecurity, die speziell auf [[CORS-Konfigurationen]] und deren Auswirkungen eingehen.