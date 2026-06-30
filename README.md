# Kay-Tour

A reusable Claude Skill for building gay nightlife travel guides (bars, food, cafes, liquor shopping, day trips) for any city — plus the Osaka guide produced with it as a worked example.

## Contents

```
skill/gay-travel-guide/         — the Claude Skill itself (install this in Claude)
├── SKILL.md                    — trigger conditions, research methodology, output structure
└── references/
    ├── city-hotels.md          — pre-researched 3★/4★/5★ hotel shortlists near major gay districts
    │                              (Osaka, Tokyo, Fukuoka, Taipei, Bangkok, Shanghai, Chengdu)
    └── country-liquor-guide.md — Korea duty-free import rules + country-by-country
                                   signature spirits (Japan, Taiwan, Thailand, China)
                                   — general-purpose, not gay-tour specific

guides/osaka/
└── ibis_final_v14.pdf          — worked example: full Osaka 4-night solo trip itinerary
                                   (Doyamacho gay bar guide, food/cafe routes, whisky
                                   shopping, day-trip options, real booking-based budget)
```

## How to install the skill

Upload `skill/gay-travel-guide/` (or a zipped `.skill` package of it) to Claude. Once installed, requests like "오사카 게이바 가이드 만들어줘" or "방콕 게이투어 코스 짜줘" will trigger it automatically. The skill also answers plain "[country] 면세 위스키 추천" or "[city] 호텔 3성 4성 5성" questions using the bundled reference data, with no gay-travel angle required.

## About the example guide

`guides/osaka/ibis_final_v14.pdf` is a real, fully-worked output: a 4-night Osaka solo trip planned around the Doyamacho gay district, built incrementally across a single planning conversation. Lodging (ibis Osaka Umeda) was finalized partway through, so this version is restructured around that confirmed booking rather than presenting it as an open comparison. It includes:

- Doyamacho bar-by-bar breakdown by crowd type (tourist-friendly, bear/mature, local/dining, Korean-operated)
- Walking/transit routes from the hotel to the district (rain vs. clear-weather options)
- Day/night bar-hopping routes and food/cafe recommendations along the way
- Day-trip options from Osaka (Kyoto, Kobe, Nara, Arima Onsen) with transit times/costs
- Whisky duty-free shopping guide with realistic 2L/$400 combo suggestions
- A pre-departure checklist

Note: the main content angle is gay travel, but the food/cafe/whisky/day-trip sections are useful as general travel reference regardless of that angle.

## License / usage

Personal travel planning content. Verify current pricing, hours, and venue status before relying on anything here — bars and hotels close/rebrand, and duty-free pricing fluctuates.
