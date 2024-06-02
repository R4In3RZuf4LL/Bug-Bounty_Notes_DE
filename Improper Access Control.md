# Improper Access Control

## Target

Reported to: U.S. Dept Of Defense
Disclosed: November 3, 2023, 5:15pm UTC
Severity: Critical (9 ~ 10)
Weakness: Improper Access Control - Generic
Bounty: None
CVE ID: None
Account details: None

## Description

### User automatically logged in as Sys Admin user on https://███/Administration/Administration.aspx

Jeder Benutzer kann unter der folgenden [[URL (Uniform Resource Locator)]] auf den Administrationsbereich zugreifen: [[URL (Uniform Resource Locator)]]: https://███.
Wenn der Benutzer die folgende Domain aufruft, wird er automatisch als "████████", also als System-Admin-Benutzer in der Anwendung, angemeldet. 
Auf diese Weise kann jeder Benutzer Dateien hochladen, Benutzer hinzufügen, Benutzerberechtigungen ändern und Benutzer löschen.

## Impact

Ein böswilliger Akteur kann die Rechte anderer Benutzer für die Anwendung ändern, Benutzer hinzufügen, Dateien hochladen und Benutzer löschen. 
Sie können der Anwendung auch falsche Informationen hinzufügen, wodurch die Integrität der Anwendung gefährdet wird. 
Mit Administratorrechten haben sie keine Einschränkungen für die Anwendung.

## System Host(s)

https://█████████

## Steps to Reproduce

1. Gehe zu der folgenden [[URL (Uniform Resource Locator)]]: https://███.

Dort werden Sie sehen, dass Sie als Sys-Admin-Benutzer angemeldet sind.

## Suggested Mitigation/Remediation Actions

Die Anwendung sollte einen Benutzer auffordern, sich zuerst zu authentifizieren, bevor er andere Aktionen auf dem System ausführen kann.