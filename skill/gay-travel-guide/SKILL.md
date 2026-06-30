---
name: gay-travel-guide
description: Use this skill whenever the user wants a gay nightlife/gay bar travel guide for a city (e.g. "오사카 게이바 가이드", "방콕 게이투어", "신주쿠 니초메 가이드"), or a PDF/summary covering gay bars, gay districts, LGBTQ+ nightlife plus surrounding food/cafe/liquor-shopping for a trip. Also trigger for updating/expanding/condensing an existing gay-travel PDF (adding bars, re-ranking top-5, adding liquor section, one-page summary). Also bundles general-purpose (non-gay) travel data — hotel shortlists by star rating near major cities, and country duty-free liquor/whisky guides with Korea's import allowance rules — so consult this skill for plain "[country] 면세 위스키" or "[city] 호텔 3성 4성 5성" questions too, even with no gay-travel angle. Covers fact-checking venues across platforms (never inventing unconfirmed ones), crowd-type categorization, lodging-to-district routing, and PDF generation.
license: Proprietary. LICENSE.txt has complete terms
---

# Gay Travel Guide Skill

Builds gay nightlife travel guides for any city: bar-by-bar research, categorized tables, route planning from lodging, and companion content (food/cafes near the district, liquor/duty-free shopping). Produces both a full detailed PDF and an optional condensed one-page summary.

## When to use this

Trigger on requests like:
- "오사카/도쿄/방콕/베를린 게이바 가이드 만들어줘"
- "도야마초/신주쿠니초메/실롬 게이투어 코스 짜줘"
- Requests to add a newly-mentioned bar to an existing guide, re-rank a "top 5", or verify a bar name against reviews
- Requests to add a liquor/whisky shopping section near a gay district
- Requests for a "one-page summary" version of an existing detailed guide

Do NOT trigger for: general (non-gay) nightlife guides, or simple one-off factual questions about a single bar that don't need a structured guide/PDF.

## Core principles (read before researching)

