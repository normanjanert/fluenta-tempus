# Scheduler Soll-Konzept

Dieses Dokument beschreibt den gewünschten Zielzustand ("Soll") für die Terminplanung
und Darstellung von Tasks in Fluenta Tempus. Es dient als Referenz für die folgende
Implementierung und Tests.

## 1. Begriffe & Hilfsfunktionen

- **Heute (`today`)**: Tagesbeginn in lokaler Zeit (`00:00`).
- **Dauer (`durationDays`)**: Anzahl Arbeitstage (>= 1) aus dem Task.
- **Fortschritt (`progress`)**: Prozentwert 0 – 100.
- **Arbeits-/Resttage**:
  - `workedDays = round(durationDays * progress/100)`
  - `remainingDays = max(1, ceil(durationDays * (1 - progress/100)))`
- **User-Start (`startDate`)**: Manuell gesetztes Datum im Task.
- **Festes Startdatum (`hasFixedStartDate`)**: true, wenn Task nicht verschoben werden darf.
- **Abhängigkeiten (`E_dep`)**: Früheste Startzeit durch Parent & Dependencies.
- **Ressourcenfenster (`E_res`)**: Frühestes Datum, an dem die Ressource wieder verfügbar ist.
- **User-Vorgabe (`E_user`)**:
  - Wenn `startDate` gesetzt: Datum als harter Start bei `hasFixedStartDate`, sonst als Mindestdatum.

Alle Datumsoperationen erfolgen in Arbeitstagen:
- `nextWorkingDay(date, resource)` und `previousWorkingDay(date, resource)` überspringen
  Wochenenden und Abwesenheiten der Ressource.
- `addWorkingDays(resource, start, days)` & `subtractWorkingDays(resource, end, days)`
  berücksichtigen Wochenenden und Absenzen.

## 2. Basis-Algorithmus pro Task

1. **Constraints sammeln**
   1. `E_dep` aus Parent/Dependencies (inkl. +1 Tag nach letzter Dependency).
   2. `E_res` aus Ressourcenverfügbarkeit, mindestens Projektstart.
   3. `E_user` (falls `startDate` gesetzt).
2. **Basisstart (`baseStart`)** = max(`E_dep`, `E_res`, `E_user` sofern Pflicht).
3. **Progress-Anker**: abhängig vom Fortschritt (siehe Fallmatrix unten).
4. **Start/End berechnen** unter Beachtung von Wochenenden/Abwesenheiten.
5. **Ressourcenverfügbarkeit aktualisieren**:
   - Für Zukunftsplanung (offene/teilweise Tasks) mit dem geplanten Ende.
   - Für abgeschlossene Tasks nur bis max(heute, tatsächliches Ende).

## 3. Fallmatrix nach Fortschritt & Starttyp

### 3.1 Fortschritt = 0 %

| Starttyp | Startberechnung | Ende | Hinweise |
| --- | --- | --- | --- |
| Kein User-Start | `start = max(baseStart, today)` → danach `nextWorkingDay/ getNextAvailableDate` | `end = addWorkingDays(start, durationDays-1)` | Task startet ab heute, respektiert Parent/Resource. |
| User-Start (nicht fix) | `start = max(baseStart, startDate, today)` | wie oben | User-Datum ist Mindestgrenze, wird aber auf heute gehoben falls in Vergangenheit. |
| Festes Startdatum | `start = max(baseStart, startDate)` ohne Verschiebung | `end` vom festen Start aus | Falls Start in Zukunft > heute, Task bleibt in der Zukunft. |

### 3.2 0 % < Fortschritt < 100 %

| Starttyp | Startberechnung | Ende | Hinweise |
| --- | --- | --- | --- |
| Kein User-Start | 1. `anchor = max(baseStart, today)`<br>2. `rawStart = subtractWorkingDays(anchor, workedDays)`<br>3. `start = max(rawStart, baseStart)` → danach `previousWorkingDay/getNextAvailableDate` | `end = addWorkingDays(start, remainingDays-1)` | Start liegt um die bereits gearbeiteten Tage in der Vergangenheit, aber nie vor Constraints. |
| User-Start (nicht fix) | Wie oben, aber `baseStart` inkl. `startDate`. | Wie oben | Start darf nicht vor `startDate` liegen. |
| Festes Startdatum | `start = startDate` (keine Automatik) | `end = addWorkingDays(start, durationDays-1)` | Fortschritt beeinflusst Farbgebung, nicht das Zeitfenster. |

