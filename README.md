# Fluenta Tempus

> "First Things First"  
> "So wenig wie möglich, so viel wie nötig"  
> "Siehst du ein Feature, töte es!"

## Was ist Fluenta Tempus?

Ein Gantt-Planer in **einer HTML-Datei**. Keine Dependencies. Kein Build-Prozess. Kein npm.

Geboren aus der Frustration mit Excel-Balken und der Überkomplexi moderner Web-Frameworks.

## Die Philosophie

### Das Manifest (manifest.md)

**Fünf Wahrheiten:**

1. **First Things First** - Position ist Priorität
2. **Position schlägt Priorität** - Task 1 vor Task 2, keine Flags
3. **Menschen haben Namen** - NJ, nicht res_x7y8z9
4. **Weglassen ist ein Feature** - Im Zweifel: löschen
5. **Dein JSON, Deine Daten** - Kein Login, keine Cloud

**Der Test:** Würde deine Mutter es verstehen? Nein? Dann ist es falsch.

## Die Datenstruktur

```json
{
  "version": "1.0.0",
  "name": "Projekt",
  "resources": [
    {
      "firstName": "Norman",
      "lastName": "Janert", 
      "shortCode": "NJ",
      "weeklyHours": 40
    }
  ],
  "tasks": [
    {
      "id": 1,  // Position = Priorität!
      "title": "Wichtigste Aufgabe",
      "owner": "NJ",
      "subTasks": [
        {
          "id": 1,
          "title": "Teil A",
          "effort": 8,
          "assignee": "NJ",
          "dependsOn": [],
          "progress": 0
        }
      ]
    }
  ]
}
```

### Die Evolution der Datenstruktur

1. **Ausgangspunkt**: 200+ Zeilen Schema mit IDs, Timestamps, ISO-Standards
2. **Magnificus-Schnitt v1**: Skill-Faktoren weg, Profile weg, Status weg
3. **Magnificus-Schnitt v2**: IDs weg! Menschen haben Namen!
4. **Technische Einsicht**: IDs als simple Nummern (1,2,3) für Referenzen
5. **Finale Form**: Position = Priorität. Task 1 ist wichtiger als Task 2.

## Die Code-Götter

Vier Wächter über die Einfachheit:

### Shiva - Der Architekt

*"Geht es ohne Framework? Wenn ja: Tu es."*

- Trigger: npm install, package.json
- Mission: Vanilla First

### Apollo - Der Designer  

*"Wo geht der Blick hin?"*

- Regel: Max 3 Farben, 8px Grid
- Mission: Klarheit durch Hierarchie

### Thanos - Der Reducer

*"Lösche 50%"*

- Trigger: Commits > 100 Zeilen
- Mission: Balance durch Deletion

### Themis - Die Testerin

*"Was kostet Geld wenn's bricht?"*

- Tests: Nur kritische Pfade
- Mission: Vertrauen ohne Overhead

## Die Architektur

```
fluenta-tempus/
├── index.html         # DAS IST ALLES
├── example.json       # Die Datenstruktur
├── CLAUDE.md          # Die Verfassung für KI
├── manifest.md        # Die Philosophie
├── README.md          # Diese Datei
├── GENESIS.md         # Die Geschichte
├── .gitignore         # Inverse: Alles verboten außer...
├── .vscode/
│   └── settings.json  # Eine Regel: markdownlint.config
└── .claude/
    ├── agents/        # Die Götter
    └── commands/      # Die Werkzeuge
```

## Die Geschichte

### Der Anfang

"Ich arbeite in Claude Code an einem React basierten Gantt-Planer, da ich es leid bin ständig farbige Balken in Excel zu aktualisieren."

### Die Transformation

1. **React + TypeScript + Vite** → 500MB node_modules
2. **Magnificus erscheint** → "Warum nicht Vanilla?"
3. **Radikale Reduktion** → Eine HTML-Datei
4. **Die Götter erwachen** → Wächter der Einfachheit
5. **Das Manifest** → Die Verfassung

### Die Erleuchtung

- "First Things First" - Die ultimative Reduktion
- Position = Priorität - Keine künstliche Komplexität
- 150 Zeilen Code - Voll funktionsfähig

## Commands in Claude Code

```bash
/start [feature]      # Neues Feature mit Götter-Review
/kill-feature [idea]  # Prüft ob Feature nötig ist
/minimize            # Ruft alle vier Götter
/vanilla [code]      # Framework → HTML/CSS/JS
/30-second-test      # Der Mutter-Test
/status              # Manifest-Compliance Check
```

## Die Prinzipien

1. **Kein NPM** - Niemals. Unter keinen Umständen.
2. **Eine Datei** - index.html ist alles
3. **30 Sekunden** - Vom Öffnen zum ersten Plan
4. **Drag & Drop** - Position ist Priorität
5. **LocalStorage** - Deine Daten, lokal

## Warum?

- **Excel ist der Feind** - Umständlich, unflexibel
- **Jira ist ein Symptom** - Zu komplex für Planung
- **React ist Overkill** - Für 90% der Anwendungen
- **npm ist die Hölle** - 500MB für eine Todo-Liste?

**Fluenta Tempus ist die Lösung.**

## Der Kodex

Jeder Beitrag muss diese Fragen bestehen:

1. **Macht es den Code größer?** → Ablehnen
2. **Braucht es Dependencies?** → Ablehnen
3. **Versteht es meine Mutter?** → Sonst vereinfachen
4. **Läuft es auf einem 2015er Phone?** → Sonst optimieren
5. **Kann man es löschen?** → Dann löschen

## Die Vision

Software die:

- In 1 Sekunde lädt
- In 30 Sekunden verstanden ist
- Überall läuft
- Niemals abstürzt
- Sich selbst erklärt

## Credits

Geboren aus einem Chat zwischen Norman und Claude (Magnificus).

*"So wenig wie möglich, so viel wie nötig."*

---

**Fluenta Tempus** - Fließende Zeit.

Keine Features. Keine Komplexität. Nur Flow.

*Magnifico.*
