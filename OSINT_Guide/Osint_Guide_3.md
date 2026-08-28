# OSINT PRIMARY TOOLS GUIDE

`we will discuss the big three here: Maltego, SpiderFoot, and Harvester.`

---
## MALTEGO 

> **Core loop:** `Question → Seed Entity → Transform → Pivot → Correlate → Validate → Evidence → Report`

## 1. How to use this guide

Maltego is a graph-based investigation platform. The important pieces are:

- **Entity** — the thing you currently know: Domain, IP, Email, Person, Alias, URL, Document, etc.
- **Transform** — an operation that turns one entity into related entities or properties.
- **Transform Set** — related transforms grouped together.
- **Machine** — a repeatable chain of transforms.
- **Data Hub / Connectors** — external data sources.
- **Graph** — the investigation workspace.

### The rule

Do **not** ask:

> “What transforms can I run?”

Ask:

> “What question do I need answered, and which transform is most likely to answer it?”

A Transform result is a **lead**, not automatically a fact. Validate important findings with an independent source.

---

# 2. The Transform Atlas

## Important scope note

Maltego's transform inventory changes with the product version, plan, installed Data Hub items, API access, and legacy/current product generation. Maltego's documentation currently describes the old Standard Transforms as **150+ transforms**, but also states that Standard Transforms are now legacy/unsupported for new users. Current Maltego also has many Data Hub/Connector integrations.

Therefore this section is organized as a **practical entity → possible transform reference**. It includes the documented Standard/legacy transforms and the major current Data Hub transform families you are likely to encounter. A transform may be unavailable in your installation or may require a particular connector/API subscription.

The names below are grouped by investigation purpose so you can find the right operation quickly.

---

## A. DOMAIN

### `Domain → DNS / infrastructure`

| Transform | Produces | Use when | Why |
|---|---|---|---|
| **To DNS Name [Using Name Schema dictionary]** | DNS Names | You suspect predictable hostnames | Tests common naming patterns |
| **To DNS Name [Find common DNS names]** | DNS Names | Beginning subdomain discovery | Checks common names such as mail, vpn, admin, etc. |
| **To DNS Name [Attempt zone transfer]** | DNS Names | Authorized DNS assessment | Checks whether a DNS zone transfer exposes records |
| **To IP Address [DNS]** | IPv4/IPv6 | You need the current host IP | Resolves DNS to IP |
| **To DNS Name - MX (mail server)** | MX Record | Investigating email infrastructure | Finds mail servers |
| **To DNS Name - NS (name server)** | NS Record | Mapping DNS hosting | Finds authoritative name servers |
| **To DNS Name - SOA** | NS Record, Email | Investigating DNS administration | Extracts SOA information |
| **To DNS Name - SPF** | Netblock, IP, DNS, MX, Domain | Investigating mail infrastructure | Shows infrastructure referenced by SPF |
| **To Domains [DNS]** | Domains | Moving from hostname to parent domains | Normalizes DNS names into domains |
| **To Website [Query ports]** | Website | Determining whether a DNS name serves HTTP(S) | Tests configured HTTP/HTTPS ports |
| **To Website [Quick lookup]** | Website | Bulk/simple website checks | Quickly tests common web presence |
| **To Website using domain [Bing]** | Website | Finding pages mentioning a domain | Search-engine pivot |
| **To Files (Interesting) [using Search Engine]** | Documents | Looking for exposed/interesting indexed files | Finds files associated with a domain |
| **To Files (Office) [using Search Engine]** | Documents | Looking for office documents | Finds indexed Office/PDF-type material |
| **To Emails @domain [using Search Engine]** | Email Addresses | Finding public addresses belonging to a domain | Useful for organization mapping |
| **To Phone Numbers [using Search Engine]** | Phone Numbers | Finding public contact numbers | Contact/organization pivot |
| **To Phone numbers [From whois info]** | Phone Numbers | WHOIS contains historical/public contact data | Extracts phones |
| **To Email address [From whois info]** | Email Addresses | WHOIS contains contact information | Extracts emails |

### When to use the Domain transforms

A good passive-first sequence is:

```text
Domain
 ↓
DNS / IP
 ↓
MX + NS + SOA + SPF
 ↓
Certificates / passive DNS / historical DNS
 ↓
Related domains
 ↓
Documents / public references
```

Use brute/common-name or zone-transfer operations only where you are authorized to perform that type of DNS activity.

---

## B. DNS NAME / HOSTNAME

| Transform | Produces | Use when | Why |
|---|---|---|---|
| **To Domains [DNS]** | Domain | You need the parent domain | Normalize hostname |
| **To IP Address [DNS]** | IPv4/IPv6 | You need infrastructure | Resolve hostname |
| **To Website [Query ports]** | Website | You want to know if it exposes web service | Web-presence check |
| **To DNS Name [Enumerate hostname numerically]** | DNS Names | You see `mail1`, `mail2`, etc. | Test numbered hostname patterns |
| **To Website [Quick lookup]** | Website | Bulk checking | Fast web validation |

Use hostname transforms when you already have a specific subdomain rather than starting another broad discovery pass.

---

# C. IP ADDRESS

| Transform | Produces | Use when | Why |
|---|---|---|---|
| **To Website mentioning IP [Bing]** | Websites | Finding public references to an IP | Search-engine pivot |
| **To Location [city, country]** | Location | Geographic context is relevant | Approximate IP geolocation |
| **To Location [country]** | Location | You only need country-level context | Coarser geographic pivot |
| **To Phone number [From WHOIS]** | Phone | Registration/contact data is available | Extract contact data |
| **To Email address [From WHOIS]** | Email | Registration/contact data is available | Extract contact data |
| **Historical Reverse WHOIS [WhoisXML]** | Domains/IPs | Investigating historical ownership relationships | Finds historical WHOIS matches |
| **Passive DNS / DNS history connectors** | DNS Names/Domains | You need historical associations | Finds names that resolved to the IP |
| **Certificate transforms** | Certificates/Domains | You want TLS-associated names | Finds certificate relationships |
| **Shodan transforms** | Services/hosts/ports | You need internet-exposure context | Enriches the IP with Shodan data |
| **GreyNoise transforms** | Internet-noise context | Assessing whether an IP is commonly observed | Helps distinguish routine scanning/noise from unusual infrastructure |
| **VirusTotal transforms** | URLs/Domains/IPs/files | Threat-intelligence investigation | Cross-source IOC enrichment |
| **AbuseIPDB transforms** | Abuse reports/context | Assessing reported malicious activity | Adds reputation/reporting context |
| **Censys transforms** | Hosts/services/certificates | Infrastructure research | Internet-wide host/certificate context |

