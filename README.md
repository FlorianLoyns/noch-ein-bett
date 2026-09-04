# Habt ihr noch ein Bett?

**Endlos-Quiz zur Bettenzuweisung für die Pflegeausbildung**

[![Code: MIT](https://img.shields.io/badge/Code-MIT-green.svg)](LICENSE) [![Inhalte: CC BY-SA 4.0](https://img.shields.io/badge/Inhalte-CC%20BY--SA%204.0-blue.svg)](LICENSE-CONTENT.md)
[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-blue)](https://florianloyns.github.io/noch-ein-bett/)
![Keine Abhängigkeiten](https://img.shields.io/badge/Abh%C3%A4ngigkeiten-keine-brightgreen)
![PWA](https://img.shields.io/badge/PWA-offline--f%C3%A4hig-teal)

Ein browser-basiertes Endlos-Quiz für Pflege-Azubis: Neue Aufnahmen kommen rein, du entscheidest unter Zeitdruck, wohin die Patient:innen gebettet werden. Richtige Entscheidungen verlängern die Schicht, Fehler kosten Zeit.

**[Jetzt spielen](https://florianloyns.github.io/noch-ein-bett/)**

## Spielprinzip

Oben steht eine neue Aufnahme (Name, Alter, Sex, Diagnose, Iso-Flags). Darunter drei Bett-Optionen mit Zimmernummer, Mitbewohner:innen-Infos und Iso-Status. Per **Tap oder Drag-&-Drop** wird die Patient:in in eines der Betten platziert.

Getestet wird, ob die gewählte Kombination hygienisch, räumlich und sozial zulässig ist — Isolationsregeln, Kohorten-Konstellationen, Geschlechtertrennung, Schutz vor Infekten bei Immunsupprimierten und psychosoziale Verträglichkeit (Alter, Sterbebegleitung).

Ein Timer läuft kontinuierlich. Richtige Entscheidung: +7 Sekunden. Falsche: −3 Sekunden. Timer bei 0 → Schichtende. Zwischen den Runden tauchen Bonus-Ereignisse auf.

## Was das Spiel trainiert

- **Isolationsregeln**: MRSA, C. difficile, Noro, VRE, Tröpfchen, 3MRGN vs. 4MRGN
- **Aerogene Isolation**: Tuberkulose, Masern, Windpocken — Einzelzimmer MIT Schleuse statt nur Einzelzimmer
- **Kohortenisolation** vs. Einzelzimmer-Pflicht
- **Immunsuppression**: Risikokonstellationen erkennen
- **Telemetrie-Pflicht**: Monitor-Bett-Bedarf erkennen
- **Pflegegrad-Balance**: hohe Pflegegrade nicht ungebremst zusammenlegen
- **Geschlechtertrennung** im Mehrbettzimmer
- **Psychosoziale Verträglichkeit**: Demenz, Unruhe, Sterbebegleitung, Altersabstand
- **Patientenidentifikation**: Namen lesen, Diagnose zuordnen, Fall erkennen
- **Entscheidungstransfer unter Zeitdruck**

## Kern-Mechanik

| Element         | Effekt                                          |
| --------------- | ----------------------------------------------- |
| Start-Timer     | 65 Sekunden, Drain 30 % langsamer als Echtzeit; Tempo steigt mit der Zahl richtiger Antworten dieser Schicht auf bis zu +45 % |
| Richtig         | +7 s Zeit, Basis +10 Punkte + Streak-Bonus      |
| Falsch          | −3 s Zeit, steigt bei mehreren Fehlern in Folge bis −6 s; Streak auf 0 |
| Streak ab 3     | Zusätzliche +3 bis +15 Bonus-Punkte pro Antwort |
| Endlos-Modus    | Patient:innen kommen bis Timer = 0              |
| Highscore       | Lokal in `localStorage` (kein Server)           |
| XP-System       | +1 XP pro richtig — dauerhafter Fortschritt     |

## Bonus-Ereignisse

**🌟 Goldener Patient** (3 % Chance pro Runde) — Punkte ×3, +10 s Zeit bei richtig. Karte wird golden umrandet.

**☕ Kaffeepause** (bei 10er-Streak) — Timer pausiert für 5 Sekunden.

**⚡ Aufnahme-Rush** (bei 12er-Streak) — 3 Runden mit doppelten Punkten und doppeltem Timer-Drain.

**🎯 Infekt-Schwarm** (bei 15er-Streak) — Die nächsten 5 Patient:innen sind alle Isolations-Fälle.

**🚨 Notfall-Ruf** (6 % Chance pro Runde, nach Aufwärmphase) — Kontrollverlust statt Zeitverlust: die Runde ist normal sichtbar, aber 5 Sekunden lang gesperrt, während der Timer ungebremst weiterläuft.

**🚫 Bett storniert** (10 % Chance pro Runde) — Mitten in einer normalen Runde fällt eine der falschen Optionen weg ("gerade durch die Notaufnahme belegt"). Die richtige Antwort bleibt immer wählbar, die Runde ist nie unlösbar.

**Identifikations-Events** (ca. 14 % Chance pro Runde) — Statt einer neuen Aufnahme kommt eine alltagsrealistische Aufgabe:

- Angehörige an der Tür — wer ist die gesuchte Person?
- Angehörige ohne Schutzkleidung aus dem Zimmer — bei wem war das?
- Blutabnahme fällig — welche:r Patient:in?
- Oberärztin will Sono beim Fall mit Diagnose X
- Chefarzt will zur Visite
- Am Telefon Hausarzt
- Antibiotika-Gabe bei 4MRGN-Fall
- Diät-Verteilung (Diabetes-Kost)
- Spezialkost für Immunsupprimierte
- Klingelalarm

Drei zufallsgenerierte Patient:innen-Kacheln stehen zur Auswahl, eine ist die richtige. Trainiert Namens-/Diagnose-Zuordnung und Aufmerksamkeit.

**🏃 Personalausfall** (10 % Chance pro Runde) — Kollegin krank, sofort −2 s Zeitstrafe.

## XP & Stations-Karriere

Jede richtige Antwort zählt dauerhaft XP an, gespeichert im `localStorage` des Browsers:

| Stufe | XP        |
| ----- | --------- |
| Auszubildende:r 1. Jahr | 0      |
| Auszubildende:r 2. Jahr | 50     |
| Auszubildende:r 3. Jahr | 150    |
| Pflegefachkraft         | 350    |
| Praxisanleitung         | 700    |
| Stationsleitung         | 1500   |
| Pflegedirektion         | 3000   |

Start-Screen zeigt aktuelle Stufe + Fortschrittsbalken. End-Screen zeigt gesammelte XP dieser Schicht und Aufstiegs-Fanfare bei erreichter Stufe.

**Progressive Schwierigkeit**: Die Karriere-Stufe bestimmt, welche Fallkategorien überhaupt vorkommen. 1. Jahr sieht nur Geschlecht/Alter-Fälle, mit jeder Stufe kommen mehr Kategorien dazu (Iso-Basics → psychosoziale Konflikte → 3MRGN/Immun → 4MRGN/Telemetrie → ab Pflegedirektion alles inkl. aerogener Isolation). Gilt für normale Runden und Identifikations-Events gleichermaßen.

Zum gezielten Testen einzelner Stufen kann die XP einmalig per URL gesetzt werden, z. B. `?setxp=1500` — die URL räumt den Parameter danach selbst auf, die XP bleibt aber gespeichert.

## Didaktischer Hintergrund

Das Spiel ist als **Übungsimpuls** konzipiert — es ersetzt keine Hygienepläne, Einrichtungs-Standards oder Stationsleitungs-Schulung. Regeln sind generalisiert, reale Entscheidungen hängen von Klinikstandard und Einzelfall ab.

Das Prinzip ist **integriertes Entscheiden unter Zeitdruck** — die Kompetenz, an der Pflegefachkräfte im Alltag gemessen werden, die aber in der Ausbildung oft zu kurz kommt, weil sie im Skills Lab kaum simuliert werden kann. Das Endlos-Format und die variablen Belohnungen lehnen sich an Suchtmechaniken erfolgreicher Lern-Apps an — gezielt eingesetzt für kognitiven Transfer statt Inhaltsvermittlung.

Eine bewusste Ausnahme von der Punkte-/Sound-Fanfare: bei korrekt gelöster Sterbebegleitung-Runde bleibt der sonst übliche Erfolgs-Sound aus. Der Inhalt bekommt keine Feier-Behandlung — auch nicht bei laufender Streak-Serie.

## Technik

- Einzelne HTML-Datei, Vanilla JavaScript, keine Frameworks
- Kein Build-Tool, kein externes CDN, keine Abhängigkeiten
- **PWA**: installierbar auf Desktop und Smartphone, offline-fähig via Service Worker
- **Drag-&-Drop + Tap** via Pointer-Events (funktioniert auf Touch und Maus)
- Highscore und XP persistent in `localStorage`
- Web Audio API für Sound-Feedback (kein Audio-File)
- Inline-SVG für Icons (kein Bild-Asset nötig)
- DSGVO-konform: keine Tracker, keine externen Ressourcen, keine Datenübertragung

## Verwendung im Unterricht

- **Einzelarbeit**: 5-10 Minuten vor oder nach einer Iso-Theoriestunde als Aufwärmer
- **Wettbewerb in der Klasse**: Wer schafft den höchsten Score in einer Schicht? Bestmarke wird lokal gespeichert und kann per Screenshot geteilt werden
- **Reflektion**: Nach einer Runde gezielt über falsche Antworten sprechen — der rote Feedback-Block nennt die verletzte Regel
- **Begleitung zum Hygieneunterricht**: als dauerhaft verfügbare Übungs-App im Browserzugriff

## Impressum

Verantwortlich: Florian Loyns. Pflichtangaben nach § 5 DDG und Kontakt: [Impressum](https://florianloyns.com/Impressum/)

## Lizenz

[MIT](LICENSE) für den Quelltext · [CC BY-SA 4.0](LICENSE-CONTENT.md) für die didaktischen Inhalte (Fälle, Fragen, Texte, Grafiken).

Nutzen, anpassen und weitergeben ist ausdrücklich erwünscht — auch im kommerziellen Kontext. Bei den Inhalten gilt: Namensnennung und Weitergabe bearbeiteter Fassungen unter denselben Bedingungen.
