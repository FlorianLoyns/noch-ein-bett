# Habt ihr noch ein Bett?

**Endlos-Quiz zur Bettenzuweisung für die Pflegeausbildung**

[![Lizenz: CC BY-NC-SA 4.0](https://img.shields.io/badge/Lizenz-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.de)
[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-blue)](https://florianloyns.github.io/noch-ein-bett/)
![Keine Abhängigkeiten](https://img.shields.io/badge/Abh%C3%A4ngigkeiten-keine-brightgreen)
![PWA](https://img.shields.io/badge/PWA-offline--f%C3%A4hig-teal)

Ein browser-basiertes Endlos-Quiz für Pflege-Azubis: Neue Aufnahmen kommen rein, du entscheidest unter Zeitdruck, wohin die Patient:innen gebettet werden. Richtige Entscheidungen verlängern die Schicht, Fehler kosten Zeit.

**[Jetzt spielen](https://florianloyns.github.io/noch-ein-bett/)**

## Spielprinzip

Oben steht eine neue Aufnahme (Name, Alter, Sex, Diagnose, Iso-Flags). Darunter fünf Bett-Optionen mit Zimmernummer, Mitbewohner:innen-Infos und Iso-Status. Per **Tap oder Drag-&-Drop** wird die Patient:in in eines der Betten platziert.

Getestet wird, ob die gewählte Kombination hygienisch, räumlich und sozial zulässig ist — Isolationsregeln, Kohorten-Konstellationen, Geschlechtertrennung, Schutz vor Infekten bei Immunsupprimierten und psychosoziale Verträglichkeit (Alter, Sterbebegleitung).

Ein Timer läuft kontinuierlich. Richtige Entscheidung: +7 Sekunden. Falsche: −3 Sekunden. Timer bei 0 → Schichtende. Zwischen den Runden tauchen Bonus-Ereignisse auf.

## Was das Spiel trainiert

- **Isolationsregeln**: MRSA, C. difficile, Noro, VRE, Tröpfchen, 3MRGN vs. 4MRGN
- **Kohortenisolation** vs. Einzelzimmer-Pflicht
- **Immunsuppression**: Risikokonstellationen erkennen
- **Geschlechtertrennung** im Mehrbettzimmer
- **Psychosoziale Verträglichkeit**: Demenz, Unruhe, Sterbebegleitung, Altersabstand
- **Patientenidentifikation**: Namen lesen, Diagnose zuordnen, Fall erkennen
- **Entscheidungstransfer unter Zeitdruck**

## Kern-Mechanik

| Element         | Effekt                                          |
| --------------- | ----------------------------------------------- |
| Start-Timer     | 65 Sekunden, Drain 30 % langsamer als Echtzeit  |
| Richtig         | +7 s Zeit, Basis +10 Punkte + Streak-Bonus      |
| Falsch          | −3 s Zeit, Streak auf 0                          |
| Streak ab 3     | Zusätzliche +3 bis +15 Bonus-Punkte pro Antwort |
| Endlos-Modus    | Patient:innen kommen bis Timer = 0              |
| Highscore       | Lokal in `localStorage` (kein Server)           |
| XP-System       | +1 XP pro richtig — dauerhafter Fortschritt     |

## Bonus-Ereignisse

**🌟 Goldener Patient** (3 % Chance pro Runde) — Punkte ×3, +10 s Zeit bei richtig. Karte wird golden umrandet.

**☕ Kaffeepause** (bei 10er-Streak) — Timer pausiert für 5 Sekunden.

**⚡ Aufnahme-Rush** (bei 12er-Streak) — 3 Runden mit doppelten Punkten und doppeltem Timer-Drain.

**🎯 Infekt-Schwarm** (bei 15er-Streak) — Die nächsten 5 Patient:innen sind alle Isolations-Fälle.

**Identifikations-Events** (ca. 14 % Chance pro Runde) — Statt einer neuen Aufnahme kommt eine alltagsrealistische Aufgabe:

- Angehörige an der Tür — wer ist die gesuchte Person?
- Blutabnahme fällig — welche:r Patient:in?
- Oberärztin will Sono beim Fall mit Diagnose X
- Chefarzt will zur Visite
- Am Telefon Hausarzt
- Antibiotika-Gabe bei 4MRGN-Fall
- Diät-Verteilung (Diabetes-Kost)
- Spezialkost für Immunsupprimierte
- Klingelalarm

Fünf zufallsgenerierte Patient:innen-Kacheln stehen zur Auswahl, eine ist die richtige. Trainiert Namens-/Diagnose-Zuordnung und Aufmerksamkeit.

**🏃 Personalausfall** (5 % Chance) — Kollegin krank, sofort −2 s Zeitstrafe.

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

## Didaktischer Hintergrund

Das Spiel ist als **Übungsimpuls** konzipiert — es ersetzt keine Hygienepläne, Einrichtungs-Standards oder Stationsleitungs-Schulung. Regeln sind generalisiert, reale Entscheidungen hängen von Klinikstandard und Einzelfall ab.

Das Prinzip ist **integriertes Entscheiden unter Zeitdruck** — die Kompetenz, an der Pflegefachkräfte im Alltag gemessen werden, die aber in der Ausbildung oft zu kurz kommt, weil sie im Skills Lab kaum simuliert werden kann. Das Endlos-Format und die variablen Belohnungen lehnen sich an Suchtmechaniken erfolgreicher Lern-Apps an — gezielt eingesetzt für kognitiven Transfer statt Inhaltsvermittlung.

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

Verantwortlich: Florian Loyns. Pflichtangaben nach § 5 DDG und Kontakt: [Impressum](https://florianloyns.github.io/Impressum/)

## Lizenz

[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.de) · Nutzen, anpassen und teilen – unter Namensnennung, nicht-kommerziell und unter gleichen Bedingungen.
