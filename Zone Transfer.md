
## Description

Unter Zonentransfer versteht man im DNS die Übertragung von Zonen auf andere DNS-Server. Dieses Verfahren wird, wie wir bereits erfahren haben, als Asynchronous Full Transfer Zone (AXFR) bezeichnet. Da ein DNS-Ausfall für ein Unternehmen meist schwerwiegende Folgen hat, werden die Zonendateien auf mehreren Nameservern nahezu ausnahmslos identisch gehalten. Bei Änderungen muss sichergestellt werden, dass alle Server über den gleichen Datenbestand verfügen. Bei der Zonenübertragung handelt es sich lediglich um die Übertragung von Dateien oder Datensätzen und die Erkennung von Unstimmigkeiten in den Datenbanken der beteiligten Server. Die Konfiguration von DNS-Servern erfordert große Aufmerksamkeit, da viele Administratoren die Funktionsweise aus der technischen Konfiguration und Fehlerbehebung nicht immer vollständig verstehen können. Dies führt zu Fehleranfälligkeit und Anfälligkeit, die zu Fehlkonfigurationen führen und das System oder sogar die gesamte Infrastruktur gefährden, indem der gesamte Inhalt der Zonen eingesehen werden kann.

## Usage:

```bash
dig axfr domain @ip
```