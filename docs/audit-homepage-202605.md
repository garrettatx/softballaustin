# Softball Austin — Homepage Audit

**Date:** 2026-05-03
**Page:** softballaustin.org (front page, ID 221)
**Theme:** Kadence + Kadence Blocks Pro
**Layout:** Fullwidth, unboxed, title hidden, no sidebar

---

## Visible Page Structure

8 sections top-to-bottom. One hidden section (Texas Hoedown) is excluded — it's intentionally hidden for seasonal reuse.

| # | Section Name | Layout | What Visitors See |
|---|-------------|--------|-------------------|
| 1 | Welcome + Photo | 2-col (55/45), wave divider below | H1 "Welcome To Softball Austin!", intro paragraph, group photo with overlapping white card containing secondary copy |
| 2 | Announcement | 2-col | "Latest News" bulleted list + Registration button + Player FAQs link |
| 3 | Our Impact | 2-col (55/45) | Stats (400+ Players, 25+ Teams, Countless Memories/Friendships) + team photo |
| 4 | Informational Block | 2-col left-golden | 6 paragraphs of league history, affiliations, Austin Chronicle award + 20th anniversary image |
| 5 | 1-2-3 Highlights | 3-col full-width | Numbered infoboxes: Connections, Inclusive Environment, Skill-Matched Leagues |
| 6 | Join a Community | 2-col with bg image | Kicker + H2 "Join a Community Built on Passion for Softball" + subhead |
| 7 | Photo Gallery | 1-col full-width | 29-image Kadence Advanced Gallery with magnific lightbox |
| 8 | *(empty paragraph)* | — | Orphan `<p></p>` block adding bottom whitespace |

**Hidden (not scored):** Texas Hoedown (3-col, hidden desktop/tablet/mobile) — seasonal block, will be reused.

---

## Findings

### P0 — Fix Now

These are bugs, typos, or SEO gaps that hurt credibility or search visibility today.

| # | Category | Issue | Fix |
|---|----------|-------|-----|
| 1 | **Typo** | "ALlies" in Join a Community kicker | Change `Softball for LGBTQ+ Community & ALlies` → `Softball for LGBTQ+ Community & Allies` |
| 2 | **Artifact** | Starter template link in welcome paragraph | Empty `<a href="https://startertemplatecloud.com/e28/#"></a>` inside the intro text. Invisible but present in source. Delete it. |
| 3 | **Orphan block** | Empty `<p></p>` at page bottom | Delete — adds unnecessary whitespace after gallery |
| 4 | **SEO** | No meta description | RankMath description is blank. Write one targeting "LGBTQ softball league Austin" (155 chars max) |
| 5 | **SEO** | No focus keyword | RankMath focus keyword is empty. Set it so on-page scoring activates. |
| 6 | **SEO** | No Open Graph image or title | Social shares pull unpredictable content. Set an OG image (team photo or logo) + title. |

### P1 — High Impact Polish

Visible design and content issues that weaken first impressions or conversion.

| # | Category | Issue | Recommendation |
|---|----------|-------|---------------|
| 7 | **Design** | No CTA in the hero section | The welcome section has no button. Registration is buried in section 2 below the fold. Add a prominent "Join the League" or "Register Now" button in the hero. |
| 8 | **Content** | Welcome card copy is generic | "Whether you're a pro or just starting out, we're thrilled to have you on board! We can't wait to make memories together." — sounds like a template. Rewrite with specifics: mention Spring/Fall seasons, Open + Women's divisions, Krieg Fields. |
| 9 | **Content** | "Countless Memories, Friendships" is a weak stat | The first two stats (400+ Players, 25+ Teams) are concrete. The third is filler. Replace with something real: "20+ Years" (est. 2004), "6 Tournament Teams," or the Austin Chronicle award year. |
| 10 | **Content** | Stats conflict: "400+" vs. "over 450 members" | Impact section says "400+ Players," informational block says "over 450 members." Pick one number, use it consistently. |
| 11 | **Content** | "Latest News" items are months old | "Spring 2026 Schedule - Feb 12, 2026" and "Spring 2026 Info - Jan 24, 2026" — it's May. If news is manual, update it. Better: use a dynamic recent posts block so it stays current. |
| 12 | **Layout** | 1-2-3 Highlights: 160px number icons | The numbers are oversized and dominate the section. The actual value props (titles + descriptions) get lost below massive numerals. Scale to 60-80px. |
| 13 | **Design** | Join a Community section has no CTA button | H2 heading + subhead but nothing to click. Add a "Register Now" or "Learn More" button. |
| 14 | **Typography** | H1 tablet size (80px) is *larger* than desktop (70px) | Font sizes are set to `[70, 80, 40]` (desktop, tablet, mobile). The tablet value should be smaller than desktop — likely should be ~56px. |

