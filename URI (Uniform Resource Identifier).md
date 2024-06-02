
## Einleitung

Ein Uniform Resource Identifier (URI) ist eine Zeichenkette, die zur Identifizierung einer Ressource im Internet verwendet wird. URIs sind von zentraler Bedeutung für die Funktionsweise des Webs und ermöglichen den Zugriff auf Ressourcen wie Webseiten, Dateien und APIs. Diese Note untersucht die technischen Aspekte von URIs, ihre Struktur und Bedeutung im Kontext von APIs und hebt ihre Relevanz für [[Bug-Bounty-Programme]] hervor.

## Struktur eines URI

Ein URI besteht aus mehreren Komponenten, die in einer spezifischen Reihenfolge angeordnet sind. Die allgemeine Struktur eines URI lautet:

```
scheme:[//authority]path[?query][#fragment]
```

### Komponenten eines URI

1. **Scheme**: Gibt das Protokoll an, das zur Ressourcenzugriff verwendet wird (z.B. `http`, `https`, `ftp`).
2. **Authority**: Optional und enthält die `userinfo` (Benutzername und Passwort), den `host` (Domainname oder IP-Adresse) und den `port` (optional).
3. **Path**: Der Pfad zur Ressource auf dem Server.
4. **Query**: Optional und enthält zusätzliche Parameter zur Identifizierung oder Modifikation der Ressource.
5. **Fragment**: Optional und referenziert einen Teil der Ressource.

### Beispiel

```
https://username:password@example.com:8080/path/to/resource?query=param#fragment
```

In diesem Beispiel:
- `scheme`: `https`
- `authority`: `username:password@example.com:8080`
  - `userinfo`: `username:password`
  - `host`: `example.com`
  - `port`: `8080`
- `path`: `/path/to/resource`
- `query`: `query=param`
- `fragment`: `fragment`

## Arten von URIs

### URL (Uniform Resource Locator)

Eine [[URL (Uniform Resource Locator)]] ist ein spezifischer Typ von URI, der den Ort einer Ressource angibt, indem er das Zugriffsprotokoll und den Ort beschreibt. URLs sind die häufigste Form von URIs und werden verwendet, um Webseiten und andere Ressourcen im Internet zu lokalisieren.

### URN (Uniform Resource Name)

Ein URN ist ein anderer Typ von URI, der eine Ressource durch einen Namen innerhalb eines bestimmten Namensraums eindeutig identifiziert. URNs sind dauerhaft und unabhängig vom Standort der Ressource.

### Unterschiede zwischen [[URL (Uniform Resource Locator)]] und [[URN (Uniform Resource Name)]]

- URLs geben den Standort und das Zugriffsprotokoll an.
- URNs sind ortsunabhängig und dienen als dauerhafte, eindeutige Identifikatoren.

## URIs in Bezug auf APIs

URIs sind ein grundlegender Bestandteil von APIs, insbesondere RESTful APIs, wo sie verwendet werden, um Endpunkte und Ressourcen zu identifizieren. Die Struktur und Handhabung von URIs sind entscheidend für die Sicherheit und Funktionalität von APIs.

### Bedeutung im Kontext von APIs

1. **Ressourcenidentifikation**: URIs identifizieren Ressourcen eindeutig, was die Organisation und den Zugriff auf [[API]]-Endpunkte erleichtert.
2. **Stateless Interactions**: In RESTful APIs ermöglichen URIs die stateless Interaktionen, bei denen jede Anfrage alle Informationen enthält, die zur Bearbeitung erforderlich sind.
3. **Hierarchische Struktur**: URIs bieten eine hierarchische Struktur, die die Navigation und Organisation von Ressourcen vereinfacht.

## URI-Schwachstellen und Bug Bounty

Im Kontext von Bug-Bounty-Programmen sind URIs von besonderem Interesse, da sie potenzielle Angriffsvektoren darstellen. Zu den häufigsten Schwachstellen gehören:

### URL Redirection

Ein Angreifer kann eine Schwachstelle ausnutzen, die es ihm ermöglicht, Benutzer auf eine bösartige Website umzuleiten. Dies kann zu Phishing-Angriffen oder der Verbreitung von Malware führen.

### Injection-Angriffe

Durch das Einfügen bösartigen Codes in Teile eines URI (z.B. Query-Parameter) können Angreifer SQL-Injection, Cross-Site Scripting (XSS) oder andere Injektionsangriffe durchführen.

### Directory Traversal

Ein Angreifer kann spezielle Zeichenfolgen in den Pfad eines URI einfügen, um Zugriff auf Verzeichnisse und Dateien außerhalb des vorgesehenen Pfads zu erhalten, was zu unberechtigtem Zugriff auf sensible Daten führen kann.

## Sicherheitsmaßnahmen

Um die Sicherheit von URIs zu gewährleisten, sollten folgende Maßnahmen ergriffen werden:

1. **Eingabevalidierung und -bereinigung**: Stellen Sie sicher, dass alle Teile eines URI ordnungsgemäß validiert und bereinigt werden, um Injektionen und andere Angriffe zu verhindern.
2. **Vermeidung von Weiterleitungen**: Minimieren Sie die Verwendung von offenen Weiterleitungen und überprüfen Sie die Ziel-URIs auf Vertrauenswürdigkeit.
3. **Zugriffskontrollen**: Implementieren Sie strenge Zugriffskontrollen, um sicherzustellen, dass Benutzer nur auf die Ressourcen zugreifen können, für die sie berechtigt sind.
4. **Protokollierung und Überwachung**: Überwachen und protokollieren Sie den Zugriff auf URIs, um verdächtige Aktivitäten zu erkennen und darauf reagieren zu können.

## Schlussfolgerung

URIs sind ein wesentlicher Bestandteil des Webs und der APIs und spielen eine entscheidende Rolle bei der Identifizierung und dem Zugriff auf Ressourcen. Sie bieten eine strukturierte und flexible Methode zur Ressourcenzugriff, sind jedoch auch anfällig für verschiedene Sicherheitsbedrohungen. Im Kontext von Bug-Bounty-Programmen sind URIs ein kritischer Untersuchungsbereich, da ihre korrekte Implementierung und Sicherung entscheidend für den Schutz von Webanwendungen ist. Durch das Verständnis der Struktur und Funktionsweise von URIs sowie der Implementierung geeigneter Sicherheitsmaßnahmen können Entwickler und Sicherheitsforscher dazu beitragen, die Sicherheit und Integrität von Webanwendungen zu gewährleisten.