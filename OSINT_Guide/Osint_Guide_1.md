# OSINT GUIDE PART 1

### OSINT TOOLS

> This is people related OSINT not company related if you want that it's in web exploitation guide passive recon 

**last updated** - `19 August 2026`
```
person's name -> name based address search Current Best: TruePeopleSearch or 192.com (us only) 
person's name -> name based username and email generator Current Best: soxoj's username-generation-guide and transform_username tool  
person's name -> person name to phone number finder Current Best: Truecaller 
Person's name -> Company or work place finding Current best: Open corporates Then pivot to country based pools  

Username ->  Username search engines Current Best: maigret 

Image -> Reverse Image search Current Best:Yandex or Google lens 
Image -> Face image people searcher Current Best:Facecheck.id or lenso.ai {none are good}
Image -> Meta data extractor tool Current Best: Exiftool or EXIF.tools 
Image -> Street view websites Current Best: Google street view,Yandex Panorama,mapillary,kartaView,panormax (to cover all)
Image -> Advance street view query engine: Overpass-turbo (very good)
Image -> Geolocator finder and hunter Current Best: GeoSeeere.com, GeoAxis.com or picarta.ai
Image -> image based hints collection base Current Best: GeoHints.com 
Image -> Shadow to location pinpointing Current Best: SunCalc,ShadowMap and ShadowFinder 

Email address -> Email-ID search engine Current Best: Epieso or holehe  
Email address -> Email-ID format predictors Current Best:mailmeteor.com/email-permutator
Email address -> Email-ID verification engines Current Best: myemailverifier.com or verifyemailaddress.org 
Email address -> Email-ID breach data searchers Current Best: IHaveBeenPwned 
Email address -> Email-ID Mail blacklist processor Current Best: MXToolbox SuperTool

Phone number -> phone number databases Current Best: Truecaller or phoneinfoga {truecaller still dominates} 

Search Engines -> Search engine dorker Current Best:Dorkgpt or pagodo 

IP address -> to Geo location and ownership Current Best:Ipinfo and ViewDNS.info
IP address -> Exposed service and devices:Shodan or censys 

Domain name/website -> WHOIS/Ownership records Current best:WhoisXML or ICANN Lookup
Domain name/website -> Subdomain enumeration Current Best:crt.sh or Amass 
Domain name/website -> archived or deleted content Current Best:Wayback machine
Domain name/Website -> Domain name to ip address Current Best:dig
Domain name/website -> exposed document's metadata Current Best: FOCA 

Company name/ownership -> Ownership and registry Current Best: OpenCorporates or Crunchbase 

flight/ship info -> real time flight tracking Current Best:FlightRadar24 
fight/ship info -> Flight history and data Current Best:FlightAware
flight/ship info -> Marine traffic and ship tracking Current Best:MarineTraffic
flight/ship info -> Vessel and container tracking Current Best: VesselFinder
flight/ship info -> for train info and past data Current Best: openrailwaymap.org {for live it varies} 
```

