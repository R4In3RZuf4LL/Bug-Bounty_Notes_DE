## Logging

Es ist wichtig, dass wir alle Scan- und Angriffsversuche protokollieren und die Rohausgabe des Tools nach Möglichkeit aufbewahren. Dies wird uns bei der Berichterstattung sehr helfen. Obwohl unsere Notizen klar und ausführlich sein sollten, übersehen wir möglicherweise etwas, und die Verwendung unserer Protokolle als Ausweichmöglichkeit kann uns helfen, entweder einem Bericht weitere Beweise hinzuzufügen oder auf eine Kundenfrage zu antworten.

### Exploitation Attempts

Die Tmux-Protokollierung ist eine ausgezeichnete Wahl für die Terminalprotokollierung, und wir sollten unbedingt Tmux zusammen mit der Protokollierung verwenden, da dadurch alles, was wir in einen Tmux-Bereich eingeben, in einer Protokolldatei gespeichert wird. Es ist auch wichtig, Missbrauchsversuche im Auge zu behalten, falls der Kunde die Ereignisse später in Beziehung setzen muss (oder in einer Situation, in der nur sehr wenige Erkenntnisse vorliegen und er Fragen zur durchgeführten Arbeit hat). Es ist äußerst peinlich, wenn Sie diese Informationen nicht vorlegen können, und es kann dazu führen, dass Sie als Penetrationstester unerfahren und unprofessionell aussehen. Es kann auch sinnvoll sein, den Überblick über Dinge zu behalten, die Sie während der Beurteilung versucht haben, aber nicht funktioniert haben. Dies ist besonders nützlich in den Fällen, in denen wir in Ihrem Bericht kaum oder gar keine Erkenntnisse haben. In diesem Fall können wir eine Beschreibung der durchgeführten Tests verfassen, damit der Leser verstehen kann, vor welchen Dingen er angemessen geschützt ist.

### Storage

Ähnlich wie bei unserer Notizen Struktur ist es eine gute Idee, einen Rahmen für die Organisation der während einer Bewertung gesammelten Daten zu entwickeln. Das mag bei kleineren Beurteilungen übertrieben erscheinen, aber wenn wir in einer großen Umgebung testen und nicht über eine strukturierte Möglichkeit verfügen, den Überblick zu behalten, vergessen wir am Ende etwas, verstoßen gegen die Einsatzregeln usw wahrscheinlich Dinge mehr als einmal zu tun, was eine große Zeitverschwendung sein kann, insbesondere während einer zeit begrenzten Beurteilung. Nachfolgend finden Sie eine empfohlene Grundstruktur für den Ordner. Möglicherweise müssen Sie diese jedoch entsprechend der Art der von Ihnen durchgeführten Bewertung oder den besonderen Umständen anpassen.

+ ***Admin***
	+ Arbeitsumfang (SoW), an dem Sie arbeiten, Ihre Notizen vom Projekt-Kickoff-Meeting, Statusberichte, Schwachstellenbenachrichtigungen usw
+ ***Deliverables***
	+ Ordner zum Aufbewahren Ihrer Ergebnisse, während Sie sie durcharbeiten. Dabei handelt es sich häufig um Ihren Bericht, kann jedoch je nach den spezifischen Kundenanforderungen auch andere Elemente wie ergänzende Tabellenkalkulationen und Foliensätze enthalten.
