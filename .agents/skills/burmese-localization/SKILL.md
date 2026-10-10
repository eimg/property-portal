---
name: burmese-localization
description: Maintain Burmese UI localization for the Property Portal web app, including its JSON catalogs and Burmese typography. Use when translating or changing the English/Burmese interface; listing content is user-authored and stays as entered.
---

# Burmese Localization

Use this skill for English/Burmese interface copy and locale behavior in the Property Portal web app.

## Project conventions

- The web app is an independent repository in `app/`. Its editable catalogs are `app/src/locales/en.json` and `app/src/locales/my.json`; read the app's `AGENTS.md` before making code changes.
- Catalog entries use the English interface string as the key. Keep the same keys in both files. `en.json` values are English; `my.json` values are Burmese.
- Read the current Burmese catalog before editing. Preserve the user's manual translations, make focused changes, and do not regenerate or replace the catalog from an older copy.
- Listing titles, descriptions, addresses, inquiry messages, and other user-entered listing content remain as authored. Keep them outside translation, using the existing `data-no-translate` boundary where applicable.
- Translate interface text and known catalog labels only. Keep the product's current English/Burmese scope, API behavior, and MMK price rules intact.

## Burmese layout

- Burmese glyphs are naturally taller. Let them use their natural metrics: Burmese mode resets letter spacing to `normal` and line height to `normal` in `app/src/index.css`. Do not introduce letter spacing or fixed line heights for Burmese text.
- Keep adjacent header controls aligned and equal in height across locales. Allow enough vertical room for Burmese glyphs; center the label within the control rather than compressing the text line box.
- Preserve the existing Myanmar font stack and responsive heading sizing unless the user requests a design change.

## Updating and checking translations

- For existing interface strings, edit the matching value in the relevant JSON catalog. If a new English interface string is introduced in code, add its key to both catalogs and the English and Burmese values respectively.
- Keep JSON valid and catalog keys synchronized. Run the app's TypeScript check and production build when code or imports change; for JSON-only edits, at minimum parse both JSON files and confirm their key sets match.
