# Cloudflare WAF: bot allowlist + expanded country block

**Date:** 2026-07-22
**Context:** Site is a US/North American softball league local to Austin, TX — essentially zero legitimate traffic from outside the US. Reviewed alongside [[cloudflare-waf-rule-secrets-scanner-202607]] and [[cloudflare-waf-rule-non-us-wplogin-202607]] after the GridPane CPU-spike incident (ticket #712295, attacker hosted in France under AS211590).

Decision: keep the denylist model (not a full US-only allowlist) but (a) add a verified-bot exception ahead of it so search crawlers can't get caught in the net, and (b) expand the risky-country list with FR — directly evidenced by this incident — plus an optional secondary tier of commonly-flagged countries.

## Rule A (new) — Allow verified bots, run first

**Order: 1** (must run before the country block, so it needs to sit above the existing rules — this pushes the current Rules 1–3 down to 2–4).

```
(cf.client.bot) or (cf.verified_bot_category in {"Search Engine Crawler" "SEO"})
```

Action: **Allow** (stops further rule processing for the request — this is what makes it an exception, not just another check).

**Why this matters now, not later:** AhrefsBot and DotBot both got hit with a 403 on `/robots.txt` during the exact window covered in this incident's access log — meaning the current 3 rules (or Cloudflare's underlying bot handling) are already clipping legitimate SEO crawlers with zero exception in place. Widening the country block without this rule first would make that worse, and could eventually catch Googlebot/Bingbot if they ever crawl from a non-US datacenter.

## Rule B — expanded "Block High Risk Countries"

Existing expression (uses `ip.geoip.country`, a field not found in Cloudflare's current documented field reference — normalizing to the documented `ip.src.country` below; functionally should be equivalent but this removes reliance on an unconfirmed/legacy field):

```
ip.geoip.country in {"BD" "BY" "CN" "ID" "IN" "KP" "KZ" "PK" "PH" "RU" "SG" "UA" "UZ" "VN"}
```

Updated expression — adds `FR`, the country this incident's attacker (AS211590) actually operated from:

```
ip.src.country in {"BD" "BY" "CN" "ID" "IN" "KP" "KZ" "PK" "PH" "RU" "SG" "UA" "UZ" "VN" "FR"}
```

Action: Block (unchanged).

### Optional secondary tier

These aren't evidenced in your own traffic — they're countries commonly flagged in general threat-intel/hosting-abuse lists (bulletproof hosting concentration, high scanning volume). Add only if you're comfortable blocking on reputation rather than direct evidence:

```
"NL" "RO" "BG" "IR" "NG" "MD" "HK" "PA" "BZ" "SC"
```

`NL` (Netherlands) is worth a second look before including — it's also a legitimate hosting/CDN/business hub, so it carries more collateral-block risk than the others in this tier for a Block (not Challenge) action.

## Resulting rule order

1. Allow Verified Bots *(new)*
2. Block High Risk Countries *(expanded, field normalized)*
3. Block Non-US WP/MX Account Setup
4. Block Common Path Scanners

Uses your 4th of 5 available custom rule slots. One slot remains — previously discussed as a candidate for an `AS211590` ASN block, still available if wanted.
