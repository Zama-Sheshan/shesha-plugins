# Shesha design standards (brand-agnostic)

General conventions for how Shesha (Ant Design 6.x, light-mode) applications should be designed and styled — distilled from the Shesha Design System reference (A. Slavchov, Senior UI/UX). These are the *standards* the comprehension layer annotates blueprints with and the styling layer enforces; they hold **regardless of brand**. A brand's concrete hex/type values live in its `*.tokens.json` (e.g. `requirements-studio.tokens.json`), not here.

## Foundations

- **Light mode only.** No dark mode.
- **Type:** system-ui / brand sans; **14px body** for dense data-entry (not 16px), 12px caption, 16px card heading, 20px section heading, 24px page title. **Weights are binary: 400 and 600** — no 500/700 for hierarchy. Never pure black for text — use the brand's near-black ink.
- **Structure by line, not shadow.** Hairline 1px borders define cards/panels/rows; avoid decorative drop shadows (reserve soft shadows for overlays/modals only).
- **Surface proportion:** the dominant surface is the muted page canvas (~30%), with white cards on top — most of a page is *not* white. Brand-primary is the single interactive anchor (CTAs, links, active states, focus rings); deep/navy or dark-brand adds chrome depth.
- **4px spacing grid:** 4/8/12/16/20/24/32/40/48. Field gap 16px vertical; 24px between sections; card padding 16 (compact) / 24 (default).
- **Radius:** ~6px for controls (buttons/inputs/selects), ~8–12px for cards/panels, pill for status badges.

## AntD 6.x token mapping (set ONCE at app level)

Map the brand tokens onto `ConfigProvider theme.token` so the whole portal inherits them — do **not** repaint every button per-form:
`colorPrimary / colorPrimaryHover / colorPrimaryActive / colorPrimaryBg`, the semantic set (`colorSuccess/Warning/Error/Info` + their `Bg/Border`), neutrals (`colorText/TextSecondary/TextTertiary`, `colorBgLayout` = canvas, `colorBgContainer` = white, `colorBorder/colorBorderSecondary`), type (`fontFamily/fontSize=14/fontSizeLG=16/...`, `fontWeightStrong=600`, line-heights), shape (`borderRadius=6/LG=8`, `controlHeight=32/SM=24/LG=40`), and per-component overrides for `Button/Input/Select/Table/Card/Tabs/Menu/Steps`. In Shesha this is the app-level theme settings — see [app-theme.md](app-theme.md). A form looks "cheap" when only per-component blocks are set (AntD primary still default blue) or only the app theme is set (no surface/card treatment) — apply **both** layers.

## Component conventions

- **Buttons:** primary = brand fill, white 600 text, radius 6; default = white, hairline border, brand-coloured border/text on hover; danger = error fill; ghost/link = transparent, brand text. Sizes 24/32/40.
- **Inputs/Select/Date:** white bg, 1px border, radius 6, 4×12 padding, 14px; focus = brand border + 4px focus ring; error = error border + error ring; disabled = canvas bg + tertiary text. Label 14/600, required asterisk in error colour, helper 12px, validation 12px error.
- **Cards/Panels:** white bg, hairline border, radius 8; panel header on the alt surface with a bottom hairline, 12×16 padding, 14/600; body 16 padding; no shadow.
- **Data table:** header row on the alt surface, 14/600, 2px bottom border; body rows white with 1px row borders; row hover = brand-tint; selected = brand-subtle; cell 14/400, 12×16 padding.
- **Status badges/pills:** pill radius, 12px/500, 2×8 padding; colour from the **status lifecycle** in the brand tokens (bg/fg/border per status). Always pair colour with a text label — never colour alone.
- **Tabs:** inactive 14/400 secondary; active = brand text + 2px brand ink-bar.
- **Section separators / micro-labels:** uppercase 11–12/600 tertiary, letter-spacing ~0.06em.
- **Alerts/banners:** tinted bg + 4px left border in the semantic colour + matching icon.

## Voice & copy

- Labels in **sentence case** ("First name", not "First Name"). Actions verb-first ("Save changes", not "OK"). Validation specific ("Enter a valid email address"). Mark **required** with an asterisk; never mark optional. Placeholders are examples ("e.g. …"), not instructions, and never replace a label.

## Anti-patterns (never)

- Pure black `#000` text; font weights other than 400/600; body >14px in dense data entry.
- Decorative drop shadows on cards/panels (borders define structure).
- Using a brand's accent/semantic colours decoratively — semantic colours are operational status signals only.
- Placeholder used as a label; removing focus rings (accessibility is non-negotiable).
- Colour alone to convey status (always pair with icon/text).

## How comprehension uses this

When `shesha-design-comprehension` annotates a blueprint region with a `recipe:` (e.g. `card`, `section-header`, `kib-strip`, `status-chip`), that recipe resolves through these standards + the brand's tokens into concrete v7 style blocks via [component-recipes.md](component-recipes.md) and [token-to-prop-mapping.md](token-to-prop-mapping.md).
