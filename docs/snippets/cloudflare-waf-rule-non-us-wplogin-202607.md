# Cloudflare WAF: harden "Block Non-US WP/MX Account Setup" rule

**Date:** 2026-07-22
**Context:** Reviewed alongside the `.env`/secrets-scanner rule hardening ([[cloudflare-waf-rule-secrets-scanner-202607]]) after the GridPane CPU-spike incident (ticket #712295). Same underlying issue: case-sensitive matching that a scanner randomizing case can walk past.

## Where

Cloudflare dashboard → softballaustin.org → **Security → Security rules → Custom rules → "Block Non-US WP/MX Account Setup"** → Edit.

## Current expression

```
(ip.src.country ne "US" and ip.src.country ne "MX" and http.request.uri.path eq "/wp-login.php")
```

## The gap

`http.request.uri.path eq "/wp-login.php"` is case-sensitive. A request to `/WP-LOGIN.PHP` or `/Wp-Login.php` skips this rule entirely, even though it's functionally the same request WordPress would happily serve.

## Hardened expression

```
(ip.src.country ne "US" and ip.src.country ne "MX" and lower(http.request.uri.path) eq "/wp-login.php")
```

Action: Block (unchanged).

## Why `eq`, not `contains`

Considered loosening the path match to `contains "wp-login.php"` to also catch trailing-slash variants (`/wp-login.php/`). Rejected for this rule specifically because it's a **Block** action: `contains` matches on substring, not path boundary, so it would also fire on any hypothetical path that merely contains that string (e.g. `/wp-login.phpxyz`). For a Block action, a false match means a full lockout with no recourse — so keep the match exact. This tradeoff is different for Log/Challenge actions, where looser matching costs nothing worse than a solvable challenge.

## Known blind spot (not fixed by this change)

This rule blocks by **country**, not by hosting provider. An attacker renting a US-region AWS/DigitalOcean/Azure box to hit `wp-login.php` sails through untouched, since their IP resolves to "US" regardless of who actually operates it. Country and infrastructure ownership aren't the same thing. If this needs to be resilient against more than the specific attacker seen in this incident, pair it with an ASN-based check (same approach as the `AS211590` block discussed for the secrets-scanner rule) — country blocking alone has a ceiling.

## Optional style cleanup (no functional change)

```
(not ip.src.country in {"US" "MX"} and lower(http.request.uri.path) eq "/wp-login.php")
```

Reads cleaner than two `ne` conditions ANDed together, and is easier to extend if a third country ever needs allowlisting.
