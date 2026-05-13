# Matrix Themes

Matrix-inspired terminal themes for Pi and Ghostty.

This repo keeps source theme files in tool-specific directories and favors symlink installs so local edits in this repo hot-reload or stay one command away from use.

## Themes

| App | Source file | Theme name | Notes |
|---|---|---|---|
| Pi coding agent | `pi/matrix-green.json` | `matrix-green` | Full Pi TUI theme with all 51 required color tokens. |
| Ghostty | `ghostty/matrix` | `matrix` | Cool dark Matrix terminal palette. |

## Install with symlinks

Run commands from repo root.

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

## Install by copy

Use copy install when you do not want the live repo linked into app config.

```bash
mkdir -p ~/.pi/agent/themes
cp pi/matrix-green.json ~/.pi/agent/themes/matrix-green.json

mkdir -p ~/.config/ghostty/themes
cp ghostty/matrix ~/.config/ghostty/themes/matrix
```

Copy installs do not update when repo files change. Re-copy after edits.

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
├── AGENTS.md
└── README.md
```
