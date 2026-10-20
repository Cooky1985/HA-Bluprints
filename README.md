# HA-Bluprints

Meine Home-Assistant-Blueprints. Jeder Blueprint hat einen eigenen Ordner mit der Blueprint-YAML-Datei und `README.md`.

## Struktur

```
HA-Bluprints/
├── .github/                  # Optionale GitHub-Konfigurationen (z. B. Workflows)
├── blueprints/               # Hauptordner für alle Blueprints
│   ├── <name>/
│   │   ├── <name>.yaml       # Die eigentliche Blueprint-Datei
│   │   └── README.md         # Dokumentation zu diesem Blueprint
│   └── ...
└── README.md                 # Diese Datei
```

## Übersicht

| Blueprint | Beschreibung | Import |
|---|---|---|
| [Licht Zeitschaltung (Zeiten per Helfer)](blueprints/schreibtisch-licht-zeitschaltung/) | Schaltet Lichter zu Uhrzeiten ein und aus, die in `input_datetime`-Helfern stehen | [![Öffnet deine Home-Assistant-Instanz und zeigt den Blueprint-Import-Dialog mit diesem Blueprint an.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FCooky1985%2FHA-Bluprints%2Fblob%2Fmain%2Fblueprints%2Fschreibtisch-licht-zeitschaltung%2Fschreibtisch_licht_zeitschaltung.yaml) |
| [Jalousie Zeitschaltung (Zeiten per Helfer)](blueprints/jalousie-zeitschaltung/) | Öffnet und schließt Jalousien zu Uhrzeiten aus `input_datetime`-Helfern | [![Öffnet deine Home-Assistant-Instanz und zeigt den Blueprint-Import-Dialog mit diesem Blueprint an.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FCooky1985%2FHA-Bluprints%2Fblob%2Fmain%2Fblueprints%2Fjalousie-zeitschaltung%2Fjalousie_zeitschaltung.yaml) |
| [Jalousie Zeitschaltung (ohne Helfer)](blueprints/jalousie-zeitschaltung-ohne-helfer/) | Öffnet und schließt Jalousien zu festen Uhrzeiten, die direkt in der Automation stehen | [![Öffnet deine Home-Assistant-Instanz und zeigt den Blueprint-Import-Dialog mit diesem Blueprint an.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FCooky1985%2FHA-Bluprints%2Fblob%2Fmain%2Fblueprints%2Fjalousie-zeitschaltung-ohne-helfer%2Fjalousie_zeitschaltung_ohne_helfer.yaml) |
| [Advanced Auto-Updater (Cloud-Backup + Geräte + Neustart)](blueprints/advanced-auto-updater/) | Installiert Updates in sicherer Reihenfolge, mit Backup vor dem Lauf, Geräte-Firmware und optionalem Neustart (Basis: Artropos) | [![Öffnet deine Home-Assistant-Instanz und zeigt den Blueprint-Import-Dialog mit diesem Blueprint an.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FCooky1985%2FHA-Bluprints%2Fblob%2Fmain%2Fblueprints%2Fadvanced-auto-updater%2Fadvanced_auto_updater.yaml) |

Zu jedem Blueprint beschreibt die `README.md` Zweck, Funktionsweise, Eingaben und Standardwerte, dazu Voraussetzungen und ein Beispiel.

## Einbinden

1. Am einfachsten über den Import-Button in der Übersicht oder in der README des Blueprints (öffnet den Import-Dialog in deinem Home Assistant). Alternativ die YAML-Datei des Blueprints nach `config/blueprints/automation/user/` kopieren.
2. In Home Assistant unter *Einstellungen → Automatisierungen & Szenen → Blueprints* prüfen, dass das Blueprint erscheint, und daraus eine Automatisierung erstellen.
