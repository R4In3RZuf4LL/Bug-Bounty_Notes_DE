# Echo (Informatik)

**Echo** ist ein einfaches allgemeines Konzept in der Informatik, das sich an das akustische Echo anlehnt. Es wird in vielen Werkzeugen eingesetzt, vor allem auf der Kommandozeile unter beinahe allen Betriebssystemen, in Programmiersprachen als Ausgabekommando und in Netzwerken. Die grundlegende Funktionsweise ist überall die gleiche wie bei der identischen Abbildung in der Mathematik: Es wird eine Eingabe (meistens in Form einer Zeichenkette) entgegengenommen und unverändert wieder ausgegeben. Hinzu kommt noch besonders im Netzwerkbereich eine wichtige Verzögerung zwischen der Eingabe und der Ausgabe bzw. deren Empfang, wie sie auch beim akustischen Vorbild vorhanden ist.

## Kommandozeile und Skriptsprachen

Das Kommando `echo` gehört bei den meisten Kommandozeilen, die in umfangreichen Programmen und insbesondere in Betriebssystemen bereitgestellt werden, zum Standardbefehlsumfang. Dieses Kommando wird dort für die Ausgabe von Zeichenketten und Variablen. "Variable (Programmierung)") auf einem Ausgabegerät wie dem Bildschirm oder einer Datei verwendet. Die Verwendung kann entweder direkt in der Kommandozeile oder innerhalb eines Shellskripts – unter DOS und Windows auch _Batch-Dateien genannt – erfolgen. Wenn der Benutzer die Ausgabe in eine Datei umleitet, kann er damit eine Textdatei erstellen oder die Ausgabe an eine vorhandene Textdatei anhängen. Einige Kommandozeileninterpreter, die das echo-Kommando unterstützen, sind bash, csh, COMMAND.COM, cmd.exe, Windows PowerShell, aber auch die Konsole des Computerspiels _Half-Life.

Einige Skriptsprachen, die ein echo-Kommando anbieten, sind für die direkte Verwendung mit einer Kommandozeile nicht vorgesehen, sondern eignen sich hauptsächlich für die Verwendung in Skriptdateien, dazu gehört beispielsweise PHP.

Unter DOS und Windows lässt sich mit dem echo-Kommando auch die Ausgabe der ausgeführten Kommandos auf dem Bildschirm steuern, wenn sie innerhalb einer Stapelverarbeitungsdate aufgerufen wurden. Dabei erscheint jede Zeile der Datei noch mal auf dem Bildschirm oder einem anderen Ausgabegerät, solange dies nicht mit `echo off` abgeschaltet wurde; `echo on` schaltet die Ausgabe wieder ein.

## Rechnerkommunikation

In Rechnernetzen kommt dieses Konzept ebenfalls häufig zum Einsatz. In der Internetprotokollfamilie gibt es das Echo-Protokoll, das für den Einsatz mit einem Echo-Netzwerkdienst vorgesehen ist. Die Aufgabe eines Servers, der diesen Netzwerkdienst empfangenen Daten unverändert zurückzusenden. Die Spezifikation erfolgt in den [[RFC 862]]

Außerdem definiert der ICMP-Standard unter anderem zwei Typen von Nachrichten echo request und echo reply. Diese Nachrichten werden auch als _Ping-_ bzw. _Pong-Pakete_ bezeichnet und sind hauptsächlich für Diagnosezwecke vorgesehen, um die Erreichbarkeit eines Rechners im Netzwerk zu überprüfen. Dabei wird an einen Zielrechner eine Nachricht mit Nutzdaten, die üblicherweise mit Buchstaben des Alphabets gefüllt sind, gesendet. Der Zielrechner empfängt diese Nachricht und schickt die gleichen Nutzdaten unverändert wieder zurück an den Empfänger. Wenn der Empfänger die Antwortnachricht erhält, wertet er die für die Übertragung (Hin- und Rückrichtung zusammen) benötigte Zeit aus. Dies geschieht beispielsweise bei dem in vielen Betriebssystemen zum Standardlieferumfang gehörenden Ping. Der ICMP-Standard wird in dem [[RFC 792]] festgelegt.

Des Weiteren wird der Begriff _Echo_ beim Telnet-Protokoll für die Option verwendet, die festlegt, ob die über die Telnet-Verbindung zurückgesendete Zeichen ausgegeben werden sollen oder nicht. Die Option hat an dieser Stelle also eine ähnliche Bedeutung wie die _on_- und _off_-Parameter bei dem _cmd.exe_-Kommando _echo_. Spezifikation in [[RFC 857]].

Ein anderes anschauliches Beispiel für die Anwendung dieses Konzepts ist der Echo-Mailer. Dabei wird von jenem der Inhalt jeder empfangenen E-Mail mitsamt Kopfdaten an den Absender zurückgeschickt.