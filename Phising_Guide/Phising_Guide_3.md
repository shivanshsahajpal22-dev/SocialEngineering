# Infrastructure setup guide part 2 

`Read the phishing guide part 2 for the full picture, since it is part 1 of infra setup.` 

## PHONE INFRA GUIDE

### RAW PHONE INFRASTRUCTURE

#### Why this isn't a regular VPS server

You already know how to spin up a VPS and run something like an FTP server — a client connects, authenticates, transfers a file, disconnects. Phone infra looks nothing like that under the hood, and the differences are exactly where people get tripped up building it for the first time.

|  | Regular VPS/API server | Phone infra (SIP/RTP) |
| --- | --- | --- |
| Connection model | Request → response, done | **Live, stateful, two-way session** — established, held open, torn down in real time |
| Protocol | HTTP | **SIP** (signaling: setup/teardown) + **RTP** (media: the actual audio) — two separate concerns, can even travel different network paths |
| Identity check | You control auth (API keys, tokens) | **STIR/SHAKEN signs the caller ID at call setup, in real time, in-band** — the terminating carrier verifies it before the call even rings. Unlike DKIM, nothing is checked later against a static record — it's live, per call |
| Failure handling | Retry logic you write | **No retry model at all.** A failed call is just failed — any callback/retry behavior (voicemail, scheduled re-dial) has to be built explicitly |
| Scaling concern | CPU/memory/connections | **Concurrent channel capacity** on your SIP trunk — a hard ceiling on simultaneous calls, not a request-rate limit |
| New problems you didn't have before | — | **Jitter, packet loss, NAT traversal for media, codec negotiation** — none of this exists in a normal web server |

### Path A vs Path B — pick this first, it decides everything else

Just like email has "use an ESP" vs "run your own MTA," phone has the same fork:

