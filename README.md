# Matrix Themes

Matrix-inspired terminal and editor themes for Pi, Ghostty, Zed, and VS Code.

This repo keeps source theme files in tool-specific directories. Pi, Ghostty, and Zed favor symlink installs for live editing; VS Code builds a real installable `.vsix` package.

## Themes

| App | Source file | Theme name | Notes |
|---|---|---|---|
| Pi coding agent | `pi/matrix-green.json` | `matrix-green` | Full Pi TUI theme with all 51 required color tokens. |
| Ghostty | `ghostty/matrix` | `matrix` | Cool dark Matrix terminal palette. |
| Zed | `zed/matrix-green.json` | `Matrix Green / Zed` | Dark editor theme derived from Pi UI colors and Ghostty terminal ANSI colors. |
| VS Code | `vscode/themes/matrix-green-color-theme.json` | `Matrix Green` | Dark VS Code theme extension using Zed/Pi UI colors and Ghostty terminal ANSI colors. |

## Install

Run commands from repo root. Pi, Ghostty, and Zed use symlinks for live editing. VS Code uses a packaged `.vsix`.

### Pi

Pi loads global custom themes from `~/.pi/agent/themes/*.json`.

```bash
mkdir -p ~/.pi/agent/themes
ln -sfn "$PWD/pi/matrix-green.json" ~/.pi/agent/themes/matrix-green.json
```

Select `matrix-green` in Pi via `/settings`, or set it in Pi settings:

```json
{
  "theme": "matrix-green"
}
```

Pi hot-reloads the active custom theme when the linked source file changes.

### Ghostty

Ghostty custom themes can live under the Ghostty config themes directory.

```bash
mkdir -p ~/.config/ghostty/themes
ln -sfn "$PWD/ghostty/matrix" ~/.config/ghostty/themes/matrix
```

Then set theme in Ghostty config:

```ini
theme = matrix
```

If your Ghostty config lives outside `~/.config/ghostty` (common on macOS GUI installs), symlink into that config directory's `themes/` folder instead.

### Zed

Zed custom themes can live under `~/.config/zed/themes`.

```bash
mkdir -p ~/.config/zed/themes
ln -sfn "$PWD/zed/matrix-green.json" ~/.config/zed/themes/matrix-green.json
```

Restart Zed or reopen the theme selector, then select `Matrix Green / Zed`.

### VS Code

Build and install the VSIX package instead of symlinking the extension directory.

```bash
cd vscode
npm run package
cd ..
code --install-extension vscode/matrix-green-theme-0.1.0.vsix
```

Reload VS Code if prompted, then run `Preferences: Color Theme` and select `Matrix Green`.

## Install by copy

Use copy install when you do not want the live repo linked into app config.

```bash
mkdir -p ~/.pi/agent/themes
cp pi/matrix-green.json ~/.pi/agent/themes/matrix-green.json

mkdir -p ~/.config/ghostty/themes
cp ghostty/matrix ~/.config/ghostty/themes/matrix

mkdir -p ~/.config/zed/themes
cp zed/matrix-green.json ~/.config/zed/themes/matrix-green.json
```

For VS Code, build and install the `.vsix`:

```bash
cd vscode
npm run package
cd ..
code --install-extension vscode/matrix-green-theme-0.1.0.vsix
```

Copy installs do not update when repo files change. Re-copy after edits, or rebuild/reinstall the VS Code `.vsix` after VS Code theme edits.

## Palette direction

- Dark green/black backgrounds.
- Neon green for active controls, links, headings, and high-emphasis UI.
- Soft mint text for long-form readability.
- Cyan-mint warnings/code accents instead of amber.
- Cool lavender error/removal accents instead of red.
- No brown, orange, or amber unless the theme direction intentionally changes.

## Edit and validate

### Pi theme

Prefer changing values in `vars`, then referencing those vars from `colors`.

```bash
jq -e '.name == "matrix-green" and (.colors | length == 51)' pi/matrix-green.json

jq -r '.colors | to_entries[] | select(.value|type=="string") | select(.value != "" and (.value|startswith("#")|not)) | .value' pi/matrix-green.json \
  | sort -u \
  | while read v; do jq -e --arg v "$v" '.vars[$v] != null' pi/matrix-green.json >/dev/null || echo "$v"; done
```

### Zed theme

Keep Zed theme colors in 8-digit `#RRGGBBAA` format.

```bash
jq -e '.name == "Matrix Green" and .themes[0].name == "Matrix Green / Zed" and .themes[0].appearance == "dark"' zed/matrix-green.json

jq -e '([.. | strings | select(startswith("#"))] | length > 0 and all(test("^#[0-9A-Fa-f]{8}$")))' zed/matrix-green.json
```

### VS Code theme

Keep `vscode/package.json` minimal: one contributed dark color theme pointing at `vscode/themes/matrix-green-color-theme.json`.

```bash
jq -e '.contributes.themes[0].label == "Matrix Green" and .contributes.themes[0].uiTheme == "vs-dark" and .contributes.themes[0].path == "./themes/matrix-green-color-theme.json" and (.categories | index("Themes"))' vscode/package.json

test -f "vscode/$(jq -r '.contributes.themes[0].path' vscode/package.json)"

jq -e '.name == "Matrix Green" and .type == "dark" and .semanticHighlighting == true and (.colors | length > 40) and (.tokenColors | length > 10) and (.semanticTokenColors | length > 10)' vscode/themes/matrix-green-color-theme.json

jq -e '([.. | strings | select(startswith("#"))] | all(test("^#[0-9A-Fa-f]{6}([0-9A-Fa-f]{2})?$")))' vscode/themes/matrix-green-color-theme.json
```

### Ghostty theme

Keep `ghostty/matrix` as Ghostty config syntax:

```ini
background = #070b12
foreground = #eef8ff
palette = 0=#070b12
```

## Repository layout

```text
matrix-themes/
├── pi/matrix-green.json
├── ghostty/matrix
├── zed/matrix-green.json
├── vscode/package.json
├── vscode/README.md
├── vscode/.vscodeignore
├── vscode/matrix-green-theme-0.1.0.vsix
├── vscode/themes/matrix-green-color-theme.json
├── AGENTS.md
└── README.md
```
