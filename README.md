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
not just a guess), the card/footer/details-box icons (`personCheck`, `document`, `trashAlert`,
`leaf`, `cloudAlert`, `bellDot`, `troubleshoot`, `reminderBell`, `lightbulb`, `history`,
`fileUser`, `messageCircleWarning`, `windows`, `apple`, `signInAttempt`, `licenseFileKey`,
`cloudCheck`, `packageBox`, `graduationCap`, `bookOpen`, `users`), and `banner/dashboardBg.png`
(the one remaining banner variant that still needs a real image).

Four other banner variants turned out simple enough to build as real HTML instead of an image —
no asset needed at all:
- `gauge` (title + two fixed illustrative "47%"/"62%" rings + a gradient line, confirmed against
  Figma node 1551:23531 "Trial Welcome") — two nested-circle `<table>`s, same pattern
  `renderCard()`'s icon badge already uses. Building this as HTML is what fixed a real bug where
  the old fixed-size image + live-text overlay drifted apart on narrow mobile widths.
- `ring` (title + a single gradient ring peeking from the banner's bottom-right corner, no
  dot-grid — BeforeDeletion/DataDeletion) — `position:absolute` circle, right-anchored (not
  left-anchored) so it stays glued to the corner at any container width, clipped by
  `overflow:hidden` on the rounded-corner banner cell.
- `dots` (title over a dot-grid texture + a solid gradient circle in the corner —
  BeforeTrialExpiredExistingCustomer) — the dot-grid is a repeating CSS `radial-gradient()`
  background (16x6 dots at 32x26px spacing), same corner-circle technique as `ring`.
- `tagline` (large centered gradient-clip-text title over 6 overlapping rounded pills — 3 navy +
  1 gold/pink accent per row, TrialExpiredExistingCustomer/LicenseExpired) — each pill is an
  absolutely-positioned `<div>` with percent-based left/width (so the pattern scales with the
  fluid container) and a fixed px top/height, positioned/sized/colored from the exact rotated
  `<rect>` geometry in the real Figma export, in the same paint order as the source SVG so the
  overlaps composite identically. Verified pixel-exact against the source via DOM measurement,
  not just eyeballed — an earlier attempt at this exact conversion (hand-coded, not measured)
  wasn't accurate enough and was reverted back to an image; this one was built from real numbers.

All four rely on `overflow:hidden` clipping a table cell, which Outlook's Word engine doesn't
honor. For `ring`/`dots`/`tagline` that means the decorative shapes show unclipped past the
banner's edge on Outlook desktop specifically — everywhere else (Gmail, Apple Mail, Outlook.com,
etc.) renders correctly.

**Not used by any template yet**, kept so a future template can adopt one without re-exporting
from Figma:
- 9 more card icons, already 40x40-at-4x (160x160px) and pre-colored to match the
  pink/green/orange bg-pairing rule: `archiveX`, `badgePlus`, `bellRing`, `bookMarked`,
  `circleDotDashed`, `fileSpreadsheet`, `fileText`, `layoutGrid`, `tvMinimalPlay`.
- 3 more banner backgrounds (520x173-at-4x / 2080x692px), background-only: `diagonalRibbonsBg`,
  `twoBlobCornersBg`, `borderedBoxBg`, `concentricRingsBg`. These are the ones whose visual
  effect still can't be done reliably in table-based email HTML: `diagonalRibbonsBg` (rotated
  shapes), `twoBlobCornersBg` (two corner-peeking shapes, not just one — the single-circle case
  is now `ring`, built as HTML), `borderedBoxBg` (a *gradient*-colored border — `border-image`
  has no Outlook equivalent), `concentricRingsBg` (nested concentric circles, more fragile than
  the single-ring case `ring` already covers).

Deliberately left out of this repo entirely:
- Banner variants from the same Figma set with real *per-recipient* data baked in (a date range,
  TB/count numbers, a specific trial-length badge) — that content has to stay as live HTML/text
  if ever built, not a flattened image.
- Banner variants simple enough to just build as live HTML — solid/gradient bars, lines, and
  dots, the same pattern `renderBannerSkeleton()`'s gradient line and `renderUsageProgressBox()`
  already use elsewhere in `components.ts`. No image needed, ever: a "skeleton rows" banner
  (already built, `renderBannerSkeleton()`), a right-aligned-title + 3 gradient bars, a single
  partial-fill gradient progress bar, stacked words each with a gradient underline, and a
  vertical gradient accent bar + status dot.
