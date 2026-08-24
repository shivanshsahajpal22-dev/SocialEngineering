# OSINT GUIDE PART 2

```
NOTE BEFORE YOU PROCEED

> Tools are listed in section 1
> Cyber OSINT (info about a company) covered in web_exploiation_Guide/Web_exploitation/Passive-recon
```

> This one is more about the people: SOCMINT, whereas the last one was focused on the geo-OSINT

- **WANT a detective like whiteboard** -> use maltego!
- I would suggest to start your search from google + dork gpt (>>> any tool)

---

## CONFIRMING YOU HAVE THE RIGHT PERSON — READ THIS BEFORE ANY BUCKET BELOW

> Every technique in this guide finds candidates, not confirmations. The actual skill — the one that separates a submission that gets accepted from one that gets rejected — is knowing when to stop trusting a lead and go verify it. This section sits above every bucket below for a reason: run it every time a pivot gives you a "match."

**The rule:** don't treat an identity as confirmed off a single bucket. One matching username is a candidate. A matching username + a matching profile photo + a chronemics timezone that agrees with a claimed location is a confirmation.

```
1. CROSS-BUCKET CORROBORATION — need at least 2 independent buckets agreeing
   before you call it confirmed, not just 2 hits inside the same bucket.
   e.g. username match ALONE = candidate. username match + email breach hit
   naming the same real name = confirmed.

2. COMMON NAME / HANDLE COLLISION CHECK — before locking onto a "John Smith"
   or a "shadow_99", ask: how common is this exact string? A rare, oddly
   spelled username carries far more weight than a common first-name-last-
   initial handle that plausibly belongs to hundreds of people.

3. STALE DATA CHECK — a username/email/phone can be RECYCLED by a different
   person after the original owner abandoned it. Breach data can be YEARS
   old. Always check "when was this last confirmed active" before trusting
   it as current.

4. PHOTO SANITY CHECK — if your IMAGE bucket gave you a face and your
   USERNAME bucket gave you a profile photo, do they actually look like the
   same person? Don't let a confident text-based match override an obvious
   visual mismatch.

5. TIMELINE PLAUSIBILITY CHECK — does the chronemics-derived timezone
   actually agree with the claimed/suspected location? Does the snowflake-
   derived account creation date make sense against the rest of the story?

6. REJECTING A LEAD IS A REAL OUTCOME, NOT A FAILURE — if a lead doesn't
   survive the checks above, log it as rejected and move on. An unsupported
   but attractive lead that gets submitted anyway is worse than no lead at
   all — it wastes review time and can send an investigation in the wrong
   direction entirely.
```

`Keep a running log as you go: what you found, where, when, and your confidence level. You will not remember three cases later why you trusted something — write it down the moment you find it.`

---

## How did we get here?

> here are chains of investigations: starting point and further movements

**IMAGE**
```
1. Try to search the image's location using the provided tools
2. You can also get the location using metadata: exiftool or a specific tool —
   although most social media strip the metadata, so only expect it on raw images
3. Identify the specific person using reverse image search/people search -> real name
4. Using reverse search via search engines can reveal social media accounts -> usernames
```
`Image is almost always the richest entry point precisely because it fans out into the most next-buckets at once — location, face, and background context are three independent pivots from one file`

`Break down videos into frames + sound`

`Sound itself is an OSINT vector although not as developed`

**If the image goes quiet (no location hit, no face match):**
```
- Drop to Part 1's geo-checklists (shadow/terrain/signage/roof/plate) — visual
  clues work even when every automated tool comes back empty
- Try a CROPPED version of the image separately — full-image and cropped-region
  reverse search frequently return completely different hit sets
- Pull the JPEG's embedded thumbnail (exiftool -b -ThumbnailImage) — it can
  preserve an original uncropped frame the visible image no longer shows
```

**USERNAME**
```
1. Perform a cross-media search using the tools provided
2. Try logging into certain social media with a username — can provide the last
   two digits of a phone number/email IDs
3. Generate variations of usernames using provided tools and generate email IDs
   based on the given name
4. GitHub repos and commits are notorious for leaking the developer's data using
   the .patch at the end of a commit
5. Use Wayback URLs on the profile pages themselves + combined with GitHub
   history -> can provide old info + unmaintained accounts
6. Once a social media account is identified, use a stylometry tool to identify
   other accounts

7. Once a social media account is identified, use chronemics to identify
   timezone -> take timestamps of posts -> convert to UTC
   a) biggest dead zone is generally the sleep time
   b) difference in weekday/weekend can tell you the work schedule
   c) meal time dips are also a thing that exists
   d) holiday correlation is also a thing
   e) if the rhythm is too regular, that's post scheduling
   f) a long drift from social media means a life event
   g) a temporal change in the rhythm can mean travelling

8. Once a social media account is identified, you can get the snowflake ID
   a) Discord: settings -> advanced -> developer mode -> messages now have a
      "copy ID" option
   b) Twitter/X: use a username-to-ID converter -> then go to
      x.com/username/status/TWEET-ID
   c) Instagram: post URL -> instagram.com/p/Code -> put that code in a
      shortcode-ID converter, then that into a snowflake decoder
   d) Mastodon -> URL directly -> instances.social/@user/<snowflake id>
```
> Reddit deleted-content recovery — Pushshift-successor tools (Arctic Shift and camas.unddit.com, or other similar sites)

