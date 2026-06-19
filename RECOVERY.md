# Home Assistant Recovery

Goal: if Home Assistant breaks, restore config, packages, integrations and entities from documented sources. Worst case should be recreating non-primary users and rotating tokens.

## Current source of truth

- Git repo: root HA config and packages
- Password store: external service credentials (`pass system/infra/...`)
- HA backup tar: full `/config` rescue snapshots, especially `.storage/`
- SMB backup share: `//10.12.40.2/homeassistant-backups`

## Google Assistant

Manual Google Assistant project:

```yaml
google_assistant:
  project_id: smarthome-test-84780
  service_account: !include smarthome-test-84780-0865b5d2c6af.json
  report_state: true
```

Service account JSON must exist in `/config` and must not be committed.

After config pull/restart:

1. Verify `https://ha.bfr.sh/api/google_assistant` returns `405 Method Not Allowed`.
2. Run `google_assistant.request_sync`.
3. In Google Home, say "sync my devices" if needed.

## Home Assistant URLs and trusted proxies

Canonical external URL:

```yaml
homeassistant:
  external_url: https://ha.bfr.sh
  internal_url: http://10.12.99.41:8123
```

`configuration.yaml` must include:

```yaml
http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 10.12.40.30   # internal reverse proxy
    - 10.255.130.2  # Colossus site-to-site WireGuard peer
```

Source: `~/Projects/own/infra/inventory/group_vars/opnsense.yml`.

## Critical files

These files must exist in `/config`:

```text
configuration.yaml
automations.yaml
scripts.yaml
scenes.yaml
packages/*.yaml
.storage/
```

`.storage/` contains users/auth, integrations, device registry, entity registry, area registry and dashboards. Do not overwrite it from a minimal Git repo.

## Git Pull rule

Do not use Git Pull add-on as blind `/config` reset source.

Safe defaults:

```yaml
git_branch: main
git_command: pull
git_remote: origin
git_prune: "false"
auto_restart: false
repeat:
  active: false
```

Avoid `git_command: reset` unless a fresh backup exists and local tracked-file edits may be discarded.

## After any config change

1. Check HA config.
2. Restart only after config is valid.
3. Create/download rescue archive:

```bash
cd /config
tar -czf /config/rescue-$(date +%F-%H%M).tgz \
  .storage configuration.yaml packages automations.yaml scripts.yaml scenes.yaml
```

4. Copy rescue archive to SMB backup share:

```text
Server: 10.12.40.2
Share: homeassistant-backups
Username: link
Password: pass system/gibson/smb-link
Workgroup: WORKGROUP
```

Incident backup path used:

```text
//10.12.40.2/homeassistant-backups/incident-2026-06-19/
```

## Recovery sequence

1. Stop Git Pull add-on.
2. Restore latest `rescue-*.tgz` or HA backup.
3. Verify API works.
4. Verify critical integrations:
   - Matter
   - OpenThread Border Router
   - MQTT
   - Ecowitt
   - Reolink
   - Mobile App
5. Recreate missing users only after config/integrations are stable.
6. Rotate exposed tokens/passwords.

## MQTT restore

External broker:

- Host: `10.12.99.24`
- Port: `1883`
- User: `homeassistant`
- Password: `pass system/infra/mqtt/homeassistant`

MQTT integration can be recreated via HA UI or config flow. Do not store password in Git.

## Token/password rotation required

Rotate after this incident:

- HA long-lived access token used during recovery
- Reolink password exposed in terminal/chat

## Known good package entities

- `input_boolean.hitzetag_erwartet`
- `light.licht_buro_benedikt_decke`
- `light.buero_benedikt_decke_02`
- `light.buero_benedikt_decke_03`
- `light.buero_benedikt_decke_04`
- `light.buero_benedikt_decke_05`
