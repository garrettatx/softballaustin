---
issue: 13
status: backlog
author: Garrett
---

# Backlog: Latest News Section Redesign

**Created:** 2026-05-03
**Issue:** [garrettatx/softballaustin#13](https://github.com/garrettatx/softballaustin/issues/13)
**Related:** [Homepage Audit](audit-homepage-202605.md) (originally #10)

## Background

The homepage Announcement section currently holds a manually-edited bulleted list of 2-3 news items. It goes stale regularly (currently showing Feb 2026 items in May). The Register button and Player FAQs moved to the hero, leaving this section thin.

### How News Works at Softball Austin

- 1-3 news items per season, max
- Two seasons per year: Spring (Feb-Jun) and Fall (Aug-Nov)
- News pages are reused or removed each season — not an ever-growing blog archive
- News is only relevant to the current season
- No need for a traditional blog/posts system

### Maintainability Constraint

This site is maintained by volunteer board members, not professional web developers. Any solution must be:
- **Simple to update** — a volunteer who logs into WP 3-4 times a year should be able to figure it out without documentation
- **Fail-safe** — if nobody updates it for 6 months, the homepage shouldn't look broken or stale
- **Low complexity** — fewer moving parts = fewer things to break when plugins update or a new volunteer takes over

## Current Problems

1. **Manual homepage edits required** — someone has to edit the homepage block content when news changes
2. **Goes stale** — news items from months ago sit on the homepage indefinitely
3. **Section feels thin** — just a heading + 3 bullets in a wide layout

## Options

### Option A: Kadence Posts block (dynamic)

Replace the manual list with a Kadence Posts block filtered to a "News" category, showing 2-3 most recent published posts (title + date only).

**Seasonal rotation:** Unpublish old season's posts, publish new ones. Homepage updates automatically.

- **Pros:** Self-updating. No homepage editing needed. Volunteers just manage posts, which is the most familiar WP workflow.
- **Cons:** If all posts are unpublished between seasons, the section is empty. Requires news to be actual WP posts (they already are — /news/ exists).
- **Mitigation:** Keep one evergreen post published, or hide the section when empty.

### Option B: Seasonal banner / callout

Replace the news list with a single seasonal message:
- Registration: "Spring 2026 Registration is Open — [Register Now]"
- In-season: "Spring 2026 is Underway — [Schedule & Standings]"
- Off-season: "Fall 2026 Registration Opens [Month] — [Get Updates]"

- **Pros:** Always relevant. 3 edits per year. Impossible to look stale — the content is the current state of the league.
- **Cons:** Doesn't surface specific announcements.

### Option C: Hybrid — seasonal banner + news list

Seasonal callout on top, compact Kadence Posts block below.

- **Pros:** Seasonal context + specific updates.
- **Cons:** More to maintain. Section gets taller.

### Option D: Remove from homepage

The nav already links to /news/. Drop the section entirely.

- **Pros:** Simplest. Can't go stale if it doesn't exist.
- **Cons:** Loses the "this league is active" signal that matters for a community org.

### Option E: Keep position, make dynamic only

Leave the Announcement section where it is, swap manual list for a Kadence Posts block.

- **Pros:** Minimal layout change.
- **Cons:** Thin section. Same empty-state risk as Option A.

## Placement Options

If the section stays on the homepage, where should it go?

| Placement | Pros | Cons |
|-----------|------|------|
| **Current position** (below hero) | Visible early, familiar location | Feels thin without the CTA buttons |
| **Below Our Impact** | Pairs with social proof (stats) | Pushes history content down |
| **Inside Informational Block sidebar** | Consolidates content | Loses the 20th anniversary image |
| **Footer widget** | Always visible, never in the way | Easy to miss |

## Recommendation

**Option B** (seasonal banner) is the best fit for how this org operates and who maintains the site. Softball Austin doesn't produce frequent news — it has seasonal milestones. A banner that reflects the current season state is:
- More useful than a news feed that goes stale
- Easier for volunteers to maintain (change one line 3x/year vs. managing posts)
- Fail-safe — even if nobody updates it for a full season, "Spring 2026 is Underway" is less embarrassing than "Spring 2026 Info — Jan 24, 2026" showing in October

If the board wants to surface specific announcements alongside the banner, **Option C** adds a Kadence Posts block underneath. But start with B and add the posts block only if there's demand.
