## [[Subfinder]]

`subfinder -dL domains.txt -all -recursive -o subdomains.txt`

Der Befehl `subfinder` wird verwendet, um Subdomains für eine gegebene Liste von Domains zu finden. Hier ist eine Erläuterung des Befehls:

- `subfinder`: Dies ist der Hauptbefehl, der das Tool `subfinder` aufruft, das zum Finden von Subdomains verwendet wird.
- `-dL domains.txt`: Dieser Teil des Befehls gibt an, dass die Domains aus der Datei `domains.txt` geladen werden sollen. Diese Datei enthält wahrscheinlich eine Liste von Domains, für die Subdomains gefunden werden sollen.
- `-all`: Dieser Schalter weist `subfinder` an, nach allen Subdomains zu suchen und nicht nur nach bekannten Subdomains, die möglicherweise in öffentlichen Suchmaschinen oder Datenbanken aufgeführt sind.
- `-recursive`: Mit diesem Schalter wird die rekursive Suche aktiviert, was bedeutet, dass `subfinder` auch nach Subdomains von Subdomains sucht.
- `-o subdomains.txt`: Hier wird angegeben, dass die gefundenen Subdomains in die Datei `subdomains.txt` geschrieben werden sollen.


Zusammengefasst sucht der Befehl `subfinder` also in der Datei `domains.txt` nach Subdomains, einschließlich aller versteckten Subdomains, und schreibt die Ergebnisse in die Datei `subdomains.txt`.


## [[CRT]]

`curl -s https://crt.sh/\?q\=\example.com\&output\=json | jq -r '.[].name_value' | grep -Po '(\w+\. \w+\. \w+)$' | anew subdomains.txt`

Dieser Befehl verwendet mehrere Werkzeuge und Techniken, um Subdomains für die Domain "example.com" zu finden und sie in einer Datei namens `subdomains.txt` zu speichern. Hier ist eine Erläuterung des Befehls:

+ `curl -s https://crt.sh/\?q\=\example.com\&output\=json`: Dieser Teil des Befehls verwendet das Curl-Tool, um eine HTTPS-Anfrage an die Webseite "[https://crt.sh](https://crt.sh/)" zu senden. Die [[URL (Uniform Resource Locator)]] enthält eine Suchanfrage für die Domain "example.com". Die Ausgabe wird im JSON-Format zurückgegeben.
    
+ `| jq -r '.[].name_value'`: Der Pipe-Operator (`|`) leitet die Ausgabe des vorherigen Befehls an das Werkzeug "jq" weiter. "jq" ist ein Befehlszeilenprogramm, das für die Verarbeitung von JSON-Daten verwendet wird. Diese spezielle Verwendung von `jq` filtert die Werte des Schlüssels `name_value` aus dem JSON-Objekt.
    
+ `| grep -Po '(\w+\. \w+\. \w+)$'`: Die Ausgabe von `jq` wird dann an das Befehlszeilenprogramm "grep" weitergeleitet. Hier wird der Parameter `-Po` verwendet, um eine Perl-kompatible reguläre Ausdrücke zu verwenden und nur die passenden Teile der Zeilen zu extrahieren. Der reguläre Ausdruck `(\w+\. \w+\. \w+)$` sucht nach Subdomains mit mindestens zwei Punkten (was typisch für Subdomains ist) und speichert sie.
    
+ `| anew subdomains.txt`: Schließlich wird das Ergebnis an das Werkzeug "anew" weitergeleitet, das die Ausgabe filtert und doppelte Zeilen entfernt. Die Ausgabe wird dann in die Datei `subdomains.txt` geschrieben.


Zusammengefasst führt dieser Befehl eine Abfrage an crt.sh durch, filtert die Ergebnisse, um die Subdomains zu extrahieren, entfernt doppelte Einträge und speichert die Ergebnisse in der Datei `subdomains.txt`.


## [[Httpx]]

`cat subdomains.txt | httpx-toolkit -l subdomains.txt -ports 80,443,8000,8080,8888 -threads 200 > subdomains_alive.txt`

+ Der Befehl verwendet zwei Hauptwerkzeuge: `cat` und `httpx-toolkit`, um HTTP- und [[HTTPS]]-Verbindungen zu den in `subdomains.txt` aufgelisteten Subdomains herzustellen und diejenigen zu identifizieren, die aktiv sind. Hier ist eine Erläuterung des Befehls:

+ `cat subdomains.txt`: Dieser Teil des Befehls verwendet das `cat`-Befehlszeilenprogramm, um den Inhalt der Datei `subdomains.txt` anzuzeigen. Es gibt den Inhalt der Datei aus und leitet ihn weiter.
    
