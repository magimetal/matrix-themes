# Matrix Pi Theme

This directory contains the custom Pi TUI theme `matrix-green`, defined in [`matrix-green.json`](./matrix-green.json).

The theme is a dark, green-forward Matrix-inspired interface. It is designed to feel like a terminal submerged in code rain without sacrificing readability during long agent sessions, tool output review, markdown reading, syntax scanning, or diff inspection.

## Source Files

- `matrix-green.json` — the actual Pi theme definition.
- `README.md` — this rationale and token map.

Pi themes are JSON files with:

- `$schema` for editor validation.
- `name` as the unique theme name.
- `vars` as reusable palette entries.
- `colors` as the required Pi UI color tokens.
- `export` as optional HTML export colors.

## Design Philosophy

The theme is built around a constrained Matrix palette: black-green backgrounds, layered green panels, neon-green accents, mint text, and cool cyan/lavender exceptions for semantic separation.

The main goals are:

1. **Terminal atmosphere without muddy colors.** The palette uses deep green-blacks rather than generic black, so panels and selections remain visible.
2. **Readable long-form output.** Default text is soft mint instead of pure neon green, reducing eye fatigue.
3. **Clear hierarchy.** Saturation increases as importance increases: dim greens for low-priority text, bright greens for active affordances, cyan/lavender for special warning/error cases.
4. **No brown / no amber.** Traditional terminal warning colors often drift into amber, orange, or brown. This theme intentionally avoids those tones to preserve the cold Matrix identity. Warning states use cyan-mint instead.
5. **Semantic color without breaking the palette.** Success remains green, warnings become cyan-mint, and errors use cool lavender over a blue-green error background. That keeps every state distinguishable without introducing red/orange/brown noise.

## Palette Variables

These `vars` are the reusable color primitives referenced by the required Pi tokens.

| Variable | Value | Rationale |
|---|---:|---|
| `bg` | `#001005` | Near-black green page/background base. It gives the whole interface a Matrix terminal floor without becoming absolute black. |
| `panel` | `#001a0a` | Slightly lifted dark green for primary message panels. It separates content from `bg` while staying subdued. |
| `panelAlt` | `#00240d` | Brighter alternate panel for custom/extension messages. It creates visible layering without adding another hue. |
| `selection` | `#003d18` | Saturated dark green selection fill. It must be obvious behind selected rows while preserving text contrast. |
| `borderDark` | `#2b7a3e` | Structured green border. It is visible on dark panels but intentionally not neon, so boxes do not dominate content. |
| `borderMuted` | `#2b7a3e` | Same value as `borderDark` to keep the UI frame consistent and avoid low-contrast hairlines. |
| `greenDim` | `#60b875` | Dim but legible green for tertiary information, comments, and low-intensity thinking states. |
| `greenMuted` | `#77c88a` | Secondary text green. It is softer than accent colors but clearer than `greenDim`. |
| `green` | `#00d15f` | Core saturated Matrix green. Used where medium emphasis is needed without full neon intensity. |
| `greenBright` | `#00ff66` | Primary neon green. Used for active controls, links, titles, bullets, and high-emphasis UI affordances. |
| `greenNeon` | `#66ff99` | Bright mint-green highlight. Used for success, headings, and function names where prominence should be high but less harsh than `greenBright`. |
| `mint` | `#b8ffd0` | Pale mint for syntax types. It reads as technical/structural while remaining inside the green family. |
| `text` | `#d9ffe5` | Default foreground. Softer than white and more readable than neon for long output. |
| `successBg` | `#00260f` | Dark green success panel. Indicates positive tool completion while keeping focus on text. |
| `pendingBg` | `#001f0c` | Low-intensity pending panel. Indicates activity without looking completed or alarming. |
| `errorBg` | `#061d22` | Dark blue-green error panel. Creates state contrast without using red/brown. |
| `cyanMint` | `#8fffd2` | Cool cyan-mint semantic accent. Used for warnings, numbers, inline code, extra-high thinking, and bash mode. |
| `lavenderCool` | `#d6ccff` | Cool lavender exception color. Used for errors and removed diff lines so negative states are clearly distinct without red/amber. |

