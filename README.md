# dex-email-icons-test

Temporary public host for DEX transactional-email icon assets (testing only), used until
`downloads.controlup.com` has write access to receive the corrected set.

- `email-icons/` — the live PNGs actually referenced by `infra-notifications-service`'s
  `emailIconsBaseUrl` (see `src/utils/emailShell/tokens.ts`). These are the final,
  color/size-corrected raster exports used in real emails.
- `sources/` — raw SVG exports pulled from the Figma "Emails Library" file
  (`K1juQw0F6Mx3HICL3MxEiD`), kept here so a new icon/element can be re-rasterized without
  needing to re-export from Figma each time.
  - `sources/elements/` — layout elements (footer, banner pieces, misc components), exported
    with generic numeric names as given by Figma's export panel.
  - `sources/all-icons-svg/` — the full icon set from Figma's icon library, original names.
