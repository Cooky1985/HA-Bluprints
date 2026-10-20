# Jalousie Zeitschaltung (Zeiten per Helfer)

Öffnet und schließt eine oder mehrere Jalousien (oder Rollläden) zu festen Uhrzeiten. Die Zeiten kommen aus zwei `input_datetime`-Helfern (nur Uhrzeit) und lassen sich deshalb bequem im Dashboard ändern. Ein `input_boolean` aktiviert oder deaktiviert die Zeitschaltung.

- **Datei:** `blueprint.yaml` (in Home Assistant als `user/jalousie_zeitschaltung.yaml`)
- **Mindestversion Home Assistant:** 2024.10.0

## Funktionsweise

- Zur Uhrzeit des Öffnen-Helfers wird `cover.open_cover` ausgeführt, zur Uhrzeit des Schließen-Helfers `cover.close_cover`.
- Die Automation arbeitet nur, solange der Aktivierungsschalter an ist. Ist er aus, passiert zu den Zeiten nichts.
- Modus `single`.

## Eingaben

| Eingabe | Bedeutung | Standard |
|---|---|---|
| `cover_target` | Jalousien oder Bereich, die geöffnet und geschlossen werden | – |
| `enable_helper` | `input_boolean`; die Zeitschaltung läuft nur, wenn er an ist | – |
| `open_time_helper` | `input_datetime` (nur Uhrzeit) mit der Öffnungszeit | – |
| `close_time_helper` | `input_datetime` (nur Uhrzeit) mit der Schließzeit | – |

„–“ heißt: keine Vorgabe, die Eingabe muss beim Anlegen der Automation gesetzt werden.

## Voraussetzungen

Ein Blueprint kann keine Helfer anlegen. Diese drei Helfer müssen vorher existieren, zum Beispiel über ein Package:

```yaml
input_boolean:
  jalousie_zeitschaltung_aktiv:
    name: Jalousie Zeitschaltung aktiv

input_datetime:
  jalousie_oeffnen:
    name: Jalousie öffnen
    has_date: false
    has_time: true
  jalousie_schliessen:
    name: Jalousie schließen
    has_date: false
    has_time: true
```

## Einbinden

1. `blueprint.yaml` auf dem Home-Assistant-Server nach `config/blueprints/automation/user/jalousie_zeitschaltung.yaml` kopieren. Den Dateinamen am besten beibehalten.
2. Unter *Einstellungen → Automatisierungen & Szenen → Blueprints* prüfen, dass das Blueprint erscheint.
3. Dort *Automatisierung erstellen* wählen und die Eingaben ausfüllen.

## Beispiel

Automation, die das Blueprint verwendet (Entitäten sind Platzhalter):

```yaml
alias: Wohnzimmer Jalousie Zeitschaltung
use_blueprint:
  path: user/jalousie_zeitschaltung.yaml
  input:
    cover_target:
      entity_id:
        - cover.wohnzimmer
    enable_helper: input_boolean.jalousie_zeitschaltung_aktiv
    open_time_helper: input_datetime.jalousie_oeffnen
    close_time_helper: input_datetime.jalousie_schliessen
```