### IP workflow

```text
IP
 ↓
DNS / passive DNS
 ↓
Certificates
 ↓
WHOIS / ASN / netblock
 ↓
Related domains
 ↓
Threat intelligence
```

**Do not infer ownership from a shared IP alone.** Shared hosting, CDNs and cloud providers create many false associations.

---

# D. NETBLOCK / CIDR

| Transform | Produces | Use when | Why |
|---|---|---|---|
| **To IP addresses [Found in Netblock]** | IPs | You need individual addresses | Expands the range |
| **To Netblocks [Cuts Netblock into smaller ones]** | Netblocks | You need smaller ranges | Breaks a range into useful units |
| **To Location [city, country]** | Location | Geographic context matters | Approximate range location |
| **To Location [country]** | Location | Broad geography is enough | Country-level context |
| **Reverse WHOIS [WhoisXML]** | Domains/IPs | Looking for historical registrations | Finds registration relationships |
| **PeeringDB / ASN / routing connectors** | ASN/network context | Studying network ownership/peering | Infrastructure attribution context |

Use netblock expansion carefully; expanding a large range can create huge graphs.

---

# E. AS / ASN

| Transform family | Produces | Use when | Why |
|---|---|---|---|
| **WHOISXML reverse WHOIS** | Domains/IPs | Finding historical WHOIS relationships | ASN-associated registration pivots |
| **IPInfo / MaxMind** | Network/location context | Understanding network ownership | Enriches infrastructure |
| **PeeringDB** | Network/ASN relationships | Investigating connectivity | Peering and network context |
| **Censys** | Hosts/certificates | Finding infrastructure in a network context | Internet-wide enrichment |
| **DomainTools / DNSDB** | Domains/DNS | Finding historical infrastructure | Passive DNS and domain pivots |

ASN results describe network allocation/organization context; they do not automatically prove that every host in the ASN belongs to the same operational entity.

---

# F. EMAIL ADDRESS

| Transform | Produces | Use when | Why |
|---|---|---|---|
| **To Person [Parse separator]** | Person | Email resembles `john.doe@...` | Generates a possible name from the address |
| **To Phone number [using Search Engine]** | Phone | Looking for public contact references | Finds indexed phone references |
| **To URLs [Show Search Engine results]** | URLs | Finding pages mentioning the email | Pivots into web references |
| **To Flickr Account** | Flickr account | Legacy/public account correlation | Historical social pivot |
| **To Myspace Account** | Myspace/Alias | Legacy account correlation | Historical social pivot |
| **PGP → Email Addresses** | Email | Investigating public PGP keys | Connects keys and addresses |
| **Have I Been Pwned connector** | Breach context | Authorized exposure research | Checks known breach exposure where available |
| **Hunter connector** | Email/domain data | Organization/contact research | Finds public professional email data |
| **People Data Labs / identity connectors** | Person/identity context | Identity research | Enriches a professional identity |
| **WhoisXML** | Domains/IPs | Reverse registration pivot | Finds registration relationships |
| **Search/Dorking transforms** | Documents/URLs | Finding public mentions | Search-engine discovery |

### Email workflow

```text
Email
 ├── Parse possible person
 ├── Search web
 ├── Search public documents
 ├── Search PGP
 ├── Search professional/identity sources
 └── Pivot to domain
```

Do not assume an email address proves the identity of the person using it.

---

# G. PERSON

| Transform family | Produces | Use when | Why |
|---|---|---|---|
| **To EmailAddress [Bing/Search]** | Emails | Finding public professional contact information | Identity → email |
| **To PhoneNumber [Bing/Search]** | Phones | Finding public contact references | Identity → phone |
| **Historical Reverse WHOIS [WhoisXML]** | Domains/IPs | Testing historical registration relationships | Person → infrastructure |
| **Search-engine transforms** | URLs/Documents | Finding public mentions | Web footprint |
| **Social/identity connectors** | Accounts/identifiers | Cross-platform identity research | Correlation |
| **OpenCorporates** | Companies/roles | Investigating corporate affiliations | Person → company |
| **OpenSanctions** | Organization/person matches | Compliance/sanctions research | Entity screening |
| **LittleSis** | Organizations/relationships | Relationship research | Public-network mapping |
| **People Data Labs** | Identity/company context | Professional identity enrichment | Person correlation |
| **FullContact** | Identity/contact context | Identity enrichment | Contact correlation |
| **Constella / Pipl / similar identity connectors** | Identity signals | Advanced identity research | Cross-source correlation |

### Person workflow

```text
Person
 ↓
Email / Phone
 ↓
Alias / social accounts
 ↓
Organizations
 ↓
Websites / documents
 ↓
Infrastructure
 ↓
Independent validation
```

For person attribution, require multiple independent signals.

---

# H. ALIAS / USERNAME

| Transform | Produces | Use when | Why |
|---|---|---|---|
| **To DNS Name [From DynDNS username]** | DNS Name | Investigating a username that may have been used with DynDNS | Historical infrastructure pivot |
| **To Phrase [Change Entity Type]** | Phrase | You want generic search transforms | Converts alias into searchable text |
| **Social account transforms** | Social accounts | Cross-platform username research | Account discovery |
| **Search-engine transforms** | URLs/Documents | Finding public references | Broad web pivot |
| **WhoisXML historical transforms** | Domains/IPs | Username appears in registration data | Infrastructure correlation |
| **Social Links** | Social profiles/content | Advanced social investigation | Platform-specific discovery |
| **Evidence transforms** | Social/web evidence | Evidence-focused collection | Preserves investigative context |

### Username workflow

```text
Alias
 ↓
Platform accounts
 ↓
Cross-links
 ↓
Email / website
 ↓
Organization
 ↓
Infrastructure
```

