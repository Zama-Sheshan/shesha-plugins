# Component recipes (Requirements Studio archetypes)

Per-component v7 style blocks — the second styling layer. Copy a recipe, fill it with the brand theme's resolved values (via [token-to-prop-mapping.md](token-to-prop-mapping.md)), and mirror the block across desktop/tablet/mobile unless the design is genuinely responsive. Values below reference the `requirements-studio` theme; swap for another brand's tokens.

Recipe names match the `recipe:` annotations the comprehension layer puts on blueprint regions, so a blueprint region maps 1:1 to a recipe here.

## page-title-band
Header band container: `background.color` = surface (white), bottom hairline (`border.borderType:"custom"`, bottom = lines.border), `stylingBox` padding `20 24`. Title text: fontSize = title (24), fontWeight 600, color ink.primary. Subtitle text: fontSize = subtitle (14–18), color ink.soft. Actions: a right-aligned `buttonGroup` (primary = brand fill, secondary = white + hairline border).

## status-chip  (refListStatus)
Bind to the status property; colour each lifecycle item from `statusLifecycle.badges.<status>` (bg/fg/border). Pill radius (9999), fontSize 12, fontWeight 500–600, padding `2 8`. Never colour-only — the label text is the status name.

## kib-strip  (Key Info Bar)
A `columns` row (equal columns) on the surface, separated by vertical hairlines or gaps. Each cell: a micro-label (uppercase 11–12/600, color ink.soft, letter-spacing 0.06em) above a value (14/400, ink.primary). `stylingBox` padding `12 16`. Bottom hairline under the strip.

## card + section-header
Card container: white `background.color`, hairline `border.all` (lines.border), `border.radius.all` = radius.lg (12), no shadow. Header strip: child container, `background.color` = surfaceAlt, bottom hairline, padding `12 16`; header text color = brand.primary (sectionHeading role), 600, fontSize cardHeader (16); optional leading icon (brand). Count badge (e.g. "7"): small pill, brand.tint bg, brand.primary text. Body: `stylingBox` padding 16.

## detail-attributes  (label-left / control-right rows)
Inside the Details card: each attribute is a 2-cell `columns` row — label cell (fixed ~96–200px, 14/600, ink.muted, right-aligned `:`) + value/control cell (fills). Row gap 8–12. Read mode shows the value; edit mode shows the control (input/select inheriting app-theme border/radius). Do NOT stack label-over-value full width.

## datatable / datalist
Header row: surfaceAlt bg, 14/600 ink.primary, 2px bottom border. Body rows: white, 1px row hairline (divider), hover = brand.tint, selected = brand.tint. Cell 14/400, padding `12 16`. Inline status cells use the status-chip recipe.

## related-panel  (count-badged rail panel)
A `card` (above) whose header carries the section-header + a count badge (right) + an inline "+" link (brand) to add. Body = a datalist of the linked items (each a row/chip with the linked label + status chip + remove "×"). Stacks vertically in the rail with 16 gap.

## rail
The right column container (fixed ~332px on desktop): vertical stack, gap 16, no own background (panels carry their own white card surfaces). Ensure the column has a complete flex/column block so it does not collapse.
