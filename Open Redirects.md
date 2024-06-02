> Seiten benutzen oft [[HTTP]] oder [[URL (Uniform Resource Locator)]] Parameter um Benutzer ohne eigene Interaktion zu einer spezifischen [[URL (Uniform Resource Locator)]] weiterzuleiten. Das kann nützlich sein, aber auch zu Open Redirects führen. Dies kann passieren, wenn ein Angreifer die Möglichkeit hat, den Wert zu dem Parameter so zu manipulieren, das er den Benutzer auf eine andere [[URL (Uniform Resource Locator)]] umleiten kann. 

## Mechanismus

Webseiten brauchen oft die Möglichkeit, ihre User automatisch umzuleiten. Zum Beispiel, passiert dieses Szenario oft, wenn unauthentifizierte Benutzer versuchen eine Webseite zu besuchen, die ein Login benötigt. Die Webseite wird diese User üblicherweise zur Login Seite umleiten, um ihn dann wieder auf seine Ursprüngliche Location zu bringen. Zum Beispiel, wenn der User sein Account Dashboard auf https://example.com/dashboard/ aufrufen will, wird er üblicherweise auf https://example.com/login weitergeleitet. Um den Benutzer nach dem Login wieder auf seine ursprüngliche Position umzuleiten, muss die Applikation sich merken, wo der Benutzer ursprünglich hin wollte. Deswegen nutzt die Seite eine Art von Redirect [[URL (Uniform Resource Locator)]] Parameter, der an die [[URL (Uniform Resource Locator)]] angehängt wird, um die ursprüngliche Position zu merken. Dieser Parameter zeigt an, wohin der Benutzer nach dem Login weitergeleitet werden soll. Zum Beispiel die [[URL (Uniform Resource Locator)]] https://example.com/login?redirect=https://example.com/dashboard. leitet nach dem Login wieder auf die Dashboard [[URL (Uniform Resource Locator)]]. Oder der Benutzer wollte zu seinen Einstellungen, dann lautet die [[URL (Uniform Resource Locator)]] https://example.com/login?redirect=https://example.com/settings. Automatische Redirects sparen dem Benutzer Zeit und bringen eine bessere Benutzer Erfahrung. 

## Angriff

Durch eine Open Redirect Attacke versucht ein Angreifer den Benutzer auszutricksen und auf eine externe Seite umzuleiten, indem er die legitime Seite an den Benutzer schickt, die diesen woanders hinleitet, wie zum Beispiel: https://example.com/login?redirect=https://attacker.com. Eine [[URL (Uniform Resource Locator)]] wie diese, kann ein Opfer dazu verleiten, den Link zu klicken, weil er denkt, dass er zu https://example.com leitet. In Wahrheit leitet der Link aber automatisch auf eine bösartige Seite um. Angreifer können eine Phishing Attacke starten und das Opfer dazu verleiten, seine example.com Benutzerdaten einzugeben. 

Eine weitere beliebte Angriffstechnik ist Referer-Based Open Redirects. Der Referer ist ein [[HTTP]]-[[Request-Header]], den Browser automatisch einbinden um zu wissen, wo der Request ursprünglich herkam. 

## Prävention

Um Open Redirects zu vermeiden, muss der Server sich sicher sein, nicht auf bösartige Seiten weiterzuleiten. Seiten implementieren oft [[URL (Uniform Resource Locator)]] Validators um sicherzugehen, dass 


## Hunting for Open Redirects