## Website

https://github.com/tomnomnom/qsreplace

## Description

Akzeptieren Sie URLs auf stdin, ersetzen Sie alle Abfragezeichenfolgenwerte durch einen vom Benutzer bereitgestellten Wert, geben Sie jede Kombination von Abfragezeichenfolgenparametern nur einmal pro Host und Pfad aus.

## Usage

### Example input file:

```
▶ cat urls.txt 
https://example.com/path?one=1&two=2
https://example.com/path?two=2&one=1
https://example.com/pathtwo?two=2&one=1
https://example.net/a/path?two=2&one=1
```

### Replace Query String Values

```
▶ cat urls.txt | qsreplace newval
https://example.com/path?one=newval&two=newval
https://example.com/pathtwo?one=newval&two=newval
https://example.net/a/path?one=newval&two=newval
```

### Append to Query String Values

```
▶ cat urls.txt | qsreplace -a newval
https://example.com/path?one=1newval&two=2newval
https://example.com/pathtwo?one=1newval&two=2newval
https://example.net/a/path?one=1newval&two=2newval
```

### Remove Duplicate URL and Parameter Combinations

```
▶ cat urls.txt | qsreplace -a 
https://example.com/path?one=1&two=2
https://example.com/pathtwo?one=1&two=2
https://example.net/a/path?one=1&two=2
```