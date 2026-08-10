# dex-email-icons-test

Temporary public host for DEX transactional-email icon assets (testing only), used until
`downloads.controlup.com` has write access to receive the corrected set.

## `email-icons/`

All assets pulled from the Figma "Emails Library" file (`K1juQw0F6Mx3HICL3MxEiD`), flat in one
folder (icons directly here, banners under `banner/`). Not every file here is referenced by code
yet — check `infra-notifications-service`'s `src/utils/emailShell/tokens.ts` /
`src/utils/emailShell/icons/icons.ts` for what `emailIconsBaseUrl` actually wires up.

**Live** (referenced by `ICON_SPECS/renderBanner()` today): `logo.png` / `logoDark.png` (the
dark-mode-swap logo, all-white wordmark, padded to the same aspect ratio as `logo.png` so it
doesn't distort at the fixed 110x39 display size — confirmed against Figma's dark-mode frames,
not just a guess), the card/footer/
details-box icons (`personCheck`, `document`, `trashAlert`, `leaf`, `cloudAlert`, `bellDot`,
`troubleshoot`, `reminderBell`, `lightbulb`, `history`, `fileUser`, `messageCircleWarning`,
`windows`, `apple`, `signInAttempt`, `licenseFileKey`, `cloudCheck`, `packageBox`,
`graduationCap`, `bookOpen`, `users`), and `banner/bannerBg.png` / `banner/taglineBg.png` /
`banner/dashboardBg.png` (the 3 banner variants wired up in `renderBanner()`). A 4th variant,
`gauge` (title + two fixed illustrative "47%"/"62%" rings + a gradient line, confirmed against
Figma node 1551:23531 "Trial Welcome"), turned out simple enough to build as real HTML — two
nested-circle `<table>`s, same pattern `renderCard()`'s icon badge already uses — so it has no
image asset at all; that's also what fixed a real bug where the old fixed-size image + live-text
overlay drifted apart on narrow mobile widths.

**Not used by any template yet**, kept so a future template can adopt one without re-exporting
from Figma:
- 9 more card icons, already 40x40-at-4x (160x160px) and pre-colored to match the
  pink/green/orange bg-pairing rule: `archiveX`, `badgePlus`, `bellRing`, `bookMarked`,
  `circleDotDashed`, `fileSpreadsheet`, `fileText`, `layoutGrid`, `tvMinimalPlay`.
- 5 more banner backgrounds (520x173-at-4x / 2080x692px), background-only — all baked-in text
  was stripped, matching how the live banners work (`renderBanner()` overlays real HTML text on
  top of a background image): `diagonalRibbonsBg`, `circleCornerPlainBg`, `twoBlobCornersBg`,
  `borderedBoxBg`, `concentricRingsBg`. These 5 specifically are the ones whose visual effect
  *can't* be done reliably in table-based email HTML (Outlook's Word rendering engine is the
  real constraint, not browsers): `diagonalRibbonsBg` (rotated shapes), `circleCornerPlainBg` /
  `twoBlobCornersBg` (a shape peeking from behind the banner's own rounded corner needs
  overflow-clipping/absolute positioning), `borderedBoxBg` (a *gradient*-colored border —
  `border-image`/gradient borders have no Outlook equivalent), `concentricRingsBg` (nested
  concentric circles, fragile across clients).

Deliberately left out of this repo entirely:
- Banner variants from the same Figma set with real *per-recipient* data baked in (a date range,
  TB/count numbers, a specific trial-length badge) — that content has to stay as live HTML/text
  if ever built, not a flattened image.
- A duplicate of the live `bannerBg.png` dots/corner-circle art.
- Banner variants simple enough to just build as live HTML — solid/gradient bars, lines, and
  dots, the same pattern `renderBannerSkeleton()`'s gradient line and `renderUsageProgressBox()`
  already use elsewhere in `components.ts`. No image needed, ever: a "skeleton rows" banner
  (already built, `renderBannerSkeleton()`), a right-aligned-title + 3 gradient bars, a single
  partial-fill gradient progress bar, stacked words each with a gradient underline, and a
  vertical gradient accent bar + status dot.
