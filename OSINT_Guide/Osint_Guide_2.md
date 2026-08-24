# OSINT GUIDE PART 2 

```
NOTE BEFORE YOU PROCEED

> Tools are listed in section 1
> Cyber OSINT (info about a company) covered in web_exploiation_Guide/Web_exploitation/Passive-recon 
```

> This one is more about the people: SOCMINT, whereas the last one was focused on the geo-OSINT

- **WANT a detective like whiteboard** -> use maltego ! 

- I would suggest to start you search from google + dork gpt (>>> any tool)

---
## How did we got here ?

> here are chains of investigations: starting point and further movements 

**IMAGE**
```
1. Try to search the image's location using the provided tools
2. You can also get the location using metadata:exif tool or sc specific
ALthough most social media's strip the meta data so only expect in raw images 
3. Identify the specific person using the reverse image search/people search -> real name 
4. Using reverse search using search engines can reveal social media accounts -> usernames
```
`Image is almost always the richest entry point precisely because it fans out into the most next-buckets at once — location, face, and background context are three independent pivots from one file`

`Break down videos into frame + sounds`

`sounds itself is a OSINT vector although not as developed`

**USERNAME**
```
1. Perform a cross-media search using the tools provided
2. Try logging into certain social media with a username can provide the last two digits of a phone number/email IDs
3. Generate variations of usernames using provided tools and generate email IDs based on the given name
4. GitHub repo and commits are notorious for leaking the developer's data using the .patch at the end of a commit  
5. Use Wayback URLs on the profile pages themselves + combined with github histrory -> can provide old info + umaintained accounts
6. Once a social media account is identified, use a stylometry tool to identify other accounts

7. Once a social media account is identified, use chronemics to identify timezone -> take timestamps of post -> convert to UTC
a) biggest dead zone is the sleep time generally
b) difference in weekday/weekend can tell you the work schedule
c) meal time dips is also a thing that exists
d) Holiday correlation is also a thing
e) if rhythm is too regular, that is post scheduling
f) A long drift from social media means a life event
g) A temporal change in the rhythm can mean travelling

8. Once a social media account is identified, you can get the snow flake id
a) Discord: setting -> advanced -> developer mode -> message have copy id setting now
b) Twitter/x: Use a username to ID coverter -> then go to x.com/username/status/TWEET-ID
c) Instagram: Post url -> instagram.com/p/Code -> put that code in shortcode-ID coverter then that into a snowfalke decoder
d) Mastodon -> URL directly -> instances.social/@user/<snowfalke id>
```
> Reddit deleted-content recovery - Pushshift-successor tools (Arctic shift and camas.unddit.com or use other sites) 

**EMAIL-IDS**
```
1. Perform email ID search using the given tools from section 1
2. Try variations of the email ID (can craft using the provided tools)
3. Search for dark web leak databases like IHaveBeenPwned for exact info try using the DeHashed/LeakCheck/InteligenceX
4. Gravatar hash pivot — Gravatar profiles are looked up by the MD5 hash of the (lowercased, trimmed) email address
this helped me once in a CTF :) !
5. With sites that are not covered in the tool, try to login in with the email
6. Try to creat user name from the email ID if nothing works  
```
`The older a social media account is the better as the OPSEC awarness grows with time` 

**IP-ADDRESS**
```
1. Try to use IP address search engines as per provided
2. If you wanna know what services are running use Shodan/Cebsys
3. Reverse DNS + WHOIS + Reverse WHOIS on any ip block you can get
4. Look for passive DNS search history on DNS history.org
5. VPN/proxy/tor exit node detection using the given tool in section 1
```
> Here is a site with a very good interface https://bgp.he.net/ nothing new, just great UI :) 

**PHONE-NUMBER**
```
1. Social media like WhatsApp, Telegram.. provide a category for phone number search
2. Get the number lookup from something like Truecaller 
3. Adding the phone number in WhatsApp/Phone contacts and then looking at profile + last seen can be a breakthrough
4. Password reset trick can also reveal name , username or email in MFA systems
```
**REALNAME**
```
1. Try making a username based on this ans well as the provided email IDs
2. Public record sites mentioned in the tools section (varies by countries)
3. People search aggregator like white pages/spokeo class service
4. Genealogy and obituary sites — surprisingly high-value for mapping a family tree, exactly the kind of pivot that unlocks the rest of the chain.
```
> DEV NOTES: More sites for the real name part that were mentioned here have to be added to the tools section of part 1! 

---
> Practitioner's note: there exist these tools which can be used to get info of the crypto payments like Etherscan which are very use full for osint

**generally each crypto platform have it's own scanner but i will list few general ones**
```
1. Blockchair – covers Bitcoin, Ethereum, Litecoin, Dogecoin, and more, with strong data-export/research features
2. OKLink – broad multichain coverage, leans toward compliance/institutional use
3. DeBank or Zapper – less "explorer," more wallet/portfolio view across EVM chains (good for seeing all your holdings at once rather than one transaction at a time)
```
**Your search may include going on common dark web site**
```
WARNING : Dark web is a dangerous place to be at , continue at your own risk
1. Ahmia (ahmia.fi) — Tor search engine, indexes onion services, filters abuse content
2. Torch — broad, less curated Tor search engine
3. Not Evil — another general-purpose Tor search engine
4. DuckDuckGo onion service — DDG search accessible over Tor
5. ProPublica onion mirror — official, good for testing Tor is working
6. BBC onion mirror — official news mirror
7. Deutsche Welle onion mirror — official news mirror
8. New York Times onion mirror — official news mirror
9. Have I Been Pwned — clearnet breach-checking tool (email/domain lookup)
10. Intelligence X (intelx.io) — searches leaks, historical paste sites, dark web sources
11. DeHashed — searchable breach/credential database
12. Recorded Future — commercial dark web threat intel platform
13. DarkOwl — commercial dark web monitoring platform
14. Flare — commercial dark web/leak monitoring platform
15. SOCRadar — commercial dark web threat intel platform
16. OSINT Framework (osintframework.com) — curated, categorized link directory including a dark web section
17. awesome-osint (GitHub) — community-maintained OSINT resource list
18. ONION SEARCH -> automated darkweb recon 
```
~Dev note: please filter this list~

> Practitioner's note: assume there is a wifi and you have it's BSD and you wanna have more info about it then go to: wigle.net  

> Practitioner's note: Fitness-tracker heatmap -> sometimes apps and fitness websites like Strava er al. can reveal the daily visit location and place, which are mostly workplace and home

This method was actually used to find military bases in a real investigation once 

> Patent information records and licensing bodies have to be real and cannot be faked! 

> bit.ly short links reveal information on adding + at the end

> what's 3 words is a platform that creates 3 words maps for every location 

## OPSEC FOR THE INVESTIGATOR — DON'T BECOME PART OF THE STORY

1. **Research personas ("sock puppets")** — dedicated accounts for investigation, with zero linkage back to your real identity, ideally aged rather than freshly created (a brand-new account trying to connect can itself tip off a HUMINT-sensitive target that they're being looked at).
2. **Browser isolation** — a separate profile or VM for research so your own logged-in accounts, cookies, and search personalization don't bias — or leak into — your results.
3. **Passive means passive.** Don't like, follow, comment, or connect during a recon phase — every interaction is visible to the target and can burn an engagement that depends on staying unnoticed.
4. **Network separation** — where the rules of engagement require it, keep your research infrastructure (VPN, IPs) separate from anything tied to your real identity.