### Geo-Osint 

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
- Visual Analysis of the image looking for anything extra you can get to lrn
```
High confidence visual bucket
1. Street sign (language,font-sign,contry specific design)
2. License plates (country/state format)
3. Road markings (lane colors, arrow styles vary by country)
4. Utility poles/power lines (design vary by country)
5. Architecture style (building material,roof tops,window style)
6. Vegetation (palm tree=topical,pin tree=nothern region)
7. Sun position (shadows indicate time of day + hemisphere)
```
```
medium confidence visual bucket
1. Vehicle types (left/right-hand drive, common brands in region)
2. Clothing styles (traditional dress, weather-appropriate clothing)
3. Currency/pricing visible in shops
4. Language on signs/billboards
5. Food/restaurant chains (regional fast food chains)
6. Sports teams logos/jerseys
```
```
low confidence visual bucket 
1. Sky color/cloud patterns
2. General landscape features
3. Building density (urban vs rural)
```
- Short shadow can indicate midday whereas long shadow can suggest early/late
- Shadow's pointing direction can tell the position on the country form equator
- Country specific road sign always exist here is list of the few
```
USA:        Green highway signs, mile markers, STOP in English
UK:         White/blue signs, "Give Way" instead of STOP
Germany:    Blue Autobahn signs, yellow federal road signs
France:     Blue motorway signs, green national road signs
Russia:     Cyrillic text, blue highway signs
Japan:      Japanese + Roman characters, blue expressway signs
Australia:  Green highway signs, metric distances
China:      Chinese characters + pinyin, blue highway signs
Brazil:     Green signs, Portuguese text
```
- license plate can determine the exact country based on the format
```
USA:    Multiple formats by state
UK:     "XX00 XXX" format (2 letters, 2 numbers, 3 letters)
EU:     Blue strip on left with country code
Russia: Letters-numbers-letters-region code
Japan:  Hiragana + numbers format
Australia: State-specific format
French: Modern french plates are white rectangles with blue bonds 
```
- vegetation can too determine the area as well the region for example
```
Palm trees          → tropical/subtropical (within 35° of equator)
Coniferous forests  → boreal/temperate northern regions
Eucalyptus trees    → Australia, California, parts of Africa
Baobab trees        → Africa, Madagascar
Cherry blossoms     → Japan, Korea, China (spring indicator)
Red soil            → Australia outback, parts of Africa, Georgia (USA)
Snow-capped peaks   → Alpine regions, Rockies, Andes, Himalayas
Rice paddies        → Southeast Asia, East Asia
Terraced farming    → Southeast Asia, Mediterranean, Andes
```
- architectural clues are always there in the nature
```
Timber framing      → Northern Europe, UK, New England USA
Adobe/mud brick     → Middle East, Southwest USA, North Africa
Soviet-era blocks   → Former Soviet states
Pagoda rooflines    → East/Southeast Asia
Onion domes         → Russia, Eastern Europe
Minarets            → Muslim-majority countries
Tropical stilted    → Southeast Asia, Pacific Islands
Scandinavian wooden → Nordic countries
```
- Infrastructure clues are always there as well
```
Power lines:
  Wooden poles       → North America, Japan
  Concrete poles     → Europe, Russia
  Underground lines  → Dense urban Europe

Traffic lights:
  Horizontal         → North America, some Asia
  Vertical           → Most of world

Driving side:
  Right-hand traffic → Most of world (steering wheel LEFT side of car)
  Left-hand traffic  → UK, Australia, Japan, India (steering wheel RIGHT)
```
- stars can be used to pin point location and time
```
1. Identify visible stars/constellations
2. Use Stellarium (stellarium.org)
3. Input suspected location
4. Adjust time until star positions match
```
- left hand and right hand traffic / wheel direction (opposite to traffic)
```
left handed

asia:
 
Japan, India, Pakistan, Bangladesh, Sri Lanka
Thailand, Malaysia, Singapore, Indonesia
Nepal, Bhutan, Myanmar

oceania:
 
Australia, New Zealand, Papua New Guinea
Fiji, Samoa, Tonga

Africa:

South Africa, Kenya, Tanzania, Uganda
Zimbabwe, Zambia, Mozambique, Botswana
Namibia, Malawi, Rwanda, Ethiopia

Caribbean/Americas:

Jamaica, Barbados, Trinidad & Tobago
Guyana, Suriname

Europe:

United Kingdom, Ireland, Malta, Cyprus
```
```
Right side

USA, Canada, Mexico
All of continental Europe
All of South America (except Guyana/Suriname)
China, Russia, Middle East
Most of Africa
```
- Differentiation based on roof
```
1. ROOF SHAPE BY REGION
Steep Pitched Roofs

Northern Europe (Germany, Scandinavia, Austria)
→ Very steep angle
→ Designed to shed heavy snow
→ Often red/orange clay tiles or dark slate

Flat/Low Pitched Roofs

Middle East, North Africa
→ Almost completely flat
→ Mud brick or concrete construction
→ Often used as living/storage space
→ Desert climate = no need for rain drainage

Gambrel Roofs (Barn style)

USA, Netherlands
→ Two slopes on each side
→ Upper slope gentle, lower slope steep
→ Classic American barn/farmhouse look

Mansard Roofs

France, French-influenced regions
→ Four sides, each with double slope
→ Top almost flat, sides very steep
→ Often with dormer windows
→ Classic Paris apartment building look

Hip Roofs

Southeast Asia (Thailand, Vietnam, Cambodia)
→ Slopes on ALL four sides
→ Often dramatically curved upward at corners
→ Ornate decorative ridge tiles

2. ROOF MATERIAL BY REGION
Clay/Terracotta Tiles

Mediterranean countries:
→ Spain, Italy, Portugal, Greece
→ Southern France
→ Latin America
→ Orange/red curved tiles
→ S-shaped or barrel style

Slate Tiles

→ UK, Ireland, Wales
→ Northern Spain (Galicia)
→ Parts of France
→ Dark grey/blue-black flat tiles
→ Very uniform appearance

Metal Roofing

→ Scandinavia (standing seam metal)
→ Russia (often painted green)
→ Rural USA/Australia (corrugated iron)
→ Canada (steel panels)

Thatched Roofs

→ UK (English countryside)
→ Netherlands
→ Parts of Africa
→ Southeast Asia (palm thatch)
→ Thick straw/reed appearance

Wooden Shingles

→ Scandinavia (Norway, Sweden)
→ Russia (Siberia)
→ Canada/Northern USA
→ Often weathered grey or dark brown

Concrete/Flat

→ Middle East
→ North Africa
→ Soviet-era Eastern Europe
→ Parts of India/Pakistan

Green Turf Roofs

→ Iceland (iconic grass roofs)
→ Norway
→ Faroe Islands
→ Literally covered in grass/vegetation

3. ROOF COLOR BY REGION

Red/Orange tiles    → Mediterranean, Latin America, Central Europe
Dark grey/black     → UK, Ireland, Northern France
Green metal         → Russia, Eastern Europe, Scandinavia
Brown/dark wood     → Nordic countries, Alpine regions
White flat          → Greece (especially islands), Middle East
Blue tiles          → Some parts of Morocco, Iran
Bright colors       → Caribbean islands
Corrugated silver   → Rural Africa, Australia, South Asia

4. SPECIFIC COUNTRY IDENTIFIERS
Russia/Eastern Europe

→ Onion dome roofs (churches)
→ Green/blue painted metal roofs
→ Soviet apartment blocks = flat concrete roofs
→ Dacha houses = simple wooden pitched roofs

Japan

→ Distinctive curved eaves sweeping upward
→ Dark grey ceramic tiles (kawara tiles)
→ Heavy looking, layered appearance
→ Ridge decorated with ornamental tiles

China

→ Similar to Japan but more elaborate
→ Upturned corners more dramatic
→ Yellow glazed tiles = imperial/temple buildings
→ Grey tiles = common residential

Scandinavia

→ Very steep pitch
→ Turf/grass roofs (Norway/Iceland)
→ Bright colored wooden houses
→ Metal standing seam roofs

Netherlands

→ Distinctive stepped gable ends
→ Very steep, narrow pitched roofs
→ Dark red/brown brick walls
→ Large windows relative to wall space

USA

→ Asphalt shingle (most common)
→ Dark grey/brown granular texture
→ Moderate pitch
→ Often with multiple gables/dormers

UK

→ Slate or grey concrete tiles
→ Moderate pitch
→ Chimney stacks very common
→ Semi-detached houses share roof line

Australia

→ Corrugated iron (older homes)
→ Terracotta tiles (suburban)
→ Low pitch (less rain in many areas)
→ Often with large verandah overhangs

India/South Asia

→ Flat concrete roof (urban)
→ Often with water tanks on roof
→ Corrugated metal (rural/informal)
→ Traditional areas = clay tiles similar to Mediterranean
```
- identifying language based on signs
```
1.  English     → No diacritics, pure Latin, most common globally
2.  Spanish     → ñ, ¿, ¡ (upside down ! and ?)
3.  French      → é, è, ê, ç, œ — lots of accents
4.  German      → ä, ö, ü, ß — unique ß character
5.  Portuguese  → ã, õ, ç — nasal vowels
6.  Polish      → ą, ę, ł, ź, ż — heavy use of tails/strokes
7.  Turkish     → ğ, ş, ı (dotless i) — unique dotless i
8.  Vietnamese  → Most complex Latin — ắ, ề, ổ — stacked accents
9.  Russian     → Cyrillic, has ы, э, ъ
10. Ukrainian   → Cyrillic but has і, ї, є
11. Serbian     → Cyrillic with ђ, љ, њ OR Latin both used
12. Arabic      → Right-to-left, cursive connected, no capitals
13. Persian     → Arabic script but adds پ چ ژ گ
14. Urdu        → Arabic script, diagonal flowing style (Nastaliq)
15. Chinese     → Only complex square characters, no alphabet
16. Japanese    → Mix of complex + rounded (あ) + angular (ア)
17. Korean      → Geometric blocks with circles and lines (한)
18. Hindi       → Devanagari — horizontal line connecting tops of letters (क ख ग)
19. Thai        → Circular loops on letters, unique curly style (ก ข ค)
20. Greek       → α, β, γ, δ — familiar but not Latin
21. Hebrew      → Right-to-left, blocky square letters, no vowels usually
22. Georgian    → Rounded curvy unique script (ა ბ გ) — only used in Georgia
23. Armenian    → Unique rounded script (Ա Բ Գ) — only used in Armenia
24. Amharic     → Ethiopian script, complex syllabic characters (አ ቡ)
25. Mongolian   → Vertical script written top to bottom

Fastest tells:
Right-to-left          → Arabic, Hebrew, Persian, Urdu
Vertical text          → Mongolian, old Chinese/Japanese
Stacked accent marks   → Vietnamese
Dotless i (ı)         → Turkish
Horizontal bar on top  → Hindi/Devanagari
Circular loops         → Thai or Sinhala
Geometric blocks       → Korean
Three scripts mixed    → Japanese
```