### 3.3 Fortschritt = 100 %

| Starttyp | Startberechnung | Ende | Hinweise |
| --- | --- | --- | --- |
| Kein User-Start | 1. `plannedStart = max(E_dep, E_res)`<br>2. `plannedEnd = addWorkingDays(plannedStart, durationDays-1)`<br>3. `end = min(plannedEnd, today)`<br>4. `start = subtractWorkingDays(end, durationDays-1)` | Ende niemals in der Zukunft; Start bleibt historisch. |
| User-Start (nicht fix) | `plannedStart = max(E_dep, E_res, startDate)` → Schritte wie oben | Ende deckelt auf heute, Start bleibt ≥ `startDate`. |
| Festes Startdatum | `start = startDate` | `end = min(addWorkingDays(start, durationDays-1), today)` | Falls Startdatum in Zukunft + 100 % → Ergebnis auf heute clamped, Task bleibt sichtbar als bereits abgeschlossen. |

## 4. Ressourcenverfügbarkeit

- **Offene & Teilweise Tasks**: Ressourcenblock entsteht ab berechnetem Start bis Ende
  (inkl. Resttage). `resourceAvailability[owner] = next working day nach Ende`.
- **Abgeschlossene Tasks**: Ressourcenblock endet bei `min(today, berechnetem Ende)`.
  Die verfügbare Zeit wird anschließend auf `max(heute, nächster Arbeitstag)` gesetzt, damit
  historische Aufgaben die Zukunftsplanung nicht nach hinten schieben.
- **Feste Startdaten**: blockieren Ressourcen unabhängig vom Fortschritt exakt im
  angegebenen Zeitraum.

Bei Überschneidungen markiert das System beide Tasks als `resource-conflict`, wenn mindestens
ein Task ein festes Startdatum erzwingt.

## 5. Darstellung

- **Abgeschlossene Tasks (100 %)**: Balken bleibt grün und zeigt historische Lage.
- **Teilweise (0 % < p < 100 %)**: Balken zeigt geplanten Rest (aus `remainingDays`), Name/Balken werden rot, wenn
  `start + remainingDays` < heute.
- **Offene Aufgaben (0 %)**: Balken startet ab heute und läuft über `durationDays`.
- **Farbkonflikte**:
  - `overdue`: Start-/Ende-Regeln verletzt (`expectedDate < today`).
  - `resource-conflict`: Überschneidung wegen festem Start (beide Tasks rot).

## 6. Test-Szenarien (manuell)

1. **Offener Task ohne User-Start**: Fortschritt 0 %, Dauer 3, keine Abhängigkeit → Start heute.
2. **Offener Task mit altem User-Start**: Startdatum 01.09., Fortschritt 0 % → Start trotzdem heute.
3. **Teilweise fertig (40 %) ohne User-Start**: Dauer 5 → Start ~3 Arbeitstage vor heute.
4. **Teilweise fertig mit User-Start 01.09.**: Sicherstellen, dass Start nicht vor 01.09. liegt.
5. **Teilweise fertig mit festem Start 15.09.**: Start bleibt 15.09., Ende ergibt Restdauer.
6. **Abgeschlossen (100 %) ohne User-Start**: Dauer 4 → Ende vor oder gleich heute, Start historisch.
7. **Abgeschlossen mit User-Start 01.09.**: Ende max. heute, Start ≥ 01.09.
8. **Abgeschlossen mit festem Start 10.09.**: Balken 10.09.–13.09., Ende gedeckelt auf heute falls Bedarf.
9. **Ressourcenkollision durch festes Startdatum**: Zwei Tasks gleiche Ressource, gleicher fixer Start → beide rot.
10. **Parent/Subtask-Kette**: Erster Subtask abgeschlossen, zweiter offen → prüfen, dass der historische Subtask nicht auf heute springt, während der offene Subtask ab heute startet.

Diese Matrix bildet die Grundlage für die nächste Implementationsrunde sowie für Regressionstests.

