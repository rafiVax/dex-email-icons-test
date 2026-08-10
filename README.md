# dex-email-icons-test

Temporary public host for DEX transactional-email icon assets (testing only), used until
`downloads.controlup.com` has write access to receive the corrected set.

## `email-icons/`

The live assets actually referenced by `infra-notifications-service`'s `emailIconsBaseUrl`
(see `src/utils/emailShell/tokens.ts`) — final, color/size-corrected raster exports used in
real emails today: `logo.png`, the card/footer/details-box icons, and `banner/bannerBg.png` /
`banner/taglineBg.png` / `banner/dashboardBg.png` (the 3 banner variants currently wired up in
`renderBanner()`).

## `email-icons/future/` and `email-icons/banner/future/`

Pre-rasterized, correctly-sized PNGs pulled from the Figma "Emails Library" file
(`K1juQw0F6Mx3HICL3MxEiD`) for icons/banner variants that **aren't used by any template yet**,
kept here so a future template can adopt one without re-exporting from Figma:

- `email-icons/future/` — 9 additional card icons already 40x40-at-4x (160x160px) and
  pre-colored to match the pink/green/orange bg-pairing rule (`archiveX`, `badgePlus`,
  `bellRing`, `bookMarked`, `circleDotDashed`, `fileSpreadsheet`, `fileText`, `layoutGrid`,
  `tvMinimalPlay`).
- `email-icons/banner/future/` — 10 additional 520x173-at-4x (2080x692px) banner background
  variants from the same Figma banner component set as the 3 live ones, **background-only —
  all baked-in text (title, status labels) was stripped**, matching how the 3 live banners work
  (`renderBanner()` overlays real HTML text on top of a background image, same pattern used
  here for `bannerBg.png`/`taglineBg.png`). Text was identified and removed by SVG path length
  (glyph-outline paths run 4000+ chars; every decorative shape in this set is under 350) and
  confirmed by rendering before/after. `skeletonRowsBg` is a flat-image version of what
  `renderBannerSkeleton()` already builds live in HTML/table code — kept here as a fallback,
  not currently used. Variants from this Figma set with real-looking data baked in (a date
  range, TB/count numbers, percentages, a specific trial-length badge) were left out entirely —
  that content has to stay as live HTML/text if ever built, not a flattened image. Also left
  out: a duplicate of the live `bannerBg.png` dots/corner-circle art.
