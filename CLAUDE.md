# Private PDF
Privacy-first PDF tools (merge, split, compress, scan, OCR in Nepali + English, organize, fill & sign,
PDF to image) for Android (Google Play) and the web, from one codebase. Everything runs on the device.

## Stack
Vite · TypeScript (strict, no UI framework) · Vitest · pdf-lib · pdf.js · tesseract.js · Capacitor 7 (Android).

## Commands
- Install: `npm ci` (postinstall copies the OCR engine and language data into `public/ocr`)
- Dev: `npm run dev` · Build: `npm run build` (runs `tsc --noEmit` then `vite build`)
- Lint / types: `npm run typecheck` (there is no ESLint config; the type check is the lint step)
- Test: `npm test` · Browser smoke test after a build: `node e2e/smoke.mjs` (needs Playwright)
- Android: `npm run android:sync`, then `cd android && ./gradlew assembleDebug`
- CI (`.github/workflows/ci.yml`) runs typecheck, test, build, the smoke test and a debug APK build.

## Key folders
- `src/core/` pure logic, no DOM: one module per tool (merge, split, compress, scan, fillSign, ...). Unit tested.
- `src/platform/` browser and Android glue: pdf.js rendering, OCR worker, save/share, background jobs.
- `src/tools/` one screen per tool · `src/ui/` shared UI · `src/i18n.ts` English + Nepali strings.
- `tests/` Vitest unit tests · `e2e/smoke.mjs` real-browser test · `android/` Capacitor project.
- `MASTER_PROMPT.md` is the source of truth for scope; `docs/FEATURES.md` describes each tool.
- `store/`, `fastlane/metadata/` Play Store listing and data-safety form.

## Product rules
- A change must fit `MASTER_PROMPT.md`. If it does not, update that document first in its own PR.
- Core tools never upload document content. `e2e/smoke.mjs` checks that no request leaves the app; keep it passing.
- Keep the Content-Security-Policy in `index.html` strict; add a host only when a feature needs it.
- Every user-facing string goes through `src/i18n.ts` in both English and Nepali.

## Rules
- Never commit secrets, signing keys (`*.jks`, `*.keystore`), `.env` files or email addresses.
- Ask before any production or store change: Play Console or store listing, `store/`, `fastlane/`,
  release tags, `release.yml`, signing config or the Google client ID.
- Keep changes focused on the requested task. One issue per PR; branch from `main` and open a PR.
- Every behaviour change needs a test. Run typecheck, tests and build before finishing, and report what
  was run and what was not verified.
- `.claude/settings.json` lets routine build and test commands run without prompts. Pushing to `main`,
  force pushes, secret changes and fastlane are blocked.