+ `| httpx-toolkit -l subdomains.txt -ports 80,443,8000,8080,8888 -threads 200`: Der Pipe-Operator (`|`) leitet den Inhalt von `subdomains.txt` an das Tool `httpx-toolkit` weiter. Dieses Tool wird verwendet, um [[HTTP]](S)-Verbindungen zu den angegebenen Subdomains herzustellen.
    
    - `-l subdomains.txt`: Dieser Schalter gibt an, dass `httpx-toolkit` die Subdomains aus der Datei `subdomains.txt` lesen soll.
    - `-ports 80,443,8000,8080,8888`: Hier werden die Ports angegeben, auf denen `httpx-toolkit` nach aktiven Verbindungen suchen soll. Die Ports 80 und 443 sind standardmäßige [[HTTP]]- und [[HTTPS]]-Ports, während die anderen Ports zusätzliche Ports für Webanwendungen sein könnten.
    - `-threads 200`: Dieser Schalter bestimmt die Anzahl der Threads, die `httpx-toolkit` verwenden soll, um die Verbindungen zu den Subdomains herzustellen. Mit 200 Threads werden die Verbindungen beschleunigt.
+ `> subdomains_alive.txt`: Schließlich wird das Ergebnis der Suche nach aktiven Subdomains in die Datei `subdomains_alive.txt` umgeleitet und dort gespeichert.


Zusammenfassend liest dieser Befehl die Subdomains aus `subdomains.txt`, stellt [[HTTP]](S)-Verbindungen zu ihnen her und speichert die aktiven Subdomains in der Datei `subdomains_alive.txt`.


## [[Naabu]]

`naabu -list subdomains.txt -c 50 -nmap-cli '-sV -sC' -o naabu_full.txt`

Der Befehl `naabu` wird verwendet, um Ports auf einer Liste von Zielen zu scannen und Informationen über diese Ports zu sammeln. Hier ist eine Erläuterung des Befehls:

- `naabu`: Dies ist der Hauptbefehl, der das Tool `naabu` aufruft, das zum Durchführen von Portscans verwendet wird.
    
- `-list subdomains.txt`: Dieser Parameter gibt an, dass die Liste der Ziele aus der Datei `subdomains.txt` geladen werden soll. Diese Datei enthält wahrscheinlich eine Liste von Subdomains, die gescannt werden sollen.
    
- `-c 50`: Dieser Parameter legt die Anzahl der gleichzeitigen Verbindungen fest, die `naabu` verwenden soll. Mit `-c 50` werden maximal 50 gleichzeitige Verbindungen zugelassen.
    
- `-nmap-cli '-sV -sC'`: Hier wird der Parameter `nmap-cli` verwendet, um spezifische Optionen an den Nmap-Scanner weiterzugeben. `-sV` führt eine Versionsabfrage durch, um Informationen über die Dienste zu erhalten, die auf den offenen Ports laufen, und `-sC` aktiviert das Skript-Scanning, um zusätzliche Informationen über diese Dienste zu sammeln.
    
- `-o naabu_full.txt`: Mit diesem Parameter wird angegeben, dass die Ergebnisse des Scans in die Datei `naabu_full.txt` geschrieben werden sollen.


Zusammengefasst führt der Befehl `naabu` also einen Portscan auf den in `subdomains.txt` aufgeführten Subdomains durch, verwendet maximal 50 gleichzeitige Verbindungen, führt Versions- und Skript-Scanning mit Nmap durch und speichert die Ergebnisse in der Datei `naabu_full.txt`.


## [[Dirsearch]]

`dirsearch -l subdomains_alive.txt -x 400,404,429,500,502, -R 5 --random-agent -t 100 -F -o directory.txt -w /wordlist/onelistforallshort.txt`

Der Befehl verwendet das Tool "dirsearch", um nach Verzeichnissen und Dateien auf den angegebenen Subdomains zu suchen. Hier ist eine Erläuterung des Befehls:

- `dirsearch`: Dies ist der Hauptbefehl, der das Tool "dirsearch" aufruft, das zum Durchführen von Directory-Bruteforce-Angriffen verwendet wird.
    
- `-l subdomains_alive.txt`: Dieser Parameter gibt an, dass die Liste der Ziel-Subdomains aus der Datei "subdomains_alive.txt" geladen werden soll. Diese Datei enthält wahrscheinlich die aktiven Subdomains, die durch vorherige Schritte identifiziert wurden.
    
