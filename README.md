# cmux-theme-picker

Interactive terminal theme picker for [cmux](https://cmux.app) (Ghostty-based terminal). Browse themes with live preview, toggle between light/dark slots, and apply changes instantly.

[![Homebrew](https://img.shields.io/badge/homebrew-ih--hugh%2Ftap-yellow?logo=homebrew)](https://github.com/ih-hugh/homebrew-tap)
![Platform](https://img.shields.io/badge/platform-macOS%20%7C%20Linux-blue)

```bash
brew tap ih-hugh/tap
brew install cmux-theme-picker
```

## Features

- **Live preview** — as you scroll through themes with ↑/↓, each one is applied to your terminal in real time
- **Light/dark slot switching** — press Tab to toggle between the light and dark theme slot, preview themes in either context
- **Color swatches** — the preview pane renders background, foreground, selection, and full ANSI palette blocks
- **Duskbox markers** — themes from the [duskbox](https://github.com/nicm/duskbox) collection are marked with ★
- **Pair mode** — pick both light and dark themes in sequence with `--pair`
- **Safe revert** — pressing Esc restores your original themes
- **Auto mode detection** — defaults to the slot matching your macOS appearance (light/dark)

## Requirements

- [cmux](https://cmux.app) (or Ghostty ≥ 1.0)
- [fzf](https://github.com/junegunn/fzf) ≥ 0.53 (for `transform-prompt` support)
- Bash 3.2+ (works on macOS `/bin/bash`)

Install fzf on macOS:

```bash
brew install fzf
```

## Install

### Homebrew (recommended)

```bash
brew tap ih-hugh/tap
brew install cmux-theme-picker
```

### Manual

```bash
# Download and install
curl -sL https://github.com/ih-hugh/cmux-theme-picker/releases/latest/download/cmux-theme-picker \
  -o ~/.local/bin/cmux-theme-picker
chmod +x ~/.local/bin/cmux-theme-picker
```

Make sure `cmux` is available in your `$PATH`. On macOS it's at:

```
/Applications/cmux.app/Contents/Resources/bin/cmux
```

Add that directory to your PATH or symlink `cmux` into `~/.local/bin/`.

## Usage

```bash
# Pick a theme (auto-detects light/dark mode)
cmux-theme-picker

# Start in dark slot mode
cmux-theme-picker --dark

# Start in light slot mode
cmux-theme-picker --light

# Pick both light and dark themes separately
cmux-theme-picker --pair
```

### Key bindings

| Key     | Action                                    |
|---------|-------------------------------------------|
| ↑ / ↓   | Browse themes (live preview)              |
| Tab     | Toggle between light ↔ dark slot          |
| Enter   | Select theme (keeps current preview)      |
| Esc     | Cancel and revert to original themes      |

### How it works

The picker uses fzf's `execute-silent` binding on the `focus` event to apply each theme as you scroll. Small helper scripts in `/tmp/cmux-theme-picker-$PID/` coordinate state (current slot, original themes) between the main picker and the fzf sub-commands.

State is split into two groups:

- **`orig-*`** files — saved once at startup, never modified. Used by Esc/revert to restore your original themes.
- **`current-*`** files — updated on every preview. Shown in the fzf preview pane so you can see what's currently applied for each slot.

## Theme directory

Themes are loaded from the following directories (first match wins):

1. `~/Library/Application Support/com.cmuxterm.app/themes/`
2. `~/.config/ghostty/themes/`
3. `/Applications/cmux.app/Contents/Resources/ghostty/themes/`
4. `/Applications/Ghostty.app/Contents/Resources/ghostty/themes/`

Add custom themes to the first directory using the same format as [Ghostty theme files](https://ghostty.org/docs/config/reference#theme).

## License

MIT