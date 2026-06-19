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
- `packages/buero_benedikt_licht.yaml`

## Root config files

Because the Git Pull add-on pulls this repo into `/config`, this repo also contains minimal root files:

- `configuration.yaml`
- `automations.yaml`
- `scripts.yaml`
- `scenes.yaml`

`configuration.yaml` enables `default_config` and package loading.