**If the username goes quiet:**
```
- Run the SAME handle through variation-generation (step 3) before giving up —
  a dot, underscore, or trailing number is often all that's changed platform
  to platform
- If cross-media search finds multiple accounts with the same handle, do NOT
  assume they're the same person — run the verification checklist above before
  merging them into one identity
```

**EMAIL-IDS**
```
1. Perform email ID search using the given tools from section 1
2. Try variations of the email ID (can craft using the provided tools)
3. Search for dark web leak databases like HaveIBeenPwned for exact info,
   try DeHashed/LeakCheck/IntelligenceX too
4. Gravatar hash pivot — Gravatar profiles are looked up by the MD5 hash of the
   (lowercased, trimmed) email address — this helped once in a CTF :) !
5. With sites that aren't covered by the tools, try logging in with the email
6. Try to craft a username from the email ID if nothing else works
```
`The older a social media account is the better as OPSEC awareness grows with time`

**If the email goes quiet:**
```
- Confirm the email is even still LIVE before spending more time on it — a
  password-reset attempt often silently confirms or denies existence
- Check workspace/SaaS signup flows ("this email is already registered") to
  silently confirm an employer without contacting the target
- Fall back to crafting a username FROM the email (step 6) and re-run the
  whole USERNAME bucket against that guess
```

**IP-ADDRESS**
```
1. Try IP address search engines as provided
2. If you want to know what services are running, use Shodan/Censys
3. Reverse DNS + WHOIS + Reverse WHOIS on any IP block you can get
4. Look for passive DNS search history on DNS history sites
5. VPN/proxy/Tor exit node detection using the tool given in section 1
```
> Here's a site with a very good interface for this: https://bgp.he.net/ — nothing new, just a great UI :)

**If the IP goes quiet (VPN/hosting-masked):**
```
- Don't trust the geolocation until you've run VPN/proxy detection FIRST —
  attributing a physical location to a VPN exit node is the single most
  common false-positive mistake in this whole bucket
- Passive DNS history can reveal a domain that once pointed here, even if the
  IP itself resolves to nothing useful today — pivot into DOMAIN work
```

**PHONE-NUMBER**
```
1. Social media like WhatsApp, Telegram etc. provide a category for phone
   number search
2. Get a number lookup from something like Truecaller
3. Adding the phone number to WhatsApp/phone contacts, then checking the
   profile + last seen, can be a breakthrough
4. The password-reset trick can also reveal a name, username, or email in
   MFA systems
```

**REALNAME**
```
1. Try making a username based on this, as well as the provided email IDs
2. Public record sites mentioned in the tools section (varies by country)
3. People search aggregators like Whitepages/Spokeo-class services
4. Genealogy and obituary sites — surprisingly high-value for mapping a
   family tree, exactly the kind of pivot that unlocks the rest of the chain.
```
> DEV NOTE: more sites for the real-name part mentioned here have to be added to the tools section of Part 1!

**If the real name goes quiet, or you get TOO MANY matches:**
```
- Too many matches (common name) -> narrow using ONE other confirmed bucket
  (a location, an age range from EXIF, an employer from LinkedIn) before you
  pick a candidate — don't just take the first plausible result
- Zero matches -> the name may be a nickname, maiden name, or misspelling;
  keep a nickname-equivalence table handy rather than treating a null result
  as final
```

---

## ADDITIONAL VECTORS — FOLDED INTO THE CHAIN ABOVE

> Everything below was originally a loose practitioner's note. Keeping the notes, just tagging each one to the bucket it actually extends so it doesn't get lost as an afterthought.

**→ Extends IP-ADDRESS**
> Practitioner's note: if you have a WiFi network's BSSID and want more info on it, go to wigle.net

**→ Extends REALNAME / ADDRESS**
> Practitioner's note: Fitness-tracker heatmap — apps and fitness websites like Strava et al. can reveal daily visit locations, mostly workplace and home.
This method was actually used to find military bases in a real investigation once.

> Patent information records and licensing bodies have to be real and cannot be faked! — an inventor/assignee field on a patent, or a callsign/license lookup, is one of the hardest identity fields to fake, since it's a legal filing requirement.

**→ Extends IMAGE / GEOLOCATION**
> what3words is a platform that creates 3-word maps for every location — useful for pinning down or communicating an exact location once you've geolocated an image, down to a 3x3m square.

**→ Extends general link/URL recon**
> bit.ly short links reveal click analytics if you add a + at the end — total clicks, referrer breakdown, geographic distribution, without needing the account owner's permission.