1. **Never invent venues.** If a name the user mentions (e.g. heard from Instagram, a friend, a screenshot) cannot be verified via web search, image OCR, or places_search, say so explicitly. Do not guess at addresses, hours, or vibe. Bar identity, opening hours, and addresses change constantly — closures within 6 months are common in dense gay districts, so treat anything not freshly verified with appropriate caution.
2. **Cross-reference multiple sources.** Use a mix of: dedicated gay-travel guide sites (TravelGay, NomadiBoys, GayCities, VisitGay[City], SushiSandwich-style blogger reviews), Google Maps / places_search (ratings + actual review text), local-language blogs (search in the destination's local language, not just English/Korean — local sites often have more current/accurate venue info), and X/Instagram only as a last resort (note that private/locked accounts can't be verified and should be flagged as such).
3. **Categorize by crowd type, not just alphabetically.** Standard categories that work across cities: Tourist/foreigner-friendly (first-timer recommendations), Bear/mature/leather, Local/all-gender/dining-bar hybrids, and a language-specific category when relevant (e.g. "Korean-operated / Korean-speaking" for Korean travelers, similarly "English-speaking owner" can matter for others). Always note: foreign-friendliness rating, language ability of staff, typical crowd age/type, cover charge or drink minimum, and weekly closed days (these vary a lot and trip-planning depends on them).
4. **Always include operational details that affect trip planning**: closed days (many bars close 1-2 weekdays), hours (especially late-night cutoff vs all-night), cash-only vs cashless-only payment systems, and physical distance/walk-time from the user's lodging.
5. **Route from lodging, not from an abstract city center.** Once the user's hotel/lodging is known, compute walk-time and transit-time from there to the gay district, and from there to companion content (cafes, restaurants, liquor shops). If lodging changes, the route section needs updating too — don't leave stale lodging-specific content in a "summary" version once the user says the hotel decision is finalized (drop hotel-comparison content from summary docs once decided, but keep route/distance info since it's still needed for navigation).

## Research workflow

1. **Identify the gay district** for the destination city (e.g. Doyamacho for Osaka, Shinjuku Ni-chōme for Tokyo, Silom/Soi Twilight for Bangkok, Schöneberg for Berlin). Web search "[city] gay district / gay town" to confirm if unfamiliar.
2. **Pull a candidate list** of bars via web_search (multiple queries: English guide sites, local-language search, "[district] gay bar guide [current year]") and places_search (search the district's coordinates for "gay bar"). Cross-check names appearing across 2+ independent sources before treating them as established/reliable.
3. **For each bar, gather**: vibe/crowd type, foreign-friendliness + language, rating + review count (note when review count is very low — under ~20 — as a reliability caveat), cost structure (cover charge, drink minimum, cashless-only, etc.), hours + closed days, address/distance from lodging, and any standout takeaways from actual review text (quote-paraphrase, don't fabricate quotes).
4. **When the user mentions a bar by name that you can't verify** (e.g. from a screenshot of an Instagram bio, a friend's recommendation): search it directly by name + district + "gay bar", try places_search with the name, and if still unconfirmed, tell the user plainly that it couldn't be verified rather than presenting fabricated details as fact. Offer to keep searching if they have more identifying details (exact spelling, building name, screenshot of a review).
5. **For companion content** (food, cafes, liquor shops): bias toward places along the actual walking route between lodging and the gay district, or within the district itself, so they fit naturally into an evening's itinerary. Use places_search with a location_bias centered on the district/lodging.
6. **For liquor/whisky shopping requests**: research realistic local pricing and availability (e.g. premium/rare items may be scarce even in their country of origin — check current scarcity/markup, don't assume duty-free=available), check the destination country's duty-free import allowance for the traveler's home country (don't assume a number — verify, since both volume caps and value caps may apply, and rules change), and propose 2-4 concrete combinations that fit within that allowance with real per-bottle prices.

## Output structure

### Full detailed PDF (when the trip needs ongoing reference, or the user is actively iterating with multiple update requests)

Use the `pdf` skill's reportlab patterns. Structure as numbered sections, e.g.:
1. Lodging/route summary (only while lodging is undecided — remove once finalized, see below)
2. Transit/access to the gay district (walk + transit routes, weather-dependent alternatives)
3. Bar tables by category (tourist-friendly / bear / local-dining / language-specific)
4. Recommended bar-hopping route(s) by goal (first-timer, bear-focused, food+drink combo, language-comfort combo)
5. Companion content: food/cafe route near the district
6. Day-trip options from the lodging city, if relevant
7. Liquor/shopping section, if relevant
8. Budget guide
9. Final ranked recommendation (e.g. "top 5 if you can only pick 5") — keep this updated as new bars get verified and added

Table formatting: keep fonts small (7.5-8pt) and column widths carefully summed to the content width to avoid overflow/clipping — this is a common failure mode with many-column bar-comparison tables. Use a distinct header color per category for visual scanning. For non-English destination content (Korean, Japanese, etc.) make sure the right font is registered (see `pdf` skill / `docx` skill guidance on CJK fonts — Noto Sans CJK or NanumGothic work for Korean).

### One-page summary PDF (when the user asks for a condensed/quick-reference version)

Strip down to only the decision-relevant tables: top-5 ranked bars with one-line reasons, route (walk/transit time only, no narrative), 2-3 liquor combos if relevant, 3-5 companion food spots, day-trip options table if relevant, and a compact budget table. No long prose explanations — table rows only, one page if possible. Explicitly drop any content that's been superseded (e.g. hotel comparison once a hotel is booked) — re-confirm with the user what's now settled vs. still open before condensing, since carrying forward stale "still deciding" content into a "final" summary is a common mistake.

## Reusable pattern note

This skill's table/section helper functions (sec(), sub(), tbl()/make_table(), box()) and CJK font setup are good candidates to keep as a small bundled script (e.g. `scripts/pdf_helpers.py`) once this skill is used across 2+ cities — at that point, factor out the boilerplate so each new city only needs new content, not re-derived formatting code. Until then, inline reportlab code following the `pdf` skill's patterns is fine.

## Bundled reference: city hotel shortlist

`references/city-hotels.md` contains a pre-researched 3★/4★/5★ hotel shortlist (near the main gay district) for cities already covered in past sessions: Osaka, Tokyo, Fukuoka, Taipei, Bangkok, Shanghai, Chengdu. Check this file first when the user asks about one of these cities — it saves a research pass. Still spot-check current rating/pricing if the info will be presented as current fact, since hotels close/rebrand. When a request covers a new city not in this list, do the research fresh following the "Notes for future cities" section at the bottom of that file, and add the new city's row to the table so it's available next time.

## Bundled reference: country liquor & duty-free guide

`references/country-liquor-guide.md` contains general-purpose (not gay-tour-specific) duty-free shopping content: Korea's current import allowance rules (2L/$400 combined cap, no bottle-count limit as of March 2025), and country-by-country signature spirits with realistic pricing for Japan, Taiwan, Thailand, and China. This section is useful as a standalone companion to *any* travel guide this skill produces — not just gay-travel content — since duty-free shopping interests most travelers regardless of trip purpose. Include it when the user asks about liquor/duty-free/shopping for a covered country, or proactively offer it as an add-on section when producing a full city guide PDF. Same caveat as the hotel file: verify current pricing before presenting as fact, since duty-free pricing and stock fluctuate.

