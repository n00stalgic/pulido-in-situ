# IN SITU

A working, anonymous art placement preview for Chas Pulido. Next.js, strict TypeScript, React, Zustand and native Canvas. The Pages preview is compiled from the same React application as a portable static file.

## Source

Decode source.tar.gz.b64 and unpack it to get the editable project:

```sh
base64 -d source.tar.gz.b64 > source.tar.gz
mkdir source && tar -xzf source.tar.gz -C source
cd source
```

## Run

Node 20.9+ is required.

```sh
npm ci
npm run dev
npm run typecheck
npm test
npm run build
```

## Implemented

- Editorial landing and responsive room editor.
- Authentic catalog snapshot from the existing public portfolio, with search, orientation and studio-listing filter. No prices or artwork years are shown.
- JPEG/PNG/WebP signature validation, 20 MB file limit, decode failure handling, pixel limit and editing-size normalization with orientation correction.
- Images processed in this tab only. No cloud upload, analytics or third-party AI.
- Canvas compositing, normalized placement, proportion-preserving corner resize, rotation, touch dragging, pinch zoom, room panning, keyboard/numeric controls, undo/redo.
- JPEG export at editing resolution with an approximate-scale notice. No claim of full-resolution original export.
- Remove/replace photograph and cleanup of temporary object URLs.

## Deliberately not enabled

Database, admin editing, cloud saving, collector inquiries, cross-device sessions, physical calibration, perspective correction, wall segmentation, occlusion and AI. The admin route denies access and exposes no protected records. This is the working core, not completion of every phase in the master brief.

## Artwork provenance and quality

163 catalog entries extracted from the current public pulido-preview repository on October 7, 2026. Dimensions, medium, titles and studio listings are imported, not invented. Availability is a catalog snapshot, not a live inventory reservation. Existing public watermarked images are reused. 24 records have a low-resolution flag. 31 older-watermark works load their existing inline image assets from the public portfolio document. Private clean masters are not published. A studio-approved higher-quality placement-asset pass is needed before final collector launch. Aspect ratios follow the actual source photographs, without cropping content.

## Privacy

Room photos are held in memory through local object URLs for this tab. They are not put in localStorage, transmitted or saved remotely. Reloading/closing the tab clears the session. Remove photograph clears this page's reference; this cannot delete files from the user's device. Exported images are intentionally saved to the user's device. The app has no runtime analytics. Loading artwork makes normal image requests to the existing public portfolio host.

## Configuration and later phases

Copy, limits and feature flags: src/config/product.ts. Tokens: src/app/globals.css. Catalog adapter: src/lib/catalog.json. Geometry and history: src/lib/geometry.ts and src/lib/store.ts. Upload pipeline: src/lib/image.ts. Supabase migration proposal lives in supabase/schema.sql and has not been applied. Do not put service-role credentials in this app. Enable a separately reviewed backend, authenticated admin and explicit consent before cloud storage or inquiries.

## Deployment

The preview index.html is a static build of src/standalone.tsx. It contains no private room image, secrets, private inventory notes or clean master files. The base64-encoded source archive contains the editable Next.js project and tests. GitHub Pages serves the root of main. No dependency on a custom domain is baked into the editor.

Rebuild the portable preview with npm run preview:build. For a standard Next.js static export under a repository path, set NEXT_PUBLIC_BASE_PATH=/pulido-in-situ before npm run build and deploy out/. A production server deployment is required before enabling APIs.

## Verification

TypeScript, normalized-coordinate unit tests, catalog provenance assertions and dependency audit pass. Browser checks cover desktop and mobile layouts, room upload, move, rotate, undo/redo, artwork switching, invalid uploads and JPEG download. Real-device Safari/touch and accessibility audit remain release checks.