## Required Pi Color Tokens

Pi requires every color token below. The Matrix theme maps each token to a palette variable rather than scattering raw hex values, keeping the theme coherent and easy to tune.

### Core UI

| Token | Uses | Assigned variable | Rationale |
|---|---|---|---|
| `accent` | Primary accent, logo, selected items, cursor | `greenBright` | The brightest Matrix green should own the most recognizable active UI moments. |
| `border` | Normal borders | `borderDark` | Borders need structure without competing with text or accent elements. |
| `borderAccent` | Highlighted borders | `green` | A saturated green distinguishes active borders while staying below full neon. |
| `borderMuted` | Subtle borders/editor frame | `borderMuted` | Keeps quieter frames visible against dark backgrounds. |
| `success` | Success states | `greenNeon` | Success should feel positive and luminous, but smoother than hard neon. |
| `error` | Error states | `lavenderCool` | Cool lavender creates immediate contrast without breaking the no-red/no-brown Matrix palette. |
| `warning` | Warning states | `cyanMint` | Cyan-mint replaces amber so warning remains distinct without introducing brown/orange. |
| `muted` | Secondary text | `greenMuted` | Secondary information stays readable but less dominant than default text. |
| `dim` | Tertiary text | `greenDim` | Low-priority text remains visible while receding into the interface. |
| `text` | Default text | `text` | Pale mint maximizes readability over long sessions without pure-white glare. |
| `thinkingText` | Thinking block text | `greenMuted` | Thinking content is important, but should not overpower final output. |

### Backgrounds and Content

| Token | Uses | Assigned variable | Rationale |
|---|---|---|---|
| `selectedBg` | Selected line background | `selection` | Deep saturated green gives selection a strong silhouette under mint text. |
| `userMessageBg` | User message panel | `panel` | Primary panel shade separates user prompts from the base background. |
| `userMessageText` | User message text | `text` | User input needs maximum legibility. |
| `customMessageBg` | Extension/custom message panel | `panelAlt` | Alternate panel shade distinguishes extension content from user messages. |
| `customMessageText` | Extension/custom message text | `text` | Custom messages remain readable and consistent with the main foreground. |
| `customMessageLabel` | Extension/custom label | `greenBright` | Labels act like UI markers, so they use the primary neon accent. |
| `toolPendingBg` | Pending tool box | `pendingBg` | Activity state stays subtle and dark until it resolves. |
| `toolSuccessBg` | Successful tool box | `successBg` | Completion is signaled through a greener panel without shouting. |
| `toolErrorBg` | Failed tool box | `errorBg` | Error panels need a distinct temperature shift while avoiding red/brown. |
| `toolTitle` | Tool title | `greenBright` | Tool headers are navigation landmarks and need high visibility. |
| `toolOutput` | Tool output text | `text` | Command and tool output should be readable above all else. |

### Markdown

| Token | Uses | Assigned variable | Rationale |
|---|---|---|---|
| `mdHeading` | Markdown headings | `greenNeon` | Headings need prominence but should feel smoother than link/action neon. |
| `mdLink` | Link text | `greenBright` | Links are interactive targets and should read as active elements. |
| `mdLinkUrl` | Link URL/details | `greenMuted` | URLs are supporting information, so they receive secondary emphasis. |
| `mdCode` | Inline code | `cyanMint` | Inline code benefits from a cool technical accent separate from prose. |
| `mdCodeBlock` | Code block content | `text` | Code blocks need the same high readability as tool output. |
| `mdCodeBlockBorder` | Code block border/fence | `borderDark` | Code fences should frame content without becoming the focus. |
| `mdQuote` | Blockquote text | `greenMuted` | Quotes are secondary to main prose, so muted green is appropriate. |
| `mdQuoteBorder` | Blockquote border | `borderDark` | Border consistency keeps quoted sections structured. |
| `mdHr` | Horizontal rule | `borderMuted` | Rules should divide content quietly rather than flash. |
| `mdListBullet` | List bullets | `greenBright` | Bullets become quick visual anchors in dense markdown. |

