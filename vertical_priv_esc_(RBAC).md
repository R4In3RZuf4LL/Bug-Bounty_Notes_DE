# Unchanged default credentials

## Summary:

1. Gehe zu [ihre-domain.teleport.sh/web/accesslists].

2. Erstellen Sie eine neue Zugriffsliste und fügen Sie der „Roles Granted“ eine Rolle hinzu, z. B. die Rolle „Reviewer“.

3. Fügen Sie einen Benutzer als Besitzer der Zugriffsliste hinzu.

4. Der Benutzer kann als Besitzer der Zugriffsliste die Rolle der Liste auf höhere Rollen ausweiten und so die Rolle seines eigenen Kontos eskalieren.

Dies ist, wie bereits erwähnt, ein verbotenes Verfahren [hier](https://goteleport.com/docs/access-controls/access-lists/reference/#access-list-ownership), 
dass Besitzer nicht kontrollieren können, welche Rollen und Eigenschaften die Zugriffsliste gewährt.

## Steps To Reproduce:

### Aus dem Konto des Organisationsinhabers

1. Gehe zu [ihre-domain.teleport.sh/web/accesslists].

2. Erstellen Sie eine neue Zugriffsliste.

3. Fügen Sie einen Benutzer als Listenbesitzer hinzu.

4. Fügen Sie „Roles Granted“ eine Rolle hinzu, z. B. die Rolle „Reviewer“.

### Aus dem Konto des Besitzers der Zugriffsliste

1. Fügen Sie der Zugriffsliste ein neues Mitglied hinzu und fangen Sie die Anfrage ab.

2. Fügen Sie die Rolle „Editor“ zu „Rollen gewährt“ hinzu.

3. Die Rolle „Editor“ wird zu „Berechtigungen gewährt“ hinzugefügt.

4. Abmelden und erneut anmelden.

5. Jetzt hat der Benutzer die Rolle „Editor“ und kann jede Aktion in der Organisation ausführen.

## Impact

- Unbefugter Zugriff: Potenzial für unbefugten Zugriff auf vertrauliche Informationen.
- Sicherheitsverletzung: Risiko, die Gesamtsicherheit des Systems zu gefährden.
- Privilegieneskalation: Verstoß gegen das Prinzip der geringsten Privilegien.
- Verstoß gegen die Richtlinien zur Zugangskontrolle: Widerspruch zur Dokumentation und den Richtlinien von Teleport.
- Risiko von Insider-Bedrohungen: Mögliche Ausnutzung durch böswillige Insider.