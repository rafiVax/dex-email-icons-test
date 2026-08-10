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
- `email-icons/banner/future/` — 5 additional 520x173-at-4x (2080x692px) banner background
  variants from the same Figma banner component set as the 3 live ones, **background-only —
  all baked-in text (title, status labels) was stripped**, matching how the 3 live banners work
  (`renderBanner()` overlays real HTML text on top of a background image). Only variants whose
  visual effect *can't* be done reliably in table-based email HTML (Outlook's Word rendering
  engine is the real constraint, not browsers) are kept as images: `diagonalRibbonsBg` (rotated
  shapes), `circleCornerPlainBg` / `twoBlobCornersBg` (a shape peeking from behind the banner's
  own rounded corner needs overflow-clipping/absolute positioning), `borderedBoxBg` (a
  *gradient*-colored border — `border-image`/gradient borders have no Outlook equivalent),
  `concentricRingsBg` (nested concentric circles, fragile across clients).

  Left out on purpose:
  - Variants with real-looking data baked in (a date range, TB/count numbers, percentages, a
    specific trial-length badge) — that content has to stay as live HTML/text if ever built,
    not a flattened image.
  - A duplicate of the live `bannerBg.png` dots/corner-circle art.
  - Variants that are simple enough to just build as live HTML — solid/gradient bars, lines, and
    dots, the same pattern `renderBannerSkeleton()`'s gradient line and `renderUsageProgressBox()`
    already use elsewhere in `components.ts`. No image needed, ever: a "skeleton rows" banner
    (already built, `renderBannerSkeleton()`), a right-aligned-title + 3 gradient bars, a single
    partial-fill gradient progress bar, stacked words each with a gradient underline, and a
    vertical gradient accent bar + status dot.
