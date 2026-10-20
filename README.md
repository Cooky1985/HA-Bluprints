# HA-Bluprints

Meine Home-Assistant-Blueprints. Jeder Blueprint hat einen eigenen Ordner mit `blueprint.yaml` und `README.md`.

## Struktur

```
HA-Bluprints/
├── .github/                  # Optionale GitHub-Konfigurationen (z. B. Workflows)
├── blueprints/               # Hauptordner für alle Blueprints
│   ├── <name>/
│   │   ├── blueprint.yaml    # Die eigentliche Blueprint-Datei
│   │   └── README.md         # Dokumentation zu diesem Blueprint
│   └── ...
└── README.md                 # Diese Datei
```

## Übersicht

| Blueprint | Beschreibung |
|---|---|
| [Licht Zeitschaltung (Zeiten per Helfer)](blueprints/licht-zeitschaltung/) | Schaltet Lichter zu Uhrzeiten ein und aus, die in `input_datetime`-Helfern stehen |
| [Jalousie Zeitschaltung (Zeiten per Helfer)](blueprints/jalousie-zeitschaltung/) | Öffnet und schließt Jalousien zu Uhrzeiten aus `input_datetime`-Helfern |
| [Jalousie Zeitschaltung (ohne Helfer)](blueprints/jalousie-zeitschaltung-ohne-helfer/) | Öffnet und schließt Jalousien zu festen Uhrzeiten, die direkt in der Automation stehen |
| [Advanced Auto-Updater (Cloud-Backup + Geräte + Neustart)](blueprints/advanced-auto-updater-cloud/) | Installiert Updates in sicherer Reihenfolge, mit Backup vor dem Lauf, Geräte-Firmware und optionalem Neustart (Basis: Artropos) |

Zu jedem Blueprint beschreibt die `README.md` Zweck, Funktionsweise, Eingaben und Standardwerte, dazu Voraussetzungen und ein Beispiel.

## Einbinden

1. Die `blueprint.yaml` des gewünschten Blueprints nach `config/blueprints/automation/user/` auf dem Home-Assistant-Server kopieren. Den Dateinamen nimmt man am besten aus der jeweiligen `README.md`, damit bestehende Automationen (`path: user/<dateiname>.yaml`) weiter passen.
2. In Home Assistant unter *Einstellungen → Automatisierungen & Szenen → Blueprints* prüfen, dass das Blueprint erscheint, und daraus eine Automatisierung erstellen.
