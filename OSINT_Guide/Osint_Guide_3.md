# DARK WEB OSINT GUIDE

`The dark web isn't a different internet — it's the same recon discipline applied to a layer that refuses to be indexed`

**WARNING:** Dark web investigation carries real legal and personal-safety risk depending on jurisdiction and what you're actually looking at. Authorized red team/CTI work only. Owner does not stand accountable for anything.

> Four things get conflated under "dark web OSINT" that are actually four different skills: finding the sites in the first place, attributing a specific persona to a real identity, gathering intelligence at scale for a defensive purpose, and knowing which of the thousand "link list" sites out there are trustworthy versus a scam. This guide keeps them separate on purpose — conflating them is how people either waste hours or get phished by a fake market mirror.

---

## PART 1: FINDING AND PIVOTING BETWEEN DARK WEB SITES

> Before any investigation technique matters at all, you need to actually find the site. This is a genuinely different problem than clearnet discovery — there's no Google index, no backlink graph, no DNS to enumerate. Everything here is about working around that absence.

### Why you can't just guess an onion address

Worth understanding before anything else, because it explains every technique below: modern (v3) onion addresses are 56-character strings derived from an ed25519 public key — cryptographically random, with no sequential structure, no brute-forceable pattern, and no registrar to query. (v2's shorter 16-character addresses were fully deprecated by the Tor Project in 2021, and anything still advertising one is already a red flag.) This means discovery is **entirely dependent on being told the address by something** — a directory, a crawl, a header, a forum post — never derived mathematically. Every technique below is a different way of being told.

### Default method: verified, community-vetted link directories

**how to get access**
```
Tor Browser — the only supported client, from torproject.org directly
(never a third-party "Tor browser" download — that's a classic malware
distribution vector, and it's worth treating any non-official source
for the browser itself with the same suspicion as an onion link)
```

```
dark.fail       -> curated, actively maintained directory of verified .onion
                    links; the most consistently community-endorsed starting
                    point, in operation since 2018
tor.taxi        -> established community link site, similarly trusted
daunt.link      -> community link site maintained by the Dread forum team
```

**Why these three specifically:** dark web communities themselves converge on the same short list — the r/onions subreddit's own automated safety notice tells new users explicitly: *"Only use marketplaces listed on daunt, tor taxi, or dark fail. Anything else is a scam."* That's not a casual recommendation, that's the community's own accumulated experience with how often everything else turns out to be a phishing clone.

**Format-specific gotcha that matters for interpreting results:** these directories exist specifically because onion *marketplace* addresses in particular are constantly cloned by scammers running visually identical phishing copies to steal login credentials or crypto deposits. A link found via a random search engine result or a "Hidden Wiki" page has a real chance of being one of these clones — verification against a trusted directory isn't optional caution, it's the actual mechanism that prevents credential theft in this specific environment.

### The decision tree — when a link doesn't resolve, or you're not sure it's real

```
1. Genuinely offline/down. Onion services have far higher churn and
   downtime than clearnet sites — seizures, voluntary shutdowns, and
   simple unreliability are all common. Not every dead link is a trap.
2. PGP-signed link mismatch. Reputable directories and forums often
   PGP-sign their official link lists specifically so a clone can't
   silently substitute itself — if a signature check fails, that's a
   real, confirmed red flag, not a false alarm.
3. Address LOOKS plausible but isn't in ANY trusted directory. Treat
   this as a probable clone by default — legitimate services want to
   be found through verified channels precisely because of the scam
   problem above; an address that exists nowhere trusted is a hole,
   not a discovery.
4. Directory itself is unfamiliar. Cross-reference the directory
   against the community meta-resources in Part 4 before trusting
   its listings at all — the "which directories are trustworthy"
   question has the exact same clone problem as individual sites.
```

### Fallback 1 — onion-to-onion crawling

Once you're on one legitimate onion page, the outbound links on that page are themselves a discovery mechanism — this is the actual analog to backlink-graph crawling on the clearnet, just manual and slower.

```bash
# TorBot — automated onion crawler, follows and catalogs outbound
# .onion links from a starting page
git clone https://github.com/DedSecInside/TorBot
cd TorBot && pip install -r requirements.txt
python3 torBot.py -u <onion_url>

# OnionSearch — aggregates results across MULTIPLE onion search
# engines (Ahmia, Torch, etc.) in one query instead of checking each
# individually, the shotgun-first-move equivalent for this bucket
git clone https://github.com/megadose/OnionSearch
pip install onionsearch
onionsearch "search term" --output results.txt
```

**Why this matters:** forums and marketplaces frequently maintain their own internal "verified links" board where established members vouch for other addresses — this is a social trust mechanism doing the same job a PGP signature does mechanically, and it's often how a genuinely new, not-yet-widely-indexed service actually gets discovered in practice.

### Fallback 2 — the Onion-Location pivot (clearnet → onion, the legitimate mechanism)

Many major organizations now advertise their onion mirror directly from their clearnet site via the `Onion-Location` HTTP response header — when Tor Browser detects it, a ".onion available" prompt appears in the address bar automatically. The Guardian, Deutsche Welle, the Internet Archive, Facebook, and several privacy-focused search engines all deploy this.

```bash
curl -sI https://example-clearnet-site.com | grep -i onion-location
```

**Where this fits:** this is a completely legitimate, publisher-endorsed discovery channel — genuinely the cleanest possible link between a clearnet identity and its onion counterpart, since the organization is announcing the connection itself rather than it being inferred.

### Fallback 3 — Certificate Transparency correlation

If a target runs both a clearnet presence and an onion service off shared infrastructure, a TLS certificate covering both hostnames (or a reused certificate authority pattern) sometimes surfaces in public Certificate Transparency logs.

```
crt.sh (from your existing search-engine list) — search by the suspected
organization/domain name, look for certificates covering unusual
subdomains or issued close in time to a known onion service's first
appearance
```

### Worked example chain

```
Chain A — from zero, following the trust chain properly
1. Start at dark.fail, confirm current PGP-signature-verified addresses
   for the category of service you're researching
2. Load one verified onion page, run TorBot against it to crawl its
   outbound links -> surfaces several related/affiliated services
3. Cross-reference each new address AGAINST dark.fail/tor.taxi before
   trusting it — an address discovered via crawling still needs
   independent verification, since a compromised or malicious page
   could itself link to a clone
4. Confirmed set of addresses logged, each with its verification
   source noted — exactly the same "log your confidence and source"
   discipline as your OSINT verification checklist
```

The tools and specific directories here will keep changing — link sites rise and fall, and any specific address in this guide should be assumed stale the moment it's written down. What doesn't change is the discipline: **never trust a single unverified source for an onion address, and treat "not listed anywhere trusted" as the default red flag rather than the exception.**

---

## PART 2: RED TEAM PERSPECTIVE — SUSPECT INVESTIGATION AND PERSONA CORRELATION

> This is attribution work: linking a pseudonymous dark web presence to a real identity, or linking multiple aliases together as the same person. It is not account compromise — that distinction matters and is addressed explicitly at the end of this section.

### Default method: anchor-first pivoting

Same principle as your existing clearnet OSINT bucket-chain — dark web work is those buckets applied to a harder-to-index layer. Before touching Tor, identify what anchor you already have, since it decides everything downstream:

```
Have a username/alias -> cross-reference across dark web forums directly
Have an email          -> pivot to breach databases FIRST, Tor second —
                            usually faster
Have a wallet address   -> blockchain explorer pivot, often faster than
                            dark web search entirely
Have a PGP key          -> fingerprint reuse search, one of the strongest
                            anchors available
Have nothing at all     -> exhaust your existing clearnet OSINT chain
                            until you produce ONE of the above, then
                            return here
```

Dark web search is almost never a cold-start technique — it's a **pivot destination** once an existing chain hands you a thread to pull.

### OPSEC — identical discipline to your existing section, higher stakes

Aged sock-puppet personas with zero linkage to real identity, Tor Browser in a dedicated isolated VM, and **passive means passive** matters more here than anywhere else in this guide series — registering on a forum to "look around," or messaging a suspect directly, burns the investigation and can carry real legal or personal-safety risk depending on what the forum actually deals in.

### The search/monitoring layer

```
Tor search engines  -> Ahmia, Torch, OnionSearch (automated aggregator) —
                        shotgun first move, same role as Aperi'Solve/DIE/
                        theHarvester play in earlier parts of this series
Breach databases     -> HaveIBeenPwned (clearnet), DeHashed, LeakCheck,
                        Intelligence X — usually higher-yield than raw
                        Tor browsing for "find their email" specifically
Forum/marketplace     -> manual browsing of known forums relevant to the
   monitoring            suspect's apparent activity, PLUS Telegram —
                        a large share of what used to live purely on
                        Tor forums has migrated there, don't skip it
                        just because "dark web" implies Tor specifically
Commercial platforms  -> Recorded Future, DarkOwl, Flare, SOCRadar —
                        continuous automated monitoring rather than a
                        one-off manual lookup, the right tier once this
                        stops being a single investigation
```

### The decision tree — when the trail goes cold

```
1. Alias appears nowhere outside the one forum post you started with.
   Genuinely possible — a single-use throwaway persona is a valid,
   common finding, not a failure of technique.
2. Cross-reference the SAME handle against breach databases before
   giving up — handle reuse across dark web and clearnet/breach data
   is extremely common, people are lazy about identity separation.
3. If the forum ITSELF has ever been breached (many major forums have
   had their own user databases leaked over the years), search that
   specific leak by username directly — a forum's own breach is
   often higher-signal than anything the suspect posted publicly.
4. PGP or wallet pivot (below) if username search comes up fully dry.
```

### Persona correlation — the core craft

Four techniques do almost all of the real deanonymization work here. None of them require breaking into anything — they exploit reuse and carelessness, the same underlying principle as nonce-reuse attacks in the cryptanalysis section of this series: **the moment the same secret/identifier gets reused across two contexts, the two contexts are linked.**

**1. PGP key fingerprint reuse.** People reuse the same PGP key across multiple forum posts, marketplace listings, and occasionally a clearnet profile, because rotating keys is inconvenient and most people don't bother. A key fingerprint is a near-perfect anchor for exactly that reason.
```
cirw.in/gpg-decoder — decodes a public key block to its embedded
identity/email (already in your Part 1 tools list under the
practitioner's note); if a suspect ever signed anything with a key
that embeds their real email, that step alone can close the loop
```

**2. Cryptocurrency wallet reuse.** The same wallet address showing up across a marketplace deposit and a clearnet donation link, or across two unrelated dark web transactions, ties activity together even when every username differs.
```
Blockchair, OKLink (already in your tools list) — trace the on-chain
transaction graph; the actual deanonymizing moment is usually when a
traced wallet touches a KYC'd exchange deposit address, since that's
the one point the chain connects back to a verified real identity
```

**3. Stylometry.** Same technique as your USERNAME bucket in the OSINT guide, applied to dark web forum posts instead of social media — function-word frequency, punctuation habits, and phrasing quirks link an alias to a clearnet identity even when every explicit identifier has been scrubbed.
```
JStylo (already in your tools list) — genuinely one of the highest-
value techniques against someone otherwise careful, because writing
style is the hardest thing to consciously and consistently mask
```

**4. Infrastructure overlap.** If a suspect runs their own hidden service, misconfigurations occasionally leak the real hosting details — an exposed status page, a favicon shared with a clearnet site, a reused SSH key fingerprint, or a TLS certificate covering both the onion service and a clearnet domain in Certificate Transparency logs.
```
crt.sh, Shodan/Censys — search for the leaked fingerprint/certificate,
then pivot to every other service sharing the same infrastructure
```

### The attribution chain — and exactly where it stops

```
1. Alias/username found on a dark web forum
2. Cross-reference the SAME handle against clearnet breach databases —
   a large share of people reuse handles across both worlds, and
   breach dumps often pair a username with its registration email
3. If the forum itself was ever breached, search that specific leak
   by username directly
4. PGP or wallet pivot per above, if the username search is dry
5. Cross-bucket corroboration per your existing OSINT verification
   checklist — at least two independent techniques agreeing before
   calling an identity confirmed, not just one lucky hit
```

**This is where attribution ends, deliberately.** Everything above confirms *who* a persona belongs to and *what's exposed* about them — it never requires logging into anything that isn't yours. The moment the goal shifts from "confirm this email belongs to this suspect" to "now access that email account" — credential stuffing a breached password, password spraying, or phishing the target directly — that's active offensive operation, not OSINT, and it needs its own explicit, separately-scoped authorization, the same discipline your Phishing Guide's ROE checklist applies to SEG-evasion techniques. Finding the door and using the door are different engagements.

### Worked example chain

```
Chain A — cross-bucket confirmation, not a lucky single hit
1. Clearnet OSINT (existing chain) surfaces a suspicious username on
   a forum post
2. Cross-reference that username in DeHashed -> hits in an old forum
   breach, paired with an email address
3. That email fed into HaveIBeenPwned -> confirms it also appears in
   two further unrelated breaches, suggesting long-term reuse rather
   than a throwaway
4. Separately, a PGP key attached to one of the suspect's dark web
   forum posts is decoded via cirw.in/gpg-decoder -> embeds the SAME
   email address independently
5. Two independent techniques (breach correlation + PGP decode) agree
   -> cross-bucket corroboration achieved, confirmed rather than
   merely a candidate, per your existing verification discipline
6. Investigation stops at attribution: logged and reported — not
   escalated into accessing the account itself
```

Same closing principle as every other bucket in this whole series: identify the anchor you have, match it to the right technique, corroborate across at least two independent methods before calling anything confirmed, and know exactly where the line between finding and using sits.

---

## PART 3: DARK WEB OSINT AND INFORMATION GATHERING — BEYOND THE RED TEAM LENS

> Part 2 was about one suspect. This is about the dark web as an ongoing intelligence *source* — the discipline that runs a company's threat-intel program, not the discipline that closes a single investigation. Different goal, different rhythm: continuous monitoring rather than a one-off pivot chain.

### Default method: passive, continuous monitoring rather than manual one-off search

The structural difference from Part 2 is cadence. A red team suspect investigation is a pivot chain that terminates in an answer. General dark web intelligence gathering is a standing program — the same source gets checked again next week, because the thing you're watching for (a leak, a mention, a listing) hasn't happened yet and might happen at any time.

```
Recorded Future, DarkOwl, Flare, SOCRadar (already in your tools list) —
built for exactly this cadence: continuous crawling, indexing, and
alerting rather than a manual query you run once and close out
```

### Ransomware leak-site monitoring — one of the highest-value sources in this entire category

Modern double-extortion ransomware groups run dedicated onion "leak sites" where stolen victim data gets posted publicly if a ransom isn't paid — this is one of the single richest, most current sources of real breach intelligence that exists anywhere, dark web or otherwise. Group identities and their leak-site addresses churn constantly (takedowns, rebrands, law enforcement seizures), which is exactly why this needs to be a monitored feed rather than a bookmark — a static list goes stale within weeks.

```
Practical approach: subscribe to a commercial CTI feed that tracks
ransomware group leak-site churn specifically (this is a standard
feature of the platforms listed above), rather than manually hunting
for current group leak-site addresses — the churn rate makes a
manually maintained list obsolete almost immediately
```

### The Telegram migration — don't let "dark web" mean "Tor only"

A large and growing share of what used to live exclusively on Tor forums and markets has moved to Telegram — better uptime, easier access, less technical overhead for both operators and buyers. A serious information-gathering program treats Telegram channel monitoring as part of the same discipline as onion forum monitoring, not a separate afterthought.

### Corporate/organizational exposure monitoring

The defensive, ongoing version of the email-breach pivot from Part 2 — instead of chasing one suspect's email, you search breach dumps and leak sites for your **own organization's domain** to catch compromised employee credentials before they're used against you.

```
Search breach databases (DeHashed, LeakCheck, IntelX — already listed)
for your own domain rather than an individual's email, on a recurring
schedule, not a one-time check
```

### Other legitimate use cases worth naming explicitly

```
Executive/VIP protection    -> monitoring for specific threats or doxxing
                                 attempts against named individuals
Brand protection            -> counterfeit goods, phishing-kit sales
                                 targeting your brand, impersonation domains
                                 for sale
Vendor/supply-chain risk     -> monitoring whether a third-party supplier's
                                 data has appeared in a breach, since their
                                 exposure becomes your exposure
Journalism/research          -> a genuinely different ethical and legal
                                 posture than red team work, covered here
                                 only to note it exists as its own
                                 discipline with its own norms, not to
                                 collapse it into this guide's scope
```

### Worked example chain

```
Chain A — a standing monitoring program catching an exposure early
1. A commercial CTI platform's ransomware-leak-site tracker flags a
   new victim posting that includes a domain matching a known vendor
   in your supply chain
2. Vendor risk pivot: the flagged data dump is cross-referenced against
   your own organization's domain in the same leak
3. A match surfaces — an employee credential pair exposed via the
   vendor's breach, not your own systems directly
4. Escalated internally as a credential-rotation priority BEFORE the
   leak is exploited, rather than discovered after an incident —
   this is the entire value proposition of continuous monitoring over
   one-off manual search: catching exposure before it's used, not
   investigating after
```

The tools and specific leak-site addresses in this space churn faster than almost anything else in this whole guide series — what's worth keeping is the cadence discipline: this is a standing program, not a pivot chain that closes.

---

## PART 4: DISCOVERING RESOURCES AND SITES ON THE DARK WEB

> A short section on purpose — this is the meta-question underneath Part 1: which directories are themselves trustworthy enough to be a starting point at all.

**Verified, community-endorsed link directories**
- [dark.fail](https://dark.fail) - Curated, PGP-verification-conscious directory of onion links, active since 2018, the most consistently community-endorsed starting point
- [tor.taxi](https://tor.taxi) - Established community link site, similarly trusted
- [daunt.link](https://daunt.link) - Community link site maintained by the Dread forum team

**Curated, publicly-auditable lists**
- [alecmuffett/real-world-onion-sites (GitHub)](https://github.com/alecmuffett/real-world-onion-sites) - A maintained, version-controlled list specifically of onion services run by *real, identifiable organizations* (news outlets, privacy tools, etc.) — genuinely useful precisely because it's auditable and each entry is attributable to a known operator

**Community meta-resources (clearnet, safe to browse, point toward onion resources)**
- r/onions wiki, r/tor wiki, r/deepweb wiki - community-maintained safety and resource guides; the r/onions community's own pinned safety notice is itself one of the better concise summaries of which directories to trust
- Dread (via onion access) - an established darknet-market-discussion forum whose endorsement is part of why dark.fail carries the trust it does — forums like this function as a social verification layer on top of the technical one

**Guaranteed-legitimate practice targets**
- Official onion mirrors — ProPublica, BBC, Deutsche Welle, New York Times (already in your Part 2 tools list) - since these are run by known, named organizations with zero ambiguity about authenticity, they're the correct place to verify your Tor setup is working before trusting anything else

**What to actively avoid — and why this isn't just caution, it's the community's own consensus**
```
"The Hidden Wiki" and similar generic-sounding directories surfaced by
a plain search engine or a random Telegram channel — the r/onions
community's own automated safety notice states this plainly: "Dont use
any sites listed on a HiddenWiki or some random shit you found on a
search engine, a telegram channel, or website. You will be scammed."
This isn't this guide being overcautious — it's citing the accumulated
experience of the community that actually deals with this daily.
```

---

Every part of this guide reduces to the same underlying discipline running through the entire series: **verify before you trust, corroborate before you confirm, and know precisely which layer of the problem you're actually solving** — finding a site is not the same skill as attributing a persona, which is not the same skill as running a standing intelligence program, which is not the same skill as knowing which directory to trust in the first place. Treating any one of these four as a substitute for the others is exactly how investigations stall, credentials get phished, or a red-team attribution finding quietly turns into an unauthorized account-access incident.