+ ***Evidence***
	+ Findings
		+ Wir empfehlen, für jedes Ergebnis, das Sie in den Bericht aufnehmen möchten, einen Ordner zu erstellen, um Ihre Beweise für jedes Ergebnis in einem Container aufzubewahren und das Zusammenstellen der exemplarischen Vorgehensweise beim Verfassen des Berichts zu erleichtern.
	+ Scans
		+ Vulnerability scans
			+ Exportieren Sie Dateien aus Ihrem Schwachstellenscanner (sofern für den Bewertungstyp zutreffend) zur Archivierung.
		+ Service Enumeration 
			+ Exportieren Sie Dateien aus Tools, die Sie zum Aufzählen von Diensten in der Zielumgebung verwenden, z. B. Nmap, Masscan, Rumble usw.
		+ Web
			+ Exportieren Sie Dateien für Tools wie ZAP- oder Burp-Statusdateien, EyeWitness, Aquatone usw.
		+ AD Enumeration
			+ JSON-Dateien von BloodHound, CSV-Dateien, die aus PowerView oder ADRecon generiert wurden, Ping Castle-Daten, Snaffler-Protokolldateien, CrackMapExec-Protokolle, Daten von Impacket-Tools usw.
	+ Notes
		+ Ein Ordner zum Aufbewahren Ihrer Notizen.
	+ OSINT
		+ Alle OSINT-Ausgaben von Tools wie Intelx und Maltego, die nicht gut in Ihr Notizendokument passen.
	+ Wireless
		+ Wenn Wireless-Tests im Umfang enthalten sind, können Sie diesen Ordner optional für die Ausgabe von Wireless-Testtools verwenden.
	+ Logging output
		+ Protokollierungsausgabe von Tmux, Metasploit und jeder anderen Protokollausgabe, die nicht in die oben aufgeführten Scan-Unterverzeichnisse passt.
	+ Misc Files
		+ Web-Shells, Payloads, benutzerdefinierte Skripte und alle anderen während der Bewertung generierten Dateien, die für das Projekt relevant sind.
+ Retests
	+ Dies ist ein optionaler Ordner, wenn Sie nach der ursprünglichen Beurteilung zurückkehren und die zuvor festgestellten Ergebnisse erneut testen müssen. Möglicherweise möchten Sie die Ordnerstruktur, die Sie bei der Erstbewertung verwendet haben, in diesem Verzeichnis reproduzieren, um Ihre Wiederholungstestnachweise von Ihren Originalnachweisen zu trennen.


Es ist eine gute Idee einen Befehl zum automatischen erstellen der Ordnerstruktur zu verwenden

```shell-session
mkdir -p ACME-IPT/{Admin,Deliverables,Evidence/{Findings,Scans/{Vuln,Service,Web,'AD Enumeration'},Notes,OSINT,Wireless,'Logging output','Misc Files'},Retest}
```

### Formatting and Redaction

Anmeldeinformationen und personenbezogene Daten (PII) sollten in Screenshots und allem, was moralisch anstößig wäre, wie z. B. grafisches Material oder vielleicht obszöne Kommentare und Sprache, geschwärzt werden. Sie können auch Folgendes in Betracht ziehen:

+ Fügen Sie dem Bild Anmerkungen wie Pfeile oder Kästchen hinzu, um die Aufmerksamkeit auf wichtige Elemente im Screenshot zu lenken, insbesondere wenn im Bild viel passiert ***(tun Sie dies nicht in MS Word)***.
+ Fügen Sie einen minimalen Rand um das Bild hinzu, damit es sich vom weißen Hintergrund des Dokuments abhebt.
+ Das Bild zuschneiden, um nur die relevanten Informationen anzuzeigen (z. B. anstelle einer Vollbildaufnahme, nur um ein einfaches Anmeldeformular anzuzeigen).
+ Fügen Sie die Adressleiste im Browser oder andere Informationen ein, die angeben, mit welcher [[URL (Uniform Resource Locator)]] oder welchem ​​Host Sie verbunden sind.

#### Screenshots

***Wo immer möglich, sollten wir versuchen, die Terminalausgabe anstelle von Screenshots des Terminals zu verwenden***. Es ist einfacher zu schwärzen, die wichtigen Teile hervorzuheben (d. h. der Befehl, den wir ausgeführt haben, in blauem Text und der Teil der Ausgabe, auf den wir aufmerksam machen wollen, in Rot), sieht im Dokument normalerweise ordentlicher aus und kann verhindern, dass das Dokument verdirbt eine riesige, unhandliche Datei, wenn wir viele Erkenntnisse haben. Wir sollten darauf achten, die Terminalausgabe nicht zu verändern, da wir eine genaue Darstellung des von uns ausgeführten Befehls und des Ergebnisses liefern möchten. Es ist in Ordnung, unnötige Ausgaben zu kürzen/auszuschneiden und den entfernten Teil mit <SNIP> zu markieren, aber niemals die Ausgabe zu ändern oder Dinge hinzuzufügen, die nicht im ursprünglichen Befehl oder in der ursprünglichen Ausgabe enthalten waren. Die Verwendung textbasierter Abbildungen erleichtert dem Kunden außerdem das Kopieren/Einfügen, um Ihre Ergebnisse zu reproduzieren. Es ist außerdem wichtig, dass alle Formatierungen des Quellmaterials, aus dem Sie einfügen, entfernt werden, bevor Sie es in Ihr Word-Dokument einfügen. Wenn Sie Text mit eingebetteter Formatierung einfügen, fügen Sie möglicherweise nicht UTF-8-codierte Zeichen in Ihre Befehle ein (normalerweise alternative Anführungszeichen oder Apostrophe), was tatsächlich dazu führen kann, dass der Befehl nicht ordnungsgemäß funktioniert, wenn der Client versucht, ihn zu reproduzieren.

