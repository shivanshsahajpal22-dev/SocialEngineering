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