A common username is weak evidence by itself.

---

# I. URL

| Transform family | Produces | Use when | Why |
|---|---|---|---|
| **To Domain** | Domain | You need the hostname's parent | Normalize URL |
| **To Website / HTTP transforms** | Website/properties | Inspecting web presence | Web enrichment |
| **To EXIF Info** (where image input is appropriate) | Metadata | Image investigation | Metadata extraction |
| **Wayback → Snapshots** | Historical snapshots | Investigating previous content | Historical web evidence |
| **urlscan.io** | URLs/domains/IPs/scan context | Investigating observed web activity | Web-scan enrichment |
| **VirusTotal** | Domain/IP/file context | Threat intelligence | IOC enrichment |
| **ThreatMiner** | Threat-intel relationships | IOC investigation | Historical threat data |
| **Search/Dorking** | Documents/URLs | Finding public references | Search-engine pivot |
| **WhoisXML** | Domains/IPs | Registration research | WHOIS relationship |
| **Website technology connectors** | Technologies | Fingerprinting public technology | Infrastructure correlation |

---

# J. WEBSITE

| Transform family | Produces | Use when | Why |
|---|---|---|---|
| **To Domain / DNS** | Domain/DNS | Normalizing infrastructure | Start infrastructure pivot |
| **BuiltWith** | Technologies/sites | Technology fingerprinting | Find technology relationships |
| **Legacy technology transforms** | Technologies/relationships | Historical technology research | Shows technology changes |
| **Tracking-code transforms** | Websites/identifiers | Site clustering | Shared analytics/advertising identifiers |
| **Wayback Machine** | Snapshots | Historical research | Compare old/new content |
| **urlscan.io** | Scan data | Web infrastructure analysis | Enrichment |
| **Censys** | Hosts/certificates | Infrastructure correlation | Internet-wide context |
| **Search/Dorking** | Documents/URLs | Public-reference discovery | Find indexed content |
| **VirusTotal** | Threat context | Suspicious website investigation | IOC enrichment |
| **TinEye** | Images | Reverse-image investigation | Find image reuse |
| **Image Analyzer** | Image properties | Image investigation | Extract/analyze image data |

---

# K. DOCUMENT / FILE

| Transform | Produces | Use when | Why |
|---|---|---|---|
| **Parse meta information** | Person/Email/Phrase/Document | You have a document and want metadata | Extracts author/contact clues |
| **To URL [Show SE results]** | URL | Document came from search results | Recover source URL |
| **To Entities [IBM Watson]** | Person/Email/Phrase/Document | NLP extraction is available | Extracts named entities |
| **To EXIF Info** | Metadata/Phrase | Image file | Extracts EXIF |
| **Search-engine document transforms** | Documents | Finding related public files | Document clustering |
| **Wayback transforms** | Historical pages | Document came from old web content | Historical validation |

### Document workflow

```text
Document
 ↓
Metadata
 ↓
Author / Email / Organization
 ↓
Source URL
 ↓
Related documents
 ↓
Historical copies
```

Metadata is useful evidence but can be removed or modified.

---

# L. IMAGE

| Transform | Produces | Use when | Why |
|---|---|---|---|
| **To EXIF Info** | Metadata | Image may contain EXIF | Camera/GPS/time/metadata clues |
| **TinEye** | Image matches | Finding image reuse | Reverse-image correlation |
| **Image Analyzer** | Image analysis | Need image-specific enrichment | Extracts/analyzes image properties |
| **Search / social image connectors** | Profiles/images | Identity or account research | Visual correlation |

Treat visual similarity as a lead; verify with independent identifiers.

---

# M. PHONE NUMBER

| Transform family | Produces | Use when | Why |
|---|---|---|---|
| **Search-engine transforms** | URLs | Finding public mentions | Web footprint |
| **WhoisXML** | Domains/IPs | Registration correlation | Historical/current WHOIS pivot |
| **Identity connectors** | Person/account context | Authorized identity research | Cross-source enrichment |
| **Social/Evidence connectors** | Public accounts/evidence | Social investigation | Public-profile correlation |

Phone numbers can be recycled or shared; verify ownership carefully.

---

# N. LOCATION / GPS

| Transform | Produces | Use when | Why |
|---|---|---|---|
| **To Location [Using nominatim.openstreetmap.org]** | Location | You have GPS coordinates | Reverse geocoding |
| **To Location (broad)** | Location | Exact address is unnecessary | Broader geocoding |
| **To Circular Area** | Circular Area | You need a radius around coordinates | Geographic scope |
| **Google Maps Geocoding** | Location | Connector is available | Geocoding/enrichment |
| **MaxMind / IPInfo** | Location | Starting from IP/netblock | Approximate network geography |

IP geolocation is approximate; it is not a person's physical location.

---

# O. TWEET / X POST — LEGACY ENTITY

| Transform | Produces | Use when | Why |
|---|---|---|---|
| **To Aliases [mentioned in Tweet]** | Alias | Extracting mentioned usernames | Account pivot |
| **To Hash Tags** | Hashtag | Mapping topics/campaigns | Topic clustering |
| **To Sentiment [IBM Watson]** | Phrase | Historical sentiment analysis | Content analysis |
| **To Stock Symbol** | Stock Symbol | Financial/social research | Market-topic extraction |
| **To URLs [Extracts links from Tweet]** | URL | Following links | Infrastructure/content pivot |
| **To Words [English]** | Phrase | Text analysis | Keyword extraction |
| **Entity extraction/NLP** | People/phrases | Identifying named entities | Content-to-entity pivot |

Current X access depends on the installed connector/data source; legacy Twitter transforms may not be available.

---

# P. PGP KEY

| Transform family | Produces | Use when | Why |
|---|---|---|---|
| **To Email Addresses [PGP]** | Email | Linking key to email | Identity correlation |
| **To Person [PGP]** | Person | Key contains identity information | Person pivot |
| **To PGP Keys [PGP]** | PGP keys | Finding related keys | Key-network research |

PGP is particularly useful when investigating technical communities, developers, researchers and historical email identities.

---

# Q. PHRASE / SEARCH TERM

