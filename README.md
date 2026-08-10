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
`cloudCheck`, `packageBox`, `graduationCap`, `bookOpen`, `users`), and `banner/dashboardBg.png` /
`banner/bannerBg.png` / `banner/taglineBg.png` / `banner/ringBg.png` (4 of the 5 banner variants
— everything except `gauge`).

Only `gauge` (title + two fixed illustrative "47%"/"62%" rings + a gradient line, confirmed
against Figma node 1551:23531 "Trial Welcome") is real HTML instead of an image — two
nested-circle `<table>`s, same pattern `renderCard()`'s icon badge already uses. No asset needed.
Building this as HTML is what fixed a real bug where the old fixed-size image + live-text overlay
drifted apart on narrow mobile widths.

`tagline`/`dots`/`ring` (the pills / dot-grid+circle / single-ring corner-peeking variants) were
each rebuilt as real HTML **twice**, verified pixel-exact via Playwright both times, and broke on
a real Gmail send both times — this isn't an Outlook-only edge case, it's Gmail itself:
1. `position:absolute` shapes + `overflow:hidden` clipping — Gmail strips the `position` property
   entirely, flattening every absolutely-positioned element back into normal block flow (the 6
   tagline pills rendered as plain stacked bars, not overlapping at all).
2. `float:right` + negative margins + `overflow:hidden` (no `position` anywhere, specifically to
   dodge the first failure) — Gmail *also* zeroes out negative margin values, so the float just
   settled at its natural resting spot: a complete, unclipped circle sitting inside the box, no
   corner-peeking effect.

Common thread: Gmail blocks every CSS mechanism that lets content escape its own box, so a shape
that genuinely bleeds past a rounded corner isn't achievable there without an image. All three
are images again, for good — don't retry this without a genuinely new mechanism, not a variation
on "let an element escape its box."

**Not used by any template yet**, kept so a future template can adopt one without re-exporting
from Figma:
- 9 more card icons, already 40x40-at-4x (160x160px) and pre-colored to match the
  pink/green/orange bg-pairing rule: `archiveX`, `badgePlus`, `bellRing`, `bookMarked`,
  `circleDotDashed`, `fileSpreadsheet`, `fileText`, `layoutGrid`, `tvMinimalPlay`.
- 4 more banner backgrounds (520x173-at-4x / 2080x692px), background-only: `diagonalRibbonsBg`,
  `twoBlobCornersBg`, `borderedBoxBg`, `concentricRingsBg`. Same reasoning as `ring`/`dots`/
  `tagline` above — anything that needs a shape to overlap or bleed past its own box isn't
  achievable in real email HTML (Gmail specifically), so these stay images: `diagonalRibbonsBg`
  (rotated shapes), `twoBlobCornersBg` (two corner-peeking shapes), `borderedBoxBg` (a
  *gradient*-colored border — `border-image` has no email-client equivalent at all),
  `concentricRingsBg` (nested concentric circles).

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
