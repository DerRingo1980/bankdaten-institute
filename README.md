# Bankdaten.de Bankinstitute-Verzeichnis

Offener, redaktionell aufbereiteter Datensatz deutscher Kreditinstitute. Gepflegt von [Bankdaten.de](https://www.bankdaten.de), einem unabhängigen Ratgeberportal zu Banken, Konten und Finanzprodukten.

## Inhaltsverzeichnis

- [Worum es geht](#worum-es-geht)
- [Warum dieses Repository existiert](#warum-dieses-repository-existiert)
- [Zielgruppe](#zielgruppe)
- [Datenquelle und Methodik](#datenquelle-und-methodik)
- [Datenstruktur](#datenstruktur)
- [Enthaltene Datensätze](#enthaltene-datensätze)
- [Besonderheit bei Direktbanken und Neobanken](#besonderheit-bei-direktbanken-und-neobanken)
- [Aktualisierung](#aktualisierung)
- [Häufige Fragen](#häufige-fragen)
- [Verwendung](#verwendung)
- [Quelle der Rohdaten](#quelle-der-rohdaten)

## Worum es geht

Die Deutsche Bundesbank veröffentlicht monatlich eine vollständige Liste aller in Deutschland tätigen Kreditinstitute mit Bankleitzahl (BLZ), BIC und weiteren technischen Merkmalen. Diese Rohdatei ist zwar amtlich und vollständig, in ihrer Originalform jedoch wenig zugänglich: uneinheitliche Zeichenkodierung, technische Feldbezeichnungen und ein hoher Anteil an internen Geschäftsfeld- und Altdaten-Einträgen, die für die praktische Nutzung wenig Mehrwert bieten.

Dieses Repository bereitet die Rohdaten auf, bereinigt und strukturiert sie und macht sie so für Entwickler, Journalisten und andere Publisher frei nutzbar. Im Mittelpunkt stehen dabei drei Institutsgruppen, die in Deutschland das Bankwesen prägen: Sparkassen, Volksbanken und Raiffeisenbanken sowie Direktbanken und Neobanken.

## Warum dieses Repository existiert

Amtliche Bankdaten sind zwar öffentlich zugänglich, liegen aber selten in einer Form vor, die sich ohne technischen Vorlauf nutzen lässt. Wer eine Bankleitzahl, eine BIC oder eine Institutsliste für eine Recherche, eine Anwendung oder einen redaktionellen Beitrag benötigt, muss die Bundesbank-Rohdatei sonst selbst bereinigen, Altdaten aussortieren und Dubletten erkennen.

Genau diese Vorarbeit übernimmt Bankdaten.de mit diesem Repository. Die Aufbereitung folgt denselben redaktionellen Qualitätsstandards, die auch auf der Website verwendet werden: nachvollziehbare Methodik, transparente Ausschlusskriterien und ein Änderungsprotokoll bei jeder Aktualisierung. Damit ist der Datensatz nicht nur eine technische Ressource, sondern auch ein Beleg für die redaktionelle Sorgfalt hinter Bankdaten.de insgesamt.

## Zielgruppe

Der Datensatz richtet sich an mehrere Nutzergruppen gleichermaßen:

- **Entwickler und Fintech-Teams**, die eine verlässliche, strukturierte BLZ- und BIC-Liste für eigene Anwendungen benötigen, etwa für IBAN-Rechner oder Kontoformulare.
- **Journalistinnen und Journalisten**, die für Recherchen zum deutschen Bankwesen belastbare, aktuelle Institutslisten brauchen.
- **Forschende und Studierende**, die sich mit der Struktur des deutschen Kreditwesens (Drei-Säulen-Modell aus privaten Banken, öffentlich-rechtlichen Sparkassen und genossenschaftlichen Volksbanken) beschäftigen.
- **Andere Publisher und Website-Betreiber**, die eigene Bankvergleiche oder Ratgeberinhalte auf einer belastbaren Datengrundlage aufbauen möchten.

## Datenquelle und Methodik

Grundlage ist die aktuelle BLZ-Datei der Deutschen Bundesbank (Stand: Juni 2026, 13.806 Datensätze). Für die Aufbereitung gelten folgende Regeln:

- Es werden ausschließlich Hauptstellen berücksichtigt (Merkmal „1“ der Bundesbank-Klassifikation), Geschäftsfeld- und Nebenstellen-Einträge (Merkmal „2“) entfallen.
- Zeichenkodierungsfehler der Originaldatei (fehlerhafte Umlautdarstellung) wurden korrigiert.
- Veraltete Institutsbezeichnungen, erkennbar am Zusatz „-alt-“, wurden entfernt, da sie keine aktuell gültigen Institutsnamen mehr abbilden.
- Feldnamen wurden von den technischen Bundesbank-Bezeichnungen in sprechende, konsistente Namen überführt.

Diese Regeln gelten einheitlich für alle drei Institutsgruppen, mit einer Ausnahme: Bei Direktbanken und Neobanken war ein zusätzlicher, redaktioneller Verifizierungsschritt nötig (siehe unten).

## Datenstruktur

Jeder Datensatz enthält folgende Felder:

- **name**: vollständige, offizielle Institutsbezeichnung
- **kurzname**: Kurzbezeichnung laut Bundesbank
- **blz**: achtstellige Bankleitzahl
- **bic**: Bank Identifier Code (sofern vorhanden)
- **plz**: Postleitzahl des Hauptsitzes
- **ort**: Sitz des Instituts

Die zusammengeführte Datei `institute-gesamt` enthält zusätzlich das Feld **gruppe** zur Unterscheidung der drei Institutsgruppen, die Datei `direktbanken-neobanken` zusätzlich das Feld **kategorie** zur Unterscheidung von Neobank und Direktbank.

## Enthaltene Datensätze

Die folgende Tabelle gibt einen Überblick über alle im Repository enthaltenen Dateien, die jeweilige Institutsgruppe sowie ein Beispielinstitut zur Einordnung:

| Datei | Institutsgruppe | Anzahl | Beispielinstitut |
|---|---|---|---|
| `data/sparkassen.json` / `.csv` | Sparkassen (Hauptstellen, bereinigt) | 248 | Hamburger Sparkasse |
| `data/volksbanken-raiffeisenbanken.json` / `.csv` | Volksbanken und Raiffeisenbanken (Hauptstellen, bereinigt) | 599 | Volksbank Chemnitz |
| `data/direktbanken-neobanken.json` / `.csv` | Direktbanken und Neobanken (kuratiert) | 13 | N26 Bank, ING-DiBa |
| `data/institute-gesamt.json` / `.csv` | Alle drei Gruppen zusammengeführt, mit Feld „gruppe“ | 860 | (alle vorgenannten) |

Bei den Volksbanken und Raiffeisenbanken wurden neben den „-alt-“-Einträgen zusätzlich technische Sammel- und Geschäftsfeldeinträge entfernt, etwa reine Geldautomaten-Bankleitzahlen (Kennzeichnung „GAA“) sowie Zentral- und Rechenzentrum-Einträge, die keine eigenständige Bankfiliale repräsentieren.

## Besonderheit bei Direktbanken und Neobanken

Anders als Sparkassen oder Volksbanken lassen sich Direktbanken und Neobanken nicht über ein gemeinsames Namensmuster aus der Bundesbank-Datei filtern, da ihre Markennamen (N26, Trade Republic, comdirect, DKB u. a.) uneinheitlich sind und teilweise als Geschäftsfeld eines größeren Instituts geführt werden (comdirect etwa als Geschäftsfeld der Commerzbank, 1822direkt als Geschäftsfeld der Frankfurter Sparkasse). Diese Gruppe wurde daher redaktionell kuratiert und einzeln gegen die Rohdaten verifiziert, mit dem Feld `kategorie` zur Unterscheidung zwischen „Neobank“ (mobile-first, ausschließlich App-basiertes Banking) und „Direktbank“ (etablierte Online-Bank ohne Filialnetz). Bekannte Marken ohne eigenständigen, eindeutig identifizierbaren BLZ-Eintrag in der amtlichen Datei (etwa Consorsbank oder netbank) wurden bewusst ausgeklammert, um keine unsicheren Zuordnungen zu veröffentlichen. Bei bundesweit tätigen Instituten mit mehreren regionalen Verteil-Bankleitzahlen (etwa Openbank mit zwölf BLZ) wird nur die repräsentative Haupt-BLZ geführt.

Weitere Institutsgruppen sind in Vorbereitung und werden sukzessive ergänzt.

## Aktualisierung

Die Bundesbank veröffentlicht ihre BLZ-Datei monatlich. Dieses Repository wird in unregelmäßigen Abständen entsprechend aktualisiert. Änderungen einzelner Institute (Fusionen, BLZ-Löschungen, Nachfolge-Bankleitzahlen) werden im jeweiligen Update-Commit dokumentiert.

## Häufige Fragen

**Sind die Daten offiziell und verlässlich?**
Ja, die zugrunde liegenden Rohdaten stammen direkt von der Deutschen Bundesbank. Die redaktionelle Bereinigung (Entfernen von Altdaten und technischen Einträgen) verändert nicht den amtlichen Charakter der zugrunde liegenden BLZ und BIC, sondern erleichtert lediglich die praktische Nutzung.

**Warum fehlen manche bekannten Direktbanken wie Consorsbank?**
Weil sich für diese Marken kein eindeutig zuordenbarer, eigenständiger Eintrag in der amtlichen Bundesbank-Datei finden ließ. Um keine unsicheren Zuordnungen zu veröffentlichen, wurden solche Fälle bewusst ausgeklammert (siehe Abschnitt „Besonderheit bei Direktbanken und Neobanken“).

**Darf ich die Daten kommerziell nutzen?**
Ja, die CC BY 4.0 Lizenz erlaubt auch kommerzielle Nutzung, solange Bankdaten.de als Quelle genannt wird (Details im Abschnitt „Verwendung“).

**Wie oft werden die Daten aktualisiert?**
In unregelmäßigen Abständen, orientiert an der monatlichen Veröffentlichung der Bundesbank. Größere Änderungen wie Bankfusionen werden zeitnah nachgezogen.

## Verwendung

Die Daten stehen unter der [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.de) Lizenz zur freien Nutzung zur Verfügung (Details siehe Datei `LICENSE` im Repository-Root), Weiterverwendung mit Quellenangabe („Bankdaten.de“, verlinkt auf https://www.bankdaten.de) ist ausdrücklich erwünscht.

Für redaktionell aufbereitete Einzelprofile der jeweiligen Institute (Konditionen, Einlagensicherung, Vor- und Nachteile) siehe die entsprechenden Ratgeberseiten auf [Bankdaten.de](https://www.bankdaten.de).

## Quelle der Rohdaten

Deutsche Bundesbank: Bankleitzahlen-Datei (BLZ-Verzeichnis), monatliche Veröffentlichung, https://www.bundesbank.de/de/aufgaben/unbarer-zahlungsverkehr/serviceangebot/bankleitzahlen
