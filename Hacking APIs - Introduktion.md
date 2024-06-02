
Forscher haben herausgefunden, dass 80% des Webtraffics durch calls an application programming interface  ([[API]]) stattfindet. Dadurch bieten sie einen großartigen Angriffsvektor. Alleine dadurch, das sie so konzipiert sind um Daten an andere Anwendungen zu übergeben. 

## Der Reiz Web APIs zu hacken

2017 schrieb "[[The Economist]]" folgende Überschrift: "The world's most valuable ressouce is not longer oil, but data". [[API]]s sind digitale Pipelines, die es einem kostbaren Gut ermöglichen im Handumdrehen um die Erde zu fliegen.

Eine [[API]] ermöglicht eine Kommunikation zwischen verschiedenen Anwendungen. Zum Beispiel muss eine Python Anwendung mit eine in Java implementierten Anwendung interagieren, was schnell zu Chaos führen kann. Durch die Nutzung von [[API]]s können Entwickler modulare Anwendungen entwerfen, die die Daten anderer Anwendungen einfach nutzen können. Zum Beispiel müssen sie nicht mehr länger ihre eigene Custom-Software entwickeln um Karten, Bezahlmethoden, Machine-Learning Algorithms oder Authentifizierungsvorgänge etc. Daraus resultierte, dass Softwareentwickler nicht schnell genug sein konnten um [[API]]s zu implementieren. Oft werden neue Technologien angewendet, ohne das sie groß auf Vulnerabilitys getestet wird und [[API]]s haben die Angriffsfläche erheblich erhöht. Oft werden sie so schwach geschützt, das ein Angreifer sie als direkte Route zu internen Daten nützen können. Aus diesem Grund sagte schon [[Gartner]] Jahre zuvor, das 2022 [[API]]s der führende Attackvektor sein wird. Deswegen brauchen wir Hacker, die die Sicherheit solcher Systeme testen und Schwachstellen melden.

## Der Ansatz

[[API]]s anzugreifen, ist gar nicht so kompliziert. Sobald man versteht, wie sie funktionieren, ist es einzig wichtig den richtigen [[HTTP]] Request abzusetzen. Allerdings bedeutet dass auch, das die Tools die normalerweiße für Bug Hunting und Webapp-Penetrationtesting nicht gut funktionieren für [[API]]s. Es bringt nichts, einen üblichen Vulnerability-Scan über eine [[API]] laufen zu lassen und nützliche Ergebnisse zu erwarten. Wenn [[API]]s nicht sorgfältig getestet werden, sind Firmen in dem Gefühl der falschen Sicherheit und riskieren kompromittiert zu werden. 

## Das [[API]] Restaurant hacken

Bevor wir beginnen, habe ich hier noch eine Metapher: Stell dir vor eine [[API]] ist ein Restaurant. wie eine [[API]] Dokumentation, das Menü beschreibt, welche Art von Sachen du bestellen kannst. Du kannst dir den Gast und den Restaurantbesitzer vorstellen. Die [[API]] ist der Kellner. Man kann Requests an den Kellner geben um aus dem Menü zu wählen und der Kellner wird es bringen. Ein [[API]] User muss nicht wissen, wie der Chef aussieht oder was im Backend (Küche) passiert. Stattdessen sollte er in der Lage sein, einem Set aus Anweisungen auszuführen um einen Request and die Küche zu senden und den passenden Response zu erhalten. Entwickler können ihre Anwendung so schreiben, um den Request so wie sie wollen auszuführen. Als [[API]] Hacker, muss man jeden Teil des fiktiven Restaurants testen. Man muss verstehen, wie das Restaurant operiert, vielleicht will man an dem Türsteher vorbei kommen, oder ein gestohlenes [[Token]] anbieten. Außerdem sollte das Menü geprüft werden um an Daten zu kommen, die nicht für einen bestimmt sind. Man kann den Kellner versuchen auszutricksen, das er einem alles gibt was er hat. Oder man überzeugt den Besitzer einem gleich die Schlüssel fürs ganze Restaurant zu geben. Es werden folgende Topics abgehandelt:

+ Verstehen wie Web Apps arbeiten und die Anatomie von Web [[API]]s
+ Die Top [[API]] Schwachstellen von einer Hacker Perspektive.
+ Die effizientesten [[API]] Hacking Tools
+ Passive und Aktive [[API]] reconnaissance um die Existenz einer [[API]] herauszufinden, exposed Secrets, und um die [[API]] Funktionalität zu analysieren.
+ Mit [[API]]s zu interagieren und mittels fuzzing zu testen.
+ Eine Vielzahl von Angriffen um [[API]] Vulnerabilitys zu finden und auszunutzen.