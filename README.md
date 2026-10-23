# tinted-adwaita-for-steam

A [tinty](https://github.com/tinted-theming/tinty) template that maps a Base16, Base24, or Tinted8 color scheme to [Adwaita-for-Steam](https://github.com/tkashkin/Adwaita-for-Steam)'s `custom.css` overrides.

## Supports base16, base24, and tinted8 color schemes.

## How it works

Adwaita-for-Steam exposes libadwaita's color variables as CSS custom properties (`--adw-*`) and loads a user stylesheet after the selected color theme via the installer's `--custom-css` option. This template generates that stylesheet, overriding the window, headerbar, sidebar, card, dialog, popover, presence, and accent-palette colors with values from the active scheme.

The generated CSS also sets `--adw-color-scheme` from the scheme's variant, so light Base16/Base24 schemes switch Steam to light mode and dark schemes stay dark.

## Color mapping

| Adwaita variable | Base16/Base24 | Tinted8 | Role |
|---|---|---|---|
| `--adw-accent-bg-*` | `base0D` | `ui.accent.normal` | Primary accent |
| `--adw-accent-fg` | `base00` | `ui.global.background.normal` | Text on accent |
| `--adw-destructive-bg-*` | `base08` | `palette.red.normal` | Destructive action |
| `--adw-error-bg-*` | `base08` | `palette.red.normal` | Error |
| `--adw-success-bg-*` | `base0B` | `palette.green.normal` | Success |
| `--adw-warning-bg-*` | `base0A` | `palette.yellow.normal` | Warning |
| `--adw-window-bg` / `--adw-view-bg` | `base00` | `ui.global.background.normal` | Main background |
| `--adw-window-fg` / `--adw-view-fg` | `base05` | `ui.global.foreground.normal` | Main text |
| `--adw-headerbar-bg` / `--adw-sidebar-bg` | `base01` | `ui.chrome.background.dark` | Bars and sidebar |
| `--adw-secondary-sidebar-bg` / `--adw-card-bg` / dialogs / popovers | `base02` | `ui.chrome.background.normal` | Elevated surfaces |
| Shades and borders | `base03` | `ui.border.normal` | Separators and shadows |
| `--adw-banner` / `--adw-user-offline` | `base03` | `palette.gray.normal` | Muted UI |
| `--adw-user-online` | `base0D` | `ui.accent.normal` | Online presence |
| `--adw-user-ingame` | `base0B` | `palette.green.normal` | In-game presence |
| `--adw-accent-*` palette | `base0D`, `base0C`, `base0B`, `base0A`, `base09`, `base08`, `base0E`, `base03` | `palette.{blue,cyan,green,yellow,orange,red,magenta,gray}` | Named accents |

Base24 schemes reuse the Base16 mapping (they are a superset of `base00`-`base0F`). Tinted8 schemes use the `ui`/`palette` structure emitted by tinted-builder.

## Setup

### 1. Install Adwaita-for-Steam

Follow the [upstream installer](https://github.com/tkashkin/Adwaita-for-Steam#installer-script) or use AdwSteamGtk.

The generated file is passed to the installer with `--custom-css`:

```sh
# Native Steam
./install.py -c adwaita --custom-css ~/.config/AdwSteamGtk/custom.css

# Flatpak Steam
./install.py -t flatpak -c adwaita --custom-css ~/.config/AdwSteamGtk/custom.css
```

Any `-c` color theme works; the generated custom CSS overrides the colors it defines.

### 2. Add to your tinty `config.toml`

```toml
[[items]]
path = "https://github.com/GooseRooster/tinted-adwaita-for-steam"
name = "tinted-adwaita-for-steam"
themes-dir = "output"
supported-systems = ["base16", "base24", "tinted8"]
hook = "mkdir -p \"$HOME/.config/AdwSteamGtk\" && cp -f \"$TINTY_THEME_FILE_PATH\" \"$HOME/.config/AdwSteamGtk/custom.css\""
```

### 3. Apply a scheme

```sh
tinty apply base16-dracula
```

Then re-run the Adwaita-for-Steam installer so the new `custom.css` is copied into Steam's skin:

```sh
./install.py -c adwaita --custom-css ~/.config/AdwSteamGtk/custom.css
# or, for Flatpak:
./install.py -t flatpak -c adwaita --custom-css ~/.config/AdwSteamGtk/custom.css
```

In the silver-hour Home Manager setup, `modules/steam-adwaita.nix` watches `~/.config/AdwSteamGtk/custom.css` and re-runs the installer automatically, and `modules/theming-tools-silver-hour.nix` adds the tinty hook above.

## Notes

- Steam client updates can reset the patched `steamui` CSS; re-run the installer (or restart the `steam-adwaita` user service) after an update.
- Light Base16/Base24 schemes are supported through the scheme variant; Base16 schemes without an explicit variant are treated as light by the template helper.
- Keep `output/` committed: tinty's `themes-dir` reads the generated files directly from the repository.

## Team

This theme is maintained by the following person(s):

| [![GooseRooster](https://github.com/GooseRooster.png?size=100)](https://github.com/GooseRooster) |
| --- |
| [GooseRooster](https://github.com/GooseRooster) |

## Community

- [Tinted Theming Home](https://github.com/tinted-theming/home)
- [#tinted-theming:matrix.org](https://matrix.to/#/#tinted-theming:matrix.org) on [Matrix](https://matrix.org/)

## License

[MIT License](./LICENSE)