| Transform family | Produces | Use when | Why |
|---|---|---|---|
| **Search-engine transforms** | URLs/Documents | General web discovery | Broad search |
| **To Files (Interesting)** | Documents | Looking for indexed files | Document discovery |
| **To Files (Office)** | Documents | Looking for office documents | Targeted document discovery |
| **To Websites - specifics [using SE]** | Websites | Searching selected sites | Focused site search |
| **WhoisXML reverse WHOIS** | Domains/IPs | Phrase appears in registration data | Infrastructure pivot |
| **NLP/entity extraction** | People/organizations/phrases | Text contains names | Entity discovery |
| **To DNS Name [From DynDNS username]** | DNS | Phrase is an alias/username | DynDNS pivot |

---

# R. ORGANIZATION / COMPANY

| Transform family | Produces | Use when | Why |
|---|---|---|---|
| **OpenCorporates** | Companies/people/registrations | Corporate research | Corporate relationships |
| **OpenSanctions** | Person/org matches | Compliance research | Sanctions/entity screening |
| **WhoisXML** | Domains/IPs | Infrastructure ownership research | Registration correlation |
| **DomainTools** | Domains/WHOIS | Domain ownership/history | Infrastructure research |
| **BuiltWith** | Technologies/sites | Technology footprint | Web technology mapping |
| **People Data Labs** | People/company data | Employee/professional research | Organizational mapping |
| **LittleSis** | People/org relationships | Public relationship research | Network analysis |
| **Search/Dorking** | Documents/URLs | Public documentation | Organization footprint |
| **Historical WHOIS** | Domains/IPs | Historical organization links | Older infrastructure |

---

# S. CERTIFICATE

| Transform family | Produces | Use when | Why |
|---|---|---|---|
| **SSL Certificate transforms** | Domains/DNS/IP context | Infrastructure investigation | Certificate relationships |
| **Censys** | Certificates/hosts | Internet-wide certificate research | Discover related infrastructure |
| **crt.sh/certificate-style data connectors** | DNS names | Certificate pivot | Find names sharing certificates |
| **VirusTotal** | Domains/IPs | Threat investigation | IOC correlation |

Certificate relationships are useful clustering signals, but shared hosting/certificates can create false positives.

---

# T. MALWARE HASH / FILE HASH

| Transform family | Produces | Use when | Why |
|---|---|---|---|
| **VirusTotal** | Files/domains/IPs/URLs | IOC investigation | Broad malware/threat enrichment |
| **Hybrid Analysis** | Malware analysis context | Sandbox research | Behavior and relationships |
| **Intezer Analyze** | Malware/family context | Code-family investigation | Malware similarity |
| **Cisco Threat Grid** | Malware analysis | Sandbox enrichment | Behavioral context |
| **PolySwarm** | Malware intelligence | Multi-engine analysis | Detection/context |
| **ThreatMiner** | Threat relationships | IOC pivoting | Threat-intel correlation |
| **MalNet / Proofpoint** | Malware/network context | Advanced threat research | Malware infrastructure relationships |
| **OpenCTI / MISP-style connectors** | Threat entities | CTI investigations | Structured threat relationships |

For malware analysis, use the graph to correlate IOCs; do not execute unknown samples on your normal workstation.

---

# U. DOMAIN / URL / IP AS THREAT IOC

For a suspicious IOC, combine:

```text
IOC
 ↓
VirusTotal
 ↓
urlscan / passive DNS
 ↓
Certificates
 ↓
ThreatMiner / OpenCTI / MISP
 ↓
Historical infrastructure
 ↓
Related IOCs
```

Useful Data Hub families include:

- Abuse.ch URLhaus
- AbuseIPDB
- AlienVault OTX
- Anomali ThreatStream
- ATT&CK/MISP
- Cofense Intelligence
- CrowdStrike Intel
- GreyNoise
- Hybrid Analysis
- Mandiant
- Recorded Future
- ThreatConnect
- ThreatMiner
- VirusTotal
- urlscan.io
- OpenCTI

Availability depends on your installed connectors and subscription.

---

# V. SOCIAL / PLATFORM ENTITIES

Current Maltego can provide platform-specific investigations through Data Hub/Evidence/connector products. Depending on the installed integration, you may encounter transforms for:

- Facebook
- Instagram
- LinkedIn
- Telegram
- TikTok
- X
- YouTube
- Reddit
- other social/public-source platforms

Typical transform families are:

```text
Profile
 → Posts/content
 → Followers/following
 → Mentions
 → Aliases
 → URLs
 → Emails/contact clues
 → Locations
 → Related profiles
```

### When to use

Use platform-specific transforms **after you have a useful seed**:

```text
Known username
Known profile
Known post
Known URL
Known email
```

Do not run every social transform blindly. Platform data changes rapidly and may require authentication or a paid connector.

---

# W. WIKIPEDIA / PUBLIC KNOWLEDGE

| Transform family | Produces | Use when | Why |
|---|---|---|---|
| **Wikipedia page → editors** | Editors | Investigating page history | Attribution of edits |
| **Wikipedia → related entities** | People/organizations/pages | Background research | Knowledge graph expansion |
| **WikiEdit-related transforms** | Editors/pages | Historical article research | Track changes and relationships |

Use these for background/context and research trails, not as proof of sensitive attribution.

---

# X. PROPERTY TRANSFORMS

Property transforms are different: they often operate on information **already stored inside an entity** rather than querying a large external dataset.

Examples include:

| Transform | Use |
|---|---|
| **To Datetime [within Properties]** | Extract dates from entity properties |
| **Property extraction/conversion transforms** | Turn existing properties into graph entities |
| **Entity type conversion transforms** | Convert one entity representation into another useful search input |

These are useful when a previous transform already gave you valuable data hidden in properties.

---

# Y. HTTP / WEB-PAGE TRANSFORMS

| Transform | Input | Use |
|---|---|---|
| **To EXIF Info** | Image | Extract image metadata |
| **HTTP/web extraction transforms** | URL/Website | Retrieve/parse public web information |
| **URL expansion/extraction** | URL/Document/social post | Reveal linked URLs |
| **Page/entity extraction** | Web content | Pull structured entities from text |

HTTP transforms can be more active than pure search/API pivots. Use them only against systems you are authorized to interact with.