**→ Extends EMAIL-IDS / REALNAME, via crypto payments**
> Practitioner's note: there exist tools to get info on crypto payments, like Etherscan, which are very useful for OSINT. Generally, each crypto platform has its own scanner, but here are a few general ones:
```
1. Blockchair – covers Bitcoin, Ethereum, Litecoin, Dogecoin, and more, with strong data-export/research features
2. OKLink – broad multichain coverage, leans toward compliance/institutional use
3. DeBank or Zapper – less "explorer," more wallet/portfolio view across EVM chains (good for seeing all your holdings at once rather than one transaction at a time)
```
`A confirmed wallet address can pivot toward a real identity too — cross-reference against known exchange deposit addresses to see which KYC'd exchange the funds eventually touched.`

**→ Dark web / breach sources — now filtered, nothing removed, just grouped**
```
WARNING: the dark web is a dangerous place to be. Continue at your own risk.

SEARCH ENGINES (start here first if you need to browse onion services)
1. Ahmia (ahmia.fi) — indexes onion services, filters abuse content — safest default
2. Torch — broad, less curated
3. Not Evil — another general-purpose Tor search engine
4. DuckDuckGo onion service — DDG search accessible over Tor
18. Onion Search — automated dark web recon

OFFICIAL MIRRORS (good for confirming Tor is working, safe to visit first)
5. ProPublica onion mirror
6. BBC onion mirror
7. Deutsche Welle onion mirror
8. New York Times onion mirror

BREACH / LEAK DATABASES (clearnet, this is usually your actual first move)
9. Have I Been Pwned — clearnet breach-checking (email/domain lookup)
10. Intelligence X (intelx.io) — leaks, historical paste sites, dark web sources
11. DeHashed — searchable breach/credential database

COMMERCIAL MONITORING PLATFORMS (paid, engagement/enterprise-scale territory)
12. Recorded Future
13. DarkOwl
14. Flare
15. SOCRadar

DIRECTORIES (for finding more sources, not sources themselves)
16. OSINT Framework (osintframework.com)
17. awesome-osint (GitHub)
```
`Practical order: breach databases first (9-11), official mirrors to confirm Tor access if you need to go further (5-8), search engines only once you know what you're looking for (1-4, 18), commercial platforms only in a funded engagement (12-15).`

---

## WORKED EXAMPLE CHAINS

**Chain A — image to confirmed identity, with a false-positive catch**
```
1. Start: a single photo, no metadata (social media stripped it)
2. Reverse image search (Yandex) returns a face match -> a name: "Alex Rivera"
3. Username search off "Alex Rivera" turns up an Instagram @alexrivera_ —
   profile photo does NOT match the original face. REJECTED per the photo
   sanity check, logged, moved on.
4. Second candidate from the same search: @a.rivera.photog — photo matches
5. Cross-media search (USERNAME step 1) finds the same handle on a
   photography portfolio site with a linked email
6. Email fed into breach-database search (EMAIL-IDS step 3) surfaces the
   same real name in a leak — REALNAME bucket now has 2 independent buckets
   agreeing (username + breach record) -> confirmed, not just a candidate
```

**Chain B — username to confirmed location, Trace Labs style**
```
1. Start: a username from a social media post
2. Cross-media search finds the same handle on 3 other platforms
3. Chronemics on the most active account: sleep-gap analysis suggests
   UTC-5, weekday posting pattern suggests a standard 9-5 schedule
4. Snowflake ID decode on the oldest post gives an exact account-creation
   timestamp, consistent with a bio claim of "here since 2019"
5. A Reddit account under the same handle, recovered via Arctic Shift, shows
   deleted posts naming a specific city — matches the UTC-5 chronemics window
6. Timeline plausibility check passes: city timezone matches chronemics,
   account age matches snowflake, cross-platform handle reuse confirmed
   across 3 independent sources -> logged, sourced, submitted
```

---

## OPSEC FOR THE INVESTIGATOR — DON'T BECOME PART OF THE STORY

1. **Research personas ("sock puppets")** — dedicated accounts for investigation, with zero linkage back to your real identity, ideally aged rather than freshly created (a brand-new account trying to connect can itself tip off a HUMINT-sensitive target that they're being looked at).
2. **Browser isolation** — a separate profile or VM for research so your own logged-in accounts, cookies, and search personalization don't bias — or leak into — your results.
3. **Passive means passive.** Don't like, follow, comment, or connect during a recon phase — every interaction is visible to the target and can burn an engagement that depends on staying unnoticed.
4. **Network separation** — where the rules of engagement require it, keep your research infrastructure (VPN, IPs) separate from anything tied to your real identity.

---

The thread running through this whole part: every bucket above finds you a CANDIDATE. Only cross-bucket corroboration, run through the verification checklist at the top, turns a candidate into something worth submitting. Breadth of technique was never the bottleneck in this guide — the discipline to reject a lead that doesn't survive verification is.