- `-x 400,404,429,500,502`: Hier werden die Statuscodes angegeben, die als erfolgreiche Antworten betrachtet werden sollen. Wenn der Statuscode einer Anfrage einem der hier aufgeführten Codes entspricht, wird er als erfolgreiche Antwort betrachtet. Die Codes 400, 404, 429, 500 und 502 werden in diesem Fall verwendet.
    
- `-R 5`: Dieser Parameter gibt die Anzahl der Wiederholungen an, die für jede Anfrage durchgeführt werden sollen. Mit `-R 5` wird jede Anfrage fünfmal wiederholt, um sicherzustellen, dass die Ergebnisse zuverlässig sind.
    
- `--random-agent`: Dieser Parameter gibt an, dass bei jeder Anfrage ein zufälliger User-Agent verwendet wird, was dazu beiträgt, die Anfragen weniger verdächtig erscheinen zu lassen.
    
- `-t 100`: Hier wird die Anzahl der Threads angegeben, die "dirsearch" verwenden soll. Mit `-t 100` werden 100 Threads für die Durchführung der Bruteforce-Angriffe verwendet, was die Geschwindigkeit erhöht.
    
- `-F`: Dieser Parameter gibt an, dass "dirsearch" versuchen soll, falsch-positive Ergebnisse zu filtern.
    
- `-o directory.txt`: Mit diesem Parameter wird angegeben, dass die Ergebnisse des Brute-Force-Angriffs in die Datei "directory.txt" geschrieben werden sollen.
    
- `-w /wordlist/onelistforallshort.txt`: Hier wird der Pfad zur Wordlist-Datei angegeben, die für den Brute-Force-Angriff verwendet werden soll. Die Datei "/wordlist/onelistforallshort.txt" enthält wahrscheinlich eine Liste von Wörtern, die für den Angriff verwendet werden sollen.


Zusammengefasst führt der Befehl "dirsearch" also einen Directory-Bruteforce-Angriff auf den angegebenen Subdomains durch, filtert falsch-positive Ergebnisse, verwendet zufällige User-Agents und speichert die Ergebnisse in der Datei "directory.txt".


## [[GAU (GetAllUrls)]]

`cat subdomains_alive.txt | getallurls > params.txt`

Der Befehl `cat subdomains_alive.txt | getallurls > params.txt` ist eine Kombination von zwei Befehlen, die verwendet werden, um alle URLs von den aktiven Subdomains zu extrahieren und in eine Datei zu schreiben. Hier ist eine Erläuterung:

- `cat subdomains_alive.txt`: Dieser Teil des Befehls verwendet das `cat`-Befehlszeilenprogramm, um den Inhalt der Datei `subdomains_alive.txt` anzuzeigen. Die Datei enthält wahrscheinlich eine Liste von aktiven Subdomains.
    
- `| getallurls`: Der Pipe-Operator (`|`) leitet den Inhalt von `subdomains_alive.txt` an das Tool `getallurls` weiter. Dieses Tool wird verwendet, um alle URLs von den angegebenen Subdomains zu extrahieren.
    
- `> params.txt`: Schließlich wird das Ergebnis der Extraktion von URLs in die Datei `params.txt` umgeleitet und dort gespeichert.


Zusammenfassend führt dieser Befehl die Extraktion von URLs von den in `subdomains_alive.txt` aufgelisteten Subdomains durch und speichert die Ergebnisse in der Datei `params.txt`.


## [[URO]]

### URO_1

`cat params.txt | uro -o filter_param.txt`

Der Befehl `cat params.txt | uro -o filter_param.txt` führt eine Filterung von URLs in der Datei `params.txt` durch, um uninteressante oder doppelte Inhalte zu entfernen, und speichert die bereinigten URLs in der Datei `filter_param.txt`. Hier ist eine Erläuterung:

+ `cat params.txt`: Dieser Teil des Befehls verwendet das `cat`-Befehlszeilenprogramm, um den Inhalt der Datei `params.txt` anzuzeigen. Diese Datei enthält wahrscheinlich eine Liste von URLs, die möglicherweise unerwünschte oder doppelte Inhalte enthalten.
    
+ `| uro -o filter_param.txt`: Der Pipe-Operator (`|`) leitet den Inhalt der Datei `params.txt` an das Tool `uro` weiter. `uro` ist ein Tool, das darauf abzielt, URLs zu filtern, um unerwünschte oder doppelte Inhalte zu entfernen. Hier wird `-o filter_param.txt` verwendet, um die bereinigten URLs in die Datei `filter_param.txt` zu schreiben.


Zusammengefasst führt dieser Befehl also eine Bereinigung der URLs in `params.txt` durch, indem uninteressante oder doppelte Inhalte entfernt werden, und speichert die bereinigten URLs in der Datei `filter_param.txt`.

