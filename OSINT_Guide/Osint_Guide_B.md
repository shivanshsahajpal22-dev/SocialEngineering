# OSINT Digital Puppet Creation Guide
## For CTF Challenges and Authorized OSINT Engagements

A digital puppet (also called a sock puppet or legend) is a fully constructed false identity used in OSINT CTFs and authorized red team engagements to conduct open-source reconnaissance without exposing the real operator's identity or tipping off the target. This guide covers building one that holds up to scrutiny.

---

## PART 1 — Operational Security Before You Build Anything

Everything in Parts 2–5 is useless if the puppet's traffic is traceable back to you. Infrastructure comes first.

### 1.1 Network Isolation
* Never access a puppet account from your real IP — ever, not even once.
* One slip permanently links your real identity to the puppet.

> **Recommended stack (in order of isolation strength):**
> * **Option A (CTF-grade):** VPN (paid, no-logs, paid with crypto) $\rightarrow$ puppet browser profile
> * **Option B (engagement-grade):** VPS (paid with crypto, registered under the puppet's identity) $\rightarrow$ Residential proxy or mobile proxy (rotated) $\rightarrow$ Puppet browser profile
> * **Option C (maximum isolation):** Air-gapped VM $\rightarrow$ Tor $\rightarrow$ Residential proxy $\rightarrow$ Puppet browser profile *(slowest, but leaves no infrastructure linkage to you whatsoever)*

### 1.2 Email Infrastructure
Never use your real email provider.

* **CTF-grade:** Proton Mail or Tutanota created over Tor/VPN *(use a phone number not linked to you for verification — see Part 2.3 for anonymous number options)*
* **Engagement-grade:** A domain registered under the puppet's identity (Namecheap + crypto payment) with a self-hosted or Proton for Business mailbox, e.g. `j.harrison@harrisonconsulting[.]net`

> **Key rule:** The email address must be plausible for the puppet's claimed profession and age — a 52-year-old retired logistics manager does not have a `gmail.com`/`anime123` address.

> For short-term burner email you can use : Temp mail/Fakeemailgenerator.com => not professional tho 

### 1.3 Burner / Anonymous Payment Method Setup

To maintain complete separation between your real-world identity and your puppet infrastructure, you need a funding mechanism that cannot be traced back to your personal bank accounts, credit cards, or legal name. 

---

#### Step 1: Procuring Anonymous Funding Sources

* **Cash-Purchased Prepaid Cards:** Purchase Visa, Mastercard, or generic reloadable debit/gift cards using physical cash at retail stores (e.g., supermarkets, convenience stores). 
  * *Implementation note:* Many regions now require ID for certain prepaid cards or limit cash reloads. Look for non-registered "gift cards" or open-loop prepaid cards that do not require an SSN or government ID to activate online (or can be registered with burner credentials).
* **Cryptocurrency via Peer-to-Peer (P2P) or Cash-to-Crypto:** 
  * Avoid centralized exchanges (CEXs) that enforce strict Know-Your-Customer (KYC) rules linked to your legal identity.
  * Instead, use privacy-focused cryptocurrencies (like Monero / XMR) obtained via decentralized exchanges (DEXs), non-custodial atomic swaps, or cash-to-crypto local meetups/ATMs where permitted.

#### Step 2: Setting Up Virtual Credit Cards (VCCs)

To safely purchase domains, hosting, proxies, or services for your puppets without exposing your real financial details, route your funds through Virtual Credit Card providers.

* **Privacy-First VCC Providers:** Use services that allow you to generate single-use or burner virtual card numbers. 
  * *Implementation note:* Fund these accounts using your non-KYC crypto or anonymized prepaid funding sources whenever possible to avoid linking your personal debit/credit card to the service.
* **Burner Billing Details:** When creating the VCC or checking out at a vendor, use generated or consistent alias billing details that match your puppet's geographical profile (ensuring ZIP codes match the proxy location if address verification systems [AVS] are enforced).

#### Step 3: Operational Security (OpSec) for Payments

* **Never Cross-Contaminate:** Never use the same payment card, crypto wallet address, or VCC account across multiple independent puppets if they need to maintain strict compartmentalization. Treat each payment source as single-use or strictly siloed.
* **IP and Browser Consistency:** When funding or generating VCCs, ensure your traffic is routed through the exact same proxy network or clean environment you intend to use for the puppet operations to prevent behavioral correlation.

### 1.4 Device Fingerprint Isolation
Each puppet gets its own:
* Dedicated browser profile (Firefox with arkenfox user.js, or a separate Chromium instance — never your daily browser)
* Unique canvas fingerprint (use CanvasBlocker or a browser that spoofs this by default)
* Unique User-Agent string consistent with the puppet's claimed device
* Timezone set to match the puppet's claimed location
* Language set to match the puppet's claimed locale
* No extensions beyond what a normal user of that persona would have

#### Using a Burner Virtual Machine (VM)

A burner virtual machine offers robust isolation by running an entirely separate operating system instance on your existing hardware, completely cutting off the profile from your host OS, files, and daily applications. 

* **Setup & Execution:** Spin up a lightweight guest OS using a hypervisor like VirtualBox, VMware, or KVM. Configure its virtual network interface to route exclusively through your proxy or VPN before opening any browser. 
* **The "Burner" Lifecycle:** Treat the VM as disposable. Keep a pristine, clean snapshot of the virtual machine immediately after setup. When a puppet session ends, simply revert the VM to that clean state or discard it entirely, erasing all local cookies, cache, and tracking artifacts instantly.
* **Caveat:** Default hypervisor graphics drivers and hardware strings can sometimes be flagged by advanced trackers, so ensure you still employ browser-level fingerprint masking (such as Canvas spoofing and consistent User-Agents) inside the VM environment.

#### Buying and Using a Burner Device

A physical burner device (such as a cheap, pre-owned smartphone or a low-cost laptop) provides the gold standard for device fingerprint isolation because it presents authentic hardware components, real mobile/residential network characteristics, and genuine sensor data that cannot be perfectly emulated by software.

* **Acquisition:** Purchase a low-cost, unlocked device entirely with cash or privacy-focused payment methods, avoiding any linkage to your personal identity, credit cards, or home address. When possible, factory-reset the device or flash a clean, privacy-respecting operating system (like GrapheneOS for supported phones).
* **Network Hygiene:** Never connect a burner device to your home Wi-Fi network or any network tied to your real-world identity. Instead, use mobile cellular data via a prepaid SIM card bought anonymously, or connect exclusively through a trusted, isolated external hotspot.
* **Operation:** Perform all puppet activities physically on the device. Because the hardware, MAC address, battery APIs, and screen sensors are entirely distinct from your primary workstation, the risk of cross-context correlation or hardware-level leaks is reduced to zero.

---

## PART 2 — Legend Construction (The Identity Itself)

### 2.1 Core Identity Fields
Decide all of these before creating any account. Inconsistency between platforms is the most common way puppets get burned.

| Field | Guidelines & Best Practices |
| :--- | :--- |
| **Full legal name** | Choose a name that is common enough to return many real search results (protects against "no results = fake person"), not so common it looks like a placeholder, and consistent with the claimed ethnicity and region (e.g., "James R. Hartley" not "John Smith"). |
| **Date of birth** | Pick a real birthdate, not a round number (Bad: `01/01/1980` \| Good: `14/03/1979`). Age should be consistent with claimed career timeline. |
| **Location** | A real city the puppet claims to live in or near. Must know basic facts about it (local sports team, major employer, neighbourhood names, local news outlet). Timezone on all devices must match this city. |
| **Occupation** | Choose something that explains why the puppet would be contacting/researching the target (a security researcher, a journalist, a vendor, a recruiter — whatever fits the engagement), has a plausible LinkedIn career trajectory, and generates natural-sounding reasons to reach out. |
| **Education** | A real university the puppet claims to have attended. Check that the claimed graduation year is consistent with the date of birth (graduated at $22 \pm 2$ years). Do not claim degrees that can be verified via a registrar lookup (some universities publish alumni directories). |
| **Backstory notes** | 3–5 bullet points of personal detail you will never post publicly but will remember if questioned: Sibling names, hometown (different from current city), first job, a hobby, a pet's name. These exist so you stay consistent across long multi-turn interactions with a target. |

> Before we begin with this section, you can always use [fakenamegenerator.com](https://www.fakenamegenerator.com/) to create fake entire persona plans 

### 2.2 Profile Photo
Never use:
* A photo of a real person (reverse-image-searchable in seconds)
* A stock photo (indexed by every reverse image search engine)
* An AI face from a well-known generator without processing 
* ThisPersonDoesNotExist.com* faces are now detectable by several tools including Hive Moderation and AI-or-Not {just use social engineering to gt the face ;)}

> **Recommended approach:**
> 1. Generate a base face using StyleGAN2 or a similar local model *(run locally — do not use web-based generators that log inputs)*.
> 2. Process it through:
>    * A slight colour grade change (desaturate slightly, adjust warmth)
>    * A mild sharpness/blur pass on the background
>    * Add a realistic background (a café, an office, outdoors) using inpainting — the plain white background is a tell.
> 3. Run your output through AI-or-Not and Hive to check detectability before committing to it.
> 4. Store it at a realistic JPEG compression level (very high quality = stock photo tell; aim for ~85% quality).
> 5. Strip all EXIF metadata before uploading anywhere (`exiftool -all= photo.jpg`).

For a more robust puppet:
* Create 3–5 variations of the same face (different lighting, slightly different angle, one "candid" crop).
* Use these across different platforms so they look like a real person's photo library rather than a single headshot reused everywhere.

### 2.3 Phone Number (for account verification)
* **CTF-grade (fast, low cost):**
  * `Silent.link` — crypto-paid eSIM, no registration required
  * `MySudo` — US/Canada numbers, compartmentalized
  * Temporary SMS services (SMS-Activate, 5sim.net or recievesms.com) for one-time verification codes only — do not use these as a "real" number the puppet gives out, only for the initial account creation code.
  * for the cheapest category you can use : Google voice or text now or text free (by it's something that should genuinely be avoided)
* **Engagement-grade (more persistent):**
  * A prepaid SIM purchased with cash in person *(check local laws — some jurisdictions require ID for SIM purchase)*
  * `Silent.link` eSIM provisioned on a dedicated device or eSIM slot that never touches your real accounts.

> **Key rule:** The phone number's country code must match the puppet's claimed location — a UK-based persona with a +1 US number is immediately suspicious.

`I have recently covered the number in the burner phone number generation in section 8 too, just a little better and practical than here so check it out too`
---

## PART 3 — Platform Presence (Account Creation and Aging)

### 3.1 Account Creation Order and Timing
Do not create all accounts on the same day — this is one of the most common automated detection signals on major platforms.

> **Recommended creation sequence:**
> * **Week 1:** Email account (the anchor — everything else registers with this) $\rightarrow$ LinkedIn (most important for professional engagement scenarios) $\rightarrow$ Twitter/X
> * **Week 2:** GitHub (if the puppet is a technical persona) $\rightarrow$ Reddit (lurk only for 2 weeks before posting)
> * **Week 3+:** Any other platform required by the specific engagement $\rightarrow$ Facebook (hardest to age convincingly — only create if required)

Between each creation: wait at least 48 hours. Never create more than 2 accounts in a single session.

### 3.2 Account Aging — the Most Important and Most Skipped Step
Platforms detect new accounts aggressively. A 3-day-old LinkedIn profile that immediately starts reaching out to targets is flagged and restricted almost immediately.

> **Minimum aging periods before operational use:**
> * **LinkedIn:** 3–4 weeks of passive activity (profile views, skill endorsement received, connection with 5–10 accounts that are not your targets)
> * **Twitter/X:** 2 weeks of posting (2–3 tweets per week, replies to real public accounts on topics matching the persona)
> * **Reddit:** 4–6 weeks; accumulate at least 50 karma before commenting in any subreddit relevant to the target
> * **GitHub:** Star 10–15 repos consistent with the persona's claimed skills; optionally fork one and make a small commit
> * **Facebook:** 8–12 weeks minimum; Facebook's account authenticity checks are the most aggressive of any major platform

During aging, the puppet should:
* Post/engage on topics consistent with the legend (not random)
* Follow/connect with real accounts in the claimed industry
* Never interact with anything related to the actual target until the operational phase begins
* Build a consistent posting time pattern matching the puppet's claimed timezone (post during their "workday hours")

### 3.3 Content Strategy Per Platform
* **LinkedIn:**
  * Complete all profile sections (headline, about, experience, education, skills, featured) — incomplete profiles are flagged.
  * Connect with real people in the claimed industry first (accept anyone who sends a request during aging).
  * Post 1 industry article comment per week during aging.
  * Recommended connection count before ops: 50+.
* **Twitter/X:**
  * Follow 40–60 accounts in the persona's interest areas.
  * Retweet 2–3 times per week, original tweet once per week.
  * Engage with at least one real conversation thread per week (reply to a public account's tweet on a neutral topic).
  * Do not follow the target account during aging.
* **Reddit:**
  * Post in 2–3 subreddits consistent with the persona's hobbies (not the subreddits relevant to the engagement target).
  * Build karma in neutral/hobby subreddits first (gaming, cooking, local city subreddit for the puppet's city).
  * Only move to target-relevant subreddits once karma > 50.
* **GitHub:**
  * Star repos in the claimed tech stack.
  * Write a realistic README on any forked repo.
  * Contribution graph does not need to be dense but should not be completely empty.

---

## PART 4 — Operational Use (Actual OSINT Engagement)

### 4.1 Pretext Development
The pretext is the cover story for why the puppet is making contact. It must answer three questions the target will consciously or unconsciously ask:

1. **Why is this person reaching out to ME specifically?** *(not "I'm doing research" — something specific to the target)*
2. **What do they want from me?** *(must be something a real person in the puppet's claimed role would plausibly want)*
3. **What's in it for me (the target)?** *(give the target a reason to engage — information they'd want, an opportunity, a flattering request for expertise)*

> **Common pretext frameworks for OSINT engagements:**
> * **Recruiter/headhunter:** *"I came across your profile while sourcing for a [role] at a [company type] — your background in [specific skill] caught my attention. Would you be open to a brief conversation?"* $\rightarrow$ Gets targets to confirm employment, skills, and often their personal email/phone voluntarily.
> * **Journalist/researcher:** *"I'm working on a piece about [industry topic] for [publication]. Your LinkedIn suggests you have direct experience with [specific area] — would you be willing to be quoted as an expert source?"* $\rightarrow$ Gets targets to disclose org structure, internal processes, sometimes technical details they are proud of.
> * **Vendor/partner:** *"We're evaluating solutions in [space] and [target company] came up as a potential partner. I wanted to connect with someone on the technical side before we engage procurement."* $\rightarrow$ Gets targets to disclose technology stack, vendor relationships, procurement contacts.
> * **Conference/community:** *"I saw your talk at [real conference the target attended] / your post in [real community they participate in] — I had a follow-up question about [specific technical detail from that content]"* $\rightarrow$ Requires prior OSINT to identify the conference/post, but extremely high response rate because it is highly specific.

### 4.2 Contact Sequencing
Never make first contact on the most sensitive platform.

> **Recommended sequence:**
> * **Step 1: Passive observation (no contact)**
>   * Follow on Twitter, read public posts, identify patterns.
>   * Note posting times, topics, emotional tone, personal mentions.
> * **Step 2: Indirect engagement (no direct message yet)**
>   * Like or comment on a public post on a neutral topic.
>   * This puts the puppet's profile in the target's notification feed and makes step 3 feel like a natural continuation.
> * **Step 3: Connection request (LinkedIn) or follow + DM (Twitter)**
>   * Send with a brief, specific personalisation line referencing something from step 1 or step 2.
>   * Do not include the actual ask in the first message.
> * **Step 4: Rapport building (1–3 exchanges before the operational ask)**
>   * Respond to whatever they say, ask a follow-up that is genuinely interesting to them, not to you.
>   * The goal is for the target to feel like they initiated the relationship direction.
> * **Step 5: Operational ask**
>   * Frame as a natural extension of the conversation.
>   * Never ask for more than one thing per exchange.

### 4.3 OSINT Collection During Engagement
While the puppet is active, collect and log:

* **From direct responses:**
  * Confirmed current employer and role
  * Confirmed personal email or phone (if offered)
  * Technology stack mentions
  * Colleague names and roles mentioned in passing
  * Travel/location disclosures ("I'm in NYC this week for...")
  * Org structure clues ("my manager / our CISO / the team lead")
* **From profile enrichment (passive, no contact required):**
  * Cross-reference LinkedIn education with university alumni directories for maiden names, graduation years, societies.
  * Check GitHub for email addresses in commit metadata: `git log --format="%ae" | sort -u`.
  * Check Twitter for location metadata in old tweets (some clients embedded GPS — tools: Tinfoleak, Twint archive).
  * Check Keybase, HaveIBeenPwned for email confirmation.
  * Run profile photo through reverse image search at each stage to find other platform presences.

Document everything with timestamps — this is your evidence log for the engagement report and demonstrates you stayed in scope.

---

## PART 5 — CTF-Specific Techniques

### 5.1 Common CTF OSINT Puppet Scenarios
* **Scenario type 1 — "Find the target's real identity"**
  You are given a puppet account (username, email, or profile) and must trace it back to a real person.
  * **Approach:**
    * Username search across all platforms: Sherlock, Maigret, WhatsMyName
    * Email $\rightarrow$ breach data: DeHashed, IntelX, HaveIBeenPwned
    * Profile photo $\rightarrow$ reverse image: Google Images, TinEye, Yandex Images *(Yandex is significantly better than Google for face matching)*
    * Writing style $\rightarrow$ search for distinctive phrases in quotes on Google
    * Timezone inference from posting times
    * Cross-reference all found accounts for consistent details
* **Scenario type 2 — "Impersonate / infiltrate a community"**
  You must create a convincing puppet to join a private community and extract information from it.
  * **Approach:**
    * Study the community's existing members before joining (writing style, shared references, inside terminology).
    * Your puppet's backstory must be plausible to that community.
    * Age the account in adjacent public communities first.
    * Join via an existing member's referral/invite if possible.
* **Scenario type 3 — "Build a sock puppet that passes scrutiny"**
  The CTF itself evaluates whether your puppet is detectable.
  * **Common detection checks the CTF will run:**
    * Reverse image search on the profile photo
    * Account creation date vs. posting history (too perfect = fake)
    * Username consistency across platforms
    * Writing style consistency
    * Metadata in any uploaded files
    * IP/ASN of account registration if the CTF controls the platform

### 5.2 Tools for CTF Puppet OSINT
* **Username enumeration:**
  * `Sherlock` — `github.com/sherlock-project/sherlock`
  * `Maigret` — `github.com/soxoj/maigret` *(more sources than Sherlock)*
  * `WhatsMyName` — `github.com/WebBreacher/WhatsMyName`
* **Email investigation:**
  * `Holehe` — `github.com/megadose/holehe` *(which sites this email registered on)*
  * `h8mail` — `github.com/khast3x/h8mail` *(breach correlation)*
  * `Emailrep.io` — API — reputation/age of an email address
* **Photo analysis:**
  * `ExifTool` — metadata extraction
  * `FotoForensics` — `ela.r0k.us` — error level analysis for manipulation
  * `AI-or-Not` — `aiornot.com` — GAN/diffusion face detection
  * `Hive Moderation` — `hivemoderation.com/demo`
* **Cross-platform correlation:**
  * `SpiderFoot` — automated OSINT correlation across sources
  * `Maltego` — relationship graph building (commercial, free tier exists)
* **Writing style:**
  * Stylometry tools — function word frequency analysis
  * Google exact-phrase search "distinctive phrase" in quotes

---

## PART 6 — Teardown (Engagement End)

At engagement end, every puppet account must be deactivated or deleted in reverse order of creation — do not leave live accounts associated with the engagement target after scope ends.

* **Per-platform deletion:**
  * **LinkedIn:** Settings $\rightarrow$ Account preferences $\rightarrow$ Close account
  * **Twitter/X:** Settings $\rightarrow$ Your account $\rightarrow$ Deactivate account *(30-day hold before permanent deletion)*
  * **Reddit:** nuke account via reddit-user-cleaner before deletion to remove post history first
  * **GitHub:** Settings $\rightarrow$ Delete this account
  * **Email:** Delete after all platform accounts are deleted *(deleting email first locks you out of platform deletion flows)*
* **Document in the engagement report:**
  * Every account created (platform, username, creation date)
  * Deletion confirmation (screenshot or date)
  * Any content posted during the engagement (for the client's records)
  * Any real people interacted with and what was disclosed to them
* **Data hygiene:**
  * Delete all locally cached session data for the puppet browser profile
  * Destroy the dedicated VM snapshot if one was used
  * Revoke any API tokens created under the puppet's accounts
  * If a phone number was used, cancel/port the SIM

---

## PART 7 — Detection Tells to Avoid (Summary Checklist)

### Infrastructure:
* [ ] Never accessed from real IP
* [ ] Timezone on device matches claimed location
* [ ] Browser fingerprint isolated from real identity
* [ ] No cross-contamination between puppet and real browsing session

### Identity:
* [ ] Profile photo passes reverse image search
* [ ] Profile photo passes AI detection tools
* [ ] EXIF stripped from all uploaded photos
* [ ] Name returns plausible search results (not zero, not millions)
* [ ] DOB is not a round number
* [ ] Career timeline is consistent with DOB
* [ ] Education dates are consistent with DOB

### Platform presence:
* [ ] Accounts not all created on the same day
* [ ] Minimum aging periods respected before operational use
* [ ] Posting times consistent with claimed timezone
* [ ] Writing style consistent across all platforms
* [ ] No engagement with target during aging phase
* [ ] Connection/follower count is non-zero before ops

### Operational:
* [ ] Pretext answers "why me, why now, what do you want"
* [ ] No direct ask in first contact message
* [ ] All interactions logged with timestamps
* [ ] Scope boundary respected throughout
* [ ] Teardown documented and confirmed

---


## PART 8 — The osint router setting up everything: quick run through 

`this section exists when the theory starts to eat you too much !`

### **PAYMENT METHODS**
`???`

### **VPS/VPN/RESIDENTIAL PROXY**

VPN  
- [Mullvad VPN](https://mullvad.net)
  - Accounts: You generate a 16-digit account number with no email, username, name or password
  - Payment method can also be routed through the [proxy store](https://digitalgoods.proxysto.re/en)
  - Downsides: There are no refunds on cash and crypto payments. It has no free tier and is less polished than mainstream VPNs.
- [Proton VPN](https://proton.me/tor) or [clearnet](https://protonvpn.com/)
   - Account: It requires a Proton account, so an email address is needed (you can register over Tor, as covered earlier)
   - You can always buy through Monero routing through the [proxy store](https://digitalgoods.proxysto.re/en) itself
 
VPS 
- [Njalla VPS](https://njal.la)
  - Signup and payment: Signup is via email or XMPP, and payment is Bitcoin, Litecoin, Monero, Ethereum or PayPal. There is no cash option
  - Cost and location: A competitor's comparison lists the entry VPS at €15 a month for 1 vCPU and 1.5 GB RAM, billed monthly, with all VPS in Sweden
- [bithost VPS](https://bithost.io)
  - How it works: You sign up with an email and password, fund your balance in crypto, and bithost deploys servers on DigitalOcean, Vultr or Linode under its own provider account.
  - Pricing: Servers start at $0.015 an hour, and Monero deposits credit in about 20 minutes.
  - Constraints: You must upload an SSH key, servers are deleted automatically if your balance runs out, backups aren't guaranteed, and withdrawals aren't allowed


RESIDENTIAL PROXIES  

> Residential proxies are not privacy tools like VPNs. The provider sees your traffic and your payment, and the pool's origin matters. Google's investigation found that many providers' ethical-sourcing claims were overstated, and in January 2026 it obtained a court order to take down domains tied to the Ipidea network. Most "best no-KYC" lists are affiliate sites, so treat rankings as marketing. 
Wikipedia

- [Byteful](https://byteful.com/proxy-service/residential-proxies) (formerly Ping Proxies) : accepts Monero among other crypto, and says it enforces an acceptable-use policy. 
Byteful
- [IPRoyal](https://iproyal.com/other-proxies/buy-a-proxy-with-crypto-bitcoin/) : KYC is only required for large plans and some restricted targets.
- [anyIP](https://anyip.io/crypto/buy-proxies-with-xmr-monero) : activates immediately on Monero payment with no KYC.
- [Froxy](https://froxy.com/en/residential-proxies) : claims ethical sourcing, but some payment methods may be unavailable because of KYC procedures

### **EMAIL**

Create Proton email through their Tor mirror => [click me, definitely not malware :)](https://protonmailrmez3lotccipshtkleegetolb73fuirgj7r4o4vfu7ozyd.onion/) or find link [here](https://proton.me/tor) + here is thier [clearnet site](https://proton.me/)
For payment, use Menero to buy a voucher in [proxystore](https://digitalgoods.proxysto.re/en) and redeem that voucher to activate the plus account 

OR 

Use Tuna Mail here => [clear site](https://tuta.com/), or you can definitely do all these activities over tor 
same proxy store method works here too for payment and activation! 

### **PHONE NUMBER**

Tier 1 — Fully Anonymous (No Identity Required)

> Prepaid SIM — Cash Purchase
```
Note: India requires Aadhaar-based KYC for all SIM purchases
under TRAI regulations — no carrier is legally exempt from this.
A prepaid SIM in India is linked to your Aadhaar by default.

For number separation without identity linkage, VOIP is the
more practical path in the Indian context.

If you still want a physical SIM:
→ Buy from a small local mobile shop (not a branded carrier store)
  in a busy market area — Nehru Place (Delhi), Linking Road (Mumbai),
  SP Road (Bangalore), Ritchie Street (Chennai)
→ Pay cash
→ The KYC requirement still applies legally — understand that
  before proceeding
```

VOIP Numbers
`This is most likely the tier you are going to operate on. + most likely the Tier 3 as the helper tier.`

| Service | Link | Notes |
|---|---|---|
| MySudo | [mysudo.com](https://mysudo.com) | Multiple numbers per account; real DID numbers not flagged as VOIP; anonymous signup |
| JMP.chat | [jmp.chat](https://jmp.chat) | XMPP-based; pay with Monero; most private option that still gives a working SMS-capable number |
| Hushed | [hushed.com](https://hushed.com) | Anonymous signup; pay with gift cards or crypto |
| Textr | [textrapp.com](https://textrapp.com) | Works well from India; supports Indian numbers |

```
Note: many services reject VOIP numbers for SMS verification;
for those, a secondary physical SIM is more reliable
```

Tier 2 — Pseudonymous (Identity Decoupled, Not Eliminated)

| Service | Link | Notes |
|---|---|---|
| Google Voice | [voice.google.com](https://voice.google.com) | Not directly available in India without a US number to port; workaround exists via VPN but unreliable |
| TextNow | [textnow.com](https://textnow.com) | Low-stakes only; ad-supported; usage logged; accessible from India |
| IndiaMART virtual numbers | [indiamart.com](https://indiamart.com) | Business-facing but worth knowing; virtual numbers for business use |

Tier 3 — Disposable / Temporary

Public SMS Sites
```
Anyone can read SMS sent to these — one-time throwaway only
Never use for any account you want to retain
```

| Site | Link |
|---|---|
| receive-smss.com | [receive-smss.com](https://receive-smss.com) |
| quackr.io | [quackr.io](https://quackr.io) |
| receivesms.co | [receivesms.co](https://receivesms.co) |
| hs3x.com | [hs3x.com](https://hs3x.com) |
| smsreceivefree.com | [smsreceivefree.com](https://smsreceivefree.com) |

Temp Number Apps
```
Number assigned to your session only, not broadcast publicly
Better than public sites; same VoIP-blocking caveats apply
```

| App | Link |
|---|---|
| Phoner | [phonerapp.com](https://phonerapp.com) |
| 2ndLine | [2ndline.co](https://www.2ndline.co) |

Operational Notes

```
Device separation   → private number should never touch your daily phone;
                       cheap second-hand Android from Nehru Place / SP Road
                       used only on public WiFi is the cleanest setup

Network separation  → do not activate or use on home WiFi or Jio/Airtel
                       connection tied to your identity; use public WiFi
                       at a cafe, library, or metro station

Signup environment  → creating a pseudonymous account from your real
                       browser on your real network defeats the purpose

India KYC reality   → there is no legal way to get a physical Indian SIM
                       without Aadhaar linkage under current TRAI rules;
                       VOIP + burner device is the practical alternative
```

### **BURNER DEVICES**

Where to Buy Physical Burner Devices in India

```
If you want a completely separate cheap device for the number:
```

| Location | What to Find |
|---|---|
| Nehru Place, Delhi | Largest electronics grey market; cheap Android phones, refurbished handsets, cash deals common |
| SP Road, Bangalore | Electronics wholesale market; budget Android devices widely available |
| Ritchie Street, Chennai | Electronics hub; wide range of budget phones, accessories |
| Linking Road / Dharavi electronics, Mumbai | Budget handsets, accessories, cash transactions normal |
| Palika Bazaar, Delhi | Underground market; very budget handsets |
| Any local mobile repair shop | Second-hand cheap Androids; cash purchase; no paperwork |

```
What to buy:
→ Any Android device under ₹1500–2000 (second-hand)
→ Does not need a SIM slot if you're using VOIP only
→ Needs WiFi capability — that is all
→ Factory reset before use
```

OR 

You can use the following virtual machine to set up the burner devices and environments

[Whonix Workstation](whonix.org)  → purpose-built for anonymity; routes all
                                    traffic through Tor by default; best option
                                    if anonymity is the primary goal;
                                    run inside VirtualBox or KVM
                        

[Qubes OS VM](qubes-os.org)  → if you're already on Qubes, spin a
                               disposable Qube for the number activity;
                               strongest isolation if your threat model
                               is host compromise; 


Plain [Ubuntu](https://ubuntu.com/) (Best) → works fine for low-threat-model use cases;
VM in VirtualBox                             no built-in anonymity features so you are
                                             responsible for network separation yourself

[Waydroid](https://waydroid.org) on Linux    → runs full Android (LineageOS based) inside
                                               a Linux container; hardware accelerated so
                                               feels native speed; no Google services by
                                               default; best option if you're already on
                                               Linux and need a mobile-only VOIP app

[Android-x86](https://android-x86.org)       → full Android environment in VirtualBox or
in VirtualBox                                  VMware; works on Windows, Mac, Linux;
                                               degoogled builds available; slower than
                                               Waydroid but more universally compatible

### **WIFI/NET ACCESS**
`use a tor + vps + anonamouse wifi setup` 

Either buy a network on the anonymous SIM we are talking about 

OR 

use a public wifi: "Libraries and cafes are best for this; visit one that provide free public wifi"
```
Mitigations if near home is unavoidable:

→ Leave your regular phone at home or in airplane mode
  (eliminates cell tower location ping)
→ Randomize MAC address on the device you're using
  (built into modern Android and Linux by default)
→ Vary the location and time
→ Tor on top handles the traffic layer regardless
```

### **AGING TIMELINE**
`???`

`Thanks for visiting the place, fellow learner, kbai :) ~ dev.`
