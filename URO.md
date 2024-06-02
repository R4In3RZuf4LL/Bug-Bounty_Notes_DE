## Website

https://github.com/s0md3v/uro/blob/main/README.md

## Description

Die Verwendung einer [[URL (Uniform Resource Locator)]]-Liste für Sicherheitstests kann mühsam sein, da es viele URLs gibt, die uninteressanten/doppelten Inhalt haben; uro möchte das lösen.  
  
Es stellt keine http-Anfragen an die URLs und entfernt Folgendes:  
  
Inkrementelle URLs, z. B. /page/1/ und /page/2/  
Blog-Beiträge und ähnliche von Menschen verfasste Inhalte, z. /posts/a-brief-history-of-time  
URLs mit demselben Pfad, aber unterschiedlichen Parameterwerten, z. B. /page.php?id=1 und /page.php?id=2  
Bilder, JS, CSS und andere „nutzlose“ Dateien

## Usage

### Installation

Die empfohlene Methode zur Installation von uro ist über pip wie folgt:

```
pip3 install uro
```

### Basic Usage

Der schnellste Weg, URO in Ihren Workflow einzubinden, besteht darin, die Daten über stdin einzuspeisen und auf Ihrem Terminal auszudrucken.

```
cat urls.txt | uro
```

#### Reading urls from a file (-i/--input)

`uro -i input.txt`

#### Writing urls to a file (-o/--output)

Wenn die Datei bereits vorhanden ist, überschreibt uro den Inhalt nicht. Andernfalls wird eine neue Datei erstellt.

`uro -i input.txt -o output.txt`

#### Whitelist (`-w/--whitelist`)

uro ignoriert alle anderen Erweiterungen außer den bereitgestellten.

`uro -w php asp html`

**Note:** Extensionless pages e.g. `/books/1` will still be included. To remove them too, use `--filter hasext`.

#### Blacklist (`-b/--blacklist`)

uro ignoriert die angegebenen Erweiterungen.

`uro -b jpg png js pdf`

**Hinweis:** uro verfügt über eine Liste „nutzloser“ Erweiterungen, die standardmäßig entfernt werden. Diese Liste wird von allen Erweiterungen überschrieben, die Sie über die Blacklist-Option bereitstellen. Seiten ohne Erweiterung, z.B. /books/1 wird weiterhin enthalten sein. Um sie ebenfalls zu entfernen, verwenden Sie `--filter hasext`.

#### Filters (-f/--filters)

For granular control, uro supports the following filters:

1. **hasparams:** only output urls that have query parameters e.g. `http://example.com/page.php?id=`
2. **noparams:** only output urls that have no query parameters e.g. `http://example.com/page.php`
3. **hasexts:** only output urls that have extensions e.g. `http://example.com/page.php`
4. **noexts:** only output urls that have no extensions e.g. `http://example.com/page`
5. **keepcontent:** keep human written content e.g. blogs.
6. **keepslash:** don't remove trailing slash from urls e.g. `http://example.com/page/`
7. **vuln:** only output urls with parameters that are know to be vulnerable.

Example: `uro --filters hasexts hasparams`