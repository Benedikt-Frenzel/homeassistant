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