### URO_2

`cat js_files.txt | uro | anew js_files.txt`

Der Befehl `cat js_files.txt | uro | anew js_files.txt` führt eine Filterung der Inhalte der Datei `js_files.txt` durch, entfernt unerwünschte oder doppelte Einträge und speichert die bereinigten Inhalte wieder in `js_files.txt`. Hier ist eine Erläuterung:

+ `cat js_files.txt`: Dieser Teil des Befehls verwendet das `cat`-Befehlszeilenprogramm, um den Inhalt der Datei `js_files.txt` anzuzeigen. Diese Datei enthält wahrscheinlich eine Liste von URLs oder Dateinamen.
    
+ `| uro`: Der Pipe-Operator (`|`) leitet den Inhalt der Datei `js_files.txt` an das Tool `uro` weiter. `uro` ist ein Tool, das darauf abzielt, unerwünschte oder doppelte Inhalte zu entfernen, wie zum Beispiel inkrementelle URLs, Blog-Beiträge oder Dateien mit ähnlichem Inhalt.
    
+ `| anew js_files.txt`: Der Pipe-Operator (`|`) leitet die bereinigten Inhalte von `uro` an das Tool `anew` weiter. `anew` ist ein Tool, das doppelte Einträge in einer Liste entfernt und nur einzigartige Einträge behält. Die bereinigten Inhalte werden dann in die Datei `js_files.txt` geschrieben, wobei die vorhandene Datei überschrieben wird.


Zusammenfassend entfernt dieser Befehl doppelte oder unerwünschte Einträge aus der Datei `js_files.txt` und speichert die bereinigten Inhalte in derselben Datei.


## [[Secretfinder]]

`cat js_files.txt | while read url; do python3 /home/user/secretfinder.py -i $url -o cli >> secret.txt; done`

Dieser Befehl verwendet eine Schleife, um eine Liste von URLs aus der Datei `js_files.txt` zu lesen und jedes einzelne davon an ein Python-Skript namens `secretfinder.py` zu übergeben. Hier ist eine Erläuterung:

- `cat js_files.txt`: Dieser Teil des Befehls verwendet das `cat`-Befehlszeilenprogramm, um den Inhalt der Datei `js_files.txt` anzuzeigen. Diese Datei enthält wahrscheinlich eine Liste von URLs, die auf JavaScript-Dateien verweisen.
    
- `| while read url; do ... ; done`: Der Pipe-Operator (`|`) leitet den Inhalt von `js_files.txt` an eine `while read`-Schleife weiter, die jede Zeile der Datei liest und sie als Variable `url` speichert.
    
- `python3 /home/user/secretfinder.py -i $url -o cli >> secret.txt`: Innerhalb der Schleife wird das Python-Skript `secretfinder.py` mit der [[URL (Uniform Resource Locator)]] als Eingabe (`-i`) aufgerufen. Das Skript wird wahrscheinlich verwendet, um nach geheimen Informationen oder Mustern in der angegebenen [[URL (Uniform Resource Locator)]] zu suchen. Die Ausgabe wird mit `-o cli` in der Befehlszeile angezeigt (`cli` steht hier wahrscheinlich für "command line interface"). Die Ergebnisse werden an die Datei `secret.txt` angehängt (`>>`).


Zusammengefasst führt dieser Befehl für jede [[URL (Uniform Resource Locator)]] in der Datei `js_files.txt` das Python-Skript `secretfinder.py` aus und speichert die Ausgabe in der Datei `secret.txt`.


## Sort

```python
with open('filter_param.txt', 'r') as file:
	lines = file.readlines()
lines.sort()
lines = lines[:100000]
with open('sorted_param_100000.txt', 'w') as file:
	file.writelines(lines)
```

Dieses Skript liest aus der Datei "filter_param.txt" und wählt dann die ersten 100.000 Zeilen aus. Anschließend werden diese Zeilen in eine neue Datei mit dem Namen "sorted_param_100000.txt" geschrieben. Hier ist eine schrittweise Erläuterung:

+ `with open('filter_param.txt', 'r') as file:`:
    
    - Hier wird die Datei "filter_param.txt" im Lese-Modus (`'r'` für "read") geöffnet. Die Verwendung des `with`-Statements sorgt dafür, dass die Datei nach dem Öffnen automatisch geschlossen wird, selbst wenn im Skript ein Fehler auftritt.
+ `lines = file.readlines()`:
    
    - Die Methode `readlines()` wird verwendet, um alle Zeilen aus der Datei zu lesen und in einer Liste namens `lines` zu speichern. Jede Zeile wird zu einem Element in dieser Liste.
