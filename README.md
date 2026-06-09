# Windows 11 — Home Assistant Theme

A Home Assistant theme inspired by Microsoft's **Fluent Design** language used in Windows 11. **No add-ons required** — works on stock Home Assistant 2026.x with native `backdrop-filter` blur on cards.

- Mica / Acrylic translucent surfaces with **real** backdrop blur on cards (via HA's built-in `--ha-card-backdrop-filter`)
- Segoe UI Variable typography (with safe fallbacks on non-Windows clients)
- 8 px card radius, 4 px control radius (Win11 corner system)
- Windows 11 accent color (`#0078D4` light / `#4CC2FF` dark)
- Single theme with `modes:` so HA's **Auto** option follows your OS theme
- Uses both the new HA 2026.x `--ha-color-*` token system and all legacy `--paper-*` / `--mdc-*` / `--primary-*` aliases — works across HA versions

| Light Mica | Dark Mica |
| :--------: | :-------: |
| `#F3F3F3` background, `#0078D4` accent | `#202020` background, `#4CC2FF` accent |

---

## Installation

### Option 1 — HACS (recommended)

1. HACS → ⋮ → **Custom repositories** → add this repo's URL with category **Theme**.
2. Find **Windows 11** in HACS → Themes, install, then restart Home Assistant.
3. Open your **Profile** (bottom-left avatar) and pick **Windows 11** as the theme. Set **Theme mode** to **Auto** to follow the OS.

### Option 2 — Manual

1. Copy [themes/windows11.yaml](themes/windows11.yaml) into your Home Assistant `config/themes/` folder.
   - Create the `themes/` folder if it does not exist.
2. Add the following to `configuration.yaml` (skip if you already have a `frontend:` block with `themes:`):

   ```yaml
   frontend:
     themes: !include_dir_merge_named themes
   ```

3. Restart Home Assistant (**Developer Tools → YAML → Restart**, or full restart).
4. Select **Windows 11** in your Profile as above.

> **Tip:** The Mica blur effect on cards looks best when the dashboard background isn't pure flat color. Set a wallpaper via a `picture` view background or the [browser_mod](https://github.com/thomasloven/hass-browser_mod) integration for the most dramatic frosted-glass look.

---

## Known limitations

### Menu and dialog blur

Cards get real `backdrop-filter` blur because HA exposes `--ha-card-backdrop-filter` as a theme variable. **Dropdown menus** (the ⋮ row-action menus) and **dialogs** (more-info, settings popups) do not have equivalent variables in HA 2026.x — and Web Awesome's `wa-popup` positioning (static layout + floating-ui transforms) breaks `backdrop-filter` even when applied directly via injected CSS.

These surfaces still receive a Mica-style translucent fill via `--ha-dialog-surface-background` and `--wa-color-surface-raised`, so they look reasonably Win11-flavored — they just don't blur the content behind them. This requires changes in the HA frontend or Web Awesome upstream; a theme alone cannot fix it.

---

## Customizing the accent color

Windows 11 lets users pick a system accent. To match yours, edit [themes/windows11.yaml](themes/windows11.yaml) and change these in **both** the `light:` and `dark:` mode blocks:

```yaml
ha-color-primary-40: '#0078D4'     # ← your accent (hex) — light mode default
ha-color-primary-60: '#4CC2FF'     # ← your accent (lighter) — dark mode default
primary-color: '#0078D4'           # ← legacy alias, same as ha-color-primary-40/60 per mode
accent-color:  '#0078D4'           # ← same as primary-color
rgb-primary-color: '0, 120, 212'   # ← same color in R, G, B
rgb-accent-color: '0, 120, 212'
```

Common Windows 11 accents:

| Name           | Hex (Light) | Hex (Dark) |
| -------------- | ----------- | ---------- |
| Default Blue   | `#0078D4`   | `#4CC2FF`  |
| Yellow Gold    | `#FFB900`   | `#FFC83D`  |
| Orange         | `#CA5010`   | `#F7630C`  |
| Red            | `#E81123`   | `#FF99A4`  |
| Pink           | `#E3008C`   | `#FF8FB9`  |
| Purple         | `#5C2E91`   | `#B4A0FF`  |
| Green          | `#107C10`   | `#6CCB5F`  |
| Teal           | `#038387`   | `#3AA0A4`  |

---

## File structure

```
ha-windows11-theme/
├── themes/
│   └── windows11.yaml      ← the theme
├── README.md
├── LICENSE
└── .gitignore
```

---

## Credits

- Color tokens & opacities derived from Microsoft's public **Fluent 2 / WinUI 3** design specifications.
- Built for Home Assistant 2026.x (also compatible with earlier versions via legacy token aliases).

## License

[MIT](LICENSE)
