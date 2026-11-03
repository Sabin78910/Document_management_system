# MASTER PROMPT

Source of truth for product scope. Every change must fit this document; if it does not, update this
document first in its own PR.

```
PRODUCT FEATURE PRIORITIES
(Dual Platform – Google Play Store + Web – November 2026)

Version 1.0 must include only (both Android and Web):
1. Merge PDF
2. Split / Extract Pages
3. Compress PDF
4. Document Scanner (Camera on Android – multipage, automatic edge detection, perspective correction, cleanup filters) + Image to PDF (both platforms)
5. Image to Text / OCR (strong Nepali + English) with searchable PDF output
6. Organize Pages (reorder, rotate, delete, insert)
7. Fill & Sign PDF
8. PDF to Image
9. Local / session job history
10. Privacy policy + privacy-first design (core tools never silently upload document content; offline-first on Android)

Do not implement in Version 1.0:
- PDF to Excel / PowerPoint
- Full free-form text editing of existing PDF body text
- AI chat / summarize / translate
- Cloud storage integrations
- Ads
- Third-party analytics that upload document content

Feature implementation order (for every tool):
1. File / camera / upload selection and validation
2. Background processing with clear progress indication
3. Output preview and native share / download
4. Core PDF or scan operation
5. Error handling with actionable messages
6. Automated + real-device / cross-browser tests
7. Accessibility + Nepali/English localization
8. Performance testing with 50 MB+ scanned PDFs
9. Privacy review (no silent uploads; original file never overwritten)

Platform-specific guidance:
- Android: Prioritize true offline capability and camera-scanner quality.
- Web: Prioritize fast drag-and-drop, responsive layout, and minimal upload of document content for core tools.

For every feature, document:
- Exact user problem solved
- Allowed inputs (noting platform differences)
- Produced outputs
- Behavior with corrupted, password-protected, scanned, or very large files
- Required permissions
- What data remains on-device or local
- Failure states and recovery paths
- How the user verifies success
```

## Roadmap

- **1.0 "Reliable & Private PDF Basics"**: the nine features above, simultaneous Play Store + Web launch,
  offline Android, responsive drag-and-drop Web, local (Android) / session (Web) history, privacy policy,
  50 MB+ scanned PDFs without freezing, accessibility, Nepali/English.
- **1.1 "Office Conversions & Polish"**: Word ↔ PDF, Excel to PDF, Protect / Unlock, Watermark,
  Extract Images, better OCR accuracy and more language packs.
- **2.0 "AI Document Assistant"**: Summarize, Translate, Chat-with-PDF, smart naming. Off by default,
  explicit consent, on-device wherever feasible.

Explicitly deferred: free-form body text editing, PDF to Excel/PowerPoint, advanced redaction,
large-scale batch processing, cloud storage integrations.

## Definition of professional

- A 50 MB scanned document does not freeze the device or browser.
- Failed operations explain what happened and offer a retry.
- Originals are never silently overwritten.
- OCR works on real Nepali and English documents, including imperfect photos.
- Users always know whether anything leaves the device. (Core tools: nothing does.)
- Every feature has tests, defined error states, and real-device / cross-browser validation.

## Acceptance criteria template (example: Compress PDF)

**User story.** As a user, I want to reduce a PDF's size so I can share it by email or WhatsApp without
making it unusable.

**Inputs.** A PDF; level Low / Medium / High. **Output.** A new PDF; original size, new size, % saved.

**Rules.** Never overwrite the original. Process in the background. No upload. Warn if the result is not
smaller or quality drops unacceptably.

**Edge cases.** Corrupted or password-protected PDF; already compressed or image-free PDF; 50–100 MB+
files; cancellation; low storage (mobile) or browser limits (web).

**Acceptance.** (1) Medium reduces a typical 10 MB scan. (2) Original unchanged. (3) Share/download
immediately. (4) Cancel stops work and discards temporary data. (5) Progress and final reduction are
shown. (6) No document content is logged or uploaded.

Apply the same structure to every feature: see `docs/FEATURES.md`.
