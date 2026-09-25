# OSINT GUIDE PART 1

## MY OSINT DASHBOARD 

> This is people related OSINT not company related if you want that it's in web exploitation guide passive recon 

**last updated** - `24 september 2026`

Organized by investigative pivot. Each category leads with the tools actually worth reaching for; anything kept only for completeness sits in a **Deprioritized** bucket at the end of that same category, not mixed in above the fold.
 
---
 
### 1. Automation Frameworks & All-in-One Platforms
 
- **Self-hosted / open-source automation:** [SpiderFoot](https://github.com/smicallef/spiderfoot) or [Recon-ng](https://github.com/lanmaster53/recon-ng) — open these first on any investigation
- **Hosted all-in-one platforms:** [Intell Base](https://intelbase.is/) — note: [OSINT CAT](https://www.osintcat.net/) also markets itself here, but its actual core function is breach-checking — see §10 (Exposed Data, Leaks & Code Repositories), listed there once to avoid duplicating the same URL under two labels
**Deprioritized:** Maltego (heavily paywalled/restricted free tier), Datasploit (unmaintained)
 
---
 
### 2. Person Identity — Name-Based Pivots
 
- Address search: [TruePeopleSearch](https://www.truepeoplesearch.com) or [192.com](https://www.192.com) (US only)
- Username/email generation: [soxoj's username-generation-guide](https://github.com/soxoj/username-generation-guide) + [transform_username](https://github.com/soxoj/transform_username)
- Phone number by name: [Truecaller](https://www.truecaller.com)
- Company/workplace: [OpenCorporates](https://opencorporates.com) — full corporate-records listing under §8, then pivot to country-based registries
**Deprioritized:** Pipl (now fully commercial/paid-only), Spokeo (heavy restrictions/paywalls on free lookups)
 
---
 
### 3. Username Pivots
 
- Search engines: [maigret](https://github.com/soxoj/maigret) or [Sherlock](https://github.com/sherlock-project/sherlock)
- Stylometry (writing-style analysis): [JStylo](https://github.com/psal/jstylo)
- Snowflake ID decoding: [singhajit.com snowflake-decoder](https://singhajit.com/tools/snowflake-decoder)
- Shortcode/ID converters: [Tweeterid.com](https://tweeterid.com) + [Instagram Shortcode Converter](https://techconverter.me/instagram-shortcode-to-date)
**Deprioritized:** WhatsMyName (older local Python wrappers lacking active updates compared to the web version/maigret)
 
---
 
### 4. Image & Visual Pivots
 
- Reverse image search: [Yandex Images](https://yandex.com/images) or [Google Lens](https://lens.google.com)
- Face search: [Facecheck.id](https://facecheck.id) or [lenso.ai](https://lenso.ai)
- Metadata extraction: [Exiftool](https://exiftool.org) or [EXIF.tools](https://exif.tools)
- Street view coverage: [Google Street View](https://www.google.com/maps), [Yandex Panorama](https://yandex.com/maps), [Mapillary](https://www.mapillary.com), [KartaView](https://kartaview.org), [Panoramax](https://panoramax.fr)
- Advanced street-view query engine: [Overpass-turbo](https://overpass-turbo.eu)
- Geolocation hunters: [GeoSeeker](https://geoseeker.com), [GeoAxis](https://geoaxis.com), [Picarta.ai](https://picarta.ai)
- Geolocation hints database: [GeoHints](https://geohints.com)
- Shadow-based location pinpointing: [SunCalc](https://www.suncalc.org), [ShadowMap](https://shadowmap.org), [ShadowFinder](https://github.com/bellingcat/ShadowFinder)
- Past world imagery: [wayback world imagery](https://livingatlas.arcgis.com/wayback/) and [google earth](https://earth.google.com/web/)
**Deprioritized:** TinEye (outperformed by modern neural reverse-search engines)
 
---
 
### 5. Email Pivots
 
- Search engines: [Epieos](https://epieos.com), [Holehe](https://github.com/megadose/holehe), [Mailmeteor](https://mailmeteor.com) — Gmail specifically: [GHunt](https://github.com/mxrch/GHunt)
- Format prediction: [Mailmeteor Email Permutator](https://mailmeteor.com/email-permutator)
- Verification: [MyEmailVerifier](https://myemailverifier.com) or [VerifyEmailAddress](https://www.verifyemailaddress.org)
- Breach data: [Have I Been Pwned](https://haveibeenpwned.com) — broader breach-search catalog in §10
- Mail blacklist check: [MXToolbox SuperTool](https://mxtoolbox.com/SuperTool.aspx)
**Deprioritized:** Skymem (drastically throttled free export limits)
 
---
 
### 6. Phone Number Pivots
 
- [Truecaller](https://www.truecaller.com) or [PhoneInfoga](https://github.com/sundowndev/phoneinfoga)
**Deprioritized:** Twilio Lookup basic carrier APIs (heavily restricted to verified commercial accounts)
 
---
 
### 7. Domain & Website Pivots
 
**Before anything manual:** [theHarvester](https://github.com/laramies/theHarvester)
 
- WHOIS/ownership: [WhoisXML](https://www.whoisxmlapi.com) or [ICANN Lookup](https://lookup.icann.org)
- Subdomain enumeration: [crt.sh](https://crt.sh) or [Amass](https://github.com/owasp-amass/amass) — crt.sh is also your certificate-transparency tool, cross-referenced in §9
- Archived/deleted content: [Wayback Machine](https://web.archive.org)
- Domain → IP resolution: [dig](https://linux.die.net/man/1/dig)
- Exposed document metadata: [FOCA](https://github.com/ElevenPaths/FOCA) or [Metagoofil](https://github.com/laramies/metagoofil)
**Deprioritized:** Sublist3r (unmaintained, broken API endpoints)
 
---
 
### 8. Company & Corporate Records
 
- Ownership/registry: [OpenCorporates](https://opencorporates.com) or [Crunchbase](https://www.crunchbase.com)
- Trademark/brand records: [WIPO Brand DB](https://www3.wipo.int/branddb/en/)
**Deprioritized:** Gleif (too narrow/niche for broad enterprise identification)
 
---
 
### 9. Infrastructure, Attack Surface & Threat Intelligence
 
*(Merged from what was previously four overlapping sections — IP-address pivots, attack-surface search engines, IP/domain reputation, and general threat intel all answer the same underlying question: "what do we know about this IP/domain/host's exposure and reputation")*
 
- IP geolocation/ownership: [Ipinfo](https://ipinfo.io) and [ViewDNS.info](https://viewdns.info)
- Exposed devices/services (attack-surface search engines): [Shodan](https://www.shodan.io), [Censys](https://search.censys.io), [FOFA](https://en.fofa.info), [ZoomEye](https://www.zoomeye.ai), [ONYPHE](https://search.onyphe.io), [Hunter Search Engine](https://hunter.how), [FullHunt](https://fullhunt.io)
- Tor/VPN detection: [IPQualityScore](https://www.ipqualityscore.com), [GetIPIntel](https://getipintel.net)
- IP/domain reputation: [AbuseIPDB](https://www.abuseipdb.com), [Cisco Talos Intelligence](https://talosintelligence.com/reputation_center), [GreyNoise](https://viz.greynoise.io), [Criminal IP](https://www.criminalip.io), [Shadowserver](https://dashboard.shadowserver.org)
- Malware intelligence (abuse.ch ecosystem): [Abuse.ch Hunting](https://hunting.abuse.ch), [MalwareBazaar](https://bazaar.abuse.ch/browse/), [YARAify](https://yaraify.abuse.ch/scan/) `to Run and test a malware [anyrun](https://any.run/)`
- General IOC/CVE feeds: [VirusTotal](https://www.virustotal.com), [AlienVault OTX](https://otx.alienvault.com), [CVE Details](https://www.cvedetails.com)
- Threat-actor profiling: [Mitre.org](https://www.mitre.org) or [Recorded Future](https://www.recordedfuture.com) (paid)
- Network/BGP intelligence: [BGP.tools](https://bgp.tools), [BGP.he.net](https://bgp.he.net)
- Traffic/infrastructure trends: [Cloudflare Radar](https://radar.cloudflare.com)
**Deprioritized:** MaxMind GeoLite legacy offline formats; older static signature scanners without cloud threat feeds; Netlas.io (redundant given Shodan/Censys coverage); ODIN (10-searches/day cap too limited vs. main alternatives); SikkerAPI (niche, redundant next to AbuseIPDB/GreyNoise); BrightCloud (overly basic interface, limited free telemetry); CertKit Certificate Search (crt.sh already covers this natively)
 
---
 
### 10. Exposed Data, Leaks & Code Repositories
 
*(Merged from three previously separate lists — code/pastebin leaks, exposed cloud storage, and breach-search engines are all the same investigative move: "has this person/org's data already leaked somewhere")*
 
- Source-code leak scanning: [Gitleaks](https://github.com/gitleaks/gitleaks) or GitHub Code Search
- Exposed cloud storage: [GrayhatWarfare](https://grayhatwarfare.com) — one of the highest-yield tools in this entire list for open S3 buckets
- Breach/leak search engines: [CheckLeaked](https://checkleaked.cc), [OSINT CAT / OsintCat](https://www.osintcat.net) (same site as the §1 cross-reference), [StealSeek](https://stealseek.com), [Venacus](https://venacus.com), CredenShow, HIB Ransomed, HEROIC.NOW, IKnowYour.Dad, Leaker (passive CLI, 10 breach DBs at once), NOX (recursive async breach/identity pivoting)
- Historical paste-site & dark-web indexing: [Intelligence X](https://intelx.io/tools)
**Deprioritized:** Pastebin basic unauthenticated scrapers (heavily rate-limited by Cloudflare)
 
---
 
### 11. Dark Web Monitoring
 
- [Ahmia](https://ahmia.fi) or [Dark.fail](https://dark.fail)
**Deprioritized:** Torch (unreliable index uptime, dead mirrors)
 
---
 
### 12. Cryptocurrency & Blockchain Tracing
 
- [Blockchair](https://blockchair.com) or [Arkham Intelligence](https://platform.arkhamintelligence.com)
**Deprioritized:** BitcoinWhosWho (archaic UI, minimal free tracking depth)
 
---
 
### 13. Social Media & Messaging Platforms
 
- Platform-aggregator search: [Social-Searcher](https://www.social-searcher.com) or [Social Analyzer](https://github.com/qeeqbox/social-analyzer)
- Instant messaging (Telegram/WhatsApp/Discord): [Telemint](https://github.com/telemint/telemint) or Telegram dorks via [Google CSE](https://cse.google.com)
**Deprioritized:** Twint (frequently breaks from X/Twitter API structural changes), ChatWatch (outdated WhatsApp status-tracking scripts)
 
---
 
### 14. Web Application & Mobile App Recon
 
- WordPress-specific scanning: [Wpscan](https://wpscan.com)
- Mobile app asset discovery (subdomains/URLs/parameters embedded in apps): [BeVigil](https://bevigil.com/search)
*(No deprioritized entries in this category yet — it's small and both tools are current)*
 
---
 
### 15. Transportation Tracking
 
- Flight (live): [FlightRadar24](https://www.flightradar24.com)
- Flight (history/data): [FlightAware](https://www.flightaware.com)
- Marine traffic: [MarineTraffic](https://www.marinetraffic.com)
- Vessel/container tracking: [VesselFinder](https://www.vesselfinder.com)
- Rail: [OpenRailwayMap](https://www.openrailwaymap.org)
**Deprioritized:** PlaneFinder (less comprehensive tracking density than FlightRadar24)
 
---
 
### 16. Search Engines — General, National & Specialized
 
**General dorking:**
[Dorkgpt](https://www.dorkgpt.com), [pagodo](https://github.com/opsdisk/pagodo), [Google Hacking Database](https://www.exploit-db.com/google-hacking-database)
 
**Custom/scoped search:**
[Google Custom Search](https://www.google.com/cse) (build a scoped engine for a recurring investigation), [Million Short](https://millionshort.com) (excludes the top N most popular results — surfaces content SEO-dominant sites bury)
 
**National — Tier 1 (Strategic Global Powers):**
[Baidu](https://www.baidu.com) (China), [Yandex](https://yandex.com) (Russia/Eastern Europe, also strongest reverse-image search), [Naver](https://www.naver.com) (South Korea), [Daum](https://www.daum.net) (South Korea), [SoGou](https://www.sogou.com) (China, indexes WeChat official-account articles)
 
**National — Tier 2 (Global):**
[Google](https://www.google.com), [Bing](https://www.bing.com), [Yahoo](https://www.yahoo.com)
 
**National — Tier 3 (Regional/Localized):**
Alleba (Philippines), Eniro (Sweden/Nordics), Gerdoo (Iran), Goo (Japan), Najdi (Slovenia), Onet.pl (Poland), Orange (France), Parseek (Iran), SAPO (Portugal), Search.ch (Switzerland), Seznam (Czech Republic), Walla (Israel), Zarebin (Iran), Coc Coc (Vietnam)
 
**Visual/clustering search:**
[Carrot2](https://search.carrot2.org) (clusters results by topic, actively maintained/open-source)
 
**Similar-site discovery:**
[SimilarSites](https://www.similarsites.com)
 
**Deprioritized:** Zanran (dead — domain now hosts an unrelated B2B product), Zapmeta (defunct since ~2003, domain blocks automated access), Mamont/mmnt.ru (technically live but Russian-only FTP index, too niche for routine use), 2lingual Search (functional but zero investigative utility, just a bilingual Google wrapper), unindexed basic Google-scraping scripts without captcha-solving or proxy pools
 
---
 
### 17. Investigative Document & Records Platforms
 
- [Google Pinpoint](https://journaliststudio.google.com/pinpoint/) — parses massive PDF/audio/email/handwritten-note troves
- [DocumentCloud](https://www.documentcloud.org) — annotate, publish, search leaked/public-record documents
- [OCCRP Aleph](https://aleph.occrp.org) — global leaked/public corporate records, court filings, sanctions lists
- [RECAP / CourtListener](https://www.courtlistener.com/recap/) — federal/state US court document archive
- [Internet Archive](https://archive.org) — the parent project behind Wayback Machine (full web-snapshot tool cross-referenced in §7); also hosts books, media, software
**Deprioritized:** De-facto (closed/absorbed), WorldWideScience.org (sunset — homepage now speaks of founding partners in the past tense), Biznar (defunct since ~2015, Deep Web Technologies wound down its federated-search line), CiteSeerX (currently broken — domain serves a placeholder instead of the actual index), Harmari Unified Listings Search (pivoted into a paid product rebranded "Neumo," no longer functions as a general search tool)
 
---
 
### 18. Personal OPSEC, Privacy Tooling & Anonymity
 
**Fingerprint / leak testing (test yourself before you test anyone else):**
[BrowserLeaks](https://browserleaks.com), [packet.guru](https://packet.guru) (WebRTC/DNS leak test, TLS/TCP fingerprinting, VPN/proxy/Tor detection with a trust score, no signup), [Tor Browser](https://www.torproject.org)

**Privacy-respecting daily-driver apps:**
```
VPN              -> Proton VPN or Mullvad VPN
Browser          -> Mullvad Browser or Firefox / Brave
Search engine    -> Kagi, SearXNG, or DuckDuckGo
Email            -> Proton Mail or Tuta Mail
Messaging        -> Threema or Signal
Cloud storage    -> Tresorit or Proton Drive
Antivirus        -> Bitdefender, ClamAV, or Sophos
Password manager -> 1Password or Bitwarden
Note-taking      -> Obsidian or Joplin
```
 
**Deprioritized:** Abandoned public proxy lists
 
---
### 19. Countries Where Google Maps / Street View Has Significant Restrictions

**Fully or largely absent from Street View:**
- **North Korea** — no Street View at all, satellite imagery is deliberately degraded by the regime, Maps shows almost no road detail
- **China** — Google Maps and Street View are blocked entirely inside China (use **[Baidu Maps](https://map.baidu.com/)** or **[Amap/Gaode](https://amap.com/)** instead — far better coverage for China than anything Google has)
- **Saudi Arabia** — Street View only recently became partially available and coverage is still very patchy outside major cities
- **Iran** — Google Maps accessible but Street View coverage is almost nonexistent, satellite imagery is limited in detail
- **Cuba** — very sparse Street View, road data is poor
- **Bhutan** — almost no Street View, government restricts most mapping
- **Turkmenistan** — effectively no Street View coverage

**Partial/restricted Street View for security reasons:**
- **Israel** — by law (Mapping and Databases Law), Google must blur military sites and certain government buildings before publishing imagery; coverage exists but with deliberate gaps
- **India** — Street View was banned for years over national security concerns, only returned in 2022 with restrictions near border regions and military installations
- **Russia** — Street View still exists but Google stopped updating it after 2022 and the existing imagery is aging fast; use **[Yandex Maps](https://yandex.com/maps/)** (specifically **[Yandex Panorama](https://yandex.com/maps/)**) which has far better and more current Russian Street View coverage
- **Ukraine** — Google paused Street View collection in active conflict zones
- **Pakistan** — Street View coverage is extremely sparse outside Islamabad and Lahore

**Countries where Google Maps exists but local alternatives are far better:**
- **China** $\rightarrow$ **[Baidu Maps](https://map.baidu.com/)** / **[Amap (Gaode)](https://amap.com/)** — Google has no usable China data
- **Russia** $\rightarrow$ **[Yandex Maps](https://yandex.com/maps/)** — much fresher and denser Street View than Google
- **Japan** — Google is good but **[Mapion](https://www.mapion.co.jp/)** and **[Yahoo Japan Maps](https://map.yahoo.co.jp/)** have detail Google misses at hyper-local level
- **South Korea** $\rightarrow$ **[Naver Maps](https://map.naver.com/)** / **[Kakao Maps](https://map.kakao.com/)** — Korean law restricts Google from having full-resolution map data, so local apps are far superior
- **Czech Republic** $\rightarrow$ **[Mapy.cz](https://en.mapy.cz/)** — significantly better hiking/rural coverage
- **Germany** — Google Street View coverage is intentionally very sparse because a large number of German residents legally opted out when Street View launched (right to be forgotten requests blurred entire streets); **[OpenStreetMap](https://www.openstreetmap.org/)** + **[Mapillary](https://www.mapillary.com/)** often have better German ground-level coverage

#### The Practical Implications for Geo-OSINT

> If your image looks like it could be China and Google Maps shows you nothing useful $\rightarrow$ switch to **[Baidu Maps](https://map.baidu.com/)** immediately, it is not a fallback, it is the primary tool for China

> If your image looks Russian and Street View imagery looks old or is missing $\rightarrow$ **[Yandex Maps Street View (Panorama)](https://yandex.com/maps/)** is what you actually want, it has coverage Google does not

> If your image looks South Korean $\rightarrow$ **[Naver Maps](https://map.naver.com/)** or **[Kakao Maps](https://map.kakao.com/)** will show you ground-level imagery in areas Google cannot legally provide at full resolution

> If your image is in a military-adjacent area in Israel, India, or Pakistan $\rightarrow$ the deliberate blur / absence on Google is itself a clue that you are near a sensitive site

> If there is simply no Street View anywhere near your candidate location $\rightarrow$ **[Mapillary](https://www.mapillary.com/)** is community-contributed ground-level imagery and covers many areas Google has never sent a car to

#### Quick Reference — What to Use Where

| Location | Primary Map Tool | Street View Equivalent |
| :--- | :--- | :--- |
| **China** | **[Baidu Maps](https://map.baidu.com/)** / **[Amap](https://amap.com/)** | Baidu Street View |
| **Russia** | **[Yandex Maps](https://yandex.com/maps/)** | Yandex Panorama |
| **South Korea** | **[Naver Maps](https://map.naver.com/)** / **[Kakao](https://map.kakao.com/)** | Naver Street View |
| **North Korea** | None useful | Literally nothing |
| **Germany (sparse)** | **[OpenStreetMap](https://www.openstreetmap.org/)** | **[Mapillary](https://www.mapillary.com/)** |
| **Japan (hyper-local)** | **[Yahoo Japan Maps](https://map.yahoo.co.jp/)** | Google still usable |
| **Global gaps** | **[Mapillary](https://www.mapillary.com/)** | Community-contributed imagery |
| **Conflict zones** | **[LiveUAMap](https://liveuamap.com/)** + **[OSM](https://www.openstreetmap.org/)** | Older cached Google imagery |

> **The bottom line for geo-OSINT:** Google Maps is not a single global truth — it is a patchwork of varying coverage, legal restrictions, deliberate blurring, and aging imagery. Knowing which country you are looking at (which your Stages 1–4 already narrow down) tells you immediately which mapping tool is actually authoritative for that region.
---

**Bonus tools**

> Putting my Instagram reels to go use :) 
```
Ip address -> what is downloaded by the user: http://iknowwhatyoudownload.com/ (this was not opening in my pc :[ )
```

> Practitioners Note: if you ever get someone pgp keys you can decode it using https://cirw.in/gpg-decoder to decode it to get the email id  

## Geo-Osint 

> practice this using geo-gusser

when looking at a picture how do you extract its location:here are clues to look at
```
1. Context
2. Foreground
3. Background
4. Map and marking
5. Trial and error 
```
**CONTEXT**
- Fastest way to get someone location is through the extraction on meta data
- Reverse image search and image based location search are effective methods
- Tools like street views form google or location query engines for location
- you can analyze the shadows in the image to extract more information


`but before you start looking at random clues go in this order`

The rule: work from broadest (eliminates the most of the world per clue)
to narrowest (confirms exact spot). Checking a "quick tell" out of order
wastes time — a road sign font means nothing until you already know
which country's font system you're comparing it against.

```
STAGE 1 — INSTANT WORLD-CUTTING CLUES (first 5 seconds)
├── LANGUAGE / SCRIPT on any visible text
│     Single biggest narrowing clue — cuts the world to a script
│     family / country group before anything else
├── STEERING WHEEL SIDE / TRAFFIC DIRECTION
│     Cuts the world in half instantly
└── ANY UNIQUE / "ONLY" TELL
      Kangaroo, baobab, gondola, longtail boat, torii gate, etc.
      If one appears → country confirmed → skip straight to Stage 6

STAGE 2 — HEMISPHERE / CLIMATE / LATITUDE
├── SHADOW DIRECTION          → Northern vs Southern Hemisphere
├── VEGETATION TYPE            → Tropical / temperate / high latitude band
└── TERRAIN TYPE               → Desert / mountain / coastline narrows further

STAGE 3 — CONTINENT / CULTURAL REGION
├── ARCHITECTURE STYLE         → Materials, roof type, window style
├── CLOTHING / PEOPLE          → Traditional dress, uniforms, religious clothing
└── ROAD MARKINGS + PLATE COLOR/SCRIPT

STAGE 4 — COUNTRY-SPECIFIC CONFIRMATION
├── ROAD SIGNS                 → Exact wording, colors, font
├── UTILITY POLES / POSTBOXES / POLICE CAR LIVERY
└── SHOP / FUEL / FAST FOOD BRANDS
      Only useful once narrowed to 2-3 candidates — checking brand
      signage against the whole world at Stage 1 is inefficient

STAGE 5 — REGION / CITY WITHIN THE COUNTRY
├── DIALECT / SCRIPT VARIANT   → Regional markers on signs
├── LOCAL-ONLY CHAIN BRANDS    → Not national chains
└── REGIONAL ARCHITECTURE VARIATION

STAGE 6 — EXACT PINPOINT
├── REVERSE IMAGE SEARCH       → Any visible landmark or storefront
├── STREET VIEW CROSS-REFERENCE→ Walk the candidate area virtually
└── UNIQUE STREET FURNITURE    → Manhole cover design, bollard style

STAGE 7 — TIME / DATE CONFIRMATION (always last)
├── SHADOW LENGTH + SUNCALC    → Against your Stage 6 candidate location
├── SEASONAL VEGETATION STATE  → Blossom, autumn color, snow
└── STAR POSITION via Stellarium → Clear night sky only
```
**HERE ARE THE MORE BROAD CHECKLISTS**
```
SHADOW ANALYSIS QUICK CHECKLIST

HEMISPHERE IDENTIFICATION
├── SHADOWS POINT NORTH
│     Sun travels south of overhead      → Northern Hemisphere
│     Countries: USA / Europe / China / Russia / Japan
│
└── SHADOWS POINT SOUTH
      Sun travels north of overhead      → Southern Hemisphere
      Countries: Australia / NZ / South Africa / Argentina / Brazil

TIME OF DAY FROM SHADOW LENGTH
├── VERY LONG shadows (several times object height)
│     Low sun angle                      → Early morning OR late afternoon
│     Combined with orange/golden light  → Sunrise OR sunset (golden hour)
│
├── MEDIUM shadows (roughly equal to object height)
│     Mid-morning OR mid-afternoon       → Roughly 9-10am OR 2-3pm
│
├── SHORT shadows (much shorter than object height)
│     Sun near overhead                  → Around midday (11am - 1pm)
│
└── NO shadows visible
      Sun directly overhead              → Near equator at solar noon ONLY
      Overcast sky                       → Cannot determine time from shadows

SHADOW DIRECTION = TIME OF DAY
├── NORTHERN HEMISPHERE
│     Shadows pointing NORTHWEST         → Morning (sun in southeast)
│     Shadows pointing NORTH             → Midday (sun in south)
│     Shadows pointing NORTHEAST         → Afternoon (sun in southwest)
│     Shadows pointing WEST              → Very early morning
│     Shadows pointing EAST              → Very late afternoon / evening
│
└── SOUTHERN HEMISPHERE
      Shadows pointing SOUTHWEST         → Morning (sun in northeast)
      Shadows pointing SOUTH             → Midday (sun in north)
      Shadows pointing SOUTHEAST         → Afternoon (sun in northwest)

LATITUDE CLUES FROM SHADOWS
├── VERY SHORT shadows even at midday
│     Sun very high overhead             → Tropical zone (within 23° of equator)
│     Countries: Indonesia / Thailand / Kenya / Brazil / Colombia
│
├── MODERATE shadows at midday
│     Sun at medium angle                → Temperate zone (23° - 60° latitude)
│     Countries: USA / Europe / China / Japan / Australia
│
└── VERY LONG shadows even at midday
      Sun stays low on horizon           → Polar / subpolar regions
      Countries: Iceland / Norway / Finland / Alaska / Southern Argentina

SEASONAL CLUES FROM SHADOWS
├── NORTHERN HEMISPHERE
│     Very short midday shadows          → June / July (summer solstice)
│     Very long midday shadows           → December / January (winter solstice)
│     Medium shadows                     → March / September (equinoxes)
│
└── SOUTHERN HEMISPHERE
      Very short midday shadows          → December / January (summer solstice)
      Very long midday shadows           → June / July (winter solstice)
      Medium shadows                     → March / September (equinoxes)

SHADOW ANGLE QUICK MATH
├── Shadow length = object height × tan(90° - sun altitude)
├── Sun altitude at midday (Northern Hemisphere)
│     90° - latitude + 23.5° (summer)
│     90° - latitude - 23.5° (winter)
│
└── Example: London (51°N) summer midday
      Sun altitude = 90 - 51 + 23.5 = 62.5°
      Shadow = very short (high sun)

SUNCALC METHOD (most reliable)
├── STEP 1  Go to suncalc.org
├── STEP 2  Drop pin on suspected location
├── STEP 3  Set the date from image context / EXIF
├── STEP 4  Adjust time slider until sun angle matches shadows
├── STEP 5  If shadows match → location AND time confirmed
└── STEP 6  Cross reference with hemisphere / season clues

SHADOW COLOR CLUES
├── BLUE tinted shadows                  → Cold climate / snow reflection / winter
├── ORANGE tinted shadows                → Golden hour (sunrise / sunset)
├── HARSH black shadows, no softness     → Tropical / desert (intense direct sun)
└── SOFT diffused shadows                → Overcast / temperate climate

SPECIAL SHADOW SITUATIONS
├── MULTIPLE shadows from one object
│     Two light sources visible          → Urban area, artificial lighting
│     Only at night                      → Cannot use for sun analysis
│
├── SHADOW ON SNOW
│     Blue shadow on white snow          → Winter / high altitude
│     Very long shadows on snow          → High latitude winter (sun very low)
│
├── SHADOWS INSIDE BUILDING / WINDOW
│     Sun coming from specific direction → Confirms compass orientation of room
│     Shadow of window frame on floor    → Can calculate sun angle precisely
│
└── NO SHADOWS AT ALL
      Completely overcast                → Cannot determine time or location
      Dense fog / rain                   → Cannot use shadow analysis

CROSS REFERENCE CHECKLIST
├── Step 1  Which way do shadows point?  → Confirms hemisphere
├── Step 2  How long are the shadows?    → Estimates time of day
├── Step 3  What direction do they point?→ Narrows hour of day
├── Step 4  How short at midday?         → Estimates latitude / season
├── Step 5  Confirm with SunCalc         → Pin exact time + location
└── Step 6  Cross check with vegetation  → Confirms season / hemisphere

QUICK TELLS
┌──────────────────────────────────────────────────┐
│ Shadows point north    → Northern hemisphere     │
│ Shadows point south    → Southern hemisphere     │
│ No shadow at midday    → Equatorial zone         │
│ Very long all day      → High latitude / winter  │
│ Blue tinted shadows    → Cold / snowy climate    │
│ Orange shadows         → Golden hour (6am/6pm)   │
│ Harsh black shadows    → Tropical / desert       │
│ Short midday shadows   → Summer OR tropics       │
│ Long midday shadows    → Winter OR high latitude │
└──────────────────────────────────────────────────┘
```
- Country specific road sign always exist here is list of the few
```
ROAD SIGNS QUICK CHECKLIST

STOP SIGN
├── Red octagon, STOP in English         → USA / Canada / Australia / NZ
├── Red octagon, STOP in French          → France / Quebec Canada
├── Red octagon, ARRÊT                   → Quebec Canada ONLY
├── Red octagon, ALTO                    → Mexico / Latin America
├── Red octagon, PARE                    → Brazil / Portugal
├── Red octagon, СТОП (Cyrillic)         → Russia / Eastern Europe
└── Red octagon, وقف (Arabic)            → Middle East / Gulf states

GIVE WAY / YIELD SIGN
├── Red/white inverted triangle, YIELD   → USA / Canada
├── Red/white inverted triangle, GIVE WAY→ UK / Australia / NZ / Ireland
├── Red/white inverted triangle, VORFAHRT→ Germany
└── Plain inverted red triangle          → Most of Europe (no text)

HIGHWAY / MOTORWAY SIGNS
├── Green background white text          → USA / Canada / Australia
├── Blue background white text           → Europe / Japan / China
├── Yellow background black text         → Germany (federal roads)
├── Blue with white A + number           → France (Autoroute)
├── Green with white M + number          → Spain (Motorway)
├── Blue with white E + number           → Pan-European E-roads
└── Green with white BR + number         → Brazil

SPEED LIMIT SIGNS
├── White circle red border, black number→ Europe / UK / Australia
├── White rectangle, black number        → USA / Canada
├── Yellow diamond, black number         → Australia (advisory speed)
└── White circle, no border, black number→ Japan / China

WARNING SIGNS
├── Yellow diamond black symbol          → USA / Canada / Australia / NZ
├── Red triangle white background        → Europe / UK / most world
├── Yellow triangle black symbol         → Japan
└── Orange diamond                       → USA (road construction only)

ANIMAL WARNING SIGNS
├── Kangaroo silhouette                  → Australia ONLY
├── Moose / elk silhouette               → Canada / Scandinavia / Russia
├── Reindeer silhouette                  → Finland / Norway / Sweden ONLY
├── Penguin silhouette                   → New Zealand / South Africa
├── Camel silhouette                     → Middle East / Australia outback
├── Elephant silhouette                  → India / Southeast Asia / Africa
└── Wombat silhouette                    → Australia ONLY

ROAD SURFACE MARKINGS
├── Yellow center + yellow edge lines    → USA / Canada
├── White center + white edge lines      → Europe / Australia
├── Yellow center + white edge lines     → Brazil / some Latin America
├── White center only, no edge           → Developing countries / rural Asia
└── Blue road markings                   → France (some older roads)

DISTANCE / MILEAGE
├── Miles on signs                       → USA / UK / Myanmar
├── Kilometers on signs                  → Rest of world
└── Both shown                           → Canada (transition areas)

LANGUAGE ON SIGNS
├── English only                         → USA / UK / Australia
├── English + French                     → Canada / some African countries
├── English + Maori                      → New Zealand
├── English + Welsh                      → Wales ONLY
├── French + local language              → France / West Africa
├── Cyrillic only                        → Russia / Belarus / Bulgaria
├── Arabic + English                     → UAE / Qatar / Jordan
├── Hebrew + Arabic + English            → Israel ONLY
├── Chinese + Pinyin + English           → China
├── Japanese + English                   → Japan (expressways)
└── Dual script (Latin + local)          → India / Malaysia / Sri Lanka

UNIQUE COUNTRY SPECIFIC SIGNS
├── Brown tourist signs                  → UK / Europe (heritage sites)
├── Blue H hospital signs (very common)  → UK
├── Green SOS boxes on highways          → France / Spain
├── Red torii gate on sign               → Japan (shrine nearby)
├── White on green, very minimal design  → Scandinavia
└── Highly decorative / colorful signs   → India / Pakistan

QUICK TELLS
┌──────────────────────────────────────────────────┐
│ ALTO on stop sign      → Mexico / Latin America  │
│ PARE on stop sign      → Brazil / Portugal       │
│ ARRÊT on stop sign     → Quebec Canada ONLY      │
│ Yellow diamond signs   → USA / Canada            │
│ Red triangle signs     → Europe / UK             │
│ Kangaroo sign          → Australia ONLY          │
│ Reindeer sign          → Scandinavia ONLY        │
│ English + Welsh        → Wales ONLY              │
│ Hebrew + Arabic        → Israel ONLY             │
│ Miles on signs         → USA / UK / Myanmar      │
│ Brown tourist signs    → UK / Europe             │
│ Torii gate sign        → Japan                   │
└──────────────────────────────────────────────────┘
```
- vegetation can too determine the area as well the region for example
```
VEGETATION QUICK CHECKLIST

TREES
├── Palm trees (tall, tropical)          → Within 35° of equator
│     Date palms (grey, spiky top)       → Middle East / North Africa
│     Coconut palms (leaning, beach)     → Tropical coastlines worldwide
│     Oil palms (very dense fronds)      → Malaysia / Indonesia / West Africa
│
├── Eucalyptus (peeling bark, grey-green)→ Australia / California / parts Africa
├── Baobab (massive trunk, tiny top)     → Sub-Saharan Africa / Madagascar ONLY
├── Acacia (flat-topped umbrella shape)  → East Africa / South Africa / Australia
├── Araucaria (umbrella pine, spiky)     → South America / Norfolk Island
├── Cherry blossom (pink spring flower)  → Japan / Korea / China (spring only)
├── Maple (bright red/orange leaves)     → Canada / Northeast USA (autumn only)
├── Olive trees (silver-green, twisted)  → Mediterranean ONLY
├── Cork oak (thick stripped bark)       → Portugal / Spain / Morocco
└── Mangrove (roots above water)         → Tropical coastlines worldwide

FORESTS
├── Dense dark conifer (pine/spruce)     → Scandinavia / Russia / Canada
├── Mixed deciduous (green + colour)     → Central Europe / Northeast USA
├── Rainforest (extremely dense green)   → Amazon / Congo / SE Asia
├── Cloud forest (misty, mossy)          → Central America / Colombia / Ethiopia
├── Bamboo forest                        → China / Japan / Southeast Asia
└── Sparse dry scrubland                 → Australia / South Africa / Spain

CROPS / FARMLAND
├── Rice paddies (flooded flat fields)   → Southeast Asia / East Asia
├── Terraced farming (stair steps)       → Southeast Asia / Andes / Mediterranean
├── Tea plantations (neat dark rows)     → India / Sri Lanka / Kenya / China
├── Coffee plantations (hilly, lush)     → Ethiopia / Colombia / Vietnam / Brazil
├── Vineyards (neat low rows)            → France / Italy / Spain / Germany / Chile
├── Lavender fields (purple)             → France (Provence) / Spain
├── Tulip fields (bright color rows)     → Netherlands ONLY
├── Wheat plains (vast flat golden)      → USA / Russia / Ukraine / Australia
├── Sugarcane (tall dense grass)         → Brazil / India / Thailand / Caribbean
└── Coca plants (low bushy rows)         → Colombia / Peru / Bolivia

GROUND / UNDERGROWTH
├── Red / orange clay soil               → Australia outback / parts Africa
├── Black volcanic soil                  → Iceland / Hawaii / Indonesia / Italy
├── White sandy soil                     → Middle East / coastal tropics
├── Heather (purple low shrub)           → UK / Scotland / Ireland / Scandinavia
├── Pampas grass (very tall silver)      → Argentina / Uruguay / Southern Brazil
├── Sagebrush (grey-green low shrub)     → USA Southwest ONLY
├── Fynbos (fine low shrubs)             → South Africa (Cape) ONLY
└── Tundra (flat, no trees, mossy)       → Northern Russia / Canada / Alaska

INSTANT COUNTRY TELLS
┌──────────────────────────────────────────────────┐
│ Baobab tree         → Africa / Madagascar ONLY   │
│ Tulip fields        → Netherlands ONLY           │
│ Eucalyptus          → Australia / California     │
│ Araucaria pine      → South America              │
│ Lavender fields     → France (Provence)          │
│ Sagebrush           → USA Southwest ONLY         │
│ Fynbos              → South Africa ONLY          │
│ Oil palm plantation → Malaysia / Indonesia       │
│ Cherry blossom      → Japan / Korea / China      │
│ Maple autumn color  → Canada / NE USA            │
│ Olive trees         → Mediterranean ONLY         │
│ Pampas grass        → Argentina / Uruguay        │
└──────────────────────────────────────────────────┘
```
- based on the visible terrain 
```
TERRAIN / LANDSCAPE QUICK CHECKLIST

MOUNTAINS
├── PERFECTLY CONE SHAPED volcano
│     Snow-capped, near ocean             → Japan (Fuji) / Indonesia / Philippines
│     Tropical jungle surroundings        → Indonesia / Papua New Guinea
│     Pacific coastline nearby            → Mexico / Central America
│
├── JAGGED SHARP peaks (like teeth)
│     Extreme height, glaciers visible    → Himalayas (Nepal / Tibet / Pakistan)
│     Green valleys below                 → Swiss / Austrian Alps
│     Very sharp dramatic spires          → Patagonia (Argentina / Chile) ONLY
│     Granite dome shapes                 → Yosemite USA / Madagascar
│
├── ROUNDED smooth peaks
│     Green covered, misty                → UK / Ireland / Scandinavia
│     Brown/bare at top                   → Scotland Highlands specifically
│     Very wide flat topped               → South Africa (Table Mountain)
│
├── FLAT TOPPED mountains (mesa / butte)
│     Red/orange rock                     → USA Southwest (Utah / Arizona) ONLY
│     Green flat top                      → Brazil (tepui) / Venezuela
│     Brown flat top                      → South Africa / Namibia
│
└── KARST LIMESTONE towers
      Green jungle covered pillars        → China (Guilin) / Vietnam (Ha Long)
      Surrounded by turquoise water       → Vietnam (Ha Long Bay) ONLY
      Inland dry setting                  → China specifically

DESERTS
├── SAND DUNES (tall, curved)
│     Orange/red sand                     → Namibia / UAE / Saudi Arabia
│     White/pale sand                     → Sahara / Libya / Algeria
│     Pink sand                           → Jordan (Wadi Rum) ONLY
│     Very tall massive dunes             → Namibia (Sossusvlei) ONLY
│
├── ROCKY DESERT (flat, stony)
│     Red rock surface                    → Australia outback ONLY
│     Grey/brown gravel flat              → Gobi (Mongolia / China)
│     Black volcanic rock                 → Iceland / Hawaii / Canary Islands
│     White salt flat                     → Bolivia (Salar de Uyuni) / USA (Bonneville)
│
├── CANYON / GORGE
│     Red/orange layered rock walls       → USA (Grand Canyon / Utah) ONLY
│     Narrow slot canyon                  → USA / Jordan (Petra)
│     Green river at bottom               → USA / Turkey / Ethiopia
│
└── SCRUBLAND / SEMI ARID
      Red soil, sparse dry bush           → Australia outback
      Grey green low scrub                → USA Southwest / Mexico
      Dry yellow grass, flat              → East Africa (savanna)
      Dry yellow grass, hilly             → South Africa / Zimbabwe

COASTLINES
├── FJORDS (steep cliffs straight into water)
│     Very dramatic, near vertical walls  → Norway ONLY
│     Similar but smaller scale           → New Zealand / Chile / Alaska
│
├── CLIFFS
│     White chalk cliffs                  → UK (Dover) / France (Normandy)
│     Black volcanic cliffs               → Iceland / Canary Islands / Hawaii
│     Red sandstone cliffs                → Australia / Portugal (Algarve)
│     Orange limestone cliffs             → Malta / Croatia / Greece
│
├── BEACHES
│     White sand + turquoise water        → Caribbean / Maldives / SE Asia
│     Black sand beach                    → Iceland / Hawaii / New Zealand
│     Pink sand beach                     → Bahamas / Bermuda / Greece (rare)
│     Grey/brown sand, cold looking water → UK / Northern Europe
│     Red sand beach                      → Prince Edward Island Canada ONLY
│     Green sand beach                    → Hawaii (Papakolea) ONLY
│
├── TROPICAL COASTLINE
│     Mangroves at water edge             → Southeast Asia / Caribbean / Africa
│     Coral visible through water         → Great Barrier Reef / SE Asia / Red Sea
│     Palm lined beach                    → Within 35° of equator
│     Rice paddies near coast             → Southeast Asia / Indonesia
│
└── ROCKY COASTLINE
      Granite boulders on beach           → Seychelles ONLY
      Limestone stacks in water           → Australia (Twelve Apostles)
      Basalt hexagonal columns            → Ireland (Giants Causeway) / Iceland
      Small rocky islands covered green   → Greece / Croatia / Norway

PLAINS / FLATLANDS
├── GRASSLAND / SAVANNA
│     Flat, dry yellow grass, acacia trees→ East Africa (Kenya / Tanzania)
│     Flat, dry, red soil visible         → Australia outback
│     Flat, green, very wide open         → Argentina (Pampas) / Uruguay
│     Flat, green, hedgerows dividing     → UK / France / Netherlands
│
├── STEPPE
│     Flat, dry, treeless, windy looking  → Mongolia / Kazakhstan / Russia
│     Similar but colder, snow possible   → Siberia / Northern Kazakhstan
│
├── WETLANDS
│     Flat, reeds, many waterways         → Netherlands / Bangladesh / Louisiana
│     Flooded forest                      → Amazon / Congo / Southeast Asia
│     Salt marshes, coastal               → UK / France Atlantic coast
│
└── AGRICULTURAL PLAINS
      Perfect grid fields                 → USA Midwest / Canada prairies
      Irregular shaped fields             → Europe / UK
      Flooded paddy fields                → Southeast Asia / East Asia
      Circular irrigation patterns        → USA / Saudi Arabia (seen from above)

RIVERS / LAKES
├── RIVERS
│     Brown/red muddy wide river          → Amazon / Congo / Mississippi
│     Bright blue clear river             → New Zealand / Scandinavia / Alps
│     Green river through canyon          → USA Southwest / Turkey
│     Very wide flat slow river           → Russia / Siberia / Bangladesh
│
├── LAKES
│     Bright blue, snow peaks around      → Switzerland / Austria / Canada Rockies
│     Turquoise glacial lake              → Canada (Banff) / New Zealand / Patagonia
│     Pink lake                           → Australia (Lake Hillier) ONLY
│     Salt lake, white edges              → Bolivia / USA (Great Salt Lake)
│     Very large, looks like sea          → Great Lakes USA / Lake Victoria Africa
│
└── GLACIERS
      Glacier descending into sea         → Iceland / Greenland / Alaska
      Glacier on mountain side            → Alps / Himalayas / Andes / Rockies
      Glacier lake at bottom (grey water) → Patagonia / Iceland / New Zealand

INSTANT COUNTRY TELLS
┌──────────────────────────────────────────────────┐
│ Perfect cone volcano      → Japan / Indonesia    │
│ Red rock mesa / butte     → USA Southwest ONLY   │
│ Karst limestone pillars   → China / Vietnam      │
│ Pink desert sand          → Jordan (Wadi Rum)    │
│ Massive orange sand dunes → Namibia              │
│ White chalk cliffs        → UK / France          │
│ Black sand beach          → Iceland / Hawaii     │
│ Granite boulders on beach → Seychelles ONLY      │
│ Basalt hexagonal columns  → Ireland / Iceland    │
│ Fjords                    → Norway / NZ / Chile  │
│ Pink lake                 → Australia ONLY       │
│ Turquoise glacial lake    → Canada / Patagonia   │
│ Sharp spire mountains     → Patagonia ONLY       │
│ Flat topped tepui         → Venezuela / Brazil   │
│ Salt flat white mirror    → Bolivia ONLY         │
└──────────────────────────────────────────────────┘
```
- from people and clothing visible in the image 
```
PEOPLE / CLOTHING QUICK CHECKLIST

TRADITIONAL / NATIONAL DRESS
├── SOUTH ASIA
│     Sari (long draped fabric, women)    → India / Sri Lanka / Bangladesh
│     Salwar Kameez (tunic + trousers)    → India / Pakistan / Bangladesh
│     Lungi (wraparound cloth, men)       → Bangladesh / South India / Myanmar
│     Dhoti (white wraparound, men)       → Rural India ONLY
│     Topi (white cap, men)               → Nepal / Northern India
│
├── SOUTHEAST ASIA
│     Longyi (wraparound skirt, men+women)→ Myanmar ONLY
│     Ao Dai (fitted silk tunic, women)   → Vietnam ONLY
│     Batik patterned clothing            → Indonesia / Malaysia
│     Conical straw hat (non la)          → Vietnam ONLY
│     Sarong common on both genders       → Indonesia / Malaysia / Pacific Islands
│
├── EAST ASIA
│     Hanbok (wide skirt, short jacket)   → Korea ONLY
│     Kimono / Yukata (formal occasions)  → Japan ONLY
│     Qipao / Cheongsam (fitted dress)    → China / Hong Kong
│     Mandarin collar shirts common       → China
│
├── MIDDLE EAST
│     Thobe (long white robe, men)        → Saudi Arabia / Gulf states
│     Keffiyeh (red+white checked scarf)  → Saudi Arabia / Jordan
│     Keffiyeh (black+white checked)      → Palestine / Jordan
│     Abaya (full black robe, women)      → Saudi Arabia / Gulf states
│     Niqab (face veil, eyes only)        → Saudi Arabia / Gulf states
│     Hijab (headscarf only)              → Muslim-majority countries globally
│     Kandura (white robe) + Agal         → UAE / Qatar / Kuwait ONLY
│
├── CENTRAL ASIA
│     Chapan (colorful robe, men)         → Uzbekistan / Tajikistan
│     Kalpak (white felt pointed hat)     → Kyrgyzstan ONLY
│     Tubeteika (small embroidered cap)   → Uzbekistan / Kazakhstan
│     Yurt + traditional felt clothing    → Mongolia / Kazakhstan
│
├── AFRICA
│     Kente cloth (colorful woven strips) → Ghana ONLY
│     Dashiki (colorful loose shirt)      → West Africa broadly
│     Shuka (red checked cloth)           → Maasai Kenya / Tanzania ONLY
│     Boubou (long wide robe)             → West Africa / Senegal
│     Djellaba (long hooded robe)         → Morocco / Algeria / Tunisia
│     Basotho blanket (geometric pattern) → Lesotho ONLY
│
├── EUROPE
│     Lederhosen (leather shorts)         → Bavaria Germany / Austria ONLY
│     Dirndl (dress + apron)              → Bavaria Germany / Austria ONLY
│     Kilt (tartan skirt, men)            → Scotland ONLY
│     Clog shoes visible                  → Netherlands
│     Saami clothing (colorful trim)      → Lapland Norway/Sweden/Finland
│
├── AMERICAS
│     Poncho (woven, colorful)            → Andes (Peru / Bolivia / Ecuador)
│     Sombrero (very wide brim)           → Mexico ONLY
│     Mola (colorful panel clothing)      → Panama (Kuna people) ONLY
│     Indigenous feather headdress        → Amazon Brazil / Central America
│
└── PACIFIC
      Grass skirt / tapa cloth            → Pacific Islands broadly
      Lei (flower garland around neck)    → Hawaii / Pacific Islands
      Lavalava (wraparound cloth)         → Samoa / Tonga / Fiji

RELIGIOUS CLOTHING
├── Turban (large, colorful)              → Sikh India / Punjab ONLY
├── Turban (white, simple)                → Muslim scholars / West Africa
├── Kippah (small round cap)              → Jewish / Israel
├── Buddhist monk orange robes            → Thailand / Myanmar / Sri Lanka
├── Buddhist monk yellow/red robes        → Tibet / Mongolia / Bhutan
├── Orthodox priest black robes + hat     → Russia / Greece / Eastern Europe
├── Catholic nun habit                    → Global but common Latin America / Europe
└── Hare Krishna robes (orange/white)     → India / Global urban areas

HEADWEAR
├── Fez (red flat-topped cylinder)        → Morocco / Turkey / Egypt
├── Keffiyeh (checkered headscarf)        → Middle East
├── Pagri (tied turban)                   → India / Pakistan
├── Deerstalker hat                       → UK / Scotland
├── Akubra (wide brim felt)               → Australia ONLY
├── Gaucho beret (flat, black/grey)       → Argentina / Uruguay ONLY
├── Ushanka (fur ear-flap hat)            → Russia / Eastern Europe / Mongolia
├── Conical bamboo hat                    → Vietnam / China / Southeast Asia
├── Karakul (lamb fur hat)                → Central Asia / Afghanistan
└── Baseball cap worn backwards           → USA / Global youth culture

FOOTWEAR
├── Wooden clogs                          → Netherlands / Belgium
├── Geta (wooden sandals)                 → Japan (traditional)
├── Huarache sandals (woven leather)      → Mexico ONLY
├── Juttis (pointed embroidered)          → India / Pakistan
└── Barefoot common in streets            → Parts of Southeast Asia / Pacific Islands

UNIFORMS
├── POLICE
│     Bobby helmet (tall black dome)      → UK ONLY
│     Blue beret + blue uniform           → France
│     Khaki uniform + beret               → India / Pakistan / many ex-British
│     Grey/olive uniform                  → Russia / Eastern Europe
│     Brown uniform + star badge          → USA (sheriff) ONLY
│
├── MILITARY
│     Red ceremonial uniform + bearskin   → UK (Buckingham Palace) ONLY
│     Olive drab very common              → USA / most NATO
│     Digital camo pattern                → USA / Canada / modern armies
│     Blue/white naval uniform            → Global but prominent Russia/France
│
├── SCHOOL UNIFORMS
│     Blazer + tie + grey trousers        → UK / Australia / NZ / Hong Kong
│     White shirt + dark trousers/skirt   → Japan / South Korea / SE Asia
│     No uniform, casual clothes          → USA / Canada / most of Europe
│     White shirt + blue/red neckerchief  → Cuba / China (Young Pioneers)
│
└── WORKERS
      High-vis yellow/orange vest common  → UK / Australia / Europe
      Conical hat + simple clothes        → Vietnam / China (rural workers)
      White lab coat common               → Global (medical / scientific)
      Salwar kameez work clothing         → India / Pakistan (very common)

BODY FEATURES / MARKINGS
├── Bindi (dot on forehead, women)        → India / Hindu communities globally
├── Mehndi / henna on hands              → India / Middle East / North Africa
├── Lip plate (disc in lower lip)         → Ethiopia (Mursi tribe) ONLY
├── Neck rings (coils on neck)            → Myanmar (Kayan) / Thailand
├── Full body tattoos visible             → Samoa / Polynesia / Maori NZ
└── Scarification patterns               → Sub-Saharan Africa broadly

QUICK TELLS
┌──────────────────────────────────────────────────┐
│ Kilt                      → Scotland ONLY        │
│ Lederhosen / Dirndl       → Bavaria / Austria    │
│ Sombrero                  → Mexico ONLY          │
│ Longyi wraparound         → Myanmar ONLY         │
│ Ao Dai fitted tunic       → Vietnam ONLY         │
│ Hanbok wide skirt         → Korea ONLY           │
│ Maasai red shuka          → Kenya / Tanzania     │
│ Kente cloth strips        → Ghana ONLY           │
│ Akubra wide brim          → Australia ONLY       │
│ Bobby helmet police       → UK ONLY              │
│ Orange monk robes         → Thailand / SE Asia   │
│ Red+white keffiyeh        → Saudi / Jordan       │
│ Conical non la hat        → Vietnam ONLY         │
│ Kalpak pointed hat        → Kyrgyzstan ONLY      │
│ Basotho blanket           → Lesotho ONLY         │
│ Gaucho beret              → Argentina / Uruguay  │
│ Ushanka fur hat           → Russia / Central Asia│
│ Bearskin guard hat        → UK (palace) ONLY     │
│ School blazer + tie       → UK / Australia / NZ  │
│ White + neckerchief       → Cuba / China kids    │
└──────────────────────────────────────────────────┘
```
- based on any animal visible in the photo
```
ANIMALS QUICK CHECKLIST

INSTANTLY UNIQUE (seen = country confirmed)
├── Kangaroo                             → Australia ONLY
├── Koala                                → Australia ONLY
├── Wombat                               → Australia ONLY
├── Platypus                             → Australia ONLY
├── Tasmanian Devil                      → Tasmania Australia ONLY
├── Lemur                                → Madagascar ONLY
├── Kiwi bird                            → New Zealand ONLY
├── Giant Panda                          → China ONLY
├── Komodo Dragon                        → Indonesia (Komodo) ONLY
├── Quokka                               → Australia (Rottnest Island) ONLY
├── Capybara (wild, large rodent)        → South America ONLY
├── Jaguar                               → Central / South America ONLY
├── Llama / Alpaca (wild)                → Peru / Bolivia / Chile ONLY
├── Bison roaming free                   → USA / Canada (national parks)
└── Polar Bear                           → Arctic (Canada / Norway / Russia)

LARGE MAMMALS
├── ELEPHANTS
│     Very large ears, flat back         → Africa (savanna / forest)
│     Smaller ears, domed back           → India / Sri Lanka / SE Asia
│     Used as working animal             → Thailand / Myanmar / Sri Lanka
│
├── RHINO
│     Two horns, grey, square lip        → Africa (Kenya / South Africa)
│     One horn, grey, folded skin        → India / Nepal ONLY
│
├── HIPPO
│     In or near river                   → Sub-Saharan Africa ONLY
│
├── GIRAFFE
│     Open savanna, acacia trees         → East / Southern Africa ONLY
│
├── ZEBRA
│     Open grassland, mixed herds        → East / Southern Africa ONLY
│
├── LION
│     Open savanna, dry grass            → Sub-Saharan Africa
│     Forested / grassland (rare)        → India (Gir Forest) ONLY
│
├── TIGER
│     Dense jungle or grassland          → India / Bangladesh / Russia (Siberia)
│     Managed reserve setting            → India most likely
│
├── GORILLA
│     Dense rainforest                   → Congo / Rwanda / Uganda ONLY
│
├── MOOSE
│     Boreal forest / wetlands           → Canada / Scandinavia / Russia
│     Near road in forest                → Canada / Alaska / Scandinavia
│
├── GRIZZLY / BROWN BEAR
│     Near salmon river / forest         → Alaska / Canada / Russia
│     Mountain setting                   → USA Rockies / European Alps
│
└── REINDEER / CARIBOU
      Semi-domesticated near people      → Finland / Norway / Sweden
      Wild large herds                   → Canada / Alaska / Russia

MEDIUM MAMMALS
├── RED FOX in urban setting             → UK / Western Europe
├── RACCOON near buildings               → USA / Canada ONLY
├── COYOTE in suburban area              → USA / Mexico ONLY
├── DINGO (wild dog, sandy)              → Australia ONLY
├── HYENA near settlement                → Sub-Saharan Africa / North Africa
├── BABOON near roads                    → South Africa / East Africa
├── MACAQUE monkey near temples          → India / Japan / Southeast Asia
├── PROBOSCIS MONKEY (huge nose)         → Borneo (Malaysia) ONLY
├── SNOW MONKEY in hot spring            → Japan ONLY
└── MEERKAT standing upright             → South Africa / Namibia / Botswana

BIRDS
├── EMU (large flightless, grey)         → Australia ONLY
├── OSTRICH (very large, long neck)      → Africa / Australia (farmed)
├── CASSOWARY (blue head, black)         → Australia / Papua New Guinea
├── FLAMINGO (pink, standing in water)   → East Africa / Caribbean / Spain
├── TOUCAN (huge colorful beak)          → Central / South America ONLY
├── MACAW (large colorful parrot)        → South / Central America ONLY
├── PELICAN on coast                     → USA / Australia / Africa / Europe
├── PUFFIN (black/white, colorful beak)  → Iceland / UK / Norway / Canada
├── PENGUIN (wild)
│     Large Emperor / King               → Antarctica / South Georgia
│     Smaller Jackass penguin            → South Africa ONLY
│     Small blue penguin                 → New Zealand / Australia
├── CONDOR (huge black soaring)          → Andes (Peru / Chile / Argentina)
├── BALD EAGLE soaring                   → USA / Canada ONLY
├── RED KITE (rusty red, forked tail)    → UK / Spain / Germany
├── PEACOCK roaming free                 → India / Sri Lanka
├── HORNBILL (large curved beak)         → Southeast Asia / Africa
└── BIRDS OF PARADISE                   → Papua New Guinea ONLY

REPTILES
├── CROCODILE in river                   → Australia / Africa / SE Asia
├── ALLIGATOR near water                 → USA (Florida / Louisiana) ONLY
├── GHARIAL (long thin snout)            → India / Nepal ONLY
├── MONITOR LIZARD near water            → Southeast Asia / Australia / Africa
├── IGUANA roaming freely                → Caribbean / Central America
├── CHAMELEON in tree                    → Madagascar / East Africa
└── COBRA near settlement                → India / Southeast Asia / Africa

MARINE / COASTAL
├── GREAT WHITE SHARK (news / signs)     → South Africa / Australia / USA
├── DUGONG / MANATEE                     → Southeast Asia / Caribbean / Africa
├── SEA LION on rocks                    → USA Pacific / South America / NZ
├── FUR SEAL colony on beach             → South Africa / New Zealand / Alaska
├── WHALE SHARK near boats               → Philippines / Mexico / Australia
└── CLOWNFISH / REEF FISH visible        → Indo-Pacific / Red Sea / Caribbean

FARM / WORKING ANIMALS
├── WATER BUFFALO plowing field          → Southeast Asia / India / China
├── CAMEL as transport                   → Middle East / North Africa / Central Asia
├── YAK carrying loads                   → Tibet / Nepal / Mongolia ONLY
├── ELEPHANT being ridden                → Thailand / India / Sri Lanka
├── DONKEY common on roads               → Middle East / North Africa / Mediterranean
├── ZEBU (humped cattle)                 → India / East Africa / Southeast Asia
└── LLAMA carrying loads                 → Peru / Bolivia ONLY

INSECTS / SMALL CREATURES
├── GIANT CENTIPEDE visible              → Southeast Asia / Australia / South America
├── SCORPION common                      → Middle East / North Africa / SW USA
├── TARANTULA crossing road              → South America / SW USA / Southeast Asia
└── FIREFLIES at dusk                    → USA / Japan / Southeast Asia

INSTANT COUNTRY TELLS
┌──────────────────────────────────────────────────┐
│ Kangaroo                  → Australia ONLY       │
│ Lemur                     → Madagascar ONLY      │
│ Giant Panda               → China ONLY           │
│ Snow monkey in hot spring → Japan ONLY           │
│ One horned rhino          → India / Nepal ONLY   │
│ Proboscis monkey          → Borneo ONLY          │
│ Quokka                    → Australia ONLY       │
│ Capybara wild             → South America ONLY   │
│ Alligator                 → USA Southeast ONLY   │
│ Gharial                   → India / Nepal ONLY   │
│ Bald eagle                → USA / Canada ONLY    │
│ Condor soaring            → Andes ONLY           │
│ Birds of paradise         → Papua New Guinea     │
│ Yak carrying loads        → Tibet / Nepal        │
│ Jackass penguin           → South Africa ONLY    │
│ Dingo                     → Australia ONLY       │
│ Raccoon near buildings    → USA / Canada ONLY    │
│ Llama carrying loads      → Peru / Bolivia       │
└──────────────────────────────────────────────────┘
```
- architectural clues are always there in the nature
```
ARCHITECTURAL QUICK CHECKLIST

BUILDING MATERIAL
├── Timber framing (exposed wood beams)  → UK / Germany / Northern Europe
├── Adobe / mud brick                    → Middle East / North Africa / SW USA
├── Red brick dense rows                 → UK / Netherlands / Eastern USA
├── White painted plaster                → Greece / Mediterranean / Middle East
├── Concrete Soviet blocks               → Former Soviet states / Eastern Europe
└── Bamboo / wood stilts                 → Southeast Asia / Pacific Islands

WINDOWS
├── Very large windows, thin walls       → Netherlands / Scandinavia
├── Small windows, thick walls           → Middle East / North Africa
├── Shutters on every window             → France / Italy / Spain
├── Sash windows (slide up/down)         → UK / USA (older buildings)
└── Floor-to-ceiling glass               → Modern Scandinavia / Japan

WALLS
├── Exposed red/orange brick             → UK / Netherlands / Eastern USA
├── White smooth plaster                 → Greece / Mediterranean
├── Wooden horizontal planks             → Scandinavia / New England USA
├── Wooden vertical planks               → Rural Japan / Korea
├── Stone walls (rough cut)              → UK countryside / Ireland / France
└── Corrugated iron walls                → Rural Africa / Pacific Islands

BALCONIES
├── ornate iron railings                 → France / Italy / Spain
├── Wide wooden balconies                → Scandinavia / Alpine Europe
├── Concrete slab balconies              → Soviet bloc / Eastern Europe
├── No balconies at all                  → UK terraced houses
└── Ornate carved wood balconies         → Rural Russia / Turkey

RELIGIOUS BUILDINGS
├── Gold onion dome                      → Russia / Orthodox countries
├── Blue dome white walls                → Greece (Cyclades islands) ONLY
├── Gothic tall stone spires             → Western Europe
├── Minaret (tall thin tower)            → Muslim-majority country
├── Pagoda tiered tower                  → East / Southeast Asia
├── Wooden stave church                  → Norway ONLY
└── Corrugated iron church               → Pacific Islands / rural Africa

UNIQUE STRUCTURES
├── Windmills (white, rotating)          → Netherlands ONLY
├── Canal houses (tall narrow)           → Netherlands / Belgium
├── Trulli (cone stone roofs)            → Puglia, Italy ONLY
├── Rondavel (round thatched hut)        → South Africa / Zimbabwe
├── Longhouse (very long wooden)         → Malaysia / Borneo / Scandinavia
└── Ger / Yurt (round felt tent)         → Mongolia / Central Asia ONLY

FENCES / WALLS
├── Dry stone walls (no mortar)          → UK / Ireland countryside
├── White picket fence                   → USA / Australia
├── Tall concrete security walls         → South Africa / Latin America
├── Bamboo fencing                       → Japan / Southeast Asia
└── Mud brick compound walls             → West Africa / Middle East

QUICK TELLS
┌─────────────────────────────────────────────────┐
│ Windmill                → Netherlands           │
│ Trulli cone roof        → Italy (Puglia) ONLY   │
│ Yurt / Ger              → Mongolia ONLY         │
│ Blue dome               → Greece (Cyclades)     │
│ Gold onion dome         → Russia                │
│ Wooden stave church     → Norway                │
│ Canal narrow houses     → Netherlands           │
│ Rondavel round hut      → Southern Africa       │
│ Adobe mud brick         → Middle East / SW USA  │
│ Soviet concrete blocks  → Eastern Europe        │
└─────────────────────────────────────────────────┘
```
- Infrastructure clues are always there as well
```
INFRASTRUCTURE QUICK CHECKLIST

POWER POLES
├── Wooden poles          → USA / Canada / Japan
├── Concrete poles        → Europe / Russia / China
└── No visible poles      → Dense urban Europe (underground)

TRAFFIC LIGHTS
├── Horizontal mount      → USA / Canada / some Asia
└── Vertical mount        → Europe / UK / Australia / most world

ROAD CENTER LINE
├── Yellow center line    → USA / Canada / Brazil / Japan
└── White center line     → Europe / Australia / most world

POSTBOXES
├── Red standalone        → UK
├── Yellow               → Germany / France / Spain
├── Blue                 → USA
├── Green                → Ireland / Italy / China
└── Orange               → Netherlands

POLICE CARS
├── Black and white       → USA
├── Yellow/blue checker   → UK
├── Green and yellow      → Australia / New Zealand
└── All white + stripe    → Russia / Eastern Europe

BUS TYPE
├── Red double decker     → UK / Hong Kong
├── Yellow school bus     → USA / Canada ONLY
├── Tuk-tuk / rickshaw    → Thailand / India / Sri Lanka
└── Jeepney               → Philippines ONLY

MANHOLE COVERS
├── Ornate / artistic     → Japan
├── City crest cast iron  → UK / Europe
└── Plain grey concrete   → Developing world

FUEL STATION BRAND
├── Petrobras             → Brazil
├── PEMEX                 → Mexico
├── Lukoil / Rosneft      → Russia
├── Indian Oil / HPCL     → India
├── Sinopec / PetroChina  → China
└── Pertamina             → Indonesia
```
- stars can be used to pin point location and time
```
1. Identify visible stars/constellations
2. Use Stellarium (stellarium.org)
3. Input suspected location
4. Adjust time until star positions match
```
- traffic / wheel direction / license plate / car-info 
```
STEP 1: Which SIDE of road are cars driving on?

├── LEFT SIDE of road (overtaking on right)
│     → Steering wheel on RIGHT side of car
│     → Go to Step 2
│
└── RIGHT SIDE of road (overtaking on left)
      → Steering wheel on LEFT side of car
      → Go to Step 3

STEP 2: LEFT-HAND TRAFFIC countries
├── ASIA
│     Circular script on plates            → Thailand / Sri Lanka
│     Japanese characters on plates        → Japan
│     Hindi / Devanagari on plates         → India / Nepal
│     English only, yellow rear plate      → Malaysia / Singapore
│
├── OCEANIA
│     Plates vary by state, kangaroo       → Australia
│     Similar but smaller country code     → New Zealand
│
├── AFRICA
│     Yellow/white plates, English         → South Africa / Kenya
│     Green/white plates                   → Zimbabwe / Zambia
│
├── EUROPE
│     Blue strip left, GB/UK letters       → United Kingdom
│     Blue strip left, IRL letters         → Ireland
│     Blue strip left, M letters           → Malta
│     Blue strip left, CY letters          → Cyprus
│
└── CARIBBEAN
      English text, island name on plate   → Jamaica / Barbados / Trinidad

STEP 3: RIGHT-HAND TRAFFIC countries
├── NORTH AMERICA
│     Varies wildly by state, no EU strip  → USA
│     Province name on top or bottom       → Canada
│     Green/white, Spanish text            → Mexico
│
├── EUROPE (all right-hand traffic)
│     Blue EU strip on left + country code
│       D   = Germany    F  = France
│       E   = Spain      I  = Italy
│       NL  = Netherlands B = Belgium
│       PL  = Poland     RO = Romania
│       SE  = Sweden     NO = Norway
│       CH  = Switzerland A = Austria
│     White plate, black text, EU strip    → Most of EU
│     Yellow plate front AND rear          → Netherlands specifically
│     Yellow rear only, white front        → UK (older pre-2021 style)
│
├── RUSSIA / EASTERN EUROPE
│     Cyrillic letters on plate            → Russia / Belarus
│     RUS code, white plate black text     → Russia
│     UA code                              → Ukraine
│
├── MIDDLE EAST
│     Arabic script on plate               → Saudi / UAE / Jordan / Egypt
│     Hebrew + English on plate            → Israel
│     Both Arabic and Latin               → Lebanon / Morocco / Tunisia
│
├── EAST ASIA
│     Chinese characters, red background   → China (government vehicles)
│     Chinese characters, white plate      → China (standard)
│     Korean characters on plate           → South Korea
│
└── SOUTH AMERICA
      All right-hand traffic
      MERCOSUR blue strip on left          → Brazil / Argentina / Chile
      Spanish text, country flag strip     → Colombia / Peru / Venezuela

STEP 4: PLATE COLOR QUICK ID
├── All yellow (front + rear)              → Netherlands
├── Red background                         → China government / diplomatic
├── Green background                       → Electric vehicle (many countries)
├── Black plate white text                 → Germany (vintage) / some US states
├── Blue plate white text                  → France (old) / some EU diplomatic
├── White plate blue EU strip left         → Standard EU country
├── No EU strip, highly variable           → USA / Canada / Australia
└── Arabic script only                     → Gulf states (Saudi / UAE / Kuwait)

QUICK CHEAT SHEET
┌─────────────────────────────────────────────────┐
│ LEFT traffic  → UK, Australia, Japan, India      │
│ RIGHT traffic → USA, Europe, China, Russia       │
│ Yellow plates → Netherlands (both) / UK (rear)   │
│ EU blue strip → All European Union countries     │
│ Arabic script → Gulf / Middle East               │
│ Cyrillic      → Russia / Belarus / Ukraine       │
│ No strip,     → USA / Canada / Australia         │
│ wildly varied                                    │
└─────────────────────────────────────────────────┘
```
- Differentiation based on roof
```
STEP 1: Is the roof FLAT?
├── YES → Has water tanks on top           → India / South Asia
│         Soviet-style concrete blocks     → Eastern Europe / Russia
│         Sandy/arid surroundings          → Middle East / North Africa
└── NO  → Step 2

STEP 2: Is it covered in GRASS or TURF?
├── YES → Iceland / Norway / Faroe Islands
└── NO  → Step 3

STEP 3: Do the eaves CURVE UPWARD at corners?
├── YES → Slight curve, dark grey tiles    → Japan
│         Dramatic curve, ornate ridge     → China
│         Very dramatic curve upward       → Thailand / Vietnam
└── NO  → Step 4

STEP 4: What is the MATERIAL?

├── ORANGE / RED clay curved tiles
│     Hot dry surroundings                 → Spain / Italy / Portugal / Greece
│     Tropical surroundings               → Latin America
│     Central European city               → Austria / Hungary / Croatia
│
├── DARK GREY / BLACK slate tiles
│     Chimney stacks visible              → UK / Ireland / Wales
│     Steep narrow houses                 → Northern France
│
├── GRASS / TURF covered                  → Iceland / Norway (Step 2)
│
├── GREEN painted METAL
│     Onion domes nearby                  → Russia
│     Plain urban buildings               → Eastern Europe / Baltic
│
├── CORRUGATED IRON / METAL
│     Rural setting                       → Australia / New Zealand
│     Urban informal housing              → Sub-Saharan Africa
│     Tropical rural                      → Southeast Asia
│
├── ASPHALT SHINGLE (dark granular)
│     Suburban sprawl                     → USA / Canada
│     Similar but steeper                 → Canada specifically
│
├── WOODEN SHINGLES (weathered grey/brown)
│     Coastal or forested setting         → Scandinavia / Canada
│     Siberian wilderness                 → Russia (Siberia)
│
└── VERY STEEP dark wood or slate
      Alpine mountains visible            → Switzerland / Austria / Bavaria
      Forested flat land                  → Scandinavia / Finland

STEP 5: Any UNIQUE ROOF SHAPES?
├── Stepped gable ends (staircase profile) → Netherlands
```
- identifying language based on signs
```
STEP 1: Right-to-left text?
├── YES → Flowing cursive horizontal    → Arabic
│         Diagonal flowing (Nastaliq)   → Urdu
│         Blocky square letters         → Hebrew
│         Arabic + adds پ چ ژ           → Persian
└── NO  → Step 2

STEP 2: Vertical text (top → bottom)?
├── YES → Mongolian (only vertical script)
└── NO  → Step 3

STEP 3: Latin alphabet (A-Z)?
├── NO  → Step 4
└── YES → Step 5

STEP 4: Which non-Latin script?
├── Looks like А Б В (Russian-style)    → Cyrillic family
│     Has ы э ъ                         → Russian
│     Has і ї є                         → Ukrainian
│     Has ђ љ њ                         → Serbian
│     Similar to Russian                → Bulgarian
├── Horizontal bar on top (क ख)         → Hindi / Nepali
├── α β γ δ familiar but not Latin      → Greek
├── Circular loops (ก ข ค)             → Thai
├── Rounded curvy unique (ა ბ გ)        → Georgian
├── Complex syllabic (አ ቡ)             → Amharic
└── Rounded unique (Ա Բ Գ)             → Armenian

STEP 5: Complex square CJK characters?
├── YES → Mixed with rounded あ or angular ア → Japanese
│         Only complex squares               → Chinese
│         Geometric blocks with circles 한   → Korean
└── NO  → Step 6

STEP 6: Any special/accented characters?
├── NO  → English / Malay / Indonesian / Swahili
└── YES → Step 7

STEP 7: Which character?
├── ñ  or  ¿ ¡                          → Spanish
├── ß  or  ä ö ü                        → German
├── é  è  ê  ç  œ                       → French
├── ã  õ  ç                             → Portuguese
├── ı  (dotless i)  ğ  ş               → Turkish
├── ắ  ề  ổ  (stacked accents)          → Vietnamese
├── ą  ę  ł  ź  ż                       → Polish
├── č  ř  ž  š  (háček ˇ)              → Czech / Slovak
├── ő  ű  (double acute)                → Hungarian
├── å  æ  ø                             → Norwegian / Danish
├── å  ä  ö  (no æ/ø)                  → Swedish
└── ș  ț  â  î                          → Romanian
```
- based on shop signs and shop culture
```
FOOD STALLS / SHOP SIGNAGE QUICK CHECKLIST

CONVENIENCE STORE CHAINS
├── 7-Eleven (very dense concentration)  → Thailand (most per capita globally)
├── 7-Eleven + FamilyMart + Lawson       → Japan / Taiwan / China
├── FamilyMart dominant                  → Japan / South Korea / Taiwan
├── Lawson dominant                      → Japan / China
├── CU / GS25 / Emart24                  → South Korea ONLY
├── Spar                                 → Netherlands / UK / South Africa
├── Tesco Express                        → UK / Thailand / Hungary
└── Coles Express / IGA                  → Australia ONLY

SUPERMARKET CHAINS
├── Woolworths / Coles                   → Australia ONLY
├── Pick n Pay / Shoprite / Checkers     → South Africa ONLY
├── Lidl / Aldi green logo               → Europe broadly
├── Carrefour (blue/red/white)           → France / Spain / Belgium / Middle East
├── Tesco (red + blue stripe)            → UK / Ireland / Central Europe
├── Sainsburys / Morrisons / Asda        → UK ONLY
├── Walmart / Target / Kroger            → USA ONLY
├── Loblaws / Metro / Sobeys             → Canada ONLY
├── Migros / Coop (orange/red)           → Switzerland ONLY
├── Rewe / Edeka                         → Germany ONLY
├── Mercadona                            → Spain ONLY
├── Pão de Açúcar / Extra               → Brazil ONLY
└── Reliance Fresh / Big Bazaar          → India ONLY

FAST FOOD REGIONAL TELLS
├── Jollibee (red bee mascot)            → Philippines ONLY
├── Lotteria                             → South Korea / Vietnam / Japan
├── Mos Burger                           → Japan / Taiwan / Southeast Asia
├── Popeyes very dominant                → USA / Canada
├── Whataburger (orange/white striped)   → USA South / Texas ONLY
├── In-N-Out Burger                      → USA West Coast ONLY
├── Nandos (flame logo)                  → South Africa / UK / Australia
├── Wimpy                                → South Africa / UK
├── Steers / Debonairs                   → South Africa ONLY
├── Hungry Jacks                         → Australia ONLY (Burger King rebranded)
├── Max Burgers                          → Sweden ONLY
├── Bob's Burgers                        → Brazil ONLY
├── Hesburger                            → Finland / Baltic states ONLY
└── Hardees dominant                     → USA Southeast / Middle East

STREET FOOD STALLS
├── Pad Thai / Som Tam stall             → Thailand
├── Pho cart / stall                     → Vietnam ONLY
├── Banh Mi stand                        → Vietnam ONLY
├── Satay grill on street               → Malaysia / Indonesia / Singapore
├── Taco / burrito cart                  → Mexico / USA Southwest
├── Arepa stall                          → Colombia / Venezuela ONLY
├── Empanada cart                        → Argentina / Chile / Colombia
├── Kebab / doner stand                  → Turkey / Germany / Middle East
├── Jerk chicken drum barrel             → Jamaica / Caribbean ONLY
├── Bunny chow stall                     → South Africa (Durban) ONLY
├── Roti canai stall                     → Malaysia / Singapore ONLY
├── Dim sum cart                         → China / Hong Kong / SE Asia
├── Takoyaki / yakitori stall            → Japan ONLY
├── Currywurst stand                     → Germany (Berlin) ONLY
└── Borek / simit cart                   → Turkey ONLY

DRINK / BEVERAGE BRANDS
├── Efes beer signs                      → Turkey ONLY
├── Tusker beer                          → Kenya ONLY
├── Castle / Black Label beer            → South Africa ONLY
├── XXXX / VB / Tooheys beer             → Australia ONLY
├── Kingfisher beer signs                → India ONLY
├── Chang / Singha beer                  → Thailand ONLY
├── Tiger beer dominant                  → Singapore / Malaysia / Vietnam
├── Bintang beer                         → Indonesia ONLY
├── Modelo / Corona dominant             → Mexico ONLY
├── Quilmes beer                         → Argentina ONLY
├── Cristal beer                         → Peru / Chile
├── Brahma / Skol / Antarctica           → Brazil ONLY
├── Keo / Leon beer                      → Cyprus ONLY
└── Mythos / Fix beer                    → Greece ONLY

TELECOM / PHONE SHOP SIGNS
├── Jio / Airtel / BSNL / Vi             → India ONLY
├── Turkcell / Vodafone TR / Turk Telekom→ Turkey ONLY
├── MTN / Vodacom / Cell C               → South Africa / Sub-Saharan Africa
├── M-Pesa signage (mobile money)        → Kenya / East Africa ONLY
├── Safaricom green shops                → Kenya ONLY
├── Orange / SFR / Bouygues              → France / French-speaking countries
├── Telstra / Optus / Vodafone AU        → Australia ONLY
├── NTT Docomo / SoftBank / au           → Japan ONLY
├── SK Telecom / KT / LG U+              → South Korea ONLY
├── China Mobile / China Unicom          → China ONLY
└── Digicel shops                        → Caribbean / Pacific Islands ONLY

PHARMACY / DRUGSTORE SIGNS
├── Boots (blue logo)                    → UK / Ireland / Thailand
├── CVS / Walgreens / Rite Aid           → USA ONLY
├── Shoppers Drug Mart                   → Canada ONLY
├── Priceline Pharmacy                   → Australia ONLY
├── Apotheke (green cross)               → Germany / Austria / Switzerland
├── Pharmacie (green cross, France)      → France / Belgium / Luxembourg
├── Cruz Verde / Cruz Blanca             → Chile / Colombia / Mexico
└── Green cross sign broadly             → European pharmacy (universal)

PRODUCT PACKAGING CLUES
├── Cyrillic text on packaging           → Russia / Eastern Europe
├── Arabic text on packaging             → Middle East / North Africa
├── Thai script on packaging             → Thailand
├── Hebrew text on packaging             → Israel
├── Korean script on packaging           → South Korea
├── Chinese + English on packaging       → China / Taiwan / Singapore
├── Metric units only (ml, kg)           → Most of world (non-USA)
├── Imperial units (oz, lbs, fl oz)      → USA primarily
└── Dual metric + imperial               → Canada / UK

MARKET / BAZAAR TYPES
├── Night market (many small stalls lit) → Taiwan / Thailand / Malaysia
├── Wet market (fresh meat/fish/veg)     → Southeast Asia / Hong Kong / China
├── Souq / souk (covered narrow lanes)   → Middle East / North Africa ONLY
├── Grand Bazaar style (arched ceiling)  → Turkey / Iran / Central Asia
├── Floating market (stalls on boats)    → Thailand / Vietnam / Indonesia
├── Sunday car boot / flea market        → UK / Australia / New Zealand
├── Farmers market (organic focus)       → USA / Western Europe / Australia
└── Mercado (covered, colorful stalls)   → Latin America broadly

QUICK TELLS
┌──────────────────────────────────────────────────┐
│ Jollibee red bee           → Philippines ONLY    │
│ Hungry Jacks               → Australia ONLY      │
│ Whataburger orange stripe  → USA South ONLY      │
│ M-Pesa signage             → Kenya / East Africa │
│ Safaricom green            → Kenya ONLY          │
│ Bunny chow stall           → South Africa ONLY   │
│ Currywurst stand           → Germany ONLY        │
│ Pho cart                   → Vietnam ONLY        │
│ Banh Mi stand              → Vietnam ONLY        │
│ Takoyaki stall             → Japan ONLY          │
│ Borek / simit cart         → Turkey ONLY         │
│ Jerk chicken drum barrel   → Jamaica ONLY        │
│ Efes beer signs            → Turkey ONLY         │
│ Tusker beer                → Kenya ONLY          │
│ Kingfisher beer            → India ONLY          │
│ Bintang beer               → Indonesia ONLY      │
│ Hesburger                  → Finland ONLY        │
│ Max Burgers                → Sweden ONLY         │
│ Quilmes beer               → Argentina ONLY      │
│ Roti canai stall           → Malaysia ONLY       │
└──────────────────────────────────────────────────┘
```
- boat and harbors verification 
```
BOATS / HARBORS QUICK CHECKLIST

FISHING BOATS
├── Longtail boat (long engine arm)      → Thailand ONLY
├── Banca boat (outriggers both sides)   → Philippines ONLY
├── Jukung (small outrigger, colorful)   → Indonesia / Bali ONLY
├── Dhow (triangular lateen sail)        → Oman / UAE / East Africa / Yemen
├── Felucca (small wooden sail)          → Egypt / Nile ONLY
├── Sampan (flat bottom, oar steered)    → China / Vietnam / Hong Kong
├── Coracle (round wicker boat)          → Wales / India (Bhutan)
├── Dinghy / trawler, plain white        → UK / Ireland / Northern Europe
├── Colorful wooden hull, Mediterranean  → Greece / Turkey / Croatia / Italy
└── Pirogues (carved wooden canoe)       → West Africa / Pacific Islands

COMMERCIAL / CARGO VESSELS
├── Massive container ships              → Major ports globally (Singapore / Rotterdam)
├── River barges (long flat)             → Europe (Rhine / Danube) / China / Russia
├── Traditional junk (red sails)         → China / Hong Kong ONLY
├── Ferry with cars on deck              → Scandinavia / Greece / UK / Japan
└── Small inter-island ferries           → Philippines / Indonesia / Greece

HARBOR / PORT CLUES
├── HARBOR TYPE
│     Natural fjord harbor               → Norway / New Zealand / Chile
│     Artificial modern container port   → Singapore / Rotterdam / Shanghai
│     Old stone quay walls               → UK / Ireland / Malta / Croatia
│     Floating pontoon docks             → USA / Australia / Scandinavia
│     Wooden stilted dock                → Southeast Asia / Pacific Islands
│
├── HARBOR SURROUNDINGS
│     Whitewashed buildings on cliffs    → Greece (Santorini / Mykonos)
│     Red/orange roofed town behind      → Croatia / Montenegro / Italy
│     Green hills steep into water       → Norway / New Zealand
│     Flat desert behind harbor          → UAE / Saudi / Oman
│     Dense urban skyscrapers            → Hong Kong / Singapore / Dubai
│     Wooden fishing shacks on stilts    → Southeast Asia / Pacific
│
└── HARBOR EQUIPMENT
      Red/green buoys (standard)         → Most of world (IALA system)
      Yellow buoys dominant              → Some Asian waters
      Wooden fish traps in water         → Southeast Asia / Middle East
      Lobster pot stacks on dock         → UK / Ireland / Canada / Maine USA
      Massive cranes, container stacks   → Major global shipping ports

SEA / WATER COLOR
├── Turquoise / crystal clear            → Caribbean / Maldives / Mediterranean
├── Deep blue, very clear                → Open Pacific / Atlantic / Mediterranean
├── Green tinted                         → North Sea / Baltic / Irish Sea
├── Brown / murky                        → River mouths / Bangladesh / Indonesia
├── Grey, choppy, cold looking           → North Atlantic / UK / Norway
└── Red tinted (algae bloom)             → Specific seasonal event anywhere

BOAT ACTIVITY CLUES
├── Rice barge on river                  → Vietnam / Myanmar / Thailand
├── Gondola in canal                     → Venice Italy ONLY
├── Abra (wooden water taxi)             → Dubai Creek ONLY
├── Water taxi, very busy crossing       → Bangladesh / Vietnam / Philippines
├── Speedboat towing skiers              → USA / Australia / Western Europe
└── Reed boat (totora reeds)             → Lake Titicaca Peru / Bolivia ONLY

QUICK TELLS
┌──────────────────────────────────────────────────┐
│ Longtail boat          → Thailand ONLY           │
│ Banca outrigger        → Philippines ONLY        │
│ Gondola                → Venice Italy ONLY       │
│ Dhow triangular sail   → Middle East / E Africa  │
│ Felucca                → Egypt / Nile ONLY       │
│ Red sailed junk        → China / Hong Kong       │
│ Abra water taxi        → Dubai ONLY              │
│ Reed boat              → Lake Titicaca ONLY      │
│ Coracle round boat     → Wales / India           │
│ Jukung outrigger       → Bali / Indonesia        │
│ Lobster pots on dock   → UK / Ireland / Canada   │
│ Whitewashed cliff port → Greece                  │
└──────────────────────────────────────────────────┘
```
- night and artificial lighting quick checklist
```
NIGHT / ARTIFICIAL LIGHTING QUICK CHECKLIST

STREET LIGHT COLOR
├── Warm orange / yellow (sodium)        → UK / older Europe / Middle East
├── Cool white / blue-white (LED)        → Modern cities globally post 2015
├── Warm white LED                       → Japan / South Korea / Modern Europe
├── Green tinted lights                  → Some older US cities / Latin America
└── Inconsistent mix of colors           → Developing world / older infrastructure

NEON SIGNS / SIGNAGE
├── Dense Chinese neon / LED signs       → Hong Kong / China / Taiwan
├── Dense Korean neon signage            → South Korea (Gangnam / Seoul)
├── Japanese neon + vending machines lit → Japan (very dense vending machines)
├── Arabic neon signs                    → Middle East / North Africa
├── Cyrillic neon signs                  → Russia / Eastern Europe
├── Hebrew neon signs                    → Israel ONLY
├── Thai script neon                     → Thailand
└── Latin script only neon               → USA / Europe / Latin America

LIGHT DENSITY FROM ABOVE
├── Extremely dense bright grid          → USA (city grid pattern)
├── Dense but organic pattern            → Europe / older cities
├── Very dense + river visible dark      → Major river cities (London/Paris/Cairo)
├── Sparse with dark gaps                → Developing world / rural areas
├── Almost no light                      → North Korea ONLY (from satellite)
└── Single bright spot in darkness       → Remote mining / oil facility

SPECIFIC LIGHTING CLUES
├── VENDING MACHINES lit on street       → Japan ONLY (extremely dense)
├── Red lanterns hanging lit             → China / Chinese New Year / Vietnam
├── Neon cross on buildings              → South Korea (churches everywhere)
├── Green mosque lights at night         → Muslim-majority countries
├── Orthodox church lit gold at night    → Russia / Greece / Eastern Europe
├── Blue/white lit buildings             → Greece / Israel (national colors)
├── Casino strip lighting                → Las Vegas USA / Macau China / Monaco
└── Stadium floodlights visible          → Urban areas globally

TRAFFIC LIGHT COLOR
├── Standard red/amber/green             → Most of world
├── Countdown timer on lights            → China / South Korea / India
├── Flashing green before red            → Some Eastern European countries
└── Pedestrian light makes sound         → Japan / South Korea / Australia

QUICK TELLS
┌──────────────────────────────────────────────────┐
│ Dense vending machines lit  → Japan ONLY         │
│ Almost no light from above  → North Korea ONLY   │
│ Neon cross on buildings     → South Korea        │
│ Green mosque lights         → Muslim countries   │
│ Red lanterns hanging        → China / Vietnam    │
│ Hebrew neon signs           → Israel ONLY        │
│ Casino strip lighting       → Las Vegas / Macau  │
│ Orange sodium streetlights  → UK / older Europe  │
│ Countdown traffic timers    → China / India      │
└──────────────────────────────────────────────────┘
```
- sports and recreation checklist
```
SPORTS / RECREATION QUICK CHECKLIST

PITCH / FIELD TYPE
├── CRICKET PITCH (flat oval, 22 yard strip center)
│     White flannels, red ball            → UK / Australia / NZ / South Africa
│     Colored kit, white ball             → India / Pakistan / Sri Lanka / UAE
│
├── BASEBALL DIAMOND (90° infield dirt)
│     Large stadiums, very common         → USA / Canada
│     Smaller setups                      → Japan / Cuba / Dominican Republic
│
├── AUSTRALIAN RULES FOOTBALL
│     Oval pitch, tall H + behind posts   → Australia ONLY
│
├── RUGBY PITCH (H posts, rectangular)
│     Very common, village level          → UK / NZ / South Africa / France
│     Less common                         → Australia / Argentina / Japan
│
├── AMERICAN FOOTBALL (yard lines visible)
│     Very wide H posts                   → USA / Canada ONLY
│
├── GAELIC FOOTBALL / HURLING pitch       → Ireland ONLY
│
└── SEPAK TAKRAW court (rattan ball net)  → Southeast Asia ONLY

COURT TYPES
├── Basketball court (very common)        → USA / Global urban areas
├── Petanque / boules gravel court        → France / Southern Europe ONLY
├── Padel court (glass walls)             → Spain / Latin America / Middle East
├── Gateball court                        → Japan / China (elderly sport)
└── Volleyball on beach very common       → Brazil / USA / Europe coastal

WINTER SPORTS
├── Ski lifts on mountain                 → Alps / Rockies / Andes / Japan
├── Outdoor ice rink natural              → Canada / Russia / Scandinavia
├── Curling rink                          → Canada / Scotland / Scandinavia
└── Ski jump structure visible            → Norway / Austria / Finland / Japan

WATER SPORTS
├── Surfing (beach break, consistent)     → Australia / Hawaii / Portugal / South Africa
├── Dragon boat racing                    → China / Hong Kong / Taiwan / Vietnam
├── Waka ama (outrigger canoe racing)     → New Zealand / Pacific Islands ONLY
└── Kite surfing common                   → Netherlands / Brazil / Australia

RECREATIONAL CLUES
├── Pétanque players in park              → France / Southern Europe
├── Chess players in public park          → Russia / Eastern Europe / China
├── Tai chi in park at dawn               → China / Taiwan / SE Asian Chinese communities
├── Sumo wrestling                        → Japan ONLY
├── Kabaddi being played                  → India / Bangladesh / Pakistan
├── Capoeira on street                    → Brazil ONLY
├── Zorbing / bungee common               → New Zealand (adventure capital)
└── Polo being played                     → Argentina / UK / Pakistan / UAE

STADIUM CLUES
├── Very large, modern, air conditioned   → UAE / Qatar / Saudi (indoor stadiums)
├── Old Victorian brick stadium           → UK ONLY
├── Wooden bleachers, small               → USA rural / Latin America
├── Standing terraces still in use        → Eastern Europe / South America
└── Retractable roof stadium              → USA / Australia / Canada / Japan

QUICK TELLS
┌──────────────────────────────────────────────────┐
│ Cricket oval pitch        → British Commonwealth │
│ Baseball diamond          → USA / Japan / Cuba   │
│ AFL oval + behind posts   → Australia ONLY       │
│ Gaelic football           → Ireland ONLY         │
│ Petanque gravel court     → France / S Europe    │
│ Sepak takraw net          → SE Asia ONLY         │
│ Sumo ring                 → Japan ONLY           │
│ Capoeira on street        → Brazil ONLY          │
│ Dragon boat racing        → China / SE Asia      │
│ Waka ama canoe            → NZ / Pacific ONLY    │
│ Kabaddi being played      → India / Pakistan     │
│ Curling rink              → Canada / Scotland    │
│ Polo match                → Argentina / UK       │
│ Victorian brick stadium   → UK ONLY              │
└──────────────────────────────────────────────────┘
```
**Since the location is now identified to the narrowest possible region, let's hop on the map engine and get to the very location !**

### STAGE MANUAL MAP HUNT (THE FINAL SWEEP)

You have a confirmed country, a probable city or region, and a shortlist of 2–4 candidate areas. This is where you stop using clues from the image and start moving through the map manually until reality matches what you see.

---

#### SET UP YOUR WORKING AREA FIRST

```
Open two windows side by side:
  Left  → the image you are locating
  Right → Google Maps (or Apple Maps / Yandex Maps as backup)

Drop a pin on your best candidate area first
Switch to Satellite view
Zoom until individual buildings / blocks are distinguishable
DO NOT open Street View yet — satellite orientation comes first
```

---

#### STEP 1 — ORIENT FROM ABOVE BEFORE GOING TO GROUND LEVEL

```
In satellite view, identify ONE structural feature from the image
that is visible from above:

  Road layout shape     → straight grid / curved / radial / irregular
  Building footprint    → rectangular / courtyard / L-shaped / detached
  Roof color / material → visible from satellite in most cities
  Green space           → park, garden, tree line adjacent to the scene
  Water feature         → river bend, canal, coastline edge
  Parking lot shape     → distinctive in satellite view

Scan your candidate area for that single feature
When you find a match → that block or that road is your entry point
If you find zero matches across the whole candidate area → wrong area,
expand outward or move to your next candidate
```

---

#### STEP 2 — NARROW TO A CANDIDATE BLOCK

```
Once you have an entry point from satellite:

  Look at the road network immediately around it
  Count how many roads intersect your candidate block
  Match that count and angle to what is visible in the image
  (a T-junction looks very different from a 4-way cross or a curve)

From the image extract one more architectural anchor:
  Which direction does the main building face? (shadow tells you this)
  Is the building on a corner, mid-block, or at a road end?
  Is there a visible gap / alley / car park immediately adjacent?

Cross-reference these two things simultaneously on satellite:
  Road geometry + building position together
  When both match → you are on the correct block
```

---

#### STEP 3 — DROP INTO STREET VIEW AND WALK IT

```
Click into Street View at the edge of your candidate block
NOT at the midpoint — start at a corner so you can see
down two roads simultaneously

Move through Street View methodically:
  ONE direction at a time
  Check left and right at every junction before advancing
  Do not skip ahead — a single storefront or pole or fence
  can be the confirmation you need

What to match first in Street View:
  1. Sky / roofline silhouette against the image
     (this is fast and eliminates the wrong side of a block immediately)
  2. Road surface width and marking
  3. Pavement / sidewalk material and width
  4. Any vegetation (single tree, hedge, planter) adjacent to the scene
  5. Storefront / signage if visible in the original image
  6. Building entrance position and door color / style
```

---

#### STEP 4 — THE COMPASS LOCK (CONFIRM YOUR FACING DIRECTION)

```
Once you think you have the right block in Street View:

  Look at the shadow direction in the original image
  Note which direction in Street View the shadow would point
  Rotate your Street View view until shadows in Street View
  would match the original if it were the same time of day

  If the facing direction is consistent with the shadow analysis
  from Stage 7 → you have confirmed orientation
  If it is not → you are either on the wrong block or
  facing the wrong way — rotate 180° and re-check before moving

This is the step most people skip and it is why they end up
one block off or on the wrong side of the street
```

---

#### STEP 5 — FINAL PIN CONFIRMATION

```
You have the right block and the right facing direction.
Now identify the single most specific detail in the image:

  Most specific = least likely to repeat anywhere else nearby

  Good specifics:
    A building with a unique color combination
    A mural or wall art
    A specific shopfront with a readable name
    A distinctive lamp post or bollard style
    A utility box or postbox in an unusual position
    A crack / patch in the road surface (visible in Street View)
    A drain / grate in a specific position relative to the kerb

  Bad specifics (too common to confirm):
    "A red car parked outside"   → not fixed
    "A tree on the left"         → too common
    "A white building"           → too common

Find that one specific detail in Street View
When it matches → place your final pin at that exact position
```

---

#### THE MINDSET FOR THE MANUAL HUNT

```
Do not try to match everything at once
Pick ONE anchor feature, find it, then use it as a base
to find the next feature from there

The guy who narrows 40 blocks to 1 is not doing anything
clever — he is doing this loop very fast:

  Pick a feature → scan → match or discard → move to next block
  repeat until only one block matches two features simultaneously

Speed comes from:
  Committing to a candidate and testing it fully before moving on
  Never trying to hold two candidate blocks in your head at once
  Using satellite to pre-eliminate whole streets before Street View
  (Street View is slow — satellite culls first, Street View confirms)

When you are stuck:
  Go back to the image and find a feature you have not used yet
  The image always has more information than you have extracted
  Change the zoom level on satellite — sometimes a feature is only
  visible at a specific zoom (a narrow alley, a small courtyard)
  Try Yandex Maps Street View or Apple Maps Look Around as a backup
  — they cover some areas where Google Street View is absent or outdated
```
