# Traceability public page

Static, framework-free page behind the invoice/batch QR codes
(`https://trace.kihera-farms.com/t/{token}`). See `docs/domains/traceability.md`.

## What it does
Reads the token from the path (`/t/{token}`, or `?t=` dev fallback), calls the
Supabase RPC `public.trace_disclosure(token)` with the **publishable/anon key**
(public by design — it ships in the mobile app), and renders exactly the §3
payload that function returns. All exposure is decided server-side; an unknown
or revoked token renders "Record not found".

## Deploy on Netlify
This directory **is** the publish root of the `kihera-trace` site
(`trace.kihera-farms.com`). So:
- `/index.html` and `/kihera-logo.png` are served at the site root, and
- the redirect in `_redirects` (`/t/* → /index.html`, 200) makes every token
  path serve the page, while real files (the logo) are served directly.

The QR (`documents.dart`), this redirect and the page's token parser all agree
on the **`/t/{token}` path shape**. No build step, no secrets: the only key here
is the public anon key.
