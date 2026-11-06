# Grok Workspace Archive — Pre-Import Verification

## Source artifact

- Filename: `grok-workspace(1).zip`
- Size: 7,133,508 bytes
- SHA-256: `c4ff5042369b039822a67af017ac714c188bb1ccd56f282bbfc13377954f9d6f`
- ZIP entries: 322 total (250 files and 72 directory entries)
- Uncompressed payload: 10,906,442 bytes

These values were measured from the archive available to the working environment on 2026-11-06. They identify the source artifact for the planned import.

## Screenshot/image inventory

The archive contains these image assets (the README should embed the two actual application preview screenshots; icon and Open Graph assets are supporting artwork):

- `.grok/favicon-128.png`
- `.vercel/output/static/__grok/icon-180.png`
- `.vercel/output/static/__grok/install/assets/homescreen/ob-ipad.png`
- `.vercel/output/static/__grok/install/assets/homescreen/ob-phone.png`
- `.vercel/output/static/og.jpg`
- `public/__grok/icon-180.png`
- `public/__grok/install/assets/homescreen/ob-ipad.png`
- `public/__grok/install/assets/homescreen/ob-phone.png`
- `public/og.jpg`
- `screenshots/app-builder-preview-mobile.png`
- `screenshots/app-builder-preview.png`

## Observed top-level areas

The ZIP listing includes `.grok/`, `.tanstack/`, `.vercel/`, `attachments/`, `public/`, `screenshots/`, `scripts/`, `server/`, and `src/`, plus root configuration and project files. This is an archive listing, not a code audit or runtime validation.

## Transfer and import status

The archive was readable and its ZIP directory was inventoried. **The archive has not yet been uploaded to GitHub or extracted into repository Git objects.** The PR's current branch contains planning/verification documentation only. A runner with access to the archive bytes and authenticated GitHub Git Data API upload capability must perform the actual extraction and import.

Do not interpret this pre-import inventory as proof that the repository contains these files. After import, compare the repository tree against a fresh extraction manifest and verify the two screenshot paths resolve in GitHub.