| Path | What it is | Pros | Cons | Who it's for |
| --- | --- | --- | --- | --- |
| **A. Cloud phone platform (leased call-flow logic)** — [Twilio Voice](https://www.twilio.com/en-us/voice), [Telnyx Voice API](https://telnyx.com/products/voice-api), [Vonage Voice API](https://www.vonage.com/communications-apis/voice/), [Plivo Voice](https://www.plivo.com/voice/), [Sinch Voice](https://www.sinch.com/products/voice/) | You call their API, they run the actual SIP/media infrastructure | Fastest to launch, no server ops, STIR/SHAKEN signing handled for you once verified | Less low-level control over call routing/codecs | 95% of businesses — this is your default |
| **B. Your own PBX/softswitch on cloud machines** — [Asterisk](https://www.asterisk.org/), [FreeSWITCH](https://freeswitch.com/) | You run the actual telephony server | Full control, custom routing logic, lower cost at very high volume | You own uptime, security patching, codec/jitter tuning, and the STIR/SHAKEN signing integration | Only if you need something the platforms genuinely can't do |
| **C. Cloud contact-center service** — [Amazon Connect](https://aws.amazon.com/connect/) | Managed queues, agents, reporting on top of AWS telephony | No call-flow code needed for standard contact-center patterns | Less flexible for custom logic outside their model | Support/sales teams needing agents+queues fast, not custom IVR logic |

**My honest default: start with Path A.** Twilio or Telnyx get you a verified, signed, working number in under a day. Move to Asterisk/FreeSWITCH only once you have a concrete reason the platform can't do (e.g. extremely custom media processing, or cost at massive scale).

### Building it — the actual components, Path A

1. **Sign up with your chosen provider and verify your business.** This single step is the most important one in the entire guide — it's what determines your STIR/SHAKEN attestation level later (see the dedicated section below). In Twilio this is under Trust Hub; in Telnyx it's Business Identity verification.
2. **Provision your SIP trunk.** With Path A you don't manually configure SIP — the provider's console gives you a trunk endpoint and credentials. Note your trunk's **concurrent channel limit** (this is your real capacity ceiling, not a rate limit — it's "how many calls at once," not "how many calls per second").
3. **Buy/port your number(s).** Porting an existing number takes 1–4 weeks; don't cancel the old service until porting confirms complete.
4. **Set caller name (CNAM)** and branded calling if your provider offers it (covered in its own section below).
5. **Build the call-flow logic.** This is where SIP's "live session" nature shows up in your code: instead of a single request handler, you write a state machine — call rings → answered → menu played → input collected → transferred/queued → ended. Twilio uses TwiML (XML) or a programmable Voice SDK; Telnyx and Plivo have their own call-control APIs; all follow this same event-driven shape.
6. **Wire the redirector (front door) in front of your call-control API and webhook receivers** — covered in its own section below, build this before the call-flow logic goes live.
7. **Wire the webhook receiver** behind the redirector for call events (ringing, answered, ended, failed) — these arrive asynchronously, after the fact, same pattern as email/SMS delivery receipts.
8. **Add retry/voicemail logic explicitly.** Since SIP has no retry queue, build: voicemail detection (most providers expose an API flag for this), a callback queue for missed calls, and a scheduled re-dial with backoff if that matters for your use case.
9. **Wire recording/voicemail storage** to encrypted storage with access control and a retention policy, separate from the call-flow service itself.
10. **Load-test concurrent channel capacity** before launch — fire N simultaneous test calls up to your trunk's stated limit and confirm call quality holds.

### Redirector (Front Door) — build this before the call-flow service touches the internet

Put a redirector/reverse proxy in front of every public endpoint related to calling, *before* building the actual IVR/call-flow logic behind it — this protects the webhook receiver and call-control API from day one.

Put a redirector in front of:

- Your **call-control API** (where your app triggers outbound calls or configures flows)
- **Webhook receivers** for call events (ringing, answered, ended, failed)
- Any **web-based IVR builder or recording-playback endpoint** you expose
- **Click-to-call** web widgets, if you have them

Checklist:

- [ ] HTTPS only, auto-renewing certs
- [ ] Rate limits and request-size limits on every endpoint
- [ ] A WAF for bots and common attacks
- [ ] Webhook routes: allow only the provider's published IPs where available, **and still verify every signature** (Twilio signs with `X-Twilio-Signature`; Telnyx and others have their own HMAC scheme — check your provider's webhook security docs)
- [ ] Internal call-flow services never exposed directly to the internet
- [ ] Two or more redirector instances with health checks
- [ ] Load balancer + WAF, or Nginx/Envoy/Caddy, defined in IaC

**Fast-rebuild notes (legitimate DR, not label-evasion):** keep the redirector config as its own IaC module, separate from call-flow logic; version your WAF/rate-limit rules so a fresh instance inherits protection immediately; keep an idle secondary redirector ready to promote via DNS/load-balancer switch so call-event webhooks aren't dropped mid-rebuild.

---

### The System Hardening

**Access Control**
- SSH key-only authentication — disable password login entirely
- Disable root login — require a non-root user, then `sudo`
- Fail2ban or similar — auto-ban IPs after repeated failed login attempts
- Non-default SSH port
- IP allow-listing for SSH/admin access
- **SIP registration requires strong, non-default credentials per extension/trunk** — weak SIP auth is the #1 entry point for toll fraud
- **PBX/SBC admin web console restricted to VPN/known-IP only**, never exposed publicly (FreePBX's web GUI being internet-facing is one of the most common real-world compromise stories)
- **Disable anonymous/guest SIP calls entirely** unless you have a specific, deliberate reason to allow them

**Firewall / Network Exposure**
- Default-deny inbound; minimal open ports
- Egress filtering
- **Restrict the SIP signaling port (5060/5061) to your trunk provider's known IPs only** — don't leave it open to the whole internet; open SIP ports are constantly scanned specifically for toll-fraud exploitation
- **Narrow the RTP media port range to the minimum you actually need**, and firewall it to expected media-relay/SBC IPs rather than leaving the full 10000–20000 range open to anyone
- **Put a Session Border Controller (SBC) in front of any self-hosted PBX** — it plays the same role as your web redirector, but for SIP/RTP: it's the thing the internet touches, so Asterisk/FreeSWITCH itself never faces the internet directly

**OS and Software Hygiene**
- Regular patching; minimal installed software; disable unused services
- **Patch Asterisk/FreeSWITCH on their own cycle** — PBX software has a well-documented history of being a toll-fraud target with its own CVEs, separate from general OS patching
- **Disable and remove unused SIP extensions/trunks immediately**, don't leave dormant ones configured "just in case"

**Logging and Monitoring**
- Centralized, tamper-resistant logging
- Intrusion detection
- **Wire unusual call-destination patterns into your intrusion detection, not just billing alerts** — by the time a toll-fraud spike shows up on your bill, it's already cost you; catching it in near-real-time via call logs is the actual defense
- **Alert on concurrent-channel usage spikes outside normal business hours** — toll fraud disproportionately happens nights/weekends when no one's watching the dashboard
- **Ship CDRs (call detail records) off-host** for audit, same reasoning as mail logs

**Secrets and Credentials**
- No hardcoded credentials; secrets manager; least privilege
- **SIP trunk credentials and provider API keys in the secrets manager**, rotated on a schedule
- **Recording/voicemail storage access keys kept separate and least-privilege from call-flow service credentials** — a compromised call-flow process shouldn't automatically have access to every historical recording

**Data Protection**
- Encryption at rest; no sensitive data lingering
- **Encrypt call recordings and voicemail at rest**, not just access-controlled
- **Confirm the payment-capture pause actually works end to end if taking card payments by phone** — card numbers must never reach a recording or a log, this is a PCI requirement, not a nice-to-have
- **Enforce retention limits automatically** (auto-delete past the policy window), not as a manual cleanup task someone has to remember

---

## NOT GETTING FLAGGED

This is the section that actually determines whether your calls ring through or show "Spam Likely." Think of each subsection below as one knob a carrier's spam-scoring model is watching — get each one genuinely right, in this order, and you build real trust instead of chasing a label after the fact.

### Caller ID verification level (STIR/SHAKEN attestation A/B/C)

**What this actually is, in plain terms:** every call your trunk places gets digitally signed at the moment it's set up, and the signature says how confident your carrier is that you're really allowed to use that number as your caller ID. There are three levels:

- **A (Full Attestation)** — the carrier knows exactly who you are and confirms you're authorized to use this specific number. This is what you want.
- **B (Partial Attestation)** — the carrier knows who you are, but can't confirm you own the specific number you're calling from (common with unverified trunk setups).
- **C (Gateway Attestation)** — the carrier can't verify the caller's identity at all. This is a near-automatic "Spam Likely" sentence.

**How to actually get to A:**

1. Go to your provider's trust/identity verification flow — [Twilio Trust Hub](https://www.twilio.com/docs/trust-hub), [Telnyx Business Identity](https://telnyx.com/), or the equivalent in Vonage/Plivo/Sinch.
2. Submit real business documentation: legal entity name, EIN/business registration number, physical address, and a working business website/contact.
3. Link every number you plan to call from to this verified business profile — attestation is evaluated per number, not once for your whole account.
4. Wait for verification (typically 24–72 hours depending on provider).
5. **Confirm it actually worked** — place a real test call to a phone where you (or a colleague) can check the carrier's own app, which on many Android/iOS carrier apps shows a "Verified"/checkmark indicator when A-attestation is applied.

**If you're stuck at B/C:** it almost always means either your business verification is incomplete, or you're calling from a number not properly linked to your verified trust profile (e.g. a number ported in but not re-associated). Go back and re-check the number-to-profile link first — it's the most common miss.

- [ ] Business verification submitted and approved with your provider
- [ ] Every outbound number explicitly linked to the verified profile
- [ ] Confirmed A-attestation on a real test call
- [ ] Re-check attestation after any number porting event — it can reset

### Caller Name (CNAM) and Branded Calling

**What this is:** CNAM is the business name shown on a basic caller ID display (works on most carriers by default once set). Branded calling goes further — showing your logo, verified checkmark, and even the reason for the call on supporting apps/devices.

**How to set it up:**

1. **CNAM** — set directly in your provider's console (Twilio: Phone Numbers → your number → Caller ID Name; similar in others). This can take 24–72 hours to propagate across carriers since CNAM lookups are often cached by third-party CNAM databases, not instant.
2. **Branded calling** — this requires enrolling with the specific programs carriers support:
   - **Verizon / T-Mobile / AT&T branded calling** is generally accessed via your voice provider's partnership with **[Hiya](https://www.hiya.com/)** or **First Orion** — ask your provider (Twilio, Telnyx, etc.) which branded-calling partner they integrate with, since this isn't usually self-serve directly with the carrier.
   - Expect to submit your logo, business name, and the specific reason-for-calling text per call type (e.g. "Appointment Reminder," "Delivery Update") — generic branding without a reason code gets rejected by some programs.

- [ ] CNAM set in provider console
- [ ] CNAM verified showing correctly on a real test call (check after 48+ hours for propagation)
- [ ] Branded calling enrollment submitted through your provider's partner program, if offered

### Call volume pattern (burst vs. steady)

**Why this matters:** carriers' spam models specifically watch for calls firing in bursts — hundreds of calls in the same few seconds is a textbook robo-dial signature. A human-operated or well-designed sales dialer never does this.

**How to build it correctly:**

1. **Never call directly from your application code.** Route every outbound call through a queue (SQS, Pub/Sub, or a simple Redis-backed job queue) feeding a dialer worker.
2. **Build a pacing layer in the worker** that spaces calls out — a simple token-bucket limiter (e.g. "max 5 calls initiated per second," well under your trunk's concurrent-channel ceiling) smooths out what would otherwise be a burst.
3. **Spread large campaigns across hours**, not minutes. If you have 10,000 calls to make, that's a multi-hour dial campaign, not a 10-minute blast.
4. **Add jitter** (small random delays between dials) rather than perfectly even intervals — perfectly metronomic timing is itself a pattern some detection systems flag as automated.

- [ ] Outbound dialing goes through a queue + worker, never direct-from-app
- [ ] Token-bucket/rate limiter caps calls-initiated-per-second well under trunk capacity
- [ ] Large campaigns scheduled across hours, not fired all at once
- [ ] Small randomized jitter added between dials

### Abandoned call rate, answer rate, hang-up rate

**What each one is and why carriers watch it:**

- **Abandoned call rate** = calls where the recipient answers but no agent/message is ready within a few seconds (classic auto-dialer overflow symptom). Regulators (FTC/FCC in the US) and carriers both treat this as a hard compliance number.
- **Answer rate** = % of dialed calls actually picked up — a sudden *drop* is itself a signal something changed (a number just got labelled, for instance).
- **Hang-up rate** (specifically early hang-ups, within 2–3 seconds) = recipients hanging up almost immediately, which reads as "this already looked like spam to the human," independent of whatever score the carrier assigned.

**How to actually control abandoned calls:**

1. If using a predictive/auto-dialer, configure it so it **never dials more lines than you have agents ready to receive** — most contact-center platforms (including Amazon Connect) have a built-in "pacing ratio" setting; keep it conservative rather than maximizing throughput.
2. Set a **hard ceiling alert at 2.5%** abandoned rate (below the common 3% legal/carrier threshold) so you catch drift before it becomes a violation.
3. If a call can't be immediately handled, play a clear pre-recorded message identifying your business within the first 2 seconds of connection — required in many jurisdictions and reduces the "silent call = spam" perception.

**How to track these three together:**

- Pull them from your provider's call-detail-record (CDR) export or dashboard (Twilio Voice Insights, Telnyx Call Control API events) daily.
- Chart answer rate and hang-up rate over time per number — a declining trend on one specific number, isolated from the rest, usually means that number specifically got flagged somewhere, even before you see it in a labelling-company lookup.
- [ ] Abandoned call rate tracked daily, alert threshold set at 2.5%
- [ ] Answer rate tracked per number, not just account-wide
- [ ] Early hang-up rate tracked as a separate metric from total hang-ups
- [ ] Pre-recorded identification message plays within 2 seconds if no agent is ready

### Number reputation history

**The mentor-level point here:** a number's reputation isn't reset by you acquiring it — if you're buying a number from a provider's shared pool (rather than registering a brand-new one), it may carry history from whoever used it before you. This is the single most overlooked cause of "why is my brand-new setup already labelled."

**How to actually check before first use:**

1. Submit the number to **[Free Caller Registry](https://www.freecallerregistry.com/fcr)** — this single registration distributes your business info to the three major spam-analytics companies (First Orion, Hiya, TNS) that most US carriers rely on to label calls.
2. Separately check status directly with **[Hiya](https://www.hiya.com/)** and **[TNS](https://tnsi.com/)**, since Free Caller Registry distributes your submission but doesn't guarantee instant propagation to every partner.
3. If your provider assigns numbers from a shared pool, **explicitly ask for a number with no prior labelling history**, or request a number type (newly released block) less likely to carry baggage — this is a real, normal request to make of Twilio/Telnyx support.
4. **Re-check weekly**, not just once — reputation can degrade even on a number you've used responsibly if someone else is spoofing it (neighbor spoofing is a real, separate problem from your own usage).

- [ ] Every new number submitted to Free Caller Registry before first use
- [ ] Reputation independently checked with Hiya and TNS
- [ ] Weekly recheck scheduled, not a one-time setup step
- [ ] Shared-pool numbers specifically requested as "no prior flag history" where your provider allows it

### Call duration patterns (many short calls)

**Why this is a signal:** a cluster of calls that all last 1–3 seconds looks identical to a dialer hitting disconnected numbers or voicemail boxes at scale — a classic scanning/robo-dial fingerprint, even if your actual calls are legitimate.

**How to monitor and fix it:**

1. Pull average call duration from your CDRs, segmented by campaign/number — don't look at an account-wide average, which hides a problem on one specific number or list segment.
2. If average duration drops sharply, the most common real causes are: (a) dialing a stale/bad list with many disconnected numbers, (b) a bug causing calls to be ended prematurely by your own call-flow logic, (c) dialing into a block of numbers that's mostly voicemail (check voicemail-detection rate alongside duration).
3. Clean your calling list against number-validation services (many SIP providers, including Twilio's **Lookup API**, offer a "line type" check — landline/mobile/VoIP/disconnected) *before* dialing, not after you notice a duration problem.

- [ ] Average call duration tracked per campaign/number, not account-wide
- [ ] List validated against a number-lookup/line-type check before each campaign
- [ ] Sharp duration drops investigated same-day, not at the next weekly review

### Consent on file (for marketing/auto-dialed calls)

**The compliance reality:** in the US, TCPA requires prior express written consent for marketing calls/texts using an autodialer or prerecorded voice, and this isn't a "nice to have" record — it's your actual legal defense if a recipient complains or sues.

**How to build this properly:**

1. Store one consent record per contact, with: timestamp, exact method (web form, verbal on a prior call, SMS keyword), the exact wording/disclosure shown or read to them, and the specific phone number it covers.
2. **Gate the auto-dialer itself** on this record — the dialing system should query the consent store and skip any contact without a valid, current record, automatically, not as a manual list-scrub step beforehand.
3. Cross-reference against the **National Do Not Call Registry** ([donotcall.gov](https://www.donotcall.gov/) for businesses to check registered numbers) before every marketing campaign, even for contacts with consent on file — some jurisdictions require honoring DNC regardless of consent for certain call types.
4. Keep consent records for as long as your legal counsel advises (commonly several years) — this is a "get a lawyer to confirm" item, not a technical one.

- [ ] One consent record per contact: timestamp, method, exact wording, number covered
- [ ] Dialer automatically checks consent before every call, not a manual pre-filter
- [ ] National/state Do-Not-Call lists checked before every marketing campaign
- [ ] Retention period for consent records confirmed with legal counsel

### Number usage consistency (steady set vs. constant cycling)

**The direct point:** using the same small set of numbers consistently is what lets carriers and labelling databases build positive history on them over time. Constantly adding and dropping numbers resets that trust clock every time, and — this is the part people miss — number-cycling itself is a pattern labelling companies specifically watch for and penalize, independent of any one number's individual behavior.

**How to actually manage this:**

1. Decide your steady number set up front, sized to your real concurrent-call needs, not inflated "just in case."
2. Any number you add goes through the **full verification + Free Caller Registry process** described above — never a quick swap-in without it.
3. If a number is damaged (genuinely needs replacing, not just "got one complaint"), retire it deliberately and document why, rather than abandoning it quietly and grabbing a fresh one — a documented retirement reason matters if you ever need to dispute a labelling decision later.
4. Track "numbers added/removed per month" as its own internal metric — if that number is trending up, that's worth investigating on its own, since it's often a symptom of underlying quality problems being "solved" by rotation instead of being fixed.

- [ ] Fixed, sized-to-need number set, not grown ad hoc
- [ ] Every new number fully verified + registered before use
- [ ] Number retirements documented with reason
- [ ] "Numbers churned per month" tracked as its own metric

### Destination patterns (unusual countries, premium numbers)

**What this catches:** toll fraud — attackers who gain access to your calling account and use it to dial premium-rate or international numbers they profit from, running up your bill and burning your reputation simultaneously.

**How to lock this down:**

1. In your provider's console, set a **geographic permissions/allow-list** restricting which countries your account can call — Twilio calls this "Voice Geographic Permissions," Telnyx has an equivalent in its portal. Enable only the countries you actually do business in.
2. Block premium-rate number ranges explicitly if your provider exposes that control, or confirm with support that your account-level country permissions already cover it.
3. Set a **daily spend cap** and an alert well below your normal expected spend, so an anomaly trips an alert long before it becomes a shocking bill.
4. Review the destination-country list monthly against your actual customer base — remove any country you no longer serve.

- [ ] Geographic/country calling permissions restricted to actual business need
- [ ] Premium-rate ranges blocked or confirmed covered by country restrictions
- [ ] Daily spend cap and alert configured, set below normal expected spend
- [ ] Destination list reviewed monthly

### Recording and compliance consistency

**What's easy to miss:** recording-consent law varies by state/country (some are one-party consent, some require all-party consent), and inconsistent application across campaigns is itself a legal exposure, separate from spam-flagging.

**How to implement it correctly:**

1. Determine the strictest applicable jurisdiction across everywhere you call, and default to that standard account-wide rather than trying to branch logic per state/country (simpler to maintain, safer by default).
2. Play a recording-disclosure message automatically at the start of any recorded call, built into the call-flow logic itself — not a step your agents remember to do manually.
3. Store recordings in encrypted storage (e.g. S3 with server-side encryption) with access logging, and set an explicit retention/deletion policy rather than keeping everything indefinitely.

- [ ] Recording-consent standard set to the strictest applicable jurisdiction
- [ ] Disclosure message built into the automated call flow, not manual
- [ ] Encrypted storage with access logs and a defined retention period

### Emergency calling readiness (E911 / local equivalent)

**Why this belongs in a "not getting flagged" section at all:** it's not a spam-avoidance item, it's a compliance requirement that gets audited separately — but it's commonly skipped by teams focused entirely on outbound sales/marketing calling, so it's worth flagging here explicitly.

**How to set it up:**

1. Register a physical address for every number used on a staff desk phone or softphone, through your provider's E911 registration (Twilio: Emergency Calling settings per number).
2. Test it periodically — not by actually calling 911, but by confirming the registered address is current whenever staff relocate or a remote worker's address changes.

- [ ] Every staff/desk-phone number has a current registered emergency address
- [ ] Address re-verified whenever a remote worker's location changes

---

## TEST BEFORE LAUNCH CHECKLIST

- [ ] Place test calls from real external phones (not softphone-to-softphone) across 2–3 major carriers (e.g. one Verizon, one AT&T/T-Mobile line)
- [ ] Confirm caller ID + CNAM displays correctly on the receiving screen
- [ ] Confirm STIR/SHAKEN attestation shows A-level where the carrier's app exposes it
- [ ] Call the number back — confirm it answers with a clear greeting naming the business
- [ ] Walk the full IVR flow end to end — every menu option, transfer, voicemail path
- [ ] Trigger a deliberate abandoned-call scenario in a test campaign, confirm it's logged correctly and under threshold
- [ ] Confirm webhook events fire correctly for ringing/answered/ended/failed
- [ ] Kill the primary carrier/trunk in a controlled test, confirm failover to backup within expected time
- [ ] Test emergency calling (E911) registration from a staff phone
- [ ] Confirm recording announcement plays and the recording lands encrypted in storage
- [ ] Run a real test campaign at low volume (10–20 calls) through the full pacing/queue pipeline before any real campaign at scale

---

## IFRA QUICK AGAIN SETUP

> Goal: fix the actual cause and fail over to infrastructure that was already clean and ready — not spin up a fresh identity to outrun the flag. Number-cycling gets the *new* number flagged faster, since carriers link it back to the same verified business via your Free Caller Registry / STIR/SHAKEN profile, not just the number itself.

### Diagnosis checklist

- [ ] Stop outbound dialing on the affected number/campaign immediately
- [ ] Check labelling-company status directly: [Free Caller Registry](https://www.freecallerregistry.com/fcr) submission status, [Hiya](https://www.hiya.com/), [TNS](https://tnsi.com/)
- [ ] Pull abandoned-call rate, hang-up rate, and answer rate for the last 24–72 hours from your CDR/dashboard
- [ ] Check STIR/SHAKEN attestation level on a fresh test call — did it drop from A to B/C?
- [ ] Review recent destination pattern for unusual countries/premium numbers (possible toll fraud, not a reputation issue at all)
- [ ] Check consent records for the specific calls that triggered complaints, if any exist
- [ ] Confirm the exact root cause in writing before touching any infrastructure

### Quick re-run checklist and steps

- [ ] Fix the identified root cause (lower abandoned rate via pacing changes, close a consent gap, adjust destination allow-list, etc.)
- [ ] Fail over outbound traffic to your **pre-configured second carrier/trunk** while the primary number is under dispute
- [ ] File a dispute with the labelling company through your provider's support channel, referencing the specific fix made
- [ ] Resume calling at **reduced volume** on the backup — roughly 20–30% of normal, ramping back up over hours, not instantly
- [ ] This only works in hours because the backup trunk/number was **already verified and registered with Free Caller Registry in advance** — that's the actual speed lever, not the rebuild itself
- [ ] Monitor abandoned/hang-up rates hourly for the first day back
- [ ] Log the incident, root cause, and fix applied in your runbook for next time

# SMS INFRA GUIDE

## RAW SMS INFRASTRUCTURE

### Why this isn't a regular VPS server, and isn't an email MTA either

You know how to stand up a VPS for something like FTP hosting — you run the server, control the whole stack. SMS infra deliberately isn't that, and understanding *why* changes how you build the pipeline around it.

- **You almost never run your own SMS "server."** The provider ([Twilio](https://www.twilio.com/en-us/messaging/sms), [Telnyx](https://telnyx.com/products/sms), [Vonage](https://www.vonage.com/communications-apis/sms/), [Plivo](https://www.plivo.com/sms/), [Sinch](https://www.sinch.com/products/sms/)) operates the actual carrier-facing SMPP infrastructure — you just call their HTTP API. There's no equivalent of running your own Postfix here; Path B (self-hosted) barely exists for SMS the way it does for email or phone.
- **Pre-authorized gating, not after-the-fact reputation.** Email lets you send without registering anywhere (you just risk being filtered). SMS requires registration *before the pipe even opens* — brand/campaign approval (10DLC in the US, DLT in India) happens before a single message can flow, not as a reputation system that kicks in after the fact.
- **Stateless per message, but throughput-capped like a token bucket.** Each message is one API call, but your registered campaign has a hard messages/second ceiling enforced by the carrier — this is architecturally closer to a quota system than email's soft reputation scoring.
- **No hop-by-hop relay.** It's always a direct path: your API call → provider → carrier → handset. No equivalent of SMTP's multi-hop relay chain to reason about.

### Pick a provider — what actually differs between them

| Provider | Good for | Notes |
| --- | --- | --- |
| [Twilio](https://www.twilio.com/en-us/messaging/sms) | Fastest to prototype, best documentation, handles 10DLC registration in-dashboard | Slightly pricier per message at scale |
| [Telnyx](https://telnyx.com/products/sms) | Lower cost at volume, direct carrier connections | Dashboard less polished than Twilio's |
| [Vonage](https://www.vonage.com/communications-apis/sms/) | Strong international/country-specific routing | Less US-10DLC-focused UX |
| [Plivo](https://www.plivo.com/sms/) | Competitive pricing, decent API | Smaller ecosystem/community |
| [Sinch](https://www.sinch.com/products/sms/) | Strong in India/APAC, handles DLT registration assistance | More enterprise-sales-oriented onboarding |

**My honest default:** Twilio to start (fastest to get a working, compliant pipeline live), with a second provider (Telnyx is a common pairing) wired in as backup from day one — not as an afterthought.

### The actual components you're building

Since you never run the SMPP server yourself, what you're building is the **pipeline around the provider's API**:

1. **Brand/campaign registration — do this first**, since it gates the pipe and takes the longest (days to weeks). Covered in full detail in its own section below.
2. **Queue** — message schema: recipient, campaign/template ID, personalization payload, idempotency key, which number/campaign pool to send from.
3. **Worker service** — calls the provider's send API, stores initial `submitted` status.
4. **Throttle layer (token/leaky bucket) — the SMS-specific piece.** Your registered campaign has a hard messages/second ceiling. Build a rate limiter in the worker matching that exact number (e.g. using a simple Redis-backed token bucket, or a library like `limiter` in Node or `ratelimit` in Python), so excess volume queues instead of getting throttled or filtered carrier-side.
5. **Redirector (Front Door)** — covered in detail below, build before the pipeline goes live.
6. **Webhook receiver (behind the redirector)** — delivery receipts (sent/delivered/failed/filtered) and inbound replies (STOP/HELP) arrive here, async.
7. **Event store / state machine** — `queued → submitted → delivered/failed/filtered → replied`, same shape as the email pipeline.
8. **One-time-code (OTP) fraud guard** — a dedicated pre-send filter, separate from the general throttle: country allow-list check, per-number/per-IP rate limit, CAPTCHA gate upstream, short-code expiry. Most providers (Twilio Verify, for instance) offer a managed OTP service with fraud-guard built in — worth using instead of hand-rolling this.

### Build order

1. Redirector stood up and tested
2. Brand/campaign registration completed — this gates everything else
3. Queue + message schema defined
4. Worker service built, calling the provider API, enforcing the registered throughput via token-bucket
5. Webhook receiver wired behind the redirector (delivery receipts + STOP/HELP)
6. Event store/state machine connecting both
7. OTP fraud-guard layer added before any code-sending path goes live
8. Do-not-contact service wired to auto-consume STOP events

---

### The System Hardening

**Access Control**
- SSH key-only authentication — disable password login entirely
- Disable root login — require a non-root user, then `sudo`
- Fail2ban or similar — auto-ban IPs after repeated failed login attempts
- Non-default SSH port
- IP allow-listing for SSH/admin access
- **Provider API keys scoped to minimum needed permission** — a send-only key for the worker, never an account-admin key used for day-to-day sending
- **Separate API keys per campaign/number pool** (system vs. marketing) — a leaked marketing key shouldn't be able to blast OTP traffic or vice versa
- **Brand/campaign registration dashboard access restricted separately** from general cloud IAM — it's a compliance-critical account, treat it like one

**Firewall / Network Exposure**
- Default-deny inbound; minimal open ports
- Egress filtering
- **Webhook receiver: allow-list the provider's published source IPs where available, in addition to signature verification** — defense in depth, not either/or
- **If you run your own short-link redirect service, treat it as public attack surface** — same WAF/rate-limiting as your main redirector, since it's a web-facing endpoint even though the "real" work happens at the provider

**OS and Software Hygiene**
- Regular patching; minimal installed software; disable unused services
- **Patch your short-link/redirect service and webhook-handler app on the same cycle as everything else** — it's small, but it's still internet-facing code you wrote

**Logging and Monitoring**
- Centralized, tamper-resistant logging
- Intrusion detection
- **Dedicated alerting on your OTP-send endpoint specifically** — separate from general SMS metrics, since this is the exact path fraud (SMS pumping) targets
- **Log delivery receipts and STOP events centrally** — this is your compliance audit trail, not just an operational metric

**Secrets and Credentials**
- No hardcoded credentials; secrets manager; least privilege
- **CAPTCHA/verification service keys (hCaptcha, Turnstile) kept in the secrets manager**, never embedded in frontend code where they can be extracted and swapped
- **Separate credentials for system vs. marketing sends**, same reasoning as the API-key scoping above

**Data Protection**
- Encryption at rest; no sensitive data lingering
- **Never log full phone numbers in plaintext in application logs** — mask/partially redact (e.g. `+1••••••1234`)
- **Encrypt consent records at rest** — they contain name, number, IP, and timestamp, which is real PII
- **OTP codes never logged in plaintext anywhere**, including debug logs

---

## NOT GETTING FLAGGED

Each of these is a distinct lever carriers and your provider are watching. Walk through each one deliberately rather than treating this as a single checklist — getting the registration pieces right up front saves you from firefighting filtered messages later.

### Brand registration status

**What this actually is:** in the US, your "brand" is your business's identity record with **[The Campaign Registry](https://www.campaignregistry.com/)** (TCR) — the independent body that all major carriers rely on to vet who's sending 10DLC traffic. Every campaign you register sits under this brand.

**How to actually register it:**

1. Most people don't register directly with TCR — you register through your messaging provider's dashboard (Twilio: Messaging → Regulatory Compliance → Brand Registration; Telnyx has an equivalent under Messaging Profiles).
2. You'll need: your exact legal business name (must match your EIN/tax records precisely — small mismatches are the #1 cause of rejection), legal address, EIN, business website with visible Terms and Privacy Policy, and a support contact.
3. Submit and wait — TCR checks your details against business databases automatically; approval is often same-day to a few days if details are clean, longer if anything needs manual review.
4. **Confirm status is "Verified," not just "Submitted."** Check this explicitly in your provider's dashboard — a pending or unverified brand means your campaigns underneath it will be throttled or rejected even if the campaign itself looks fine.
5. Re-verify if your legal name, address, or EIN ever changes — an out-of-date brand record is a common cause of sudden, unexplained filtering months after a clean launch.

- [ ] Brand status shows "Verified" (not "Submitted"/"Pending") in your provider dashboard
- [ ] Legal name matches EIN records exactly, character for character
- [ ] Website has a visible, real Terms of Service and Privacy Policy page
- [ ] Re-verification triggered on any business detail change

### Campaign registration status

**What this is:** a campaign is the registered *purpose* of a specific message stream under your brand — e.g. "Account Alerts," "Marketing," "Two-Factor Authentication." Carriers assign trust and throughput per campaign, not per brand alone.

**How to register one correctly:**

1. In your provider's dashboard, create a new campaign under your verified brand, selecting the correct **use case** (TCR has a fixed list: Customer Care, Marketing, 2FA, Account Notifications, etc. — pick the one that actually matches what you're sending, not the one with the highest throughput allowance).
2. Write the **sample messages** field with real examples matching exactly what you'll actually send, including your business name and any opt-out language — reviewers compare live traffic against these samples.
3. Describe your **opt-in method** accurately (web form, keyword text-in, verbal) — this is checked both at registration and later if a dispute arises.
4. One registered campaign per distinct purpose — don't register a single "general" campaign and funnel every message type through it; mismatched content against a narrow registration is a leading cause of filtering.
5. Confirm status shows **"Active"**, not "Pending," before sending any real traffic.

- [ ] Each distinct message purpose has its own registered campaign — no catch-all campaign
- [ ] Sample messages match real production wording exactly, including opt-out text
- [ ] Use-case category accurately reflects actual content
- [ ] Status confirmed "Active" before launch

### Content-to-registered-sample match

**The practical trap here:** marketing/product teams iterate on copy constantly, but your registered samples are what carriers compare live traffic against. Drift between the two is one of the most common causes of a previously-working campaign suddenly getting filtered.

**How to actually prevent drift:**

1. Keep your registered sample messages in a shared doc or wiki accessible to whoever writes SMS copy — not buried in a provider dashboard only ops can see.
2. Before shipping any new message variant, diff it mentally (or literally) against the registered sample: same general structure, same business identification, same opt-out mechanism referenced.
3. If the *purpose* of the campaign changes meaningfully (e.g. "shipping updates" starts including promotional upsells), **re-register as a new campaign or update the existing registration** — don't just change the content and hope.
4. Build a lightweight internal review step (even just a Slack approval) before any new SMS template variant ships to production, specifically checking it against the registered sample.

- [ ] Registered samples kept somewhere accessible to whoever writes copy
- [ ] New message templates reviewed against the registered sample before shipping
- [ ] Campaign re-registered (not just content silently changed) if the purpose shifts

### Opt-in / consent proof

**Why this matters beyond "being nice":** TCPA and most other countries' equivalents require provable consent, and "provable" means you need the actual record, not just a policy that says you require it.

**How to build the record properly:**

1. Store per contact: timestamp of opt-in, exact method (web form field, keyword reply, verbal), the **exact wording** shown or read to them at the time (not your current policy text — what was actually shown then), and the specific phone number it applies to.
2. If opt-in is via a web form, also log the IP address and user-agent of the submission as supporting evidence.
3. Never pre-tick a consent checkbox, bundle it into general Terms acceptance, or import a purchased/scraped list claiming implied consent — none of these hold up, and they're the fastest way to a filtered campaign and a legal problem simultaneously.
4. Make the record queryable by phone number, so support can answer "did this person actually opt in" in seconds during a dispute, not by searching through old marketing exports.

- [ ] Consent record stores timestamp, method, exact wording, and number
- [ ] Web-form opt-ins also log IP/user-agent
- [ ] No pre-ticked boxes, no bundled consent, no purchased lists
- [ ] Record is queryable by phone number for support/dispute resolution

### Throughput compliance (registered send-rate limits)

**What this actually enforces:** your campaign registration comes with a specific messages-per-second (or per-day, depending on tier) ceiling set by the carrier based on your trust score and registration tier. Exceeding it doesn't just get the excess messages dropped — it can flag the whole campaign.

**How to implement this correctly in your pipeline:**

1. Find your exact registered throughput in your provider's dashboard (Twilio shows this per Messaging Service; it changes as your trust score with TCR improves over time).
2. Implement a **token-bucket rate limiter** in your worker service set to that exact number, not the provider account's theoretical max — a common library choice is Redis-backed (`redis-cell` module, or a simple Lua script) so the limit holds across multiple worker instances, not just per-process.
3. When the queue exceeds the throttle, **let it queue, don't drop or force through** — a growing queue depth is a normal, expected signal under high load, not a problem to "fix" by raising your own limiter above the registered ceiling.
4. Alert on sustained queue growth — it tells you either (a) you need to request a throughput increase from TCR/your provider (a real, normal request as your trust score improves), or (b) your campaign volume estimate was wrong.

- [ ] Exact registered throughput confirmed in provider dashboard
- [ ] Token-bucket limiter in the worker matches that number precisely
- [ ] Limiter is shared/centralized across worker instances, not per-process
- [ ] Sustained queue growth triggers an alert, not a silent limiter bypass

### Link quality (own domain vs. public shorteners)

**The direct problem:** carrier content filters specifically flag public shorteners (bit.ly, tinyurl, t.co, etc.) because they're disproportionately used in SMS phishing (smishing) campaigns — using one instantly raises your filter risk regardless of what the link actually points to.

**How to set up your own short-link domain properly:**

1. Register a short domain (something like `acme.sms` style, or a short subdomain of your main domain, e.g. `a.acmeco.com`).
2. Run it through a simple redirect service you control — this can be as lightweight as a single endpoint behind your redirector that does a 301 redirect to the real destination, with the mapping stored in your own database.
3. Serve it over HTTPS with a valid certificate — an HTTP-only short link is itself a red flag to both filters and recipients.
4. Before each campaign, check the **destination** domain (not just your short-link domain) against [Google Safe Browsing](https://transparencyreport.google.com/safe-browsing/search) and [Spamhaus DBL](https://www.spamhaus.org/lookup/) — a clean short-link domain pointing to a flagged destination still gets you filtered.

- [ ] Own short-link domain in use, no public shorteners anywhere in production
- [ ] Short-link domain served over valid HTTPS
- [ ] Destination domains checked against Safe Browsing/Spamhaus DBL before each campaign
- [ ] Redirect mapping stored and auditable, not a third-party black box

### Opt-out functionality (STOP working immediately)

**Why this is non-negotiable, not just best practice:** carriers actively test opt-out compliance (sending STOP to sampled numbers and confirming it's honored), and failure here is one of the few things that can get a campaign suspended outright, fast.

**How to implement and verify it:**

1. Most providers (Twilio, Telnyx) handle the core STOP/START keyword logic **automatically** at the carrier/platform level — confirm this is actually enabled on your messaging service/number, don't assume it's on by default.
2. Build your own webhook handler for the STOP event regardless, so it also writes to your **own** do-not-contact/suppression database — relying solely on the provider's built-in handling means your own system doesn't know the contact opted out, which causes problems the next time you try to send through a different channel/campaign.
3. Test the full set of keywords: STOP, STOPALL, UNSUBSCRIBE, CANCEL, END, QUIT — confirm each triggers suppression and a single confirmation reply, not multiple.
4. Test HELP separately — confirm it replies with real contact information, not a generic "message not understood."

- [ ] Carrier/provider-level STOP handling confirmed enabled (not assumed)
- [ ] Your own webhook also writes STOP events to your internal suppression list
- [ ] All STOP keyword variants tested end to end
- [ ] HELP tested, returns real contact info

### Complaint / spam report rate

**The tracking gap most teams have:** complaint rate needs to be tracked **per campaign**, not just account-wide — a single bad campaign can hide inside a healthy account average and keep running until it causes real damage.

**How to actually monitor it:**

1. Pull filtered/blocked error codes from your provider's delivery-receipt webhooks, tagged by campaign ID, into your own dashboard or a simple spreadsheet if you're early-stage.
2. Set a per-campaign alert threshold — if one specific campaign's filtered/complaint rate spikes while others stay flat, pause that campaign specifically, not the whole account.
3. Cross-reference spikes against recent content changes for that campaign (ties back to the content-drift section above) before assuming it's a carrier-side issue.

- [ ] Complaint/filtered rate tracked per campaign, not account aggregate only
- [ ] Per-campaign alert threshold set
- [ ] Spike response pauses the specific campaign, not the whole account

### Sender content consistency

**What carriers actually compare:** the same opening identification and structural pattern across every message in a campaign. Inconsistency — even innocent variation from different team members writing copy — reads similarly to how inconsistent branding reads as spoofing in email.

**How to enforce it practically:**

1. Define a fixed opening pattern per campaign (e.g. always starting with `"AcmeCo:"`) and bake it into your message template system as a non-optional prefix, not something copywriters type manually each time.
2. Don't quietly increase send frequency for a campaign without updating its registration — a sudden jump from "one message a week" to "daily" on the same registered campaign can itself trigger review.
3. Keep tone/structure consistent across variants — this is as much about carrier filtering as it is about not training your own recipients to distrust your texts.

- [ ] Fixed identification prefix enforced at the template level, not manually typed
- [ ] Frequency changes trigger a registration review, not a silent ramp-up
- [ ] Tone/structure kept consistent across all templates in a campaign

### Number type trust level (long code vs. toll-free vs. short code)

**The mentor point here:** this isn't just a cost/speed tradeoff — using a number type mismatched to your actual volume is itself a filtering risk, independent of everything else being correctly registered.

| Type | Best for | Approval time | Throughput |
| --- | --- | --- | --- |
| **10DLC (long code)** | Alerts, support, low–medium marketing | Days (brand) + days (campaign) | Lowest, tiered by trust score |
| **Toll-free** | Alerts, support, small–medium volume | Days to weeks (verification) | Medium |
| **Short code** | High volume, urgent, time-sensitive | 8–12 weeks | Highest |

**How to choose correctly:** estimate your actual busiest-day/hour volume (same method as the email volume-estimation approach) and match it to the type whose throughput tier comfortably covers it — pushing volume far beyond what your registered type supports is itself what triggers carrier-side filtering, separate from any content or consent issue.

- [ ] Number type matched to realistic peak volume, not just current low volume
- [ ] Migration path identified in advance (e.g. 10DLC → short code) if growth is expected, since short-code approval alone takes 8–12 weeks

### Country-specific registration (beyond the US)

**India specifically — DLT, and it's mandatory, not optional:**

1. Register as a Principal Entity on **any one** DLT portal — common ones: [Jio TrueConnect](https://trueconnect.jio.com), [Vodafone Idea ViLPower](https://www.vilpower.in/), or Airtel's portal. One approval cross-syncs to the others (typically within 5–7 days).
2. Register your **Headers** (sender IDs) under that entity.
3. Register your exact **message templates** — Indian carriers match templates programmatically against what's actually sent, so wording precision matters even more here than with US 10DLC samples.
4. As of the PE-TM (Principal Entity–Telemarketer) linkage requirement, you also need to bind your entity to your SMS provider's telemarketer ID on the DLT platform — your provider (Sinch, Twilio, etc.) can usually guide this step directly since they're the ones with an existing TM ID.

**Other countries:** UK, EU, and others each have their own sender-ID pre-registration or content rules — check with your specific provider's compliance documentation per country before launching there, since requirements change and vary by destination.

- [ ] DLT entity registered on one portal, cross-sync confirmed
- [ ] Headers and exact message templates registered
- [ ] PE-TM chain binding completed with your provider's telemarketer ID
- [ ] Country-specific rules checked with your provider before launching in any new market

### One-time-code (OTP) fraud protection

**The fraud pattern this defends against:** "SMS pumping" — attackers programmatically trigger your OTP flow toward premium or international numbers they control, and you're billed for every message while gaining nothing.

**How to build the guard:**

1. Use a **country allow-list** on your OTP-sending endpoint — if your real user base is in 3 countries, don't allow OTP requests to the other 190.
2. Rate-limit per phone number *and* per originating IP/device fingerprint — a single number requesting 10 OTPs in a minute, or a single IP cycling through many numbers, are both clear fraud signals.
3. Add a **CAPTCHA** (e.g. hCaptcha, Cloudflare Turnstile) in front of the OTP trigger for any public-facing flow (signup/login) — this alone blocks the majority of automated pumping attempts.
4. Set a **short expiry** on codes (5–10 minutes) and invalidate on use.
5. If your provider offers a managed verification product (Twilio Verify, for instance), it bundles much of this fraud-guard logic — worth using instead of hand-rolling it, especially early on.
6. **Alert on volume spikes** specifically on the OTP-sending path, separate from your general SMS monitoring, since this is the path fraud actually targets.

- [ ] Country allow-list on the OTP endpoint
- [ ] Per-number and per-IP/device rate limits
- [ ] CAPTCHA in front of any public-facing OTP trigger
- [ ] Short code expiry, invalidated on use
- [ ] Dedicated alerting on OTP-path volume spikes, separate from general SMS metrics

---

## TEST BEFORE LAUNCH CHECKLIST

- [ ] Send test messages to real phones on 2–3 major carriers
- [ ] Confirm sender ID/number displays correctly
- [ ] Send STOP from a test phone — confirm suppression + confirmation within seconds
- [ ] Send HELP — confirm correct contact info in the reply
- [ ] Click every link type from a real phone — confirm correct landing page over HTTPS
- [ ] Confirm the delivery-receipt webhook fires correctly on a test send
- [ ] Deliberately exceed your throttle in a test environment — confirm the token-bucket queues rather than overshooting to the provider
- [ ] Trigger a test OTP flow — confirm the fraud guard blocks an out-of-pattern request (e.g. rapid repeat requests)
- [ ] Confirm staging genuinely cannot reach real phone numbers
- [ ] If sending to India or another DLT/registration-required country, send one live test message and confirm it isn't silently dropped before a real campaign goes out

---

## IFRA QUICK AGAIN SETUP

> Goal: fix the actual cause and fail over to a provider/campaign that was already registered and clean — not cycle numbers to outrun filtering. Carriers link new traffic back to the same registered brand via TCR, so a fresh number under the same brand inherits the same scrutiny.

### Diagnosis checklist

- [ ] Pause the affected campaign/number immediately
- [ ] Pull recent "filtered"/"blocked" error codes from provider logs — note the exact error code, not just "it failed"
- [ ] Compare sent content against the registered sample — look for drift in wording, frequency, or opt-out language
- [ ] Check complaint rate specifically for this campaign, not account-wide
- [ ] Check whether a volume burst exceeded your registered throughput and triggered carrier-side throttling
- [ ] Check if a linked domain got flagged on Safe Browsing/Spamhaus DBL since the last check
- [ ] For India/DLT traffic: confirm the template ID still matches exactly what's registered
- [ ] Confirm the exact root cause in writing before resuming anything

### Quick re-run checklist and steps

- [ ] Fix the identified root cause (align content to the registered sample, fix throttle config, swap a flagged link domain, address the complaint source)
- [ ] Fail over to your **pre-configured second SMS provider/route** while the primary campaign is under review
- [ ] Open a dispute/review request with the provider, referencing the fix made
- [ ] Resume sending at reduced volume, ramp to full registered rate over hours/days, not instantly
- [ ] This only works in hours because the backup provider's number/campaign was **already registered and tested in advance** — that's the actual speed lever
- [ ] Monitor delivery rate and filtered-error rate hourly for the first day back
- [ ] Log the incident and root cause in your runbook
