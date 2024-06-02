## Website

https://github.com/bitquark/shortscan

## Description

Shortscan wurde entwickelt, um schnell festzustellen, welche Dateien mit kurzen Dateinamen auf einem IIS-Webserver vorhanden sind. Sobald ein kurzer Dateiname identifiziert wurde, versucht das Tool, automatisch den vollständigen Dateinamen zu identifizieren.  
  
Zusätzlich zu den Standarderkennungsmethoden verwendet Shortscan auch einen einzigartigen Prüfsummenvergleichsansatz, um zu versuchen, den langen Dateinamen zu finden, wobei der kurze Dateiname auf dem proprietären Prüfsummenalgorithmus von Windows zur Vermeidung von Kurznamenkollisionen basiert (mehr zu dieser Forschung zu einem späteren Zeitpunkt).

## Usage

> Shortscan ist mit minimaler Konfiguration einfach zu verwenden. Die grundlegende Verwendung sieht folgendermaßen aus:

```
shortscan http://example.org/
```

> In diesem Beispiel werden mehrere benutzerdefinierte Header durch mehrmalige Verwendung von --header/-H festgelegt:

```
shortscan -H 'Host: gibson' -H 'Authorization: Basic ZGFkZTpsMzN0'
```

Die folgenden Optionen ermöglichen weitere Optimierungen:

```
Shortscan v0.7 · an IIS short filename enumeration tool by bitquark
Usage: shortscan [--wordlist FILE] [--header HEADER] [--concurrency CONCURRENCY] [--timeout SECONDS] [--output type] [--verbosity VERBOSITY] [--fullurl] [--stabilise] [--patience LEVEL] [--characters CHARACTERS] [--autocomplete mode] [--isvuln] URL

Positional arguments:
  URL                    url to scan

Options:
  --wordlist FILE, -w FILE
                         combined wordlist + rainbow table generated with shortutil
  --header HEADER, -H HEADER
                         header to send with each request (use multiple times for multiple headers)
  --concurrency CONCURRENCY, -c CONCURRENCY
                         number of requests to make at once [default: 20]
  --timeout SECONDS, -t SECONDS
                         per-request timeout in seconds [default: 10]
  --output type, -o type
                         output format (human = human readable; json = JSON) [default: human]
  --verbosity VERBOSITY, -v VERBOSITY
                         how much noise to make (0 = quiet; 1 = debug; 2 = trace) [default: 0]
  --fullurl, -F          display the full URL for confirmed files rather than just the filename [default: false]
  --stabilise, -s        attempt to get coherent autocomplete results from an unstable server (generates more requests) [default: false]
  --patience LEVEL, -p LEVEL
                         patience level when determining vulnerability (0 = patient; 1 = very patient) [default: 0]
  --characters CHARACTERS, -C CHARACTERS
                         filename characters to enumerate [default: JFKGOTMYVHSPCANDXLRWEBQUIZ8549176320-_()&'!#$%@^{}~]
  --autocomplete mode, -a mode
                         autocomplete detection mode (auto = autoselect; method = HTTP method magic; status = HTTP status; distance = Levenshtein distance; none = disable) [default: auto]
  --isvuln, -V           bail after determining whether the service is vulnerable [default: false]
  --help, -h             display this help and exit
  --version              display version and exit
```