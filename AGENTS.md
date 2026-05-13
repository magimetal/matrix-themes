<!--THIS IS A GENERATED FILE - DO NOT MODIFY DIRECTLY, FOR MANUAL ADJUSTMENTS UPDATE `AGENTS_CUSTOM.MD`-->
# ALWAYS READ THESE FILE(S)
- @AGENTS_CUSTOM.md

# PROJECT KNOWLEDGE BASE

**Generated:** 2026-05-13
**Commit:** none (not a git repo)
**Branch:** none (not a git repo)

## OVERVIEW
Matrix terminal theme repo. Holds one Pi TUI theme JSON plus one Ghostty theme file, both using cold green/cyan/lavender palette direction.

## STRUCTURE
```
matrix-themes/
├── pi/matrix-green.json     # Pi TUI theme: 51 required color tokens, vars-driven
├── ghostty/matrix           # Ghostty terminal theme: ANSI palette + UI colors
└── README.md                # Pi theme rationale and token map
```

## WHERE TO LOOK
| Task | Location | Notes |
|------|----------|-------|
| Change Pi agent UI colors | `pi/matrix-green.json` | Edit `vars` first, then token refs in `colors`. |
| Check Pi token rationale | `README.md` | Long-form rationale maps tokens to palette variables. |
| Change Ghostty palette | `ghostty/matrix` | Uses Ghostty `key = value` syntax, no extension. |
| Compare terminal vs Pi mood | `ghostty/matrix`, `pi/matrix-green.json` | Ghostty is cooler blue-black; Pi is greener black. |

## CONVENTIONS
- Pi theme name: `matrix-green`; keep unique if copied into Pi theme search paths.
- Pi theme keeps all 51 `colors` tokens. Missing token breaks Pi theme validation/loading.
- Pi colors reference `vars` names, not scattered raw hex, except schema/name metadata.
- Pi warning/error avoid amber/red/brown: warning=`cyanMint`, error=`lavenderCool`.
- Default Pi prose/tool output uses soft mint `text`, not neon green.
- Ghostty file has no `.conf` suffix; keep as `ghostty/matrix` unless install target requires rename.

## ANTI-PATTERNS (THIS PROJECT)
- Do not introduce brown, orange, amber, or warm warning colors into Matrix palette.
- Do not make all readable text neon; reserve brightest green for affordances and landmarks.
- Do not edit Pi `colors` token names unless Pi schema changes; values may change, keys must stay.
- Do not assume README link path is canonical. Actual Pi theme source is `pi/matrix-green.json`.

## UNIQUE STYLES
- Palette identity: black-green backgrounds, neon green affordances, cyan-mint warning/code accents, lavender negative-state exception.
- Pi panels use layered greens: `bg` < `panel` < `panelAlt` < `selection`.
- Ghostty uses haunted cool tones from corrected Matrix/Pi intent, not exact Pi variable parity.

## COMMANDS
```bash
# Validate Pi theme JSON and required token count
jq -e '.name == "matrix-green" and (.colors | length == 51)' pi/matrix-green.json

# Find Pi color refs that do not resolve to vars
jq -r '. as $r | .colors | to_entries[] | select(.value|type=="string") | select(.value != "" and (.value|startswith("#")|not)) | .value' pi/matrix-green.json | sort -u | while read v; do jq -e --arg v "$v" '.vars[$v] != null' pi/matrix-green.json >/dev/null || echo "$v"; done

# Install locally for Pi manual testing
mkdir -p ~/.pi/agent/themes && cp pi/matrix-green.json ~/.pi/agent/themes/
```

## NOTES
- Repo has no package manager, build system, test runner, CI, or git metadata.
- Pi loads project themes from `.pi/themes/*.json`; this repo currently stores source under `pi/`, so copy or link before live Pi testing.
- README favors symlink installs so edits in this repo update tool config without copy drift.
- Optional Pi `export` colors are present and map HTML export surfaces back to theme vars.
