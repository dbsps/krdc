# Deploying krisroadruck.com on Cloudflare

## The plan
- Canonical: https://www.krisroadruck.com/ (matches existing indexed URLs)
- roadruck.com: 301s everything to the canonical, path-preserving
- Email: unaffected. MX records stay exactly as they are on both zones.

## Steps
1. Cloudflare dashboard -> Workers & Pages -> Create -> Pages.
   Either connect a git repo containing this folder, or use direct upload:
   `npx wrangler pages deploy . --project-name=krisroadruck`
2. Custom domains on the Pages project: add www.krisroadruck.com
   (and krisroadruck.com if you want Pages to own the apex).
3. Apex -> www: Rules -> Redirect Rules on the krisroadruck.com zone:
   - When: Hostname equals krisroadruck.com
   - Then: Dynamic redirect, 301, expression:
     concat("https://www.krisroadruck.com", http.request.uri.path)
4. roadruck.com -> canonical: Redirect Rule on the roadruck.com zone:
   - When: Hostname contains roadruck.com (or All incoming requests)
   - Then: Dynamic redirect, 301, expression:
     concat("https://www.krisroadruck.com", http.request.uri.path)
   Requires a proxied DNS record on roadruck.com (A record to 192.0.2.1,
   proxied, is the standard placeholder) so the rule has traffic to act on.
5. Verify: curl -I https://roadruck.com/anything -> 301 ->
   https://www.krisroadruck.com/anything
6. When the flat-file blog lands, add path mappings to _redirects so old
   WordPress URLs resolve. Do NOT let /blog/* 404 — that equity is 17 years old.

## What's in this folder
- index.html — the site, self-hosted fonts, canonical + OG pointing at
  www.krisroadruck.com
- fonts/ — six woff2 subsets (~69KB total), no third-party font requests
- _headers — security + cache headers
- _redirects — path-level redirects (host-level ones live in dashboard rules)
- favicons, webmanifest, og-image
