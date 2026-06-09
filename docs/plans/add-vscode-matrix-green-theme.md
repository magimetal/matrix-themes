# Plan: Add VS Code Matrix Green Theme

## Objective
Add VS Code flavor of existing Matrix theme, using Zed/Pi palette approach and official VS Code color theme shape.

## Observed Context
- Existing source themes: `pi/matrix-green.json`, `ghostty/matrix`, `zed/matrix-green.json`.
- Zed theme exists and matches user preference: dark Matrix UI palette plus Ghostty ANSI terminal palette.
- VS Code docs define color themes through extension manifest `package.json` `contributes.themes` entries with `label`, `uiTheme`, and `path`.
- VS Code color theme file uses `colors`, `tokenColors`, optional `semanticHighlighting`, and `semanticTokenColors`.

## Decision
Create minimal VS Code theme extension folder, not standalone theme JSON only.

Why: VS Code discovers reusable themes through extension manifest contribution points. Bare JSON can be generated/imported for local experimentation, but `package.json` + `themes/*.json` is smallest usable repo artifact for installing, symlinking, packaging later, and validating manifest-to-theme linkage without speculative marketplace work.

## Target Files
- `vscode/package.json` — minimal VS Code extension manifest with one contributed dark color theme.
- `vscode/themes/matrix-green-color-theme.json` — VS Code color theme definition.
- `README.md` — add VS Code row, install instructions, validation commands, layout entry.

## Palette Mapping
Use Zed/Pi source values, converting Zed `#RRGGBBAA` to VS Code-supported `#RRGGBB` or `#RRGGBBAA` only where alpha is useful for UI overlays.

Core palette:
- Background: `#001005`
- Side/panel: `#001a0a`
- Elevated/input: `#00240d`
- Selection/active: `#003d18`
- Border: `#2b7a3e`
- Muted green: `#77c88a`
- Dim green: `#60b875`
- Neon green: `#00ff66`
- Green: `#00d15f`
- Mint text: `#d9ffe5`
- Cyan accent/warning/code: `#8fffd2`
- Lavender error/delete/conflict: `#d6ccff`
- Ghostty terminal background/foreground: `#070b12`, `#eef8ff`

Guardrail: no brown, orange, amber, or warm red. Negative states use lavender; warnings use cyan-mint.

## Tasks

### 1. Create VS Code extension manifest
What: Add minimal manifest declaring one theme.
Where: `vscode/package.json`.
Why: VS Code requires `contributes.themes` for discoverable/installable color theme artifact.
Acceptance criteria:
- JSON parses.
- Contains `name`, `displayName`, `description`, `version`, `publisher`, `engines.vscode`, `categories: ["Themes"]`.
- Contains `contributes.themes[0]` with `label: "Matrix Green"`, `uiTheme: "vs-dark"`, `path: "./themes/matrix-green-color-theme.json"`.
Guardrails:
- Do not add activation events, commands, dependencies, build tooling, or marketplace-only config.
Verification:
```bash
jq -e '.contributes.themes[0].label == "Matrix Green" and .contributes.themes[0].uiTheme == "vs-dark" and .contributes.themes[0].path == "./themes/matrix-green-color-theme.json" and (.categories | index("Themes"))' vscode/package.json
```

### 2. Create VS Code color theme JSON
What: Add VS Code color theme based on Zed UI colors, Pi syntax intent, and Ghostty terminal ANSI palette.
Where: `vscode/themes/matrix-green-color-theme.json`.
Why: Supplies actual workbench, syntax, semantic, and terminal colors.
Acceptance criteria:
- JSON parses.
- Top-level `name` is `Matrix Green`.
- Top-level `type` is `dark`.
- Has non-empty `colors`, `tokenColors`, `semanticHighlighting: true`, and `semanticTokenColors`.
- Uses `#RRGGBB`/`#RRGGBBAA` colors only.
- Maps terminal ANSI colors from `ghostty/matrix` into `terminal.ansi*` keys.
- Includes common workbench keys for editor, side bar, activity bar, status bar, tabs, panels, lists, inputs, buttons, badges, notifications, scrollbar, minimap, diff, git decorations, terminal.
- Includes TextMate scopes for comments, strings, numbers/constants, keywords/operators, functions, types/classes, variables, properties, punctuation, tags, headings/markup.
- Includes semantic tokens for `namespace`, `class`, `interface`, `enum`, `type`, `typeParameter`, `function`, `method`, `macro`, `parameter`, `variable`, `property`, `enumMember`, `decorator`, `keyword`, `comment`, `string`, `number`, `regexp`, `operator`.
Guardrails:
- Do not make all editor text neon; use `#d9ffe5` for foreground.
- Keep neon green for active controls, keywords, links, headings, cursor, activity badge.
- Use cyan for warnings/code accents/numbers; lavender for errors/deletions/conflicts.
- Do not copy Zed-only key names directly unless VS Code supports equivalent color IDs.
Verification:
```bash
jq -e '.name == "Matrix Green" and .type == "dark" and .semanticHighlighting == true and (.colors | length > 40) and (.tokenColors | length > 10) and (.semanticTokenColors | length > 10)' vscode/themes/matrix-green-color-theme.json

jq -e '([.. | strings | select(startswith("#"))] | all(test("^#[0-9A-Fa-f]{6}([0-9A-Fa-f]{2})?$")))' vscode/themes/matrix-green-color-theme.json

jq -e '.colors["terminal.background"] == "#070b12" and .colors["terminal.foreground"] == "#eef8ff" and .colors["terminal.ansiGreen"] == "#46ff8f" and .colors["terminal.ansiYellow"] == "#8fffd2"' vscode/themes/matrix-green-color-theme.json
```

