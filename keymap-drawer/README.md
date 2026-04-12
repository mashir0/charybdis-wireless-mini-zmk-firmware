# Keymap Drawer (`custom_qwerty`)

Diagrams for [`config/keymaps/custom_qwerty.keymap`](../config/keymaps/custom_qwerty.keymap) live here. Physical layout uses [`config/qwerty.json`](../config/qwerty.json) (`default_transform`).

## Regenerate YAML from the ZMK keymap

```bash
keymap -c keymap-drawer/config.yaml parse -z config/keymaps/custom_qwerty.keymap -o keymap-drawer/custom_qwerty.yaml
```

Then set the first line to use the Charybdis physical layout name (the parser may emit `custom_qwerty`; `draw` needs a known matrix or `-j`):

```yaml
layout: {zmk_keyboard: qwerty}
```

If `draw` cannot resolve `qwerty`, omit the `layout` key in YAML and pass the layout explicitly (recommended for this repo):

## Render SVG

Full diagram:

```bash
keymap -c keymap-drawer/config.yaml draw keymap-drawer/custom_qwerty.yaml \
  -j config/qwerty.json -l default_transform \
  -o keymap-drawer/custom_qwerty.svg
```

Base layer only (thumbnail):

```bash
keymap -c keymap-drawer/config.yaml draw keymap-drawer/custom_qwerty.yaml \
  -j config/qwerty.json -l default_transform -s Base \
  -o keymap-drawer/base/custom_qwerty.svg
```

Requires [keymap-drawer](https://github.com/caksoylar/keymap-drawer) (`pip install keymap-drawer`).
