# Home Assistant Packages

## Aktivierung in `configuration.yaml`

Falls Packages noch nicht aktiv sind:

```yaml
homeassistant:
  packages: !include_dir_named packages
```

Dann Datei nach Home Assistant kopieren:

```text
/config/packages/hitzetag_lueften.yaml
```

Danach in Home Assistant:

1. Einstellungen → System → Reparaturen / YAML prüfen
2. Konfiguration prüfen
3. Home Assistant neu starten oder relevante YAML neu laden

## Enthalten

- `hitzetag_lueften.yaml`
  - Helper `input_boolean.hitzetag_erwartet`
  - konfigurierbare Schwellen als `input_number`
  - Forecast-Helper
  - Morgen-Lüften Notification
  - Fenster-Schließen Notification
  - Abend-OG-Lüften Notification
- `klingel_lichtsignal.yaml`
  - lässt bei einem Klingeln an Haustür oder Tor die Esszimmerleuchte und das Leselicht im Wohnzimmer dreimal blinken
  - sichert den vorherigen Lichtzustand und stellt ihn anschließend wieder her
- `luftentfeuchter.yaml`
  - Tuya-Luftentfeuchter Tank-voll-Erkennung über Leistungsabfall
  - berücksichtigt Keller-Luftfeuchte, damit Zielwert/Standby keine falsche Warnung auslöst
  - konfigurierbare Watt- und Feuchte-Schwellen als `input_number`
  - Push-Notification, keine Geräte-Steuerung
- `hausklima.yaml`
  - Zonen-Sensoren für EG, OG und Keller
  - Temperatur-/Feuchte-Durchschnittswerte
  - Außen-Temperatur-Deltas fürs Lüften
- `wartung.yaml`
  - passive Batterie-Warnungen
  - passive Offline/unknown-Geräteübersicht
  - passive Luftqualitäts- und Feuchtebewertung
  - keine Notifications, keine Geräte-Automatik
- `xiaomi_fans.yaml`
  - Custom Component `syssi/xiaomi_fan` (https://github.com/syssi/xiaomi_fan)
  - Xiaomi Smart Tower Fan 2 (xiaomi.fan.p45) im Schlafzimmer
  - 2x Xiaomi Smart Desktop Air Circulation Fan (xiaomi.fan.p70), Office + Küche
  - Tokens via `!secret` aus `secrets.yaml`, Quelle: `pass system/infra/xiaomi-tokens`
  - passive Entities, keine Automatik, keine Notifications
  - Voraussetzung: Custom Component unter `/config/custom_components/xiaomi_miio_fan/` installiert
- `ventilatoren.yaml`
  - Helper für Ventilatoren-Empfehlungen im Dashboard `Wetter & Lüften`
  - Schwellen für OG/EG-Temperatur, Innen/Außen-Delta und maximale Außen-Lüftungstemperatur
  - reiner Empfehlungs-/Dashboard-Support, keine Automation und keine Gerätesteuerung
