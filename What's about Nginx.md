# Was ist Nginx? Ein grundlegender Blick darauf, was es ist und wie es funktioniert.


Nginx, ist ein Open-Source-Webserver, der seit seinem anfänglichen Erfolg als Webserver auch als Reverse-Proxy, [[HTTP-Cache]] und Load-Balancer verwendet wird.

Einige namhafte Unternehmen, die Nginx einsetzen, sind:
"Autodesk, Atlassian, Intuit, T-Mobile, GitLab, DuckDuckGo, Microsoft, IBM, Google, Adobe, Salesforce, VMware, Xerox, LinkedIn, Cisco, Facebook, Target, Citrix Systems, Twitter und Apple, Intel und viele mehr" (Quelle).

Nginx wurde ursprünglich von Igor Sysoev mit seiner ersten Veröffentlichung im Oktober 2004 erstellt. Igor konzipierte die Software zunächst als Antwort auf das C10k-Problem, bei dem es um das Leistungsproblem bei der Verarbeitung von 10.000 gleichzeitigen Verbindungen geht.

Da seine Wurzeln in der Leistungsoptimierung unter dem Maßstab liegen, übertrifft Nginx in Benchmark-Tests häufig andere gängige Webserver, insbesondere in Situationen mit statischem Inhalt und / oder hohen gleichzeitigen Anforderungen, weshalb zur Stromversorgung seines Hostings Kinsta Nginx nutzt.

	+ Wie funktioniert Nginx?
	+ Nginx vs Apache Nutzungsstatistik
	+ So prüfst du, ob du NGINX oder Apache ausführst

## Wie funktioniert Nginx?

Nginx wurde entwickelt, um eine geringe Speichernutzung und eine hohe Parallelität zu gewährleisten. Anstatt für jede Webanforderung neue Prozesse zu erstellen, verwendet Nginx einen asynchronen, ereignisgesteuerten Ansatz, bei dem Anforderungen in einem einzigen Thread verarbeitet werden.

Mit Nginx kann ein Master-Prozess mehrere Worker-Prozesse steuern. Der Master verwaltet die Worker-Prozesse, während die Worker die eigentliche Verarbeitung durchführen. Da Nginx asynchron ist, kann jede Anforderung vom Worker gleichzeitig ausgeführt werden, ohne andere Anforderungen zu blockieren.

Zu den allgemeinen Funktionen von Nginx gehören:

	+ Reverse proxy mit Caching
	+ IPv6
	+ Load Balancing
	+ FastCGI Unterstützung mit Caching
	+ WebSockets
	+ Umgang mit statischen Dateien, Indexdateien und automatischer Indizierung
	+ TLS/SSL mit SNI

## Nginx vs Apache Nutzungsstatistik
 
Apache ist ein weiterer beliebter Open-Source-Webserver. Bezogen auf die Anzahl der Rohdaten ist Apache der beliebteste Webserver und wird laut W3Techs von 43,6% (nach 47% im Jahr 2018) aller Websites mit einem bekannten Webserver verwendet. Nginx belegt mit 41,8% einen knappen zweiten Platz.

Netcraft führte eine Umfrage über 233 Millionen Domains durch und stellte eine Apache-Auslastung von 31,54% und eine Nginx-Auslastung von 26,20% fest.



Webserver-Entwickler: Marktanteil von Domains
Webserver-Entwickler: Marktanteil von Domains (Bildquelle: Netcraft)

Während Apache die beliebteste Option ist, ist Nginx der beliebteste Webserver unter Websites mit hohem Datenverkehr.

Wenn du die Nutzungsraten nach Datenverkehr aufschlüsselst, Nginx versorgt:

	+ 60,9% der 100.000 beliebtesten Websites (gegenüber 56,1% im Jahr 2018)
	+ 67,1% der 10.000 beliebtesten Websites (gegenüber 63,2% im Jahr 2018)
	+ 62,1% der 1.000 beliebtesten Websites (gegenüber 57% im Jahr 2018)

Tatsächlich wird Nginx von einigen der ressourcenintensivsten Websites verwendet, darunter Netflix, die NASA und sogar WordPress.com.

Die Verwendung von Apache bewegt sich dagegen in die entgegengesetzte Richtung, wenn der Datenverkehr einer Seite zunimmt. Es versorgt:

	+ 24,0% der 100.000 beliebtesten Websites (gegenüber 27,1% im Jahr 2018)
	+ 18,8% der 10.000 beliebtesten Websites (gegenüber 21,5% im Jahr 2018)
	+ 16,6% der 1.000 beliebtesten Websites (gegenüber 16,2% im Jahr 2018)

Wenn wir uns die Google Search-Begriffe seit 2004 ansehen, sehen wir, dass Apache stetig zurückgeht, während NGINX ein leichtes Wachstum verzeichnet.



Nginx vs Apache
Nginx vs Apache

Auch wenn du bedenkst, dass NGINX eine bessere Leistung erzielt, ist es nicht verwunderlich, dass Websites mit hohem Datenaufkommen sich für NGINX über Apache entscheiden. Sieh dir unseren ausführlichen Vergleich von Nginx vs Apache.
So prüfst du, ob du NGINX oder Apache ausführst

Auf den meisten Websites kannst du einfach den Server [[HTTP-Header]] überprüfen, um festzustellen, ob Nginx oder Apache angezeigt wird. Du kannst [[HTTP-Header]] anzeigen, indem du die Registerkarte Netzwerk in Chrome Devtools öffnest. Oder du kannst die Überschriften in einem Tool wie Pingdom oder GTmetrix überprüfen.

Der [[HTTP-Header]] zeigt jedoch möglicherweise nicht immer den zugrunde liegenden Webserver an. Befindet sich deine WordPress-Seite beispielsweise hinter einem Proxy-Dienst wie Cloudflare, wird im Server [[HTTP-Header]] stattdessen Cloudflare angezeigt.



Nginx [[HTTP]] Header
Nginx [[HTTP]] Header

Bringen Sie alle Ihre Anwendungen, Datenbanken und WordPress Seiten online und unter einem Dach unter. Unsere ausgewählte, leistungsstarke Cloud-Plattform umfasst:

	+ Einfache Einrichtung und Verwaltung über das MyKinsta Dashboard
	+ 24/7 Experten Support
	+ Die beste Hardware und das beste Netzwerk der Google Cloud Platform, unterstützt durch Kubernetes für maximale Skalierbarkeit
	+ Eine Cloudflare-Integration auf Unternehmensebene für Geschwindigkeit und Sicherheit
	+ Globale Reichweite mit bis zu 35 Rechenzentren und 260 PoPs weltweit

