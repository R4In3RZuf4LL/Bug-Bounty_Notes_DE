+ GET
	+ ist die gebräuchlichste Methode. Mit ihr wird eine Ressource (zum Beispiel eine Datei) unter Angabe eines [[URI (Uniform Resource Identifier)]] vom Server angefordert. Als Argumente in dem [[URI (Uniform Resource Identifier)]] können auch Inhalte zum Server übertragen werden, allerdings soll laut Standard eine GET-Anfrage nur Daten abrufen und sonst keine Auswirkungen haben (wie Datenänderungen auf dem [[Server]] oder ausloggen). (siehe unten)
+ POST
    + schickt Daten (z. B. den Inhalt einer Datei) zur weiteren Verarbeitung zum Server, diese werden als Inhalt der Nachricht übertragen und können beispielsweise aus Name-Wert-Paaren bestehen, die aus einem HTML-Formular stammen. Es können so neue Ressourcen auf dem Server entstehen oder bestehende modifiziert werden. POST-Daten werden im Allgemeinen nicht von Caches zwischengespeichert. Zusätzlich können bei dieser Art der Übermittlung auch Daten wie in der [[GET-Methode]] an den [[URI (Uniform Resource Identifier)]] gehängt werden. (siehe unten)
+ HEAD
    + weist den Server an, die gleichen [[HTTP-Header]] wie bei GET, nicht jedoch den Nachrichtenrumpf mit dem eigentlichen Dokumentinhalt zu senden. So kann zum Beispiel schnell die Gültigkeit einer Datei im Browser-Cache geprüft werden, und Dateigrößen können im Voraus abgerufen und durch die Content-Length-Zeile erlesen werden.
+ PUT
    + dient dazu, eine Ressource (zum Beispiel eine Datei) unter Angabe des Ziel-URIs auf einen Webserver hochzuladen. Besteht unter der angegebenen Ziel-[[URI (Uniform Resource Identifier)]] bereits eine Ressource, wird diese ersetzt, ansonsten neu erstellt.
+ PATCH
    + Ändert ein bestehendes Dokument ohne dieses wie bei PUT vollständig zu ersetzen. Wurde durch [[RFC 5789]] spezifiziert.
+ DELETE
    + löscht die angegebene Ressource auf dem [[Server]].
+ TRACE
    + liefert die Anfrage so zurück, wie der Server sie empfangen hat. So kann überprüft werden, ob und wie die Anfrage auf dem Weg zum Server verändert worden ist – sinnvoll für das [[Debugging]] von Verbindungen.
+ OPTIONS
    + liefert eine Liste der vom [[Server]] unterstützten Methoden und Merkmale.
+ CONNECT
    + wird von Proxyservern implementiert, die in der Lage sind, [[SSL-Tunnel]] zur Verfügung zu stellen.

RESTful Web Services verwenden die unterschiedlichen Anfragemethoden zur Realisierung von Webservices. Insbesondere werden dafür die [[HTTP-Anfragemethoden]] GET, POST, PUT/PATCH und DELETE verwendet.

[[WebDAV]] fügt die Methoden PROPFIND, PROPPATCH, MKCOL, COPY, MOVE, LOCK und UNLOCK zu [[HTTP]] hinzu. 