### 3. Validate manifest-to-theme linkage
What: Check manifest path points to existing theme file and theme shape matches contribution.
Where: `vscode/package.json`, `vscode/themes/matrix-green-color-theme.json`.
Why: Prevents installable extension from listing broken theme path.
Acceptance criteria:
- Manifest path resolves from `vscode/` directory.
- Theme type dark matches manifest `uiTheme: vs-dark`.
Verification:
```bash
test -f "vscode/$(jq -r '.contributes.themes[0].path' vscode/package.json)"
jq -e 'input as $theme | .contributes.themes[0].uiTheme == "vs-dark" and $theme.type == "dark"' vscode/package.json vscode/themes/matrix-green-color-theme.json
```

### 4. Update README
What: Document VS Code source artifact, install path, and validation.
Where: `README.md`.
Why: Keeps repo usage complete across Pi, Ghostty, Zed, VS Code.
Acceptance criteria:
- Themes table includes VS Code row with `vscode/themes/matrix-green-color-theme.json` and `Matrix Green`.
- Install section includes symlink or copy workflow for VS Code extension folder.
- Edit/validate section includes VS Code validation commands above.
- Repository layout includes `vscode/package.json` and `vscode/themes/matrix-green-color-theme.json`.
Guardrails:
- Do not remove existing Pi/Ghostty/Zed docs.
- Do not claim Marketplace publishing unless explicitly added later.
Verification:
```bash
rg -n 'VS Code|vscode/package.json|matrix-green-color-theme.json|Matrix Green' README.md
```

### 5. Manual VS Code smoke test
What: Install local extension folder and select theme.
Where: VS Code UI / user extension dir.
Why: Confirms artifact works beyond JSON shape checks.
Acceptance criteria:
- VS Code lists `Matrix Green` in Color Theme picker.
- Editor, side bar, terminal, status bar, selections, syntax colors match Matrix/Zed direction.
Manual verification options:
```bash
mkdir -p ~/.vscode/extensions/matrix-green-theme
cp -R vscode/* ~/.vscode/extensions/matrix-green-theme/
# Then reload VS Code and run: Preferences: Color Theme -> Matrix Green
```
Alternative for symlink install:
```bash
mkdir -p ~/.vscode/extensions
ln -sfn "$PWD/vscode" ~/.vscode/extensions/matrix-green-theme
```
Guardrails:
- Use copy/symlink local extension only; no `vsce`, no publishing, no generated scaffold.

## Risks / Unknowns
- Unknown: exact VS Code color ID coverage desired. Mitigation: cover common workbench/editor/terminal IDs first; inspect with VS Code after install.
- Unknown: semantic token appearance across languages. Mitigation: enable `semanticHighlighting` and use standard token selectors from official docs.
- Risk: VS Code silently ignores unknown color IDs. Mitigation: keep IDs from VS Code Theme Color Reference/common built-ins and use manual smoke test.

## Final Verification Set
Run from repo root:
```bash
jq -e '.contributes.themes[0].label == "Matrix Green" and .contributes.themes[0].uiTheme == "vs-dark" and .contributes.themes[0].path == "./themes/matrix-green-color-theme.json" and (.categories | index("Themes"))' vscode/package.json

test -f "vscode/$(jq -r '.contributes.themes[0].path' vscode/package.json)"

jq -e '.name == "Matrix Green" and .type == "dark" and .semanticHighlighting == true and (.colors | length > 40) and (.tokenColors | length > 10) and (.semanticTokenColors | length > 10)' vscode/themes/matrix-green-color-theme.json

jq -e '([.. | strings | select(startswith("#"))] | all(test("^#[0-9A-Fa-f]{6}([0-9A-Fa-f]{2})?$")))' vscode/themes/matrix-green-color-theme.json

jq -e '.colors["terminal.background"] == "#070b12" and .colors["terminal.foreground"] == "#eef8ff" and .colors["terminal.ansiGreen"] == "#46ff8f" and .colors["terminal.ansiYellow"] == "#8fffd2"' vscode/themes/matrix-green-color-theme.json

rg -n 'VS Code|vscode/package.json|matrix-green-color-theme.json|Matrix Green' README.md
```