---

# Z. WAYBACK MACHINE

| Transform | Use |
|---|---|
| **To Snapshots [Wayback Machine]** | Find historical versions of a URL |
| **Snapshot → URL/page pivots** | Move from historical content back into the graph |
| **Historical page/content transforms** | Compare old and current infrastructure/content |

Best for:

```text
Current domain
 ↓
Historical snapshots
 ↓
Old pages
 ↓
Old emails / names / URLs
 ↓
Old infrastructure
```

Historical data is especially valuable when current pages have been deleted or changed.

---

# AA. WHOISXML

WhoisXML has many specialized transforms. The important families are:

| Input | Possible output | Use |
|---|---|---|
| Domain | related domains/IPs | WHOIS/domain research |
| IP | domains/IPs | Reverse WHOIS |
| IPv6 | domains/IPs | Reverse WHOIS |
| Netblock | domains/IPs | Registration clustering |
| ASN | domains/IPs | Historical registration research |
| Organization | domains/IPs | Infrastructure association |
| Person | domains/IPs | Historical registration research |
| Phone | domains/IPs | Registration correlation |
| Phrase | domains/IPs | Search WHOIS records |
| URL | domains/IPs | Registration correlation |
| Alias | domains/IPs | Historical registration correlation |

Use these when your question is specifically about **registration/ownership relationships over time**.

---

# AB. DNSDB / PASSIVE DNS

Use DNSDB/Farsight-style transforms when you want:

```text
Domain → historical DNS
IP → historical DNS names
DNS name → historical associations
```

This is one of the strongest infrastructure pivots because it lets you investigate **relationships that may no longer exist in current DNS**.

---

# AC. DOMAINTOOLS

Use DomainTools-style transforms for:

- domain registration
- WHOIS history
- reverse WHOIS
- DNS history
- related domains
- infrastructure relationships
- registration/contact clues

Best when the question is:

> “What other infrastructure is or was associated with this domain/person/organization?”

---

# AD. BUILTWITH / TECHNOLOGY

Use BuiltWith when the question is:

> “What technology does this website use, and where else does that technology appear?”

Typical pivots:

```text
Website
 ↓
Technology
 ↓
Other websites
```

Useful for:

- technology clustering
- finding related sites
- identifying common infrastructure patterns
- historical technology changes

Do not treat a common CMS/library as proof of common ownership.

---

# AE. SHODAN / INTERNETDB

Use for **public internet-exposure context**:

```text
IP
 ↓
Ports/services
 ↓
Host metadata
 ↓
Related infrastructure
```

Good for:

- defensive asset discovery
- exposure assessment
- threat-intelligence enrichment
- identifying publicly visible services

This is not a substitute for an authorized vulnerability assessment.

---

# AF. CENSYS

Use Censys-style transforms when the question involves:

- internet-wide hosts
- certificates
- services
- domains
- infrastructure relationships

Typical chain:

```text
Domain/IP
 ↓
Certificate / host
 ↓
Related names
 ↓
Related hosts
```

Especially useful for certificate and infrastructure clustering.

---

# AG. URLSCAN

Use urlscan when you want to understand a publicly observed web page/URL:

```text
URL
 ↓
Scan
 ↓
Domains / IPs / resources
 ↓
Related web infrastructure
```

Good for suspicious websites, phishing investigations and web-infrastructure correlation.

---

# AH. VIRUSTOTAL

Use VirusTotal for IOC enrichment:

```text
Domain
IP
URL
Hash
 ↓
VirusTotal
 ↓
Historical detections / relationships / related indicators
```

Best when investigating:

- malware
- phishing
- suspicious domains
- suspicious IPs
- files/hashes
- known campaigns

Treat detections as intelligence signals, not definitive attribution.

---

# AI. ABUSE.CH / OTX / THREATMINER / OPENCTI / THREATCONNECT

Use threat-intelligence connectors when the question is:

> “Has this indicator appeared in known threat intelligence, and what is it connected to?”

Typical chain:

```text
IOC
 ↓
Threat-intelligence source
 ↓
Campaign / malware / actor / infrastructure
 ↓
Related IOCs
```

Cross-check high-value claims between independent CTI sources.

---

# AJ. SEARCH / DORKING TRANSFORMS

Use search transforms when you need **discovery**, not authoritative ownership data.

Examples:

```text
Domain → indexed documents
Domain → indexed pages
Phrase → websites
Phrase → documents
Email → public references
IP → public references
```

Best for finding:

- public documents
- old pages
- mentions
- contact information
- indexed files
- public technical references

Search results are discovery leads; verify the underlying page/source.

---

# AK. EVIDENCE TRANSFORMS

Evidence transforms are useful when your investigation is centered around public social/web evidence.

Use them when you need to:

```text
Find public content
 ↓
Capture/structure evidence
 ↓
Correlate it in Graph
 ↓
Preserve provenance
```

Use **Graph** for relationships and **Evidence** for evidence-centric collection workflows.

---

# AL. GEO / MAP / LOCATION TRANSFORMS

Use when you already have:

- GPS
- address
- IP-derived location
- organization location
- public place information

Typical chain:

```text
GPS
 ↓
Location
 ↓
Map / nearby context
```

Do not overstate precision. IP geolocation and inferred locations are generally approximate.

---

# AM. SPECIALIZED DATA HUB CONNECTORS

Maltego's current Data Hub includes many specialized integrations. The exact availability changes, but major documented connector families include:

```text
Abuse.ch URLhaus
AbuseIPDB
Aleph
AlienVault OTX
Anomali ThreatStream
ATT&CK / MISP
Censys
Cofense Intelligence
Constella
CrowdStrike Intel
CrowdStrike ThreatGraph
Cybersixgill
DarkOwl
DomainTools
DNSTwist
Etherscan
Farsight DNSDB
Flashpoint
Google Maps Geocoding
GreyNoise
Have I Been Pwned
Hunter
Hybrid Analysis
IPInfo
Mandiant
MaxMind
NIST NVD
OpenCorporates
OpenCTI
OpenSanctions
PeeringDB
People Data Labs
PhoneSearch
Pipl
PolySwarm
Recorded Future
Shodan
Social Links
SpyCloud
SSL Certificate
ThreatConnect
ThreatMiner
TinEye
urlscan.io
VirusTotal
Wayback Machine
WhoisXML
```

