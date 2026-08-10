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
- `email-icons/banner/future/` — 16 additional 520x173-at-4x (2080x692px) banner background
  variants from the same Figma banner component set as the 3 live ones. Most have
  "Discover. Evaluate. Succeed." baked into the image; a few (`weeklyReportHeaderBg`,
  `trialSplitBadgeBg`, `areaChartStatsBg`, `statCardsProgressBg`) don't and look like
  sub-components for a different (e.g. usage-report) email family rather than the trial-welcome
  banner. `skeletonRowsBg` is a flat-image version of what `renderBannerSkeleton()` already
  builds live in HTML/table code — kept here as a fallback, not currently used.
