# Jalousie Zeitschaltung (ohne Helfer)

Öffnet und schließt eine oder mehrere Jalousien (oder Rollläden) zu zwei festen Uhrzeiten. Dieses Blueprint braucht keine Helfer: Die Zeiten werden direkt in der Automation eingestellt, zum Ein- und Ausschalten dient der normale Schalter der Automation.

Sollen die Zeiten im Dashboard einstellbar sein, ist stattdessen [`jalousie-zeitschaltung`](../jalousie-zeitschaltung/) die richtige Wahl.

- **Datei:** `blueprint.yaml` (in Home Assistant als `user/jalousie_zeitschaltung_ohne_helfer.yaml`)
- **Mindestversion Home Assistant:** 2024.10.0

## Funktionsweise

- Zur Öffnungszeit wird `cover.open_cover` ausgeführt, zur Schließzeit `cover.close_cover`.
- Es gibt keine Bedingung. Ein- und ausgeschaltet wird die Automation über ihren normalen Schalter.
- Modus `single`.

## Eingaben

| Eingabe | Bedeutung | Standard |
|---|---|---|
| `cover_target` | Jalousien oder Bereich, die geöffnet und geschlossen werden | – |
| `open_time` | Uhrzeit zum Öffnen | `07:00:00` |
| `close_time` | Uhrzeit zum Schließen | `22:00:00` |

„–“ heißt: keine Vorgabe, die Eingabe muss beim Anlegen der Automation gesetzt werden.

## Voraussetzungen

Keine. Das Blueprint braucht keine Helfer.

## Einbinden

1. `blueprint.yaml` auf dem Home-Assistant-Server nach `config/blueprints/automation/user/jalousie_zeitschaltung_ohne_helfer.yaml` kopieren. Den Dateinamen am besten beibehalten.
2. Unter *Einstellungen → Automatisierungen & Szenen → Blueprints* prüfen, dass das Blueprint erscheint.
3. Dort *Automatisierung erstellen* wählen und die Eingaben ausfüllen.

## Beispiel

Automation, die das Blueprint verwendet (Entitäten sind Platzhalter):

```yaml
alias: Wohnzimmer Jalousie
use_blueprint:
  path: user/jalousie_zeitschaltung_ohne_helfer.yaml
  input:
    cover_target:
      entity_id:
        - cover.wohnzimmer
    open_time: "07:00:00"
    close_time: "22:00:00"
```
