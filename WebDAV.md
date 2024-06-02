WebDAV (Web-based Distributed Authoring and Versioning) ist ein Netzwerkprotokoll zur Bereitstellung von Dateien über das Internet. Es basiert auf dem Hypertext Transfer Protocol ([[HTTP]]/1.1), unterstützt mittlerweile aber auch [[HTTPS]]. Mit WebDAV können ganze Verzeichnisse übertragen werden. Zudem ist eine Versionskontrolle spezifiziert. WebDAV ist definiert im Standard [[RFC 4918]]. Auf WebDAV bauen unter anderem die Protokolle [[CalDAV]] und [[CardDAV]] auf, welche zur Synchronisation von Kalender- bzw. Adressdaten verwendet werden. 

## Vorteile von WebDAV

Durch die enorme Verbreitung des World Wide Web zählt der von [[HTTP]] genutzte Port 80 zu den Ports, die bei Firewalls in der Regel nicht blockiert werden. Während bei anderen Übertragungsmethoden wie dem File Transfer Protocol ([[FTP]]) oder [[SSH]] (in Verbindung mit scp oder SFTP) oftmals zusätzlich Ports der Firewall geöffnet werden müssen, ist das bei WebDAV nicht nötig, da es auf [[HTTP]] aufbaut und daher nur Port 80 benötigt. Das Öffnen von zusätzlichen Ports einer [[Firewall]] erhöht den Zeit- und Arbeitsaufwand für Systemadministratoren und birgt unter Umständen zusätzliche Sicherheitsrisiken. Zudem kann der Server innerhalb eines bestehenden [[HTTP]]-Servers implementiert werden.

Mittlerweile gibt es für jedes Betriebssystem (inklusive Smartphones) direkte WebDAV-Implementierungen, die es ermöglichen, WebDAV ins System einzubinden oder zumindest per Dateimanager darauf zuzugreifen.

Da auch Benutzerrechte unterstützt werden, ist es eine echte und weitaus sicherere Alternative gegenüber [[Samba]]- oder Windows-Freigaben, besonders beim Fernzugriff. 
