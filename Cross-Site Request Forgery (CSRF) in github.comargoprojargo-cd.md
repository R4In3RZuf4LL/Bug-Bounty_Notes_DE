# Cross-Site Request Forgery (CSRF) in github.com/argoproj/argo-cd

CVE-2024-22424
Severity: High

## Impact

Die Argo CD-[[API]] vor den Versionen 2.10-rc2, 2.9.4, 2.8.8 und 2.7.16 ist anfällig für einen Cross-Server-Request-Forgery-Angriff (CSRF), 
wenn der Angreifer die Möglichkeit hat, HTML in eine Seite auf der zu schreiben gleiche übergeordnete Domäne wie Argo CD.
Bei einem CSRF-Angriff wird ein authentifizierter Argo CD-Benutzer dazu verleitet, eine Webseite zu laden, die Code zum Aufrufen von Argo CD-[[API]]-Endpunkten im Namen des Opfers enthält. 
Beispielsweise könnte ein Angreifer einem Argo CD-Benutzer einen Link zu einer Seite senden, die harmlos aussieht, 
aber im Hintergrund einen Argo CD-[[API]]-Endpunkt aufruft, um eine Anwendung zu erstellen, auf der Schadcode ausgeführt wird.
Argo CD verwendet die „Lax“-Cookie-Richtlinie von SameSite, um CSRF-Angriffe zu verhindern, bei denen der Angreifer eine externe Domäne kontrolliert. 
Die bösartige externe Website kann versuchen, die Argo CD-[[API]] aufzurufen, aber der Webbrowser weigert sich, das Argo CD-Authentifizierungstoken mit der Anfrage zu senden.
Viele Unternehmen hosten Argo CD auf einer internen Subdomain, beispielsweise https://argo-cd.internal.example.com. 
Wenn ein Angreifer bösartigen Code beispielsweise auf https://test.internal.example.com/ platzieren kann, kann er dennoch einen CSRF-Angriff durchführen. 
In diesem Fall verhindert das „Lax“-SameSite-Cookie nicht, dass der Browser das Authentifizierungs-Cookie sendet, da das Ziel eine übergeordnete Domäne der Argo CD-[[API]] ist.
Browser blockieren solche Angriffe im Allgemeinen, indem sie [[CORS-Richtlinien]] auf sensible Anfragen mit sensiblen Inhaltstypen anwenden. 
Anfrage“ für POSTs mit dem Inhaltstyp „application/json“ und fragen die Ziel-[[API]]: „Dürfen Sie Anfragen von meiner Domain annehmen?“ 
Wenn die Ziel-[[API]] nicht mit „Ja“ antwortet, blockiert der Browser die Anfrage.
Vor den gepatchten Versionen überprüfte Argo CD nicht, ob Anfragen den richtigen Inhaltstyp-Header enthielten. 
Ein Angreifer könnte also die [[CORS-Prüfung]] des Browsers umgehen, indem er den Inhaltstyp auf etwas einstellt, das als „nicht sensibel“ gilt, beispielsweise „Text/Plain“. 
Der Browser würde die Preflight-Anfrage nicht senden und Argo CD würde die Inhalte (die eigentlich immer noch JSON sind) gerne akzeptieren und die angeforderte Aktion ausführen (z. B. das Ausführen von Schadcode).

## Patches

Ein Patch für diese Schwachstelle wurde in den folgenden Argo-CD-Versionen veröffentlicht:
 
	+ 2.10-rc2
	+ 2.9.4
	+ 2.8.8
	+ 2.7.16

Der Patch enthält eine bahnbrechende [[API]]-Änderung. Die Argo CD-[[API]] akzeptiert keine Nicht-GET-Anfragen mehr, die application/json nicht als Inhaltstyp angeben. 
Die Liste der akzeptierten Inhaltstypen ist konfigurierbar und es ist möglich (aber nicht empfehlenswert), die Inhaltstypprüfung vollständig zu deaktivieren.

## Workarounds

Die einzige Möglichkeit, das Problem vollständig zu beheben, ist ein Upgrade.

### Credits

Das Argo CD-Team möchte An Trinh of Calif seinen Dank aussprechen, der das Problem vertraulich gemäß unseren Richtlinien gemeldet und einen hilfreichen Blogbeitrag zur Beschreibung des Problems veröffentlicht hat. 
Wir möchten uns auch für die aktive Teilnahme an der Überprüfung des Patches bedanken.

#### References

	+ Das Problem wurde ursprünglich in einem GitHub-Problem gemeldet