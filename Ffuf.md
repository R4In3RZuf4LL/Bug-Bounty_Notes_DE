
## Website

https://github.com/ffuf/ffuf

## Description

Ein fortgeschrittener Web-Fuzzer

## Examples

### Typical directory discovery

Durch die Verwendung des FUZZ-Schlüsselworts am Ende der [[URL (Uniform Resource Locator)]] (`-u`):

```
ffuf -w /path/to/wordlist -u https://target/FUZZ
```

### Virtual host discovery (without DNS records)

Angenommen, die Standardantwortgröße des virtuellen Hosts beträgt 4242 Byte, können wir alle Antworten dieser Größe herausfiltern (`-fs 4242`), während wir den Host-Header fuzzen:

```
ffuf -w /path/to/vhost/wordlist -u https://target -H "Host: FUZZ" -fs 4242
```

### GET parameter fuzzing

Das Fuzzing des GET-Parameternamens ist der Verzeichniserkundung sehr ähnlich und funktioniert, indem das `FUZZ`-Schlüsselwort als Teil der [[URL (Uniform Resource Locator)]] definiert wird. Dies setzt auch eine Antwortgröße von 4242 Byte für ungültige GET-Parameternamen voraus.

```
ffuf -w /path/to/paramnames.txt -u https://target/script.php?FUZZ=test_value -fs 4242
```

Wenn der Parametername bekannt ist, können die Werte auf die gleiche Weise gefuzzt werden. Dieses Beispiel geht davon aus, dass ein falscher Parameterwert den HTTP-Antwortcode 401 zurückgibt.

```
ffuf -w /path/to/values.txt -u https://target/script.php?valid_name=FUZZ -fc 401
```

### POST data fuzzing

Dies ist ein sehr einfacher Vorgang, wiederum durch die Verwendung des `FUZZ`-Schlüsselworts. In diesem Beispiel wird nur ein Teil der POST-Anfrage gefuzzt. Wir filtern erneut die 401-Antworten heraus.

```
ffuf -w /path/to/postdata.txt -X POST -d "username=admin\&password=FUZZ" -u https://target/login.php -fc 401
```

### Maximum execution time

Wenn Sie nicht möchten, dass ffuf unbegrenzt läuft, können Sie die Option `-maxtime` verwenden. Dies stoppt **den gesamten** Vorgang nach einer bestimmten Zeit (in Sekunden).

```
ffuf -w /path/to/wordlist -u https://target/FUZZ -maxtime 60
```

Bei der Arbeit mit Rekursion können Sie die maximale Ausführungszeit **pro Job** mit `-maxtime-job` steuern. Dies wird den aktuellen Job nach einer bestimmten Zeit (in Sekunden) stoppen und mit dem nächsten fortfahren. Neue Jobs werden erstellt, wenn die Rekursionsfunktionalität ein Unterverzeichnis erkennt.

```
ffuf -w /path/to/wordlist -u https://target/FUZZ -maxtime-job 60 -recursion -recursion-depth 2
```

Es ist auch möglich, beide Flags zu kombinieren, um die maximale Ausführungszeit pro Job sowie die gesamte Ausführungszeit zu begrenzen. Wenn Sie keine Rekursion verwenden, verhalten sich beide Flags gleich.

### Using external mutator to produce test cases

In diesem Beispiel werden wir JSON-Daten fuzzen, die über POST gesendet werden. [Radamsa](https://gitlab.com/akihe/radamsa) wird als Mutator verwendet.

Wenn `--input-cmd` verwendet wird, zeigt ffuf Übereinstimmungen als ihre Position an. Dieser Positionswert wird dem Aufrufer als Umgebungsvariable `$FFUF_NUM` zur Verfügung stehen. Wir verwenden diesen Positionswert als Seed für den Mutator. Die Dateien example1.txt und example2.txt enthalten gültige JSON-Nutzlasten. Wir gleichen alle Antworten ab, filtern jedoch den Antwortcode `400 - Bad request` heraus:

```
ffuf --input-cmd 'radamsa --seed $FFUF_NUM example1.txt example2.txt' -H "Content-Type: application/json" -X POST -u https://ffuf.io.fi/FUZZ -mc all -fc 400
```

Es ist natürlich nicht sehr effizient, den Mutator für jede Nutzlast aufzurufen, daher können wir die Nutzlasten auch vorab generieren, wobei [Radamsa](https://gitlab.com/akihe/radamsa) weiterhin als Beispiel verwendet wird:

```
# Generiere 1000 Beispieldaten
radamsa -n 1000 -o %n.txt example1.txt example2.txt

# Dies resultiert in den Dateien 1.txt ... 1000.txt
# Jetzt können wir die Nutzdaten einfach in einer Schleife aus der Datei für ffuf lesen

ffuf --input-cmd 'cat $FFUF_NUM.txt' -H "Content-Type: application/json" -X POST -u https://ffuf.io.fi/ -mc all -fc 400
```

### Configuration files

Beim Ausführen von ffuf wird zunächst überprüft, ob eine Standardkonfigurationsdatei vorhanden ist. Der Standardpfad für eine `ffufrc`-Datei ist `$XDG_CONFIG_HOME/ffuf/ffufrc`. Sie können eine oder mehrere Optionen in dieser Datei konfigurieren, und sie werden bei jedem nachfolgenden ffuf-Job angewendet. Ein Beispiel für eine ffufrc-Datei finden Sie [hier](https://github.com/ffuf/ffuf/blob/master/ffufrc.example).

Eine detailliertere Beschreibung der Speicherorte von Konfigurationsdateien finden Sie im Wiki: [https://github.com/ffuf/ffuf/wiki/Configuration](https://github.com/ffuf/ffuf/wiki/Configuration)

Die über die Befehlszeile bereitgestellten Konfigurationsoptionen überschreiben die aus der Standard-`ffufrc`-Datei geladenen Optionen. Hinweis: Dies gilt nicht für CLI-Flags, die mehr als einmal angegeben werden können. Ein Beispiel hierfür ist das `-H` (Header)-Flag. In diesem Fall werden die über die Befehlszeile angegebenen `-H`-Werte denjenigen aus der Konfigurationsdatei _hinzugefügt_.

Darüber hinaus können Sie, falls Sie eine Reihe von Konfigurationsdateien für verschiedene Anwendungsfälle verwenden möchten, dies tun, indem Sie den Pfad der Konfigurationsdatei mit dem Befehlszeilen-Flag `-config` definieren, das den Dateipfad zur Konfigurationsdatei als Parameter übernimmt.