The connector list is not a promise that every item is available on every plan.

---

# 3. Which Transform Should I Pick?

When you are stuck, use this decision tree.

```text
I have a DOMAIN
 ├─ Need current infrastructure?
 │    → DNS / IP / MX / NS / SOA / SPF
 │
 ├─ Need historical infrastructure?
 │    → DNSDB / Wayback / DomainTools / WhoisXML
 │
 ├─ Need related websites?
 │    → BuiltWith / certificates / passive DNS
 │
 ├─ Need public mentions?
 │    → Search / Dorking
 │
 └─ Need threat context?
      → VirusTotal / OTX / ThreatMiner / OpenCTI / CTI connector


I have an IP
 ├─ Need domains?
 │    → Passive DNS / reverse DNS / DNSDB
 ├─ Need infrastructure?
 │    → Censys / Shodan
 ├─ Need ownership/history?
 │    → WHOIS / WhoisXML / DomainTools
 ├─ Need reputation?
 │    → AbuseIPDB / GreyNoise / VirusTotal
 └─ Need geography?
      → IPInfo / MaxMind


I have an EMAIL
 ├─ Need possible identity?
 │    → Parse separator / identity connectors
 ├─ Need public references?
 │    → Search
 ├─ Need historical crypto identity?
 │    → PGP
 ├─ Need organization?
 │    → Domain + professional/identity connectors
 └─ Need exposure?
      → Have I Been Pwned / authorized exposure sources


I have an ALIAS
 ├─ Need accounts?
 │    → Social connectors / Evidence
 ├─ Need web references?
 │    → Search
 ├─ Need infrastructure?
 │    → WhoisXML / DynDNS where applicable
 └─ Need identity?
      → Cross-correlate email + website + organization


I have a PERSON
 ├─ Need contact?
 │    → Email / phone
 ├─ Need organizations?
 │    → OpenCorporates / professional data
 ├─ Need websites?
 │    → Search
 └─ Need historical infrastructure?
      → Reverse WHOIS


I have a HASH
 ├─ Need malware context?
 │    → VirusTotal / Hybrid Analysis / Intezer
 ├─ Need infrastructure?
 │    → Threat intelligence
 └─ Need related indicators?
      → OpenCTI / MISP / ThreatMiner / CTI connectors
```

---

# 4. Real Investigation Chains

## Domain investigation

```text
Domain
 ↓
DNS
 ↓
IP
 ↓
MX / NS / SOA / SPF
 ↓
Passive DNS
 ↓
Certificates
 ↓
Related domains
 ↓
Historical data
 ↓
Threat intelligence
 ↓
Public documents
```

## Person investigation

```text
Person
 ↓
Email / Phone
 ↓
Alias
 ↓
Social accounts
 ↓
Website
 ↓
Organization
 ↓
Documents
 ↓
Infrastructure
 ↓
Independent validation
```

## Username investigation

```text
Alias
 ↓
Platform accounts
 ↓
Cross-links
 ↓
Email / Website
 ↓
Organization
 ↓
Identity confirmation
```

## Threat IOC investigation

```text
Domain / IP / URL / Hash
 ↓
VirusTotal / OTX / ThreatMiner
 ↓
Passive DNS
 ↓
Certificates
 ↓
urlscan
 ↓
Historical infrastructure
 ↓
Related indicators
 ↓
Campaign / actor correlation
```

---

# 5. Machines vs Individual Transforms

Use an individual Transform when you want to control the investigation.

Use a Machine when you already understand the chain and want to repeat it.

Example:

```text
Domain
 ↓
DNS
 ↓
IP
 ↓
WHOIS
 ↓
Related infrastructure
```

First learn the individual transforms. Then automate the repeatable sequence.

---

# 6. Validation and Evidence

For every important relationship ask:

```text
What is the source?
When was it observed?
Is it current?
Can another source confirm it?
Is this a fact or an inference?
```

Use labels such as:

```text
CONFIRMED
STRONG
MODERATE
WEAK
UNKNOWN
REJECTED
```

And distinguish:

```text
OBSERVED
INFERRED
HYPOTHESIZED
```

### Example

Bad:

```text
IP → Domain → Person
Therefore the person owns the server.
```

Better:

```text
IP resolves to domain.
Domain historically appears in WHOIS associated with person.
Independent source links person to organization.
Confidence: MODERATE.
Ownership still requires confirmation.
```

---

# 7. The Quick Reference You Actually Need

```text
DOMAIN
→ DNS → IP → MX/NS/SOA/SPF → PASSIVE DNS → CERTS → RELATED DOMAINS

IP
→ DNS → PASSIVE DNS → ASN/NETBLOCK → WHOIS → CERTS → SHODAN/CENSYS → CTI

EMAIL
→ PERSON → ALIAS → SEARCH → PGP → DOMAIN → IDENTITY SOURCES

PERSON
→ EMAIL → PHONE → ALIAS → SOCIAL → ORGANIZATION → DOCUMENTS → INFRASTRUCTURE

ALIAS
→ SOCIAL → SEARCH → EMAIL → WEBSITE → ORGANIZATION

URL
→ DOMAIN → IP → DNS → CERTIFICATE → WAYBACK → URLSCAN → VIRUSTOTAL

DOCUMENT
→ METADATA → AUTHOR → EMAIL → ORGANIZATION → SOURCE URL → RELATED DOCUMENTS

IMAGE
→ EXIF → REVERSE IMAGE → PROFILE / WEBSITE → VALIDATION

HASH
→ VIRUSTOTAL → HYBRID ANALYSIS → INTEZER → THREAT INTEL → INFRASTRUCTURE

THREAT IOC
→ VT / OTX / THREATMINER / OPENCTI → DNS → CERTS → HISTORICAL DATA
```

---

# 8. Final Rule

The best Maltego investigator is not the person who runs the most transforms.

It is the person who can look at an entity and immediately think:

```text
What can this entity tell me?
        ↓
Which transform answers that question?
        ↓
What new entity did I get?
        ↓
Is it worth pivoting?
        ↓
Can I independently validate it?
```

