
## Was ist Subresource Integrity (SRI)?

Subresource Integrity (SRI) ist eine Sicherheitsmaßnahme im Web, die es Browsern ermöglicht, die Integrität von Ressourcen wie Skripten und Stylesheets zu überprüfen. Diese Überprüfung erfolgt durch das Einbetten eines kryptografischen Hashes in das HTML-Tag, welches die Ressource lädt. 

### Beispiel:

```html
<script src="https://example.com/script.js" integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxQ6zPC7JSRjGd5I5qFuKiv1p+0Uexg" crossorigin="anonymous"></script>
```

## Bedeutung von SRI

- **Sicherheit:** Durch die Verwendung von SRI wird sichergestellt, dass die geladene Ressource nicht manipuliert wurde, was vor Man-in-the-Middle-Angriffen schützt.
- **Integrität:** SRI garantiert, dass die geladenen Daten exakt mit den vom Entwickler bereitgestellten übereinstimmen.

## Fehlende SRI in Bug-Bounty Programmen

### Relevanz

Fehlende SRI-Attribute stellen ein erhebliches Sicherheitsrisiko dar, da manipulierte Ressourcen nicht erkannt werden. Dies ist besonders kritisch bei der Nutzung von Content Delivery Networks (CDNs) oder anderen externen Ressourcen. In Bug-Bounty Programmen kann das Fehlen von SRI-Hashes als Sicherheitslücke gemeldet werden.

### Identifikation von `missing-sri`

1. **Quellcode-Analyse:**
   - Überprüfen Sie den HTML-Quellcode auf `<script>`- und `<link>`-Tags ohne `integrity`-Attribut.
   
2. **Automatisierte Tools:**
   - Verwenden Sie Tools und Browser-Erweiterungen, die Webseiten auf fehlende SRI-Attribute überprüfen.

## Exploitation

### Möglichkeiten der Ausnutzung

Ein Angreifer könnte fehlende SRI-Hashes ausnutzen, um bösartige Inhalte einzuschleusen. Dies könnte geschehen, indem der Angreifer die Kontrolle über die extern gehosteten Ressourcen übernimmt oder den Netzwerkverkehr abfängt und manipuliert. Szenarien, die in Betracht gezogen werden müssen, umfassen:

- **Man-in-the-Middle-Angriffe:** Angreifer könnten den Netzwerkverkehr abfangen und die Ressource durch eine bösartige Version ersetzen.
- **Kompromittierte CDN:** Wenn ein CDN gehackt wird, können alle darüber ausgelieferten Ressourcen manipuliert werden, wenn sie nicht durch SRI abgesichert sind.

### Praktische Durchführung

1. **Manuelle Prüfung:**
   - Untersuchen Sie den HTML-Quellcode der Zielwebseite auf `<script>`- und `<link>`-Tags, die externe Ressourcen laden.
   - Notieren Sie sich alle Tags, die kein `integrity`-Attribut enthalten.
   
2. **Automatisierte Tools:**
   - **Subresource Integrity Checker:** Ein Tool, das den Quellcode einer Webseite analysiert und fehlende SRI-Attribute identifiziert.
   - **Browser-Erweiterungen:** Erweiterungen wie „SRI Checker“ für Chrome und Firefox können Webseiten automatisch auf fehlende SRI-Attribute überprüfen.

### Tools zur Identifikation und Überprüfung

- **SRI Hash Generator:** Generiert SRI-Hashes für eine gegebene Ressource. Ein Beispiel ist der [SRI Hash Generator von SRI Hash](https://www.srihash.org/).
- **Burp Suite:** Kann verwendet werden, um den Netzwerkverkehr zu analysieren und zu überprüfen, ob Ressourcen ohne SRI-Hash geladen werden.
- **Subresource Integrity Checker:** Ein spezifisches Tool zur Überprüfung von Webseiten auf fehlende SRI-Attribute. Ein Beispiel ist [sri-checker](https://github.com/tealblue/sri-checker).

### Beispiel für eine Bug-Bounty-Meldung

**Titel:** Fehlende Subresource Integrity (SRI) für externes Skript

**Beschreibung:**
Auf der Webseite `https://example.com` wurde festgestellt, dass das externe Skript `https://cdn.example.com/script.js` ohne Subresource Integrity (SRI) geladen wird. Dies stellt ein Sicherheitsrisiko dar, da die Integrität der geladenen Ressource nicht sichergestellt ist.

**Schritte zur Reproduktion:**
1. Öffnen Sie die Webseite `https://example.com`.
2. Untersuchen Sie den Quellcode und finden Sie das `<script>`-Tag, das auf `https://cdn.example.com/script.js` verweist.
3. Beachten Sie das Fehlen des `integrity`-Attributs.

**Erwartetes Ergebnis:**
Das `<script>`-Tag sollte ein `integrity`-Attribut enthalten, das einen kryptografischen Hash der Ressource enthält.

**Empfohlene Lösung:**
Fügen Sie das `integrity`-Attribut zum `<script>`-Tag hinzu, um die Integrität der geladenen Ressource sicherzustellen.