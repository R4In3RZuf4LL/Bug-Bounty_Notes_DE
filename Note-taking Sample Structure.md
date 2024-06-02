## Note-taking Sample Structure

Es gibt keine allgemeingültige Lösung oder Struktur für das Notieren, da jedes Projekt und jeder Tester anders ist. Die folgende Struktur hat sich unserer Meinung nach als hilfreich erwiesen, sie sollte jedoch an Ihren persönlichen Arbeitsablauf, Ihren Projekttyp und die spezifischen Umstände, denen Sie während Ihres Projekts begegnet sind, angepasst werden. Einige dieser Kategorien sind beispielsweise möglicherweise nicht für eine anwendungsorientierte Bewertung anwendbar und erfordern möglicherweise sogar zusätzliche Kategorien, die hier nicht aufgeführt sind.

+ ***Angriffspfad*** – Ein Überblick über den gesamten Pfad, wenn Sie während eines externen Penetrationstests Fuß fassen oder während eines internen Penetrationstests einen oder mehrere Hosts (oder die AD-Domäne) kompromittieren. Skizzieren Sie den Pfad anhand von Screenshots und Befehlsausgaben so genau wie möglich, damit Sie ihn später leichter in den Bericht einfügen können und sich nur um die Formatierung kümmern müssen.  
  
+ ***Anmeldeinformationen*** – Ein zentraler Ort, an dem Sie Ihre gefährdeten  Anmeldeinformationen und Geheimnisse jederzeit aufbewahren können.  
  
+ ***Ergebnisse*** – Wir empfehlen, für jedes Ergebnis einen Unterordner zu erstellen, dann unsere Beschreibung zu verfassen und sie zusammen mit allen Beweisen (Screenshots, Befehlsausgabe) im Ordner zu speichern. Es lohnt sich auch, in Ihrem Notiztool einen Abschnitt zum Aufzeichnen von Befundinformationen einzurichten, um diese für den Bericht besser organisieren zu können.  
  
+ ***Schwachstellen-Scan-Forschung*** – Ein Abschnitt, in dem Sie sich Notizen zu Dingen machen können, die Sie mit Ihren Schwachstellen-Scans recherchiert und ausprobiert haben (damit Sie nicht am Ende bereits durchgeführte Arbeiten wiederholen müssen).  
  
+ ***Forschung zur Dienstaufzählung*** – Ein Abschnitt, in dem Sie Notizen darüber machen können, welche Dienste Sie untersucht haben, welche Ausnutzungsversuche fehlgeschlagen sind, welche Schwachstellen/Fehlkonfigurationen sich versprechen usw.  
  
+ ***Webanwendungsforschung*** – Ein Abschnitt zum Notieren interessanter Webanwendungen, die mit verschiedenen Methoden gefunden wurden, wie zum Beispiel Subdomain-Brute-Forcing. Es ist immer gut, eine gründliche externe Subdomain-Aufzählung durchzuführen, bei internen Bewertungen nach gängigen Web-Ports zu suchen und ein Tool wie Aquatone oder EyeWitness auszuführen, um einen Screenshot aller Anwendungen zu erstellen. Notieren Sie sich beim Durchsehen des Screenshot-Berichts die Anwendungen, die Sie interessieren, die von Ihnen ausprobierten allgemeinen/Standard-Anmeldeinformationspaare usw.  
  
+ ***AD-Enumerationsforschung*** – Ein Abschnitt, in dem Sie Schritt für Schritt zeigen, welche Active Directory-Enumeration Sie bereits durchgeführt haben. Notieren Sie alle Interessenbereiche, die Sie später in der Beurteilung durchgehen müssen.  
  
+ ***OSINT*** – Ein Abschnitt, um den Überblick über interessante Informationen zu behalten, die Sie über OSINT gesammelt haben, sofern diese für das Engagement relevant sind.  
  
+ ***Verwaltungsinformationen*** – Manche Menschen finden es möglicherweise hilfreich, einen zentralen Ort zum Speichern von Kontaktinformationen für andere Projektbeteiligte wie Projektmanager (PMs) oder Kundenkontaktpunkte (POCs) zu haben, sowie eindeutige Ziele/Flags, die in den Rules of Engagement (RoE) definiert sind. und andere Elemente, auf die Sie im Laufe des Projekts häufig verweisen. Es kann auch als laufende To-Do-Liste verwendet werden. Wenn zum Testen Ideen auftauchen, die Sie durchführen müssen oder ausprobieren möchten, für die Sie aber keine Zeit haben, notieren Sie sie sorgfältig hier, damit Sie später darauf zurückkommen können.  
  
+ ***Scoping-Informationen*** – Hier können wir Informationen über IP-Adressen/CIDR-Bereiche im Geltungsbereich, Webanwendungs-URLs und alle vom Client bereitgestellten Anmeldeinformationen für Webanwendungen, VPN oder AD speichern. Es könnte auch alles andere enthalten, was für den Umfang der Bewertung relevant ist, sodass wir die Informationen zum Umfang nicht ständig erneut öffnen müssen und sicherstellen, dass wir nicht vom Umfang der Bewertung abweichen.  
  
+ ***Aktivitätsprotokoll*** – umfassende Nachverfolgung aller Aktivitäten, die Sie während der Bewertung durchgeführt haben, um mögliche Korrelationen zu Ereignissen zu ermitteln.  
  
+ ***Payload-Protokoll*** – Ähnlich wie beim Aktivitätsprotokoll ist die Verfolgung der von Ihnen verwendeten Payloads (und eines Datei-Hashs für alles, was hochgeladen wurde, und des Upload-Speicherorts) in einer Client-Umgebung von entscheidender Bedeutung. Mehr dazu später.