**Use the graph to build a defensible chain of relationships — not a giant pile of entities.**

--
## SPIDERFOOT 

# SpiderFoot Practical Guide

SpiderFoot is an OSINT automation tool for discovering and correlating information about domains, IPs, hostnames, emails, usernames, and related infrastructure.

## 1. Install on Linux

```bash
git clone https://github.com/smicallef/spiderfoot.git
cd spiderfoot
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python3 sf.py -l 127.0.0.1:5001
```

Open:

```text
http://127.0.0.1:5001
```

Use the installation/startup instructions from the repository if the commands differ for your version.

## 2. Start a Scan

Go to **New Scan**.

For a domain investigation:

```text
Target: example.com
```

Start with the default scan configuration. For a first pass, passive/public-data modules are preferable.

Useful module categories:

* DNS / subdomains
* WHOIS/RDAP
* SSL certificates
* IP/ASN information
* Search engines
* URLs
* Email addresses
* Usernames
* Technology detection

Let the scan finish, then work from the results rather than blindly enabling every module.

## 3. Follow the Results

Treat SpiderFoot findings as leads and pivot through useful relationships.

A typical workflow:

```text
domain
  ↓
subdomain
  ↓
IP address
  ↓
ASN / hosting provider
```

Or:

```text
domain
  ↓
email
  ↓
username
  ↓
public profile
```

Useful things to investigate:

### Subdomains

Look for:

```text
api.example.com
dev.example.com
staging.example.com
vpn.example.com
mail.example.com
```

Check whether they currently resolve and whether they actually belong to the target.

### Certificates

Certificate data can reveal hostnames that aren't linked from the main website.

Use these as candidates, then verify whether the hostname is still active.

### IPs

When a hostname produces an IP, pivot into:

* ASN
* Network owner
* Hosting/cloud provider
* Related infrastructure

Be careful with cloud providers and shared hosting: an IP belonging to a provider doesn't mean the target owns the entire IP range.

### Emails and usernames

Use discovered identifiers to find additional public relationships.

Don't assume two accounts belong to the same person just because they share a username. Look for multiple independent indicators.

## 4. Validate Interesting Findings

SpiderFoot is good at collection but can produce stale or incorrect results.

For every important finding, ask:

```text
Where did this come from?
Is it current?
Can I confirm it with another source?
Does it actually belong to the target?
```

Example:

```text
SpiderFoot:
staging.example.com → certificate database

Verify:
DNS → does it resolve?
Certificate → is the hostname current?
HTTP → only if permitted by the assessment scope
```

Classify findings as:

```text
Confirmed
Probable
Unverified
False positive
```

## 5. Practical Workflow

For an external asset-discovery assessment:

```text
Target domain
     ↓
Run SpiderFoot
     ↓
Collect subdomains / IPs / certificates
     ↓
Remove duplicates
     ↓
Pivot interesting assets
     ↓
Independently verify
     ↓
Document confirmed assets
```

A useful investigation table:

| Finding             | Source      | Verified | Confidence |
| ------------------- | ----------- | -------- | ---------- |
| api.example.com     | DNS         | Yes      | High       |
| staging.example.com | Certificate | No       | Medium     |
| 203.0.113.20        | DNS         | Yes      | High       |

## 6. Useful Tips

**Don't scan everything immediately.** Start with the modules relevant to your question.

**Don't trust a single source.** Correlate results.

**Watch for historical data.** A hostname or IP relationship may no longer be current.

**Use repeated scans for asset monitoring.** Compare results over time to identify new subdomains, certificates, IPs, or providers.

**Stay within authorization.** Discovering an asset does not automatically authorize active testing against it.

### Quick workflow

```text
Scope → Scan → Pivot → Correlate → Verify → Report
```
--

# theHarvester — Practical OSINT Guide

> Use only against domains and infrastructure you own or are explicitly authorized to investigate.

## 1. Install & Configure

### Kali Linux

On current Kali installations, theHarvester is normally already available. If you need to install it manually:

```bash
sudo apt update
sudo apt install theharvester
```

Check the installation:

```bash
theHarvester -h
```

For source installations, the current project uses Python 3.12+ and `uv`.

### API keys

theHarvester reads `api-keys.yaml` from locations including:

```text
~/.theHarvester/
 /etc/theHarvester/
 /usr/local/etc/theHarvester/
```

The first matching configuration is used.

On Kali:

```bash
sudo nano /etc/theHarvester/api-keys.yaml
```

Example:

```yaml
apikeys:

  censys:
    id: YOUR_ID
    secret: YOUR_SECRET

  github:
    key: YOUR_GITHUB_TOKEN

  shodan:
    key: YOUR_SHODAN_KEY

  hunter:
    key: YOUR_HUNTER_KEY
```

Only configure providers you actually intend to use. Protect the file:

```bash
sudo chmod 600 /etc/theHarvester/api-keys.yaml
```

The exact fields vary by provider and version, so use the generated template rather than inventing field names.

---

## 2. Core Commands

Basic domain search:

```bash
theHarvester -d example.com -b crtsh
```

Use several sources:

```bash
theHarvester -d example.com -b crtsh,certspotter,subdomaincenter
```

Use the default broad source set:

```bash
theHarvester -d example.com -b all
```

**Important:** `all` does not mean every available source. Current versions classify sources into activity tiers; `all` runs the P0 sources, while P1/P2 sources must be explicitly selected.

See the sources supported by your installed version:

```bash
theHarvester -h
```

For a specific investigation, explicitly selecting sources is usually better than blindly using `all`.

---

## 3. Source Modules — What to Use and When

Think of the sources as different **OSINT datasets**, not interchangeable search engines.

### Certificate / hostname discovery

**`crtsh`**

```bash
theHarvester -d example.com -b crtsh
```

Uses Certificate Transparency data.

**Use when:** your primary objective is discovering subdomains and historical hostnames.

Example:

```text
example.com
   ↓
api.example.com
dev.example.com
mail.example.com
```

Pair it with:

```bash
-b crtsh,certspotter
```

`certspotter` is another Certificate Transparency source and is useful for corroborating certificate-derived hostnames.

---

### Search-engine sources

