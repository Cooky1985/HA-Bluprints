# Abwesenheit - Geräte ausschalten und überwachen

[![Öffnet deine Home-Assistant-Instanz und zeigt den Blueprint-Import-Dialog mit diesem Blueprint an.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2FCooky1985%2FHA-Bluprints%2Fmain%2Fblueprints%2Fabwesenheit-ausschalten%2Fabwesenheit_ausschalten.yaml)

Schaltet gewählte Geräte aus, sobald alle gewählten Personen das Haus verlassen haben, und prüft danach regelmäßig (Standard: alle 5 Minuten), ob sie wirklich aus sind. Was noch an ist, wird erneut ausgeschaltet. Sobald jemand wieder nach Hause kommt, endet die Überwachung.

- **Datei:** `abwesenheit_ausschalten.yaml`
- **Mindestversion Home Assistant:** 2024.10.0

## Funktionsweise

- Ausgelöst wird bei jeder Zustandsänderung einer der gewählten Personen. Weiter geht es nur, wenn keine von ihnen mehr "zuhause" ist.
- Nach der Wartezeit schaltet das Blueprint alle Geräte aus, die nicht `off`, `unavailable` oder `unknown` sind (`homeassistant.turn_off`).
- Danach wartet es das Prüfintervall ab und wiederholt die Prüfung, solange niemand zuhause ist. Geräte, die wieder angegangen sind, werden erneut ausgeschaltet.
- Kommt jemand nach Hause, wird der laufende Durchgang beendet (Modus `restart`). Optional endet die Überwachung nach einer maximalen Anzahl Prüfungen.
- Geräte, die nach dem Ausschalten einen anderen Zustand als `off` melden (z. B. `idle` bei manchen Media Playern), werden bis zum Ende der Überwachung immer wieder ausgeschaltet.

## Eingaben

| Eingabe | Bedeutung | Standard |
|---|---|---|
| `persons` | Personen, die alle weg sein müssen | – |
| `targets` | Entitäten, die ausgeschaltet und überwacht werden | – |
| `leave_delay` | Wartezeit nach dem Verlassen in Minuten | `2` |
| `check_interval` | Prüfintervall in Minuten | `5` |
| `max_checks` | Maximale Prüfungen, 0 = bis jemand nach Hause kommt | `0` |

„–“ heißt: keine Vorgabe, die Eingabe muss gesetzt werden.

## Voraussetzungen

Für jede Person eine `person`-Entität mit Standorterkennung (z. B. Companion-App).

## Einbinden

**Per Button:** Oben auf den Button klicken (beim ersten Mal fragt My Home Assistant nach der Adresse deiner Instanz), im Dialog auf *Vorschau* und dann auf *Blueprint importieren* klicken. Gespeichert wird er standardmäßig als `Cooky1985/abwesenheit_ausschalten`.

**Manuell:** `abwesenheit_ausschalten.yaml` nach `config/blueprints/automation/user/abwesenheit_ausschalten.yaml` auf dem Home-Assistant-Server kopieren.

Danach unter *Einstellungen → Automatisierungen & Szenen → Blueprints* das Blueprint öffnen und *Automatisierung erstellen* wählen.

## Beispiel

```yaml
alias: Haus verlassen - alles aus
use_blueprint:
  path: Cooky1985/abwesenheit_ausschalten.yaml
  input:
    persons:
      - person.anna
      - person.ben
    targets:
      - light.wohnzimmer
      - switch.steckdose_tv
    check_interval: 5
```
