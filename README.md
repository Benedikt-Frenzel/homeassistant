# ha-packages

Home Assistant package repo for `/config` checkout.

Expected checkout path via Git Pull add-on:

```text
/config
```

Packages live in:

```text
/config/packages/
```

`configuration.yaml` needs:

```yaml
homeassistant:
  packages: !include_dir_named packages
```

## Packages

- `packages/hitzetag_lueften.yaml`
- `packages/hausklima.yaml`
- `packages/wartung.yaml`
- `packages/buero_benedikt_licht.yaml`
- `packages/argus_monitoring.yaml`
- `packages/ventilatoren.yaml`

## Root config files

Because the Git Pull add-on pulls this repo into `/config`, this repo also contains minimal root files:

- `configuration.yaml`
- `automations.yaml`
- `scripts.yaml`
- `scenes.yaml`

`configuration.yaml` enables `default_config` and package loading.

## Git Pull add-on config

Recommended config:

```yaml
git_branch: main
git_command: pull
git_remote: origin
git_prune: "false"
repository: "ssh://git@codeberg.org/HerrBenedikt/ha-packages.git"
auto_restart: false
restart_ignore:
  - ".gitignore"
  - "README.md"
  - "packages/README.md"
repeat:
  active: false
  interval: 300
deployment_key_protocol: ed25519
```

Use SSH deployment key without passphrase. Put private key into add-on `deployment_key` setting.

## Conflict policy

This repo is source of truth for tracked files:

- `configuration.yaml`
- `automations.yaml`
- `scripts.yaml`
- `scenes.yaml`
- `packages/*.yaml`
- `dashboards/*.yaml`

Avoid editing tracked files directly on Home Assistant. Change them in Git, push, then Git Pull add-on pulls.

Untracked local/runtime files stay local and are ignored:

- `secrets.yaml` (must contain `argus_alertmanager_webhook_id` for the Argus package)
- `.storage/`
- `secrets.yaml`
- `home-assistant*.log`
- `home-assistant_v2.db*`
- backups, add-ons, custom components, `www/`

If merge conflicts happen:

1. Stop Git Pull repeat mode.
2. Backup `/config` or take HA backup.
3. Inspect conflict in add-on log / terminal.
4. If local edits are disposable, run Git Pull with `git_command: reset` once.
5. Switch back to `git_command: pull` after clean state.

Do not leave `git_command: reset` enabled unless Git is strict source of truth and local tracked-file edits may be overwritten.
