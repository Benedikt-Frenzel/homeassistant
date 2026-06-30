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
- `luftentfeuchter.yaml`
  - Tuya-Luftentfeuchter Tank-voll-Erkennung über Leistungsabfall
  - konfigurierbare Watt-Schwellen als `input_number`
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
