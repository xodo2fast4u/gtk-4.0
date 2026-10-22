<h1 align="center">gtk-4.0 Config</h1>

<p align="center">
  <a href="https://github.com/xodobyte/gtk-4.0-config">
    <img src="https://img.shields.io/badge/GitHub-gtk--4.0--config-181717?logo=github&style=for-the-badge" alt="GitHub" />
  </a>
  <a href="https://github.com/xodobyte/gtk-4.0-config/blob/main/LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-2E8B57?style=for-the-badge" alt="License: MIT" />
  </a>
  <a href="https://www.gtk.org/">
    <img src="https://img.shields.io/badge/GTK-4.0-4A90D9?logo=gnome&style=for-the-badge" alt="GTK 4.0" />
  </a>
  <a href="https://github.com/GIScience/badges#active">
    <img src="https://img.shields.io/badge/status-active-2ECC71?style=for-the-badge" alt="Status: active" />
  </a>
</p>

A personal GTK 4.0 configuration using the [Nordic](https://github.com/EliverLara/Nordic) dark theme with [Papirus-Dark](https://github.com/PapirusDevelopmentTeam/papirus-icon-theme) icons. It includes a custom light-mode CSS override, symlinks to the system theme's CSS and assets, and a `settings.ini` that wires everything into GTK 4 applications automatically.

## Disclaimer

**IMPORTANT**: This configuration is provided for personal use and educational purposes only.
Please ensure you:

- **Use responsibly**: Only apply to your own system and user account
- **Backup first**: Back up your existing `~/.config/gtk-4.0/` directory before replacing any files
- **No warranty**: The configuration is provided as-is; the author is not responsible for visual regressions or broken themes caused by upstream GTK or theme updates

> [!CAUTION]
> Overwriting your existing GTK configuration without a backup can break the appearance of your GTK applications. Always back up first.

## Why this config?

Instead of hunting down and re-applying the same theme settings after every reinstall, this repo keeps the full GTK 4 setup version-controlled in one place:

- Applies the Nordic dark theme globally via `settings.ini`
- Uses Papirus-Dark for icons and Adwaita Sans for the UI font
- Ships a handcrafted `gtk.light.css` for a clean black-on-white light mode
- Symlinks the system theme's `gtk.css`, `gtk-dark.css` and `assets/` so overrides stay minimal and the theme package stays the source of truth

## Features

- **Dark mode by default**: `gtk-application-prefer-dark-theme=1` set out of the box
- **Nordic theme**: Applies the Nordic dark theme system-wide through `settings.ini`
- **Papirus-Dark icons**: Consistent dark icon set across all GTK 4 apps
- **Light variant included**: `gtk.light.css` with custom `@define-color` tokens for a black-on-white light mode
- **Symlinked theme files**: `gtk.css`, `gtk-dark.css` and `assets/` point to the installed system theme — no duplication
- **Minimal footprint**: No build step, no scripts — plain CSS, INI and symlinks
- **Version-controlled**: Track every tweak with standard Git tooling

## Prerequisites

You need the following installed on your Linux system:

- **GTK 4** — provided by your desktop environment (GNOME 42+, or any GTK 4-capable DE)
- **Nordic GTK theme** — install it so `settings.ini` can reference it by name
- **Papirus-Dark icon theme** — used by `gtk-icon-theme-name`
- **Git** — to clone and track the repository

**Verify your themes are installed:**

```bash
ls /usr/share/themes/ | grep -i nordic
ls /usr/share/icons/ | grep -i papirus
```

Install Nordic and Papirus-Dark for your distribution if they are missing. Most distributions have them in their official repositories or AUR.

**Verify GTK 4 is present:**

```bash
gtk4-demo --version
```

## Quick Start

### 1. Back up your existing configuration

```bash
cp -r ~/.config/gtk-4.0 ~/.config/gtk-4.0.bak
```

### 2. Clone the repository

```bash
git clone https://github.com/xodobyte/gtk-4.0-config.git
cd gtk-4.0-config
```

### 3. Install required themes

Install the Nordic theme and Papirus-Dark icons before copying files, so the symlinks and `settings.ini` resolve correctly.

**Arch / Manjaro:**

```bash
sudo pacman -S papirus-icon-theme
# Nordic from AUR:
yay -S nordic-theme
```

**Debian / Ubuntu:**

```bash
sudo apt install papirus-icon-theme
# Nordic: download from https://github.com/EliverLara/Nordic/releases
```

### 4. Copy files into place

```bash
cp -r . ~/.config/gtk-4.0/
```

> [!NOTE]
> `gtk.css`, `gtk-dark.css` and `assets/` are symlinks to the installed system theme. They only resolve if the theme package is installed. If the theme is installed to a non-standard path, update the symlink targets accordingly.

### 5. Reload GTK

Log out and back in, or apply the theme immediately at runtime:

```bash
gsettings set org.gnome.desktop.interface gtk-theme 'Nordic'
gsettings set org.gnome.desktop.interface icon-theme 'Papirus-Dark'
```

GTK 4 applications re-read `~/.config/gtk-4.0/gtk.css` on launch, so restarting any app is usually enough to see the updated styles.

## Configuration

The configuration lives in two places: `settings.ini` for GTK-level settings and CSS files for colour and style overrides.

### `settings.ini`

| Key                                 | Value          | Description                                       |
| ----------------------------------- | -------------- | ------------------------------------------------- |
| `gtk-theme-name`                    | `Nordic`       | The system GTK theme to load                      |
| `gtk-icon-theme-name`               | `Papirus-Dark` | Icon theme used by GTK apps                       |
| `gtk-font-name`                     | `Adwaita Sans 11` | Default UI font and size                       |
| `gtk-cursor-theme-name`             | `default`      | Cursor theme                                      |
| `gtk-cursor-theme-size`             | `24`           | Cursor size in pixels                             |
| `gtk-application-prefer-dark-theme` | `1`            | Instructs apps to use their dark variant when `1` |

### CSS Files

| File             | Type    | Purpose                                                                                 |
| ---------------- | ------- | --------------------------------------------------------------------------------------- |
| `gtk.css`        | Symlink | Points to the system theme's primary GTK 4 CSS — loaded by GTK on every app launch     |
| `gtk-dark.css`   | Symlink | Points to the system theme's dark-mode CSS overrides                                    |
| `gtk.light.css`  | File    | Local light-mode colour token definitions using `@define-color`                         |
| `assets/`        | Symlink | Points to the system theme's asset directory (borders, shadows, window decorations)     |

### Light mode colour tokens (`gtk.light.css`)

| Token             | Value     | Role                                      |
| ----------------- | --------- | ----------------------------------------- |
| `accent_bg_color` | `#000000` | Background of accent/highlighted elements |
| `accent_fg_color` | `#ffffff` | Foreground text on accent elements        |
| `accent_color`    | `#000000` | General accent colour                     |
| `window_bg_color` | `#ffffff` | Default window background                 |
| `window_fg_color` | `#000000` | Default window text colour                |

To switch to light mode at runtime:

```bash
gsettings set org.gnome.desktop.interface color-scheme 'prefer-light'
```

To make dark mode permanent, ensure `gtk-application-prefer-dark-theme=1` remains set in `settings.ini`.

## Project Structure

| Path             | Type    | Purpose                                                                               |
| ---------------- | ------- | ------------------------------------------------------------------------------------- |
| `gtk.css`        | Symlink | Primary GTK 4 CSS (points to system theme)                                            |
| `gtk-dark.css`   | Symlink | Dark-mode CSS overrides (points to system theme)                                      |
| `gtk.light.css`  | File    | Local light-mode colour tokens via `@define-color`                                    |
| `settings.ini`   | File    | GTK 4 settings: theme, icons, font, cursor and dark-mode preference                   |
| `assets/`        | Symlink | Theme asset directory: borders, shadows, window decorations (points to system theme)  |

## Customisation

### Changing the accent colour

Open `gtk.light.css` and update the hex values:

```css
@define-color accent_bg_color #5e81ac; /* Nord blue */
@define-color accent_fg_color #eceff4;
@define-color accent_color #5e81ac;
```

Save the file and restart any GTK 4 application to see the change.

### Changing the icon theme

Edit `settings.ini`:

```ini
gtk-icon-theme-name=Papirus
```

Then apply at runtime:

```bash
gsettings set org.gnome.desktop.interface icon-theme 'Papirus'
```

### Changing the font

Edit `settings.ini`:

```ini
gtk-font-name=JetBrains Mono 11
```

Then apply at runtime:

```bash
gsettings set org.gnome.desktop.interface font-name 'JetBrains Mono 11'
```

### Switching the theme

Replace `gtk-theme-name` in `settings.ini` with any installed theme and update the symlink targets if needed:

```ini
gtk-theme-name=Adwaita
```

## Security & Privacy

- **No scripts**: There are no shell scripts or executables in this repository
- **No network access**: Everything is local CSS, INI and symlinks
- **Minimal surface**: Only the files GTK 4 reads are included

> [!IMPORTANT]
>
> 1. **Backup**: Always back up `~/.config/gtk-4.0/` before applying any new config
> 2. **System theme required**: The symlinks only resolve if the theme package is installed at the expected path
> 3. **GTK version**: These files target GTK **4.0** and are not compatible with GTK 3 apps

## Troubleshooting

### Common Issues

**Theme doesn't apply**

- Confirm Nordic is installed: `ls /usr/share/themes/ | grep -i nordic`
- Restart the GTK application — GTK 4 re-reads the CSS on every launch
- Log out and back in to reset the session's theme state

**Broken symlink errors**

- Check symlink targets: `ls -la ~/.config/gtk-4.0/`
- Verify the Nordic theme is installed at the path the symlinks point to
- Re-create the symlinks if the theme was installed to a different path

**Light mode colours look wrong**

- Verify `gtk.light.css` is being loaded by your desktop's colour-scheme setting
- Run `gsettings get org.gnome.desktop.interface color-scheme` and check the value

**Icons not showing**

- Confirm Papirus-Dark is installed: `ls /usr/share/icons/ | grep -i papirus`
- Run `gtk4-icon-browser` to inspect what icons GTK can resolve

**Cursor or font didn't change**

- Some apps cache settings; restart the app or the compositor
- Confirm `settings.ini` is at `~/.config/gtk-4.0/settings.ini`

## Contributing

Contributions are welcome! Please follow these guidelines:

1. **Fork** the repository
2. **Create a branch**: `git checkout -b fix/your-fix-name`
3. **Make your changes** — keep CSS changes minimal and well-commented
4. **Test** by restarting a GTK 4 application to confirm the change looks correct
5. **Submit a pull request** with a clear description of what you changed and why

### Code Style

- Use `@define-color` for all colour values — avoid hardcoded hex codes in selectors
- Keep selectors as specific as necessary, no more
- Comment non-obvious overrides with a short explanation
- One logical change per commit

## Support

For questions, issues, or suggestions:

- Open an issue in the repository
- Check existing issues before filing a new one
- Include your GTK version (`gtk4-demo --version`), distro, and theme version in bug reports

## License

This project is under the [MIT License](./LICENSE).
