# IN SITU - build and operations notes

## What is deployed

`index.html` at the repo root is a self-contained build of the full app
(bundle + styles inlined), published through GitHub Pages. It is produced by:

    npm install
    npm run preview:build   # writes preview.html
    cp preview.html index.html

The real source lives in `src/` (Next.js 16 + React 19 + Zustand, strict
TypeScript, native Canvas). `source.tar.gz.b64` holds the byte-exact source
archive; unpack with:

    base64 -d source.tar.gz.b64 > in-situ-source.tar.gz
    tar -xzf in-situ-source.tar.gz

`npm run dev` runs the Next app; `npm test` runs unit tests;
`npm run typecheck` must stay clean.

## Backend (Supabase, project "Pulido Art")

- Client talks to Supabase over plain REST with the anon public key in
  `src/config/product.ts`. The anon key is public by design; all access is
  enforced by row level security in `supabase/insitu.sql`.
- The SQL block creates schema `insitu` only: artworks, inquiries,
  studio_admins, RLS policies, indexes, the private
  `insitu-visualizations` storage bucket. API schema exposure is configured separately.
- Studio admins are matched by sign-in email against
  `insitu.studio_admins` (case-insensitive JWT email match). Only emails in
  that table are admins. Seed after creating the studio user:

      insert into insitu.studio_admins (email) values ('STUDIO-EMAIL-HERE') on conflict do nothing;

- If REST calls return "Invalid schema: insitu" after running the block,
  add `insitu` under Project Settings > Data API > Exposed schemas,
  preserve the existing entries, then Save.

## Privacy rules the app keeps

- Room photos never leave the device unless the collector checks the
  visualization consent box on the inquiry form.
- Share links contain only artwork id and placement. Never a photo, never
  personal data.
- Inquiries never show prices or availability promises. The studio replies
  set expectations, not the app.
- No service-role key anywhere in the app, repo, or messages.

## Status

- Core placement editor, export, share, inquiry flow, admin shell: live.
- Inquiry delivery verified against the live project (HTTP 201).
- Admin sign-in gate and invalid-session denial verified. The account owner
  must verify her authenticated dashboard, status changes and signed photo links.
- Email notifications remain off pending a sending identity decision.
- Not built yet: measurement calibration, perspective correction, cloud
  sessions, comparison mode, artwork inventory editing UI, email
  notifications (needs a sending identity decision).
