# Windows 11 — Home Assistant Theme

A Home Assistant theme inspired by Microsoft's **Fluent Design** language used in Windows 11.

- Mica / Acrylic translucent surfaces with backdrop blur
- Segoe UI Variable typography (with safe fallbacks on non-Windows clients)
- 8 px card radius, 4 px control radius (Win11 corner system)
- Windows 11 accent color (`#0078D4` light / `#4CC2FF` dark)
- Single theme with `modes:` so HA's **Auto** option follows your OS theme

| Light Mica | Dark Mica |
| :--------: | :-------: |
| `#F3F3F3` background, `#0078D4` accent | `#202020` background, `#4CC2FF` accent |

---

## Installation

### Option 1 — Manual

1. Copy [themes/windows11.yaml](themes/windows11.yaml) into your Home Assistant `config/themes/` folder.
   - Create the `themes/` folder if it does not exist.
2. Add the following to `configuration.yaml` (skip if you already have a `frontend:` block with `themes:`):

   ```yaml
   frontend:
     themes: !include_dir_merge_named themes
   ```

3. Restart Home Assistant (**Developer Tools → YAML → Restart**, or full restart).
4. Open your **Profile** (bottom-left avatar) and pick **Windows 11** as the theme. Set **Theme mode** to **Auto** to follow the OS.

### Option 2 — HACS (custom repository)

1. HACS → ⋮ → **Custom repositories** → add this repo's URL with category **Theme**.
2. Find **Windows 11** in HACS → Themes, install, then restart.
3. Select it in your profile as above.

---

## Recommended: install UIX for true Mica/Acrylic

The theme works on its own, but the real **frosted glass** effect (`backdrop-filter: blur(...)`) is delivered via [UI eXtension (UIX)](https://github.com/Lint-Free-Technology/uix) — the spiritual successor to card-mod. Without it you still get a faithful Win11 palette; cards just won't blur what's behind them.

1. HACS → Integrations → search **UI eXtension** → install, then restart Home Assistant.
2. Settings → Devices & services → **Add integration** → **UI eXtension**.
3. Hard-refresh the browser (Ctrl+F5). Cards should now blur their backdrop.

UIX manages its own frontend resource (`uix.js`) — you do **not** need to add a Lovelace resource manually.

### Already using card-mod?

UIX is a drop-in replacement for card-mod up to v4.2.1, but you should not run both at once. Per the [UIX FAQ](https://uix.lf.technology/faq/):

1. Uninstall **card-mod** (HACS → card-mod → ⋮ → Remove).
2. If you added `card-mod` via `extra_module_url`, remove that entry from `configuration.yaml` and restart HA.
3. Install **UI eXtension** as above.

> **Tip:** The Mica effect looks best when the dashboard background isn't pure flat color. You can set a wallpaper via the [browser_mod](https://github.com/thomasloven/hass-browser_mod) integration or a `picture` view background.

---

## Customizing the accent color

Windows 11 lets users pick a system accent. To match yours, edit [themes/windows11.yaml](themes/windows11.yaml) and change these in **both** the `light:` and `dark:` mode blocks:

```yaml
primary-color: '#0078D4'           # ← your accent (hex)
accent-color:  '#0078D4'           # ← same
rgb-primary-color: '0, 120, 212'   # ← same color in R, G, B
rgb-accent-color:  '0, 120, 212'
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
- Built for Home Assistant ≥ 2024.x.

## License

[MIT](LICENSE)