### Tool Diffs

| Token | Uses | Assigned variable | Rationale |
|---|---|---|---|
| `toolDiffAdded` | Added lines | `greenBright` | Additions align naturally with Matrix neon and positive change. |
| `toolDiffRemoved` | Removed lines | `lavenderCool` | Removed lines must be distinct from additions without using red/amber. |
| `toolDiffContext` | Unchanged context lines | `greenMuted` | Context should be readable but clearly less important than changed lines. |

### Syntax Highlighting

| Token | Uses | Assigned variable | Rationale |
|---|---|---|---|
| `syntaxComment` | Comments | `greenDim` | Comments should recede while remaining legible. |
| `syntaxKeyword` | Keywords | `greenBright` | Language control words deserve high recognition and quick scanning. |
| `syntaxFunction` | Function names | `greenNeon` | Functions are key landmarks, so they receive a bright but readable highlight. |
| `syntaxVariable` | Variables | `text` | Variables are frequent; default text prevents visual noise. |
| `syntaxString` | Strings | `greenMuted` | Strings remain in-family and slightly softer than identifiers. |
| `syntaxNumber` | Numbers | `cyanMint` | Numbers get a cool accent for fast recognition in data-heavy output. |
| `syntaxType` | Types | `mint` | Types are structural and benefit from a pale, precise mint. |
| `syntaxOperator` | Operators | `greenBright` | Operators are compact syntax pivots and need crisp visibility. |
| `syntaxPunctuation` | Punctuation | `greenMuted` | Punctuation should be visible but not louder than code symbols. |

### Thinking Level Borders

| Token | Uses | Assigned variable | Rationale |
|---|---|---|---|
| `thinkingOff` | Thinking disabled/off border | `borderMuted` | Off state uses the quiet frame color. |
| `thinkingMinimal` | Minimal thinking border | `greenDim` | Minimal effort is present but deliberately low-intensity. |
| `thinkingLow` | Low thinking border | `greenMuted` | Low effort steps up one visibility tier. |
| `thinkingMedium` | Medium thinking border | `green` | Medium effort uses the central Matrix green. |
| `thinkingHigh` | High thinking border | `greenBright` | High effort should be clearly active and prominent. |
| `thinkingXhigh` | Extra-high thinking border | `cyanMint` | Extra-high thinking gets the special cool accent so it is unmistakable. |

### Bash Mode

| Token | Uses | Assigned variable | Rationale |
|---|---|---|---|
| `bashMode` | Editor border when using bash mode with `!` prefix | `cyanMint` | Bash mode is operationally different from normal prompting, so it uses the distinct cyan-mint accent. |

## HTML Export Colors

The optional `export` block maps HTML export surfaces back to the same layered background system.

| Export token | Assigned variable | Rationale |
|---|---|---|
| `pageBg` | `bg` | Export pages keep the same near-black Matrix base. |
| `cardBg` | `panel` | Export cards mirror primary message panels. |
| `infoBg` | `panelAlt` | Export info blocks use the alternate panel for hierarchy. |

## Contrast and Readability Notes

- Default prose and tool output use `text` (`#d9ffe5`) rather than neon green because continuous neon text causes fatigue.
- The brightest values are reserved for navigation, active affordances, links, tool titles, bullets, and important syntax markers.
- Backgrounds are separated by small luminance steps so panels remain visible in truecolor terminals while preserving the dark terminal mood.
- Warning/error differentiation is achieved through cyan-mint and lavender, not amber/red. The result is less conventional but more faithful to the intended no-brown Matrix palette.

## Maintenance Notes

When editing `matrix-green.json`:

1. Keep all required Pi `colors` tokens present.
2. Prefer adding or changing values in `vars`, then referencing those variables from `colors`.
3. Preserve the no-brown/no-amber rule unless the theme direction intentionally changes.
4. Check message panels, tool states, markdown, diffs, syntax highlighting, thinking borders, and bash mode after changes.
5. If a token changes, update this README so the rationale remains accurate.
