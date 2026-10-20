# Licht Zeitschaltung (Zeiten per Helfer)

[![Öffnet deine Home-Assistant-Instanz und zeigt den Blueprint-Import-Dialog mit diesem Blueprint an.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FCooky1985%2FHA-Bluprints%2Fblob%2Fmain%2Fblueprints%2Flicht-zeitschaltung%2Flicht_zeitschaltung.yaml)

Schaltet ein oder mehrere Lichter zu festen Uhrzeiten ein und aus. Die Zeiten kommen aus zwei `input_datetime`-Helfern (nur Uhrzeit) und lassen sich deshalb bequem im Dashboard ändern. Ein `input_boolean` aktiviert oder deaktiviert die Zeitschaltung.

- **Datei:** `licht_zeitschaltung.yaml`
- **Mindestversion Home Assistant:** 2024.10.0

## Funktionsweise

- Zur Uhrzeit des Einschalt-Helfers wird `light.turn_on` für die gewählten Lichter ausgeführt, zur Uhrzeit des Ausschalt-Helfers `light.turn_off`.
- Die Automation arbeitet nur, solange der Aktivierungsschalter an ist. Ist er aus, passiert zu den Zeiten nichts.
- Modus `single`.

## Eingaben

| Eingabe | Bedeutung | Standard |
|---|---|---|
| `light_target` | Lichter oder Bereich, die geschaltet werden | – |
| `enable_helper` | `input_boolean`; die Zeitschaltung läuft nur, wenn er an ist | – |
| `on_time_helper` | `input_datetime` (nur Uhrzeit) mit der Einschaltzeit | – |
| `off_time_helper` | `input_datetime` (nur Uhrzeit) mit der Ausschaltzeit | – |

„–“ heißt: keine Vorgabe, die Eingabe muss beim Anlegen der Automation gesetzt werden.

## Voraussetzungen

Ein Blueprint kann keine Helfer anlegen. Diese drei Helfer müssen vorher existieren, zum Beispiel über ein Package:

```yaml
input_boolean:
  licht_zeitschaltung_aktiv:
    name: Licht Zeitschaltung aktiv

input_datetime:
  licht_an:
    name: Licht an
    has_date: false
    has_time: true
  licht_aus:
    name: Licht aus
    has_date: false
    has_time: true
```

## Einbinden

**Per Button:** Oben auf den Button klicken (beim ersten Mal fragt My Home Assistant nach der Adresse deiner Instanz), im Dialog auf *Vorschau* und dann auf *Blueprint importieren* klicken. Gespeichert wird er standardmäßig als `Cooky1985/licht_zeitschaltung.yaml`.

**Manuell:** `licht_zeitschaltung.yaml` nach `config/blueprints/automation/user/licht_zeitschaltung.yaml` auf dem Home-Assistant-Server kopieren.

Danach unter *Einstellungen → Automatisierungen & Szenen → Blueprints* das Blueprint öffnen und *Automatisierung erstellen* wählen.

## Beispiel

Automation, die das Blueprint verwendet (Entitäten sind Platzhalter; bei Import per Button lautet der Pfad `Cooky1985/licht_zeitschaltung.yaml`):

```yaml
alias: Wohnzimmer Licht Zeitschaltung
use_blueprint:
  path: user/licht_zeitschaltung.yaml
  input:
    light_target:
      entity_id:
        - light.wohnzimmer
    enable_helper: input_boolean.licht_zeitschaltung_aktiv
    on_time_helper: input_datetime.licht_an
    off_time_helper: input_datetime.licht_aus
```
