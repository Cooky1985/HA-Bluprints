# Licht Zeitschaltung (Zeiten per Helfer)

Schaltet ein oder mehrere Lichter zu festen Uhrzeiten ein und aus. Die Zeiten kommen aus zwei `input_datetime`-Helfern (nur Uhrzeit) und lassen sich deshalb bequem im Dashboard ändern. Ein `input_boolean` aktiviert oder deaktiviert die Zeitschaltung.

- **Datei:** `blueprint.yaml` (in Home Assistant als `user/licht_zeitschaltung.yaml`)
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

1. `blueprint.yaml` auf dem Home-Assistant-Server nach `config/blueprints/automation/user/licht_zeitschaltung.yaml` kopieren. Den Dateinamen am besten beibehalten.
2. Unter *Einstellungen → Automatisierungen & Szenen → Blueprints* prüfen, dass das Blueprint erscheint.
3. Dort *Automatisierung erstellen* wählen und die Eingaben ausfüllen.

## Beispiel

Automation, die das Blueprint verwendet (Entitäten sind Platzhalter):

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
