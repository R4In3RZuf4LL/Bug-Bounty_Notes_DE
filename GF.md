## Website

https://github.com/tomnomnom/gf

## Description

Ein Wrapper um [[grep]], um die Eingabe gängiger Muster zu vermeiden.

## What? Why?

Ich verwende [[grep]] häufig. Bei der Prüfung von Codebasen, beim Betrachten der Ausgabe oder beim allgemeinen Umgang mit großen Datenmengen. Am Ende verwende ich oft ziemlich komplexe Muster wie dieses:

```
▶ grep -HnrE '(\$_(POST|GET|COOKIE|REQUEST|SERVER|FILES)|php://(input|stdin))' *
```

Es kann wirklich leicht passieren, dass man bei der Eingabe all das vermasselt, und es kann schwierig sein, festzustellen, ob man keine Ergebnisse hat, weil keine zu finden sind, oder weil man Fehler beim Schreiben des Musters gemacht oder die falschen Flags ausgewählt hat.

Ich habe `gf` geschrieben, um den Muster- und Flaggenkombinationen, die ich ständig verwende, Namen zu geben. Der obige Befehl wird also einfach:

```
▶ gf php-sources
```

### Pattern Files

Die Musterdefinitionen werden in `~/.gf` als kleine JSON-Dateien gespeichert, die unter Versionskontrolle gehalten werden können:

```
▶ cat ~/.gf/php-sources.json
{
    "flags": "-HnrE",
    "pattern": "(\\$_(POST|GET|COOKIE|REQUEST|SERVER|FILES)|php://(input|stdin))"
}
```

Um die Musterlänge und -komplexität ein wenig zu reduzieren, können Sie auch eine Liste mit mehreren Mustern angeben:

```
▶ cat ~/.gf/php-sources-multiple.json
{
    "flags": "-HnrE",
    "patterns": [
        "\\$_(POST|GET|COOKIE|REQUEST|SERVER|FILES)",
        "php://(input|stdin)"
    ]
}
```


Es gibt einige weitere Beispielmusterdateien im Verzeichnis `examples`.

Sie können das Flag `-save` verwenden, um Musterdateien über die Befehlszeile zu erstellen:

```
▶ gf -save php-serialized -HnrE '(a:[0-9]+:{|O:[0-9]+:"|s:[0-9]+:")'
```

### Auto Complete

Es ist ein Skript zur automatischen Vervollständigung enthalten, Sie können also auf die Tabulatortaste klicken, um Ihre Optionen anzuzeigen:

```
▶ gf <tab>
base64       debug-pages  fw           php-curl     php-errors   php-sinks    php-sources  sec          takeovers    urls
```

#### Bash

To get auto-complete working you need to `source` the `gf-completion.bash` file in your `.bashrc` or similar:

```
source ~/path/to/gf-completion.bash
```

#### Zsh

To get auto-complete working you need to enable autocomplete (not needed if you have oh-my-zsh) using `autoload -U compaudit && compinit` or by putting it into `.zshrc`

Then `source` the `gf-completion.zsh` file in your `.zshrc` or similar:

```
source ~/path/to/gf-completion.zsh
```

Note: if you're using oh-my-zsh or similar you may find that `gf` is an alias for `git fetch`. You can either alias the gf binary to something else, or `unalias gf` to remove the `git fetch` alias.

### Using custom engines

There are some amazing code searching engines out there that can be a better replacement for [[grep]]. A good example is. It's faster (like **way faster**) and presents the results in a more visually digestible manner. In order to utilize a different engine, add `engine: <other tool>` to the relevant pattern file:

```shell
# Using the silver searcher instead of grep for the aws-keys pattern:
# 1. Adding "ag" engine
# 2. Removing the E flag which is irrelevant for ag
{
  "engine": "ag",
  "flags": "-Hanr",
  "pattern": "([^A-Z0-9]|^)(AKIA|A3T|AGPA|AIDA|AROA|AIPA|ANPA|ANVA|ASIA)[A-Z0-9]{12,}"
}
```

- Note: Different engines use different flags, so in the example above, the flag `E` has to be removed from the `aws-keys.json` file in order for ag to successfully run.

## Install

If you've got Go installed and configured you can install `gf` with:

```
▶ go get -u github.com/tomnomnom/gf
```

If you've installed using `go get`, you can enable auto-completion to your `.bashrc` like this:

```
▶ echo 'source $GOPATH/src/github.com/tomnomnom/gf/gf-completion.bash' >> ~/.bashrc
```

Note that you'll have to restart your terminal, or run `source ~/.bashrc` for the changes to take effect.

To get started quickly, you can copy the example pattern files to `~/.gf` like this:

```
▶ cp -r $GOPATH/src/github.com/tomnomnom/gf/examples ~/.gf
```

My personal patterns that I've included as examples might not be very useful to you, but hopefully they're still a reasonable point of reference.

## Usage

```
$ printf example.com | gau
$ cat domains.txt | gau --threads 5
$ gau example.com google.com
$ gau --o example-urls.txt example.com
$ gau --blacklist png,jpg,gif example.com
```