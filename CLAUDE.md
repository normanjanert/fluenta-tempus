# CLAUDE.md - Fluenta Tempus

## Das Manifest

**First Things First**  
Was oben steht, passiert zuerst.

**Weniger ist mehr**  
Siehst du ein Feature, töte es.

**Deine Daten, dein JSON**  
Kein Login. Keine Cloud. Keine Datenbank.

## Die Architektur-Regel

**KEIN NPM. KEIN BUILD. KEINE DEPENDENCIES.**

Dieses Projekt läuft in einer einzigen index.html.
Wenn du `npm install` tippst, hast du versagt.
Wenn du ein Framework vorschlägst, denk nochmal nach.

## Die vier Götter

Diese Agenten wachen über den Code:

- **shiva**: Zerstört Frameworks, baut mit Vanilla
- **apollo**: Macht sichtbar was wichtig ist  
- **thanos**: Löscht 50% des Codes
- **themis**: Testet nur was Geld kostet

Workflow: shiva → apollo → code → thanos → themis

## Projekt-Struktur

```text
fluenta-tempus/
├── index.html    # DAS IST ALLES
├── CLAUDE.md     # Diese Datei
├── manifest.md   # Die Philosophie
└── .claude/
    └── agents/   # Die vier Götter
```

## Technische Regeln

- **HTML/CSS/JavaScript only** - Keine Transpilation
- **Max 500 Zeilen Code** - Mehr ist zu viel
- **Inline alles** - Eine Datei, keine Imports
- **8px Grid** - Für alle Abstände
- **3 Farben max** - Schwarz, Grau, ein Akzent

## Datenmodell

```text
// Tasks haben IDs: 1, 2, 3 (Position = Priorität!)
// Menschen haben Namen: "NJ", nicht "res_xyz"
// 30 Sekunden vom Start zum ersten Plan
```

## Verbotene Worte

Diese Worte darfst du NIE schreiben:

- "npm", "yarn", "pnpm"
- "React", "Vue", "Angular"
- "TypeScript", "Webpack", "Vite"
- "install", "dependency", "package"

## Die eine Wahrheit

Wenn es nicht auf einem Telefon von 2015 läuft, ist es zu kompliziert.

## Bei jedem Feature frage

1. Geht es ohne? → Ja → Lass es weg
2. Geht es einfacher? → Ja → Mach es einfacher
3. Versteht es meine Mutter? → Nein → Lösch es

---

**REMEMBER**: You are building software that runs in a single HTML file.
Every line of code must justify its existence.
When in doubt, delete.

"So wenig wie möglich, so viel wie nötig."
