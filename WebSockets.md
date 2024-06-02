
## Einleitung

WebSockets sind ein Protokoll für die bidirektionale Kommunikation zwischen einem Client und einem Server über eine einzige, persistente Verbindung. Im Gegensatz zu [[HTTP]], das auf einer Anfrage-Antwort-Basis funktioniert, ermöglichen WebSockets Echtzeit-Kommunikation mit geringer Latenz. Dies ist besonders nützlich für Anwendungen, die schnelle und kontinuierliche Datenübertragung erfordern, wie Chat-Anwendungen, Online-Spiele und Echtzeit-Dashboards. In dieser Note werden die technischen Aspekte und verschiedenen Arten von WebSockets untersucht, wobei besonderes Augenmerk auf Sicherheitsaspekte und ihre Bedeutung im Kontext von Bug-Bounty-Programmen gelegt wird.

## Funktionsweise von WebSockets

### Verbindungsaufbau

Der WebSocket-Verbindungsaufbau beginnt mit einem [[HTTP-Handshake]], bei dem der Client eine [[HTTP]]-Anfrage mit einem speziellen Upgrade-Header sendet, um die Verbindung von [[HTTP]] auf WebSocket zu wechseln. Wenn der Server die Anfrage akzeptiert, wird die Verbindung auf WebSocket umgestellt.

### Datenübertragung

Nach dem erfolgreichen Handshake können Daten in beide Richtungen über die gleiche Verbindung gesendet werden. Diese Kommunikation erfolgt über Frames, die entweder Text- oder Binärdaten enthalten können. Die Verbindung bleibt offen, bis sie explizit von einer der beiden Parteien geschlossen wird.

### Vorteile von WebSockets

- **Echtzeit-Kommunikation**: Ermöglicht sofortigen Datenaustausch ohne die Notwendigkeit wiederholter Anfragen.
- **Effizienz**: Reduziert den Overhead im Vergleich zu wiederholten HTTP-Anfragen.
- **Persistente Verbindung**: Hält eine dauerhafte Verbindung aufrecht, wodurch die Latenz minimiert wird.

## Arten von WebSockets

WebSockets selbst sind ein standardisiertes Protokoll, es gibt jedoch verschiedene Implementierungen und Erweiterungen, die spezifische Anforderungen erfüllen können.

### Standard WebSockets

Die Standard-WebSocket-API wird von den meisten modernen Webbrowsern und Webservern unterstützt. Sie bietet grundlegende Funktionen für die bidirektionale Kommunikation.

### Secure WebSockets (WSS)

WSS ist die verschlüsselte Version des WebSocket-Protokolls, die [[SSL]]/[[TLS]] verwendet, um die Datenübertragung zu sichern. Dies ist besonders wichtig für Anwendungen, die sensible Daten übertragen.

### Multiplexing WebSockets

Multiplexing ermöglicht es mehreren unabhängigen Kommunikationsströmen, über eine einzige WebSocket-Verbindung zu laufen. Dies kann die Effizienz erhöhen und die Verwaltung von Verbindungen vereinfachen.

### WebSocket Subprotocols

WebSocket-Subprotokolle definieren zusätzliche Anwendungslogiken und Kommunikationsprotokolle, die auf WebSockets aufbauen. Beispiele sind:
- **STOMP (Simple Text Oriented Messaging Protocol)**: Wird häufig in Messaging-Systemen verwendet.
- **AMQP (Advanced Message Queuing Protocol)**: Ein Protokoll für Nachrichtenorientierte Middleware.
- **MQTT (Message Queuing Telemetry Transport)**: Leichtgewichtiges Protokoll für IoT (Internet of Things).

## Sicherheitsaspekte von WebSockets

### Cross-Site WebSocket Hijacking (CSWSH)

Ein Angreifer könnte versuchen, eine WebSocket-Verbindung eines authentifizierten Benutzers zu kapern, indem er eine bösartige Webseite nutzt, um Anfragen an den WebSocket-Server zu senden. Um dies zu verhindern, sollten Server die Herkunft der Anfrage überprüfen.

### Man-in-the-Middle (MitM) Angriffe

Wie bei jeder Netzwerkkommunikation besteht die Gefahr von Man-in-the-Middle-Angriffen. Die Verwendung von WSS (WebSocket Secure) ist entscheidend, um die Datenübertragung zu verschlüsseln und vor Abhörversuchen zu schützen.

### DoS (Denial of Service) Angriffe

Angreifer könnten versuchen, den WebSocket-Server durch eine Flut von Anfragen zu überlasten. Mechanismen wie Rate Limiting und IP-Blockierung können helfen, solche Angriffe zu mitigieren.

### Injection Angriffe

Unzureichend validierte Daten, die über WebSockets übertragen werden, können zu Injektionsangriffen wie SQL-Injection oder XSS (Cross-Site Scripting) führen. Eingabevalidierung und -sanitierung sind daher unerlässlich.

## WebSockets in Bezug auf Bug Bounty

WebSockets sind ein wichtiger Bereich in Bug-Bounty-Programmen, da ihre Implementierung oft komplex ist und spezifische Sicherheitsanforderungen mit sich bringt. Sicherheitsforscher konzentrieren sich häufig auf die folgenden Aspekte:

### Handshake-Sicherheit

Überprüfung, ob der WebSocket-Handshake korrekt implementiert ist und keine Schwachstellen aufweist, die von Angreifern ausgenutzt werden können.

### Datenvalidierung

Sicherstellen, dass alle Daten, die über WebSockets gesendet und empfangen werden, ordnungsgemäß validiert und sanitisiert werden, um Injektionsangriffe zu verhindern.

### [[Authentifizierung]] und Autorisierung

Überprüfung, ob WebSocket-Verbindungen sicher authentifiziert und autorisiert werden, um unbefugten Zugriff zu verhindern.

### Verbindungsmanagement

Überprüfung auf potenzielle Schwachstellen im Verbindungsmanagement, wie z.B. fehlende Rate Limits oder unzureichender Schutz vor DoS-Angriffen.

## Schlussfolgerung

WebSockets bieten eine leistungsstarke und effiziente Methode für die Echtzeit-Kommunikation in modernen Webanwendungen. Ihre korrekte Implementierung und Absicherung sind entscheidend, um die Integrität und Sicherheit der übertragenen Daten zu gewährleisten. Im Kontext von Bug-Bounty-Programmen sind WebSockets ein kritischer Prüfbereich, da sie komplexe Angriffsvektoren bieten, die sorgfältig analysiert und gesichert werden müssen. Durch das Verständnis der verschiedenen Arten von WebSockets und der potenziellen Sicherheitsrisiken können Entwickler und Sicherheitsforscher robuste und sichere WebSocket-Anwendungen entwickeln.