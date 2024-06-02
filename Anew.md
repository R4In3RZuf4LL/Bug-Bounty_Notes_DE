## Website

https://github.com/tomnomnom/anew

## Description

Hängen Sie Zeilen von [[STDIN]] an eine Datei an, aber nur, wenn sie nicht bereits in der Datei erscheinen. Gibt auch neue Zeilen an [[stdout]] aus, was ein bisschen wie ein `tee -a` ist, der Duplikate entfernt.

## Usage

Hier enthält eine Datei namens `things.txt` eine Liste mit Zahlen. `newthings.txt` enthält eine zweite Liste von Zahlen, von denen einige in `things.txt` vorkommen und andere nicht. Letzteres wird mit `anew` an die Datei `things.txt` angehängt.

```
▶ cat things.txt
Zero
One
Two

▶ cat newthings.txt
One
Two
Three
Four

▶ cat newthings.txt | anew things.txt
Three
Four

▶ cat things.txt
Zero
One
Two
Three
Four
```

Beachten Sie, dass die neuen Zeilen, die der Datei `things.txt` hinzugefügt wurden, auch an [[stdout]] gesendet werden. Dadurch können sie in eine andere Datei umgeleitet werden:

```
▶ cat newthings.txt | anew things.txt > added-lines.txt
▶ cat added-lines.txt
Three
Four
```

Um die Ausgabe in [[stdout]] anzuzeigen, aber nicht an die Datei anzuhängen, verwenden Sie die Probelaufoption `-d`.  
Um an die Datei anzuhängen, aber nichts auf [[stdout]] zu drucken, verwenden Sie den Ruhemodus `-q`.