### P2 — Design & UX Improvements

Lower urgency but would meaningfully improve the page.

| # | Category | Issue | Recommendation |
|---|----------|-------|---------------|
| 15 | **Content** | Informational block is a text wall | 6 consecutive paragraphs, no subheadings, no bold lead-ins, no visual breaks. Break into 2-3 subsections or add formatting to improve scannability. |
| 16 | **Image SEO** | Gallery images missing alt text | 29 images in the gallery; many (especially Facebook imports with numeric filenames) have no alt text. Accessibility + image search issue. |
| 17 | **Design** | Wave divider used only once | The `crvi` curve separator after the hero creates visual interest but isn't used elsewhere. Either carry the design language through (at least one more section break) or remove for consistency. |
| 18 | **Performance** | 29 gallery images on homepage | Even with lazy loading, this is heavy for a homepage. Consider limiting to 8-12 featured images + "View All Photos" link. |
| 19 | **Navigation** | "Documents" menu item has empty URL | It's a dropdown parent label with `url: ""`. Works as a hover trigger but can confuse keyboard/screen reader navigation. Consider linking it to a documents landing page or adding `#` with aria attributes. |
| 20 | **SEO** | No Organization schema | RankMath schema for Organization is empty. Set up with logo, social profiles, PO Box address. |

### P3 — Nice-to-Have

| # | Category | Issue | Recommendation |
|---|----------|-------|---------------|
| 21 | **Content** | Austin Chronicle award is from 2016 | 10 years ago. Still worth mentioning, but frame it clearly: "Named 2016 Best LGBTQ Sports League by the Austin Chronicle" is already there — just make sure it doesn't read as the league's most recent accomplishment. |
| 22 | **Design** | Footer uses 3 rows x 6 widget areas | May be over-engineered. Audit footer for redundancy. |
| 23 | **Navigation** | LeagueApps menu (Sign In, Create Account, Forgot Password) | Separate menu exists — verify it's actually displayed. If it's only used in the account drawer, that's fine. If orphaned, clean up. |
| 24 | **Gallery** | Facebook-imported filenames | `485705232_1244758860829773_...n.jpg` — not a visitor-facing issue, but makes the media library harder to manage. Consider renaming if you're ever doing a media cleanup pass. |

---

## Quick Wins — COMPLETED 2026-05-03

All P0 items fixed via MCP and verified. GitHub issues #1-6 closed.

| # | Issue | Fix Applied |
|---|-------|-------------|
| #1 | "ALlies" typo | Changed to "Allies" |
| #2 | Starter template artifact | Removed empty `<a>` tag |
| #3 | Orphan empty paragraph | Deleted from page bottom |
| #4 | Meta description + keyword | Set RankMath description (165 chars) + focus keyword "LGBTQ softball league Austin" |
| #5 | OG image + title | Set to 20th anniversary branded graphic (ID 274) + custom title |
| #6 | H1 tablet font size | Changed from `[70,80,40]` to `[70,56,40]` |

---

### Issue #7 — COMPLETED 2026-05-03

Moved Register button + Player FAQs text link from Announcement section into the hero (Welcome + Photo, column 1). Button text changed from "Registration" to "Register." Nav menu "League Registration" left as-is. Hoedown section untouched.

---

### Issues #8, #9, #11 — COMPLETED 2026-05-03

- **#8** Welcome card rewritten: "We play Spring and Fall seasons at Krieg Fields — Open and Women's divisions, all skill levels. Come for the softball, stay for the community."
- **#9** Stats updated to: 20+ Years | 400+ Members | 25+ Teams. Fixed "over 450" → "over 400" in informational block.
- **#11** Number icons scaled 160px → 70px. Copy rewritten: Find Your Team, More Than a League, All Are Welcome.

---

## Recommended Next Steps

1. **Redesign announcement section** — see [backlog-latest-news-redesign.md](backlog-latest-news-redesign.md) (#13, supersedes #10)
2. **Add CTA to Join a Community section** (#12)
3. **Gallery alt text pass** — batch update in media library (#16)
