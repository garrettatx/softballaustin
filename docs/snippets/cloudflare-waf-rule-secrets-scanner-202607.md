# Cloudflare WAF: extend "Block Common Path Scanners" rule

**Date:** 2026-07-22
**Trigger:** GridPane ticket #712295 — CPU load spike from 4 IPs (185.177.72.12/22/24/53, all in AS211590 / FR-FBW-NETWORKS) mass-probing `www.softballaustin.org` for `.env` files, SSH/AWS keys, DB dumps, and Terraform state. GridPane null-routed the 4 IPs at the nginx level; this change adds Cloudflare-side coverage so the same class of scan is blocked at the edge next time, regardless of source IP.

## Where

Cloudflare dashboard → softballaustin.org → **Security → Security rules → Custom rules → "Block Common Path Scanners"** → Edit.

## Why the existing rule didn't catch this

The current expression only checks `.bak`, `.old`, `.sql`, `.ini`, `.log`, `/.git`, `/backup`, `/wp-config` — none of which match `.env`, which was ~90% of this scanner's traffic. It also uses a plain (case-sensitive) `contains`, and this scanner deliberately randomizes case to dodge exactly that — e.g. `/aPp/%2eEnV`, `/%2eEnV%2eStAgInG`, `/%2eEnV%2eSaVe`.

## New expression (replace the existing one)

Verified against the site's live sitemap (22 real page paths) and the real `/members/`, `/ajax/loadSchedule`, `/login` traffic seen in the access log, plus the full active plugin list (Akismet, CheckView, Duplicate Page, EWWW Image Optimizer, Formidable Forms, Kadence Blocks/Conversions/Custom Fonts/Theme Kit Pro, Nginx Helper, Novamira, Rank Math SEO + PRO, Simple CAPTCHA/Turnstile, Simple History, WP fail2ban) — zero overlap with any term below.

```
(lower(http.request.uri.path) contains ".bak")
or (lower(http.request.uri.path) contains ".old")
or (lower(http.request.uri.path) contains ".sql")
or (lower(http.request.uri.path) contains ".ini")
or (lower(http.request.uri.path) contains ".log")
or (lower(http.request.uri.path) contains "/.git")
or (lower(http.request.uri.path) contains "/backup")
or (lower(http.request.uri.path) contains "wp-config")
or (lower(http.request.uri.path) contains ".env")
or (lower(http.request.uri.path) contains "credentials")
or (lower(http.request.uri.path) contains "id_rsa")
or (lower(http.request.uri.path) contains "id_dsa")
or (lower(http.request.uri.path) contains "id_ecdsa")
or (lower(http.request.uri.path) contains "id_ed25519")
or (lower(http.request.uri.path) contains ".pem")
or (lower(http.request.uri.path) contains ".aws")
or (lower(http.request.uri.path) contains ".ssh")
or (lower(http.request.uri.path) contains "phpinfo")
or (lower(http.request.uri.path) contains "actuator")
or (lower(http.request.uri.path) contains "terraform")
or (lower(http.request.uri.path) contains "token")
or (lower(http.request.uri.path) contains "api-key")
or (lower(http.request.uri.path) contains "api_key")
or (lower(http.request.uri.path) contains ".yml")
or (lower(http.request.uri.path) contains ".yaml")
```

Action: Block (unchanged).

Deliberately left out: bare `password` and `secret` (didn't appear in this attack, and are the two words most likely to collide with a real password-reset/membership-portal path on a system outside the WP install's visibility), and bare `.json`/`.xml` (too broad — the site's own sitemap is `.xml` and REST/ajax responses are `.json`).

## After deploying

Check the rule's Events count over the next few days — it should show hits if another scan hits the same patterns. Compare against the 3 events logged during this incident to confirm the new terms are actually matching.