Eine gängige Methode zum Schwärzen von Screenshots ist die Verpixelung oder Unschärfe mit einem Tool wie Greenshot. Untersuchungen haben gezeigt, dass diese Methode nicht narrensicher ist und die Wahrscheinlichkeit hoch ist, dass die Originaldaten durch Umkehrung der Pixelierungs-/Unschärfetechnik wiederhergestellt werden könnten. Dies kann mit einem Tool wie Unredacter erfolgen. Stattdessen sollten wir diese Technik vermeiden und schwarze Balken (oder eine andere feste Form) über dem Text verwenden, den wir schwärzen möchten. Wir sollten das Bild direkt bearbeiten und nicht einfach eine Form in MS Word anwenden, da jemand mit Zugriff auf das Dokument diese leicht löschen könnte. Abgesehen davon: Wenn Sie einen Blog-Beitrag oder etwas im Internet veröffentlichtes schreiben, bei dem vertrauliche Daten geschwärzt werden, sollten Sie sich nicht auf den HTML-/CSS-Stil verlassen, um zu versuchen, den Text zu verdecken (d. h. schwarzer Text mit schwarzem Hintergrund), da dies leicht passieren kann durch Markieren des Textes oder vorübergehendes Bearbeiten der Seitenquelle angezeigt werden. Im Zweifelsfall verwenden Sie die Konsolenausgabe. Wenn Sie jedoch einen Terminal-Screenshot verwenden müssen, stellen Sie sicher, dass Sie die Informationen entsprechend redigieren. Nachfolgend finden Sie Beispiele für die beiden Techniken:

#### Terminal

Normalerweise müssen nur die Anmeldeinformationen aus der Terminalausgabe entfernt werden (sei es im Befehl selbst oder in der Ausgabe des Befehls). Dazu gehören Passwort-Hashes. Bei Passwort-Hashes können Sie normalerweise einfach die Mitte herausschneiden und die ersten und letzten 3 oder 4 Zeichen stehen lassen, um zu zeigen, dass dort tatsächlich ein Hash vorhanden war. Für Klartext-Anmeldeinformationen oder andere für Menschen lesbare Inhalte, die verschleiert werden müssen, können Sie diese einfach durch einen Platzhalter <REDACTED> oder <PASSWORD REDACTED> oder ähnliches ersetzen.

Sie sollten auch eine farbcodierte Hervorhebung in Ihrer Terminalausgabe in Betracht ziehen, um den ausgeführten Befehl und die interessante Ausgabe der Ausführung dieses Befehls hervorzuheben. Dies verbessert die Fähigkeit des Lesers, die wesentlichen Teile der Beweise zu erkennen und zu erkennen, worauf er achten muss, wenn er versucht, sie selbst zu reproduzieren. Wenn Sie an einer komplexen Web-Nutzlast arbeiten, kann es schwierig sein, die Nutzlast in einer riesigen [[URL (Uniform Resource Locator)]]-codierten Anforderungstextwand herauszusuchen, wenn Sie damit nicht Ihren Lebensunterhalt verdienen. Wir sollten alle Gelegenheiten nutzen, um den Bericht für unsere Leser klarer zu gestalten, da diese am Ende der Bewertung häufig nicht über ein so tiefes Verständnis der Umgebung (insbesondere aus der Perspektive eines Penetrationstesters) verfügen wie wir.