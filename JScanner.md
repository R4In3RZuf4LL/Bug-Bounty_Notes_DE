
## Website

https://github.com/tampe125/jscanner/tree/master

## Description

> Analysieren Sie Ziel-Joomla! Installation mit verschiedenen Techniken.

## Why another tool?

> Derzeit gibt es mehrere Tools, die das installierte Joomla! erkennen. Version verfügen Sie über Online-Ressourcen und sogar ein Metasploit-Hilfsmodul. Sie führen jedoch eine sehr einfache Prüfung durch: Sie versuchen, die Manifestdatei von Joomla! herunterzuladen. und lesen Sie die darin enthaltene Version.  
> Leider ist diese Methode nicht sehr zuverlässig: Eine solche XML-Datei könnte (und wird) von gewissenhaften Webmastern gelöscht werden, außerdem ist es einfach, jede Anfrage zu blockieren, die versucht, eine „interne“ Datei abzurufen.

## The solution

> JScanner wird verschiedene Möglichkeiten ausprobieren:

1. **Lesen Sie die Manifestdatei `joomla.xml`** 100 % Genauigkeit, aber normalerweise fehlt diese Datei oder ist blockiert
2. **Scannen Sie den Ordner „com_admin“ nach SQL-Dateien.** Jedes Joomla! Die Version wird mit anderen SQL-Dateien für Updates geliefert. Durch die Überprüfung ihrer Anwesenheit kann JScanner eine Liste möglicher Kandidaten erstellen
3. **Fingerabdruck von Mediendateien** Wenn alles fehlschlägt, können wir trotzdem versuchen, Mediendateien abzurufen. Für jede Version wurde eine Signatur generiert und wir werden sie mit der Remote-Quelle vergleichen.

## Usage

```
python jscanner.py analyze -u http://www.example.com

JScanner 1.0.0 - What's under the hood?
Copyright (C) 2016 FabbricaBinaria - Davide Tampellini
===============================================================================
JScanner is Free Software, distributed under the terms of the GNU General
Public License version 3 or, at your option, any later version.
This program comes with ABSOLUTELY NO WARRANTY as per sections 15 & 16 of the
license. See http://www.gnu.org/licenses/gpl-3.0.html for details.
===============================================================================
[*] Analyzing site http://www.example.com
[*] Trying to get the exact version from the XML file...
[*] Trying to detect version using SQL installation files...
[*] Trying to detect version using media file fingerprints...

[+] Detected Joomla! versions: 3.6.3, 3.6.4
```

## Other commands

#### Enumerate users and email addresses

> Given a list of usernames or email addresses, you can check if they are actually used on target website (requires user registration being enabled).

```
python jscanner.py enumerate -u localhost/vergine -U users.txt 
JScanner 1.1.0 - What's under the hood?
Copyright (C) 2016 FabbricaBinaria - Davide Tampellini
===============================================================================
JScanner is Free Software, distributed under the terms of the GNU General
Public License version 3 or, at your option, any later version.
This program comes with ABSOLUTELY NO WARRANTY as per sections 15 & 16 of the
license. See http://www.gnu.org/licenses/gpl-3.0.html for details.
===============================================================================
[*] Checking if URL http://www.example.com is online
[+] Site http://www.example.com seems online
[*] Checking if user registration is enabled
[+] User registration seems enabled
[*] Trying to fetch CSRF token
[*] Got CSRF token: 2f48a75c99e06d8723396c14ff2c2884
[*] Trying to fetch username usage
[+] Found used username: admin
```

