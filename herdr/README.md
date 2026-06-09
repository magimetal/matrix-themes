# Herdr Matrix Theme Override

Herdr does not load a repo theme file directly. Keep this repo snippet as source documentation, then paste it into `/Users/magimetal/.config/herdr/config.toml`.

## Install

Add this block to `/Users/magimetal/.config/herdr/config.toml`:

```toml
[theme]
name = "terminal"

[theme.custom]
panel_bg = "#001a0a"
accent = "#00ff66"
surface0 = "#003d18"
surface1 = "#00240d"
surface_dim = "#001005"
overlay0 = "#60b875"
overlay1 = "#77c88a"
text = "#d9ffe5"
subtext0 = "#77c88a"
mauve = "#66ff99"
green = "#00ff66"
yellow = "#8fffd2"
red = "#d6ccff"
blue = "#8fffd2"
teal = "#8fffd2"
peach = "#8fffd2"
```

Reload Herdr after saving:

```bash
herdr server reload-config
```

## Palette notes

- Black-green backgrounds match Pi panel layering.
- Neon green marks active controls and high-emphasis UI.
- Soft mint text keeps long output readable.
- `yellow` uses cyan-mint warning color, matching Pi `cyanMint`.
- `red` uses cool lavender error color, matching Pi `lavenderCool`.
- Avoid warm warning/error hues so Herdr stays aligned with repo Matrix palette.
