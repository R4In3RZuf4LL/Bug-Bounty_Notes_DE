# [[Cross-Origin Resource Sharing]]-Richtlinien ([[CORS-Richtlinien]])

Letzte Aktualisierung: 2021-03-03

Eine[[CORS-Richtlinien]] gibt die Einstellungen an, die auf Ressourcen angewendet werden können, um [[Cross-Origin Resource Sharing]] zu ermöglichen.
[[CORS]] ist ein Verfahren, das einen zusätzlichen [[HTTP-Header]] verwendet, um einem Browser mitzuteilen, dass einer Webanwendung, die an einem Ursprung (Domäne) ausgeführt wird, der Zugriff auf ausgewählte Ressourcen eines anderen Servers an einem anderen Ursprung ermöglicht werden soll.
Jede [[API]]-Zugriffssteuerungsressource in IBM Security Verify Access kann mit einer [[CORS-Richtlinien]] konfiguriert werden, die Folgendes definiert:

   + Ob der Reverse Proxy den Preflight-Check ausführen soll oder nicht. Beispiel: OPTIONS-Überprüfung.
   + Die Ursprünge, die Anforderungen an diese Ressource ausgeben dürfen.
   + Ob der Header Access-Control-Allow-Credentials definiert werden soll oder nicht.
   + Die Header, die der Antwort auf einen Preflight-Check hinzugefügt werden.
   + Die Methoden, die in Anforderungen an diese Ressource zulässig sind.
   + Die maximale Zeit, die ein Client die Antwort auf einen Preflight-Check zwischenspeichern soll.
   + Die Header, die ein Client bereitstellen soll.

Anmerkung: 
Die Zuordnung einer [[CORS-Richtlinien]] zu einer [[API]]-Zugriffssteuerungsressource hat zur Folge, 
dass eine neue Zeilengruppe zu der Reverse-Proxy-Konfigurationsdatei hinzugefügt wird. 
Diese neue Zeilengruppe wird mit einem Kommentar markiert, der angibt, dass der Inhalt von der Maschine generiert wird und nicht manuell geändert werden sollte. 
Damit soll sichergestellt werden, dass die [[API]]-Zugriffssteuerungsmanagementkomponente nicht von manuellen Änderungen betroffen ist. 
Alle Änderungen, die von einem Administrator vorgenommen werden, werden von [[CORS-Richtlinienaktualisierungen]] überschrieben. 
Ein Beispiel der neuen Zeilengruppe: 


```http
[cors-policy:apiac_policyA]
# *************************************************************************
****************************************# 
THIS STANZA IS AUTO GENERATED. PLEASE DO NOT UPDATE AS IT MAY CAUSE PROBLEMS WITH THE [[API]] ACCESS CONTROL COMPONENT
# *************************************************************************
****************************************
handle-pre-flight = false
max-age = 0
allow-credentials = false
allow-origin = http://test.com
request-match = GET /application/endpointA HTTP/*
```


Soll eine neue [[CORS-Richtlinien]] erstellt werden, lesen Sie die Informationen in [[CORS-Richtlinien]] erstellen.
Soll einer [[API]]-Zugriffssteuerungsressource eine [[CORS-Richtlinien]] hinzugefügt werden, 
lesen Sie die Informationen in Neue Ressource erstellen und Vorhandene Ressource ändern.
Weitere Informationen zur [[CORS-Verarbeitung]] durch den Reverse Proxy finden Sie in Zeilengruppe 
[cors-policy:<Richtlinienname>] und Unterstützung für [[Cross-Origin Resource Sharing]] ([[CORS]]). 


