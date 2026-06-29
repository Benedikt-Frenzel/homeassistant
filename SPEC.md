# SPEC.md

§G
- Passive Wartungs-/Gerätegesundheits-Ebene für Home Assistant bauen: weniger Rätsel, keine neuen Notifications.
- Argus/Alertmanager passiv in Home Assistant sichtbar machen: Status erfassen, keine HA-Pushes, keine Gerätewirkung.

§C
- Keine Live-Wirkung ohne explizites Go: kein Reload, Restart, Service Call.
- Erst A: Dashboard/Status only; keine Notifications, keine Automatik.
- Keine Secrets in Repo.
- Sensible Bereiche Kamera/Personen nicht unnötig auswerten.
- Repo-YAML bleibt klein, lesbar, rollbackbar.

§I
- I1 Home Assistant package: `packages/*.yaml` via `!include_dir_named packages`.
- I2 Dashboard YAML: `dashboards/wetter-und-lueften.yaml`.
- I3 Read-only HA API nur zur Bestandsaufnahme.

§V
- V1 Wartung A erzeugt keine `automation:` mit Notify/Service-Calls.
- V2 Template-Sensoren dürfen fehlende/unavailable Entities tolerieren.
- V3 Batterie-Warnung zählt Prozent-Batterien <=20% und `battery_low` binary sensors `on`; Spannungswerte nicht als Prozent behandeln.
- V4 Gerätegesundheit filtert laute Hilfsdomains aus und zeigt nur alltagsrelevante unavailable/unknown Entities.
- V5 Dashboard zeigt Status passiv an und steuert keine Geräte.
- V6 Argus-Webhook darf nur Helper-Entities aktualisieren; keine notify.*, keine Geräte-/Haus-Service-Calls.
- V7 Argus-Webhook-ID bleibt in `secrets.yaml`/pass, nicht im Repo.

§T
| id | status | task | cites |
|---|---|---|---|
| T1 | x | `packages/wartung.yaml` mit passiven Wartungs-Template-Sensoren anlegen | V1,V2,V3,V4,I1 |
| T2 | x | Wartung-View im Wetter-&-Lüften-Dashboard ergänzen | V5,I2 |
| T3 | x | README/Validierung aktualisieren | V1,V5,I1,I2 |
| T4 | x | `packages/argus_monitoring.yaml` mit passivem Alertmanager-Webhook und Helper-Sensoren anlegen | V6,V7,I1 |
| T5 | x | Argus-View im Dashboard ergänzen | V5,V6,I2 |
| T6 | x | README/Secrets-Hinweis für Argus-Webhook ergänzen | V7 |

§B
| id | date | cause | fix |
|---|---|---|---|
