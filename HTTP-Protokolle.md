
## Einleitung

Das Hypertext Transfer Protocol ([[HTTP]]) ist das Fundament des World Wide Web und dient als Protokoll für die Übertragung von Hypertext-Dokumenten. [[HTTP]] definiert, wie Nachrichten formatiert und übertragen werden, sowie welche Aktionen Webserver und Browser als Reaktion auf verschiedene Befehle ausführen sollen. In dieser Note werden die verschiedenen Versionen und Eigenschaften des [[HTTP]]-Protokolls wissenschaftlich und technisch betrachtet, wobei der Schwerpunkt auf [[API]]-Sicherheit im Kontext von Bug-Bounty-Programmen liegt.

## HTTP/0.9

[[HTTP]]/0.9 war die erste Version des Protokolls, eingeführt 1991. Es war sehr einfach und unterstützte nur GET-Anfragen. [[HTTP]]/0.9 spezifizierte keine Header und war auf die Übertragung von reinen HTML-Dateien beschränkt.

### Merkmale
- Nur [[GET-Methode]]
- Keine [[HTTP-Header]]
- Unterstützung nur für reine HTML-Dokumente

## HTTP/1.0

[[HTTP]]/1.0, eingeführt 1996, erweiterte das ursprüngliche Protokoll erheblich. Es führte [[HTTP-Header]] ein und unterstützte zusätzliche Methoden wie POST und HEAD. Mit [[HTTP]]/1.0 wurden auch Statuscodes und die Möglichkeit eingeführt, nicht-HTML-Inhalte zu übertragen.

### Merkmale
- Unterstützung für Methoden: GET, POST, HEAD
- Einführung von [[HTTP]]-Headern
- Verwendung von Statuscodes
- Unterstützung für verschiedene MIME-Typen

## HTTP/1.1

[[HTTP]]/1.1, eingeführt 1997, ist die am weitesten verbreitete Version des [[HTTP]]-Protokolls. Es bietet zahlreiche Verbesserungen gegenüber [[HTTP]]/1.0, darunter persistente Verbindungen, Chunked Transfer Encoding und erweiterte Cache-Steuerung.

### Merkmale
- Persistente Verbindungen (Keep-Alive)
- Chunked Transfer Encoding
- Verbesserte Cache-Steuerung
- Unterstützung für zusätzliche Methoden: PUT, DELETE, OPTIONS, TRACE
- Feinere Kontrolle über [[HTTP-Header]]

## HTTP/2

[[HTTP]]/2, standardisiert 2015, bringt signifikante Leistungsverbesserungen durch binäre Protokollierung und Unterstützung für Multiplexing. Es ermöglicht parallele Übertragungen über eine einzige [[TCP]]-Verbindung und reduziert Latenzzeiten durch Kompression von Headern.

### Merkmale
- Binäres Protokoll
- Multiplexing von Anfragen
- Header-Kompression (HPACK)
- Server Push

## HTTP/3

[[HTTP]]/3, basierend auf dem QUIC-Protokoll, ist die neueste Version des [[HTTP]]-Protokolls. Es verwendet UDP anstelle von [[TCP]], um Verbindungsaufbauzeiten zu reduzieren und Latenzprobleme zu minimieren.

### Merkmale
- Verwendung von QUIC über UDP
- Schnellere Verbindungsaufbauzeiten
- Bessere Handhabung von Netzwerkwechseln und Paketverlusten
- Verbesserte Leistung und Sicherheit

## HTTP-Protokolle in Bezug auf [[API]]-Sicherheit und Bug Bounty

[[HTTP]]-Protokolle sind ein zentrales Element bei der Implementierung und Sicherung von APIs. Im Kontext von Bug-Bounty-Programmen sind verschiedene Aspekte der [[HTTP]]-Protokolle relevant:

### Sicherheitsaspekte

- **Verbindungsverschlüsselung**: Die Verwendung von [[HTTPS]] ([[HTTP]] über [[TLS]]) ist entscheidend, um Daten während der Übertragung zu schützen. [[Bug-Bounty-Programme]] prüfen oft auf die ordnungsgemäße Implementierung von [[HTTPS]].
- **Header-Sicherheit**: [[HTTP]]-Header wie `Content-Security-Policy`, `Strict-Transport-Security` und `X-Content-Type-Options` sind wichtig für den Schutz vor Angriffen wie XSS und MIME-Sniffing.
- **[[Authentifizierung]] und Autorisierung**: Mechanismen wie OAuth 2.0 und [[API]]-Schlüssel werden verwendet, um den Zugriff auf APIs zu steuern. Sicherheitsforscher untersuchen diese Implementierungen auf Schwachstellen wie fehlende oder unzureichende Autorisierung.
- **Rate Limiting und DDoS-Schutz**: [[HTTP]]/2 und [[HTTP]]/3 bieten Mechanismen zur effizienten Handhabung von Anfragen. Sicherheitsforscher prüfen auf Schwachstellen in der Implementierung von Rate Limiting und Schutzmaßnahmen gegen DDoS-Angriffe.

### Schwachstellenanalyse

- **Injektionsangriffe**: Angriffe wie SQL-Injection und Command Injection können über unsichere [[API]]-Endpunkte erfolgen. Die Analyse von [[HTTP]]-Anfragen und -Antworten hilft, solche Schwachstellen zu identifizieren.
- **Fehlkonfigurationen**: Fehlkonfigurierte [[HTTP]]-Header oder fehlerhafte Implementierungen von [[HTTP]]-Protokollen können Sicherheitslücken verursachen. [[Bug-Bounty-Programme]] suchen gezielt nach solchen Konfigurationsfehlern.
- **Protokoll-Exploits**: Protokollspezifische Schwachstellen wie die Ausnutzung von [[HTTP]]/2-CVE (Common Vulnerabilities and Exposures) sind ein Fokusbereich für Sicherheitsforscher.

## Schlussfolgerung

[[HTTP]]-Protokolle sind ein wesentlicher Bestandteil des Internets und der modernen Webentwicklung. Durch das Verständnis der verschiedenen Versionen und ihrer Eigenschaften können Entwickler und Sicherheitsforscher sicherere und leistungsfähigere Webanwendungen und APIs erstellen. Im Kontext von Bug-Bounty-Programmen spielen [[HTTP]]-Protokolle eine zentrale Rolle, da sie zahlreiche potenzielle Angriffsvektoren bieten. Eine gründliche Analyse und Absicherung dieser Protokolle ist entscheidend, um die Integrität und Sicherheit von Webdiensten zu gewährleisten.