Examples include:

```text
baidu
brave
duckduckgo
mojeek
yahoo
```

**Use when:** you're looking for indexed pages, public documents, emails, names, URLs, or hostnames that aren't present in certificate datasets.

Example:

```bash
theHarvester -d example.com -b brave,duckduckgo,yahoo
```

These sources are particularly useful after certificate-based discovery because search engines can reveal actual indexed content associated with the discovered names.

---

### DNS / subdomain datasets

Useful sources include:

```text
dnsdumpster
hackertarget
subdomaincenter
subdomainfinderc99
thc
```

**Use when:** your goal is maximum hostname discovery.

Example:

```bash
theHarvester -d example.com -b dnsdumpster,hackertarget,subdomaincenter,thc
```

Don't assume that one source finding a hostname means it is currently active. Treat the result as a candidate and verify it.

---

### Infrastructure / attack-surface databases

Examples:

```text
censys
criminalip
fullhunt
netlas
onyphe
shodan
zoomeye
```

**Use when:** you want to understand infrastructure associated with discovered hosts rather than simply finding names.

These can provide useful relationships between:

```text
hostname
   ↓
IP
   ↓
network / provider
   ↓
additional infrastructure
```

For example, Censys is particularly useful for certificate and asset relationships, while Shodan/Netlas/Criminal IP are useful when investigating internet-facing infrastructure.

Many of these require API credentials.

---

### Email discovery

Useful sources include:

```text
hunter
hunterhow
tomba
github-code
```

**Use when:** the investigation involves finding public email addresses associated with an organization.

Example:

```bash
theHarvester -d example.com -b hunter,tomba
```

This is useful for building an organization's public identity map:

```text
example.com
   ↓
john@example.com
   ↓
public username / profile
```

Treat email results as OSINT leads. Don't assume that a discovered address belongs to a particular individual without corroboration.

---

### Code / repository discovery

**`github-code`**

**Use when:** you're investigating public repositories, code references, documentation, configuration references, or organization-related development activity.

Example:

```bash
theHarvester -d example.com -b github-code
```

This is particularly useful after you've identified:

* Organization names
* Developer-related email domains
* Product names
* Public project names

Use it for **public-source discovery**, not for accessing private repositories or credentials.

---

### Breach / exposure sources

Examples include:

```text
haveibeenpwned
dehashed
intelx
leaklookup
```

**Use when:** you're conducting an authorized exposure assessment and need to determine whether an organization's public identifiers appear in breach-related datasets.

These sources often require API credentials and can have significant access/usage restrictions.

For an organization assessment, this is usually a **later-stage pivot**, after you have established the organization's domains and public email patterns.

---

### Technology / application intelligence

**`builtwith`**

**Use when:** you want to identify technologies associated with discovered websites.

Example:

```bash
theHarvester -d example.com -b builtwith
```

Useful for questions such as:

```text
What technologies are publicly associated with this domain?
Which hosts use the same technology?
```

It is more useful for **asset profiling** than basic subdomain discovery.

---

## 4. Build a Recon Workflow

Instead of running every module at once, use stages.

### Stage 1 — Hostname discovery

```bash
theHarvester -d example.com -b crtsh,certspotter,dnsdumpster,subdomaincenter
```

Goal:

```text
example.com
├── www.example.com
├── api.example.com
├── mail.example.com
└── dev.example.com
```

### Stage 2 — Search discovery

```bash
theHarvester -d example.com -b brave,duckduckgo,mojeek,yahoo
```

Look for:

* Emails
* URLs
* Public pages
* Names
* Additional hostnames

### Stage 3 — Infrastructure enrichment

After you have interesting hosts:

```bash
theHarvester -d example.com -b censys,netlas,criminalip
```

Use this to understand the infrastructure behind the names.

### Stage 4 — Email / identity discovery

```bash
theHarvester -d example.com -b hunter,tomba,github-code
```

Use this when the assessment specifically requires public email or organizational identity discovery.

### Stage 5 — Exposure checks

Only when relevant and authorized:

```bash
theHarvester -d example.com -b haveibeenpwned,intelx
```

This staged approach gives you cleaner results than throwing dozens of unrelated sources at the target.

---

## 5. Useful Advanced Options

### Run Shodan enrichment

Shodan's normal source and Shodan host enrichment are separate operations in current versions. The `-s` / `--shodan` action is used for host enrichment.

```bash
theHarvester -d example.com -s
```

Use this when you already have discovered hosts and want additional Shodan information.

### DNS brute force

theHarvester also has an active DNS brute-force capability.

Conceptually:

```text
candidate word
      ↓
DNS query
      ↓
does hostname exist?
```

**Use when:** passive sources have produced an incomplete hostname list and you are explicitly authorized to perform active DNS enumeration.

### Screenshots

Current versions also provide screenshot functionality for discovered subdomains.

**Use when:** you want a quick visual inventory of discovered web hosts rather than manually opening each one.

---

## 6. Practical Recon Recipes

### A. Find subdomains

```bash
theHarvester -d example.com -b crtsh,certspotter,dnsdumpster,subdomaincenter,thc
```

Best for:

```text
domain → hostname inventory
```

### B. Find public emails

```bash
theHarvester -d example.com -b hunter,tomba,github-code
```

Best for:

```text
domain → public email addresses
```

### C. Map infrastructure

```bash
theHarvester -d example.com -b censys,netlas,criminalip,shodan
```

Best for:

```text
hostname → IP → infrastructure
```

### D. Broad passive collection

```bash
theHarvester -d example.com -b all
```

Then follow up with specific P1/P2 sources that weren't included in `all`. Current documentation explicitly notes that `all` covers the P0 set.

### E. Save results

Use the output/file options shown by your installed version:

```bash
theHarvester -h
```

Current releases support structured output formats including JSON/JSONL/XML, which are useful if you're building an asset inventory or comparing results between assessments.

### The mindset

Don't think:

```text
"Which command finds everything?"
```

Think:

```text
What am I trying to discover?
        ↓
Which dataset is best for that?
        ↓
What did I discover?
        ↓
Which source can corroborate it?
        ↓
What should I pivot into next?
```

That's where theHarvester becomes much more useful than simply running `-b all`.
