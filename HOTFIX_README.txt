BG-W13 — STORMKIT INDEXABILITY HOTFIX
=====================================

Problem confirmed by live curl:
- HTML meta robots is index,follow
- HTTP response still returns: X-Robots-Tag: noindex

Cause:
The previous production _headers file used "/" for the root rule.
Stormkit custom-header syntax uses URL patterns such as "/*".
Therefore the production override did not replace Stormkit's default noindex header.

Fix in this package:
_headers

/*
  X-Robots-Tag: all
  X-Content-Type-Options: nosniff
  Referrer-Policy: strict-origin-when-cross-origin

Stormkit's own documentation states user-configured headers take precedence
over Stormkit defaults.

Deploy/publish this package, then verify:

curl.exe -I https://robindandelion-rwuere.stormkit.dev/

PASS condition:
- HTTP 200
- X-Robots-Tag must NOT contain "noindex"
- preferred result: X-Robots-Tag: all

The page's HTML meta robots remains:
index,follow,max-image-preview:large,max-snippet:-1,max-video-preview:-1

Do not submit sitemap/request indexing until the header noindex is gone.
