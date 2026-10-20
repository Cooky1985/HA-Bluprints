# Advanced Auto-Updater (Cloud-Backup + Geräte + Neustart)

[![Öffnet deine Home-Assistant-Instanz und zeigt den Blueprint-Import-Dialog mit diesem Blueprint an.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FCooky1985%2FHA-Bluprints%2Fblob%2Fmain%2Fblueprints%2Fadvanced-auto-updater%2Fadvanced_auto_updater.yaml)

Eigene Version (6.0.0-local) des Blueprints „Advanced Auto-Updater“ von Artropos (Basis 5.3.0, [Quelle](https://gist.github.com/Artropos/0b8ada3459b10e4d3bbf259ea71c3ace)). Installiert Updates in sicherer Reihenfolge, übersteht HA-Neustarts, wiederholt fehlgeschlagene Downloads und sendet genau eine Abschlussmeldung.

- **Datei:** `advanced_auto_updater.yaml`

## Änderungen gegenüber Version 5.3.0

- Das Backup vor dem Lauf nutzt die automatischen HA-Backup-Einstellungen (`backup.create_automatic`) und landet damit in den eingestellten Backup-Orten, z. B. OneDrive. Der Lauf wartet, bis der Backup-Manager wieder frei ist, und prüft den Erfolg über den Sensor „letztes erfolgreiches automatisches Backup“. Schlägt das Backup fehl, wird der Lauf optional abgebrochen.
- Neu: Geräte-Updates (Firmware von Zigbee, Matter, Shelly, UniFi usw.) mit Ausschlussliste und eigenem Timeout.
- Neu: optionaler HA-Neustart nach dem ganzen Lauf, aber nur wenn mindestens ein Update installiert wurde.

## Funktionsweise

- **Start:** Täglich zur eingestellten Uhrzeit, aber nur an erlaubten Wochentagen, wenn ausgewählte Updates anstehen und gerade kein Lauf aktiv ist. Zusätzlich beim HA-Start, um einen unterbrochenen Lauf (nach einem Core- oder OS-Update) fortzusetzen.
- **Aufräumen:** Hängengebliebene Zustände der Helfer werden erkannt und zurückgesetzt.
- **Backup (optional):** `backup.create_automatic`, danach Warten auf `sensor.backup_backup_manager_state` = `idle` und Prüfung über `sensor.backup_last_successful_automatic_backup`. Ohne bestätigtes Backup wird optional abgebrochen und eine Meldung gesendet.
- **Updates in fester Reihenfolge:** Add-ons, HACS und Integrationen, Geräte-Firmware, Wiederholungen für fehlgeschlagene Add-ons und HACS, Supervisor, Betriebssystem, Core. Geräte-Updates betreffen nur Update-Entitäten mit Gerät, die Installation unterstützen und nicht ausgeschlossen sind.
- **Core und OS:** Vor der Installation wird die Stufe `os_pending` bzw. `core_pending` gespeichert. Nach dem Neustart setzt der Start-Trigger den Lauf fort, prüft das Ergebnis und trägt es ins Protokoll ein.
- **Abschluss:** Genau eine Meldung über `notify.send_message` und/oder klassische `notify.*`-Dienste mit Protokoll und Release-Links, danach werden die Helfer zurückgesetzt. Optional startet HA neu (nur wenn mindestens ein Update installiert wurde, vorher läuft `homeassistant.check_config`).
- Fehler werden mit dem Logger `homeassistant.components.automation.advanced_auto_updater` ins System-Log geschrieben.

## Eingaben

| Eingabe | Bedeutung | Standard |
|---|---|---|
| `run_time` | Uhrzeit des täglichen Laufs | `03:00:00` |
| `allowed_days` | Wochentage, an denen ein Lauf starten darf | alle Wochentage |
| `create_backup_before_run` | Backup vor dem ersten Update (`backup.create_automatic`, nutzt die HA-Backup-Einstellungen, z. B. OneDrive) | an |
| `abort_on_backup_failure` | Lauf abbrechen, wenn das Backup nicht bestätigt werden konnte | an |
| `backup_timeout` | Maximale Wartezeit auf das Backup in Minuten (inkl. Cloud-Upload) | `45` |
| `notification_service` | Klassische `notify.*`-Dienste, durch Komma getrennt | leer |
| `send_message_targets` | Notify-Entitäten für `notify.send_message` | leer |
| `update_addons` | Add-on-Updates installieren | an |
| `update_hacs` | HACS- und Integrations-Updates installieren | an |
| `update_devices` | Geräte-Firmware installieren (alle weiteren Update-Entitäten mit Gerät, die Installation unterstützen) | an |
| `device_exclude` | Update-Entitäten, die nie automatisch installiert werden (z. B. Zigbee-Koordinator) | leer |
| `device_install_timeout` | Maximale Wartezeit je Geräte-Update in Minuten | `10` |
| `update_os` | OS- und Supervisor-Updates installieren | an |
| `update_core` | Core-Updates installieren | aus |
| `restart_after_run` | HA nach dem Lauf neu starten, aber nur wenn mindestens ein Update installiert wurde | an |
| `max_retries` | Wiederholungsversuche für fehlgeschlagene Add-on- und HACS-Updates | `3` |
| `install_timeout` | Maximale Wartezeit je Add-on-, HACS-, OS- oder Core-Update in Minuten | `30` |
| `log_entity` | `input_text` für das Protokoll des laufenden Updates | – |
| `stage_entity` | `input_text` für die aktuelle Stufe des Laufs | – |
| `run_active_helper` | `input_boolean`, der anzeigt, dass ein Lauf aktiv ist | – |
| `notification_sent_helper` | `input_boolean`, der anzeigt, dass die Abschlussmeldung gesendet wurde | – |

„–“ heißt: keine Vorgabe, die Eingabe muss beim Anlegen der Automation gesetzt werden.

## Voraussetzungen

Add-on-, OS- und Supervisor-Updates gibt es nur bei Installationen mit Supervisor (Home Assistant OS oder Supervised), HACS ist optional. Ist das Backup vor dem Lauf aktiviert (Standard), prüft das Blueprint den Erfolg über die Sensoren `sensor.backup_backup_manager_state` und `sensor.backup_last_successful_automatic_backup` der Backup-Integration von Home Assistant. Außerdem müssen vier Helfer existieren, zum Beispiel über ein Package. Das Protokoll wird vorn gekürzt, wenn es die maximale Länge des `input_text` überschreitet (Home Assistant erlaubt höchstens 255 Zeichen):

```yaml
input_text:
  update_log:
    name: Update-Protokoll
    max: 255
  update_stage:
    name: Update-Stufe

input_boolean:
  update_lauf_aktiv:
    name: Update-Lauf aktiv
  update_meldung_gesendet:
    name: Update-Meldung gesendet
```

## Einbinden

**Per Button:** Oben auf den Button klicken (beim ersten Mal fragt My Home Assistant nach der Adresse deiner Instanz), im Dialog auf *Vorschau* und dann auf *Blueprint importieren* klicken. Gespeichert wird er standardmäßig als `Cooky1985/advanced_auto_updater.yaml`.

**Manuell:** `advanced_auto_updater.yaml` nach `config/blueprints/automation/user/advanced_auto_updater.yaml` auf dem Home-Assistant-Server kopieren.

Danach unter *Einstellungen → Automatisierungen & Szenen → Blueprints* das Blueprint öffnen und *Automatisierung erstellen* wählen.

## Beispiel

Automation, die das Blueprint verwendet (Entitäten sind Platzhalter; bei Import per Button lautet der Pfad `Cooky1985/advanced_auto_updater.yaml`):

```yaml
alias: Auto-Updater
use_blueprint:
  path: user/advanced_auto_updater.yaml
  input:
    run_time: "03:00:00"
    update_core: false
    send_message_targets:
      - notify.mein_handy
    log_entity: input_text.update_log
    stage_entity: input_text.update_stage
    run_active_helper: input_boolean.update_lauf_aktiv
    notification_sent_helper: input_boolean.update_meldung_gesendet
```