+ `lines.sort()`:
    
    - Die Methode `sort()` wird auf der Liste `lines` aufgerufen, um die Zeilen in aufsteigender Reihenfolge zu sortieren. Dies ist wichtig, wenn die ersten 100.000 Zeilen ausgewählt werden, um sicherzustellen, dass die Auswahl konsistent ist.
+ `lines = lines[:100000]`:
    
    - Hier wird die Liste `lines` auf die ersten 100.000 Elemente beschränkt. Da die Liste zuvor sortiert wurde, enthält sie nun die ersten 100.000 Zeilen in aufsteigender Reihenfolge.
+ `with open('sorted_param_100000.txt', 'w') as file:`:
    
    - Es wird eine neue Datei namens "sorted_param_100000.txt" im Schreibmodus (`'w'` für "write") geöffnet. Auch hier wird das `with`-Statement verwendet, um sicherzustellen, dass die Datei nach dem Schreiben automatisch geschlossen wird.
+ `file.writelines(lines)`:
    
    - Die Methode `writelines()` wird verwendet, um die Zeilen aus der Liste `lines` in die geöffnete Datei zu schreiben. Jede Zeile wird in die Datei geschrieben, ohne zusätzliche Zeilenumbrüche hinzuzufügen.

Zusammenfassend liest dieses Skript die Zeilen aus der Datei "filter_param.txt", sortiert sie, wählt die ersten 100.000 Zeilen aus und schreibt sie in eine neue Datei namens "sorted_param_100000.txt".


## [[Nuclei]]

`nuclei -list sorted_param_100000.txt -c 70 -rl 200 -fhr -lfa -t Custom-Nuclei-Templates -o nuclei_example.txt -es info`

Der Befehl verwendet das Tool "nuclei", um Sicherheitsprüfungen auf einer Liste von Zielen durchzuführen, wobei benutzerdefinierte Nuclei-Templates verwendet werden. Hier ist eine Erläuterung des Befehls:

- `nuclei`: Dies ist der Hauptbefehl, der das Tool "nuclei" aufruft, das zum Durchführen von Sicherheitsprüfungen verwendet wird.
    
- `-list sorted_param_100000.txt`: Dieser Parameter gibt an, dass die Liste der Ziele aus der Datei "sorted_param_100000.txt" geladen werden soll. Diese Datei enthält wahrscheinlich eine sortierte Liste von Zielen, auf die die Sicherheitsprüfungen angewendet werden sollen.
    
- `-c 70`: Hier wird die Anzahl der gleichzeitigen Verbindungen festgelegt, die "nuclei" verwenden soll. Mit `-c 70` werden maximal 70 gleichzeitige Verbindungen zugelassen.
    
- `-rl 200`: Dieser Parameter gibt die maximale Anzahl der Redirects an, die bei einer Anfrage gefolgt werden sollen. Mit `-rl 200` werden bis zu 200 Redirects gefolgt.
    
- `-fhr`: Der Parameter `-fhr` (fastest http request) ermöglicht das schnelle Senden von [[HTTP]]-Anfragen, ohne auf die Antwort zu warten. Dies beschleunigt den Prozess der Sicherheitsprüfung.
    
- `-lfa`: Dieser Parameter gibt an, dass die Ausgabe die vollständigen Anfrage- und Antwortdaten enthalten soll.
    
- `-t Custom-Nuclei-Templates`: Hier wird angegeben, dass benutzerdefinierte Nuclei-Templates verwendet werden sollen, um die Sicherheitsprüfungen durchzuführen. Die benutzerdefinierten Templates müssen im Verzeichnis "Custom-Nuclei-Templates" gespeichert sein.
    
- `-o nuclei_example.txt`: Mit diesem Parameter wird angegeben, dass die Ergebnisse der Sicherheitsprüfungen in die Datei "nuclei_example.txt" geschrieben werden sollen.
    
- `-es info`: Hier wird das Log-Level für die Fehlerausgabe auf "info" gesetzt. Dies gibt an, dass nur Fehlermeldungen mit dem Log-Level "info" ausgegeben werden sollen.


Zusammenfassend führt der Befehl "nuclei" also Sicherheitsprüfungen auf den in "sorted_param_100000.txt" aufgeführten Zielen durch, verwendet benutzerdefinierte Nuclei-Templates, speichert die Ergebnisse in der Datei "nuclei_example.txt" und gibt Fehlermeldungen mit dem Log-Level "info" aus.


## [[Shodan]]


## [[Katana]]





