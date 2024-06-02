Kryptographie bzw. Kryptografie (altgriechisch kryptós, deutsch ‚verborgen, geheim und γράφειν gráphein, deutsch ‚schreiben‘) ist ursprünglich die Wissenschaft der Verschlüsselung von Informationen. Heute befasst sie sich auch allgemein mit dem Thema Informationssicherheit, also der Konzeption, Definition und Konstruktion von Informationssystemen, die widerstandsfähig gegen Manipulation und unbefugtes Lesen sind.

## Terminologie
Der Begriff Kryptographie bedeutet Geheimschrift. Die Kryptographie befasste sich historisch mit der Erzeugung, Betrachtung und Beschreibung von Verfahren, um „geheim zu schreiben“, also mit Verschlüsselungsverfahren. Seit Ende des 20. Jahrhunderts werden sie zur sicheren Kommunikation und für sichere Berechnungen eingesetzt.

Kryptoanalyse (auch Kryptanalyse) bezeichnet hingegen die Erforschung und Anwendung von Methoden, mit denen kryptographische Verfahren gebrochen („geknackt“), also unbefugt entschlüsselt werden können.

Ein Kryptosystem dient zur Geheimhaltung von übertragenen oder gespeicherten Texten oder Daten gegenüber Dritten.

Oft werden die Begriffe Kryptographie und Kryptologie gleichwertig benutzt, während sich z. B. beim US-Militär Kryptographie meist auf kryptographische Techniken bezieht und Kryptologie als Oberbegriff für Kryptographie und Kryptoanalyse verwendet wird. Die Kryptographie kann also auch als Teilgebiet der Kryptologie gesehen werden. 

Das Untersuchen von Merkmalen einer Sprache, die Anwendung in der Kryptographie finden (z. B. Buchstabenkombinationen), wird Kryptolinguistik genannt.

## Abgrenzung zur Steganographie
Sowohl Kryptographie als auch Steganographie haben zum Ziel, die Vertraulichkeit einer Nachricht zu schützen. Allerdings unterscheiden sie sich im Ansatzpunkt der Verfahren:

Kryptographie verschlüsselt die Nachricht. Somit sorgt sie dafür, dass eine unbeteiligte dritte Person, die die (verschlüsselten) Daten zu Gesicht bekommt, die Bedeutung nicht erfassen kann.
Steganographische Verfahren verbergen den Kanal, über den kommuniziert wird. Eine unbeteiligte dritte Person bleibt dadurch in Unkenntnis der Kommunikation.
Kryptographische und steganographische Verfahren können kombiniert werden. Beispielsweise führt eine Verschlüsselung (Kryptographie) einer Nachricht, die über einen verdeckten Kanal kommuniziert wird (Steganographie), dazu, dass selbst nach dem Entdecken und erfolgreichen Auslesen des Kanals der Inhalt der Nachricht geheim bleibt.

## Ziele der Kryptographie
Die moderne Kryptographie hat vier Hauptziele zum Schutz von Datenbeständen, Nachrichten oder Übertragungskanälen:

+ Vertraulichkeit/Zugriffsschutz: Nur dazu berechtigte Personen sollen in der Lage sein, die Daten oder die Nachricht zu lesen oder Informationen über ihren Inhalt zu erlangen.
+ Integrität/Änderungsschutz: Die Daten müssen nachweislich vollständig und unverändert sein.
+ Authentizität/Fälschungsschutz: Der Urheber der Daten oder der Absender der Nachricht soll eindeutig identifizierbar sein, und seine Urheberschaft sollte nachprüfbar sein.
+ Verbindlichkeit/Nichtabstreitbarkeit: Der Urheber der Daten oder Absender einer Nachricht soll nicht in der Lage sein, seine Urheberschaft zu bestreiten, d. h., sie sollte sich gegenüber Dritten nachweisen lassen.

Kryptographische Verfahren und Systeme dienen nicht notwendigerweise gleichzeitig allen der hier aufgelisteten Ziele.

## Methoden der Kryptographie
Kryptographische Verfahren werden in die klassischen und modernen Verfahren unterteilt.

+ Methoden der klassischen Kryptographie: Solange für die Kryptographie noch keine elektronischen Rechner eingesetzt wurden, ersetzte man bei der Verschlüsselung (zu dieser Zeit die einzige Anwendung der Kryptographie) immer vollständige Buchstaben oder Buchstabengruppen. Solche Verfahren sind veraltet und unsicher.
	+ Transposition: Die Buchstaben der Botschaft werden einfach anders angeordnet. Beispiel: Gartenzaunmethode oder Skytale.
	+ Substitution: Die Buchstaben der Botschaft werden durch jeweils einen anderen Buchstaben oder ein Symbol ersetzt; siehe monoalphabetische Substitution und polyalphabetische Substitution. Beispiele dafür sind die Caesar-Verschlüsselung und die Vigenère-Verschlüsselung.
	+ Codebuch, ebenfalls ein klassisches Verfahren.
	+ 
+ Methoden der modernen Kryptographie: Entsprechend der Arbeitsweise von Computern arbeiten moderne kryptographische Verfahren nicht mehr mit ganzen Buchstaben, sondern mit den einzelnen Bits der Daten. Dies vergrößert die Anzahl der möglichen Transformationen erheblich und ermöglicht außerdem die Verarbeitung von Daten, die keinen Text repräsentieren. Moderne Krypto-Verfahren lassen sich in zwei Klassen einteilen:
		+ Symmetrische Verfahren verwenden wie klassische kryptographische Verfahren einen geheimen Schlüssel pro Kommunikationsbeziehung und für alle Operationen (z. B. Ver- und Entschlüsselung) des Verfahrens
		+ Asymmetrische Verfahren verwenden pro Teilnehmer einen privaten (d. h. geheimen) und einen öffentlichen Schlüssel. Fast alle asymmetrischen kryptographischen Verfahren basieren auf Operationen in diskreten mathematischen Strukturen, wie endlichen Körpern, Ringen, elliptischen Kurven oder Gittern. Ihre Sicherheit basiert dann auf der Schwierigkeit bestimmter Berechnungsprobleme in diesen Strukturen. Viele symmetrische Verfahren und (kryptologische) Hashfunktionen sind dagegen eher Ad-hoc-Konstruktionen auf Basis von Bit-Verknüpfungen (z. B. XOR) und Substitutions-Tabellen für Bitfolgen. Einige symmetrische Verfahren, wie Advanced Encryption Standard, Secret-Sharing oder Verfahren zur Stromverschlüsselung auf Basis linear rückgekoppelter Schieberegister, verwenden aber auch mathematische Strukturen oder lassen sich in diesen auf einfache Weise beschreiben.