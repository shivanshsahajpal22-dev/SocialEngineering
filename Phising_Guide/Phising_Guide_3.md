# Infra building part 2 

`Make sure to read the initial part if you have not already.`

## Social engineering molding: Part 2 : for SMS 

> Part of a 3-guide set (Email / SMS / Phone). Shared foundation pieces are repeated here in short form so this guide stands alone.

## The one idea behind everything

Carriers and phones ask three questions. Fail one and you get filtered, blocked, or labelled.

| Question | Plain meaning | How SMS answers it |
| --- | --- | --- |
| **1. Who are you?** | Proof you're really you | Registered brand + campaign |
| **2. Do you behave?** | People want your texts | Opt-in, and STOP actually works |
| **3. Do you look honest?** | Nothing looks like a scam | Real links, no shorteners |

## Words you'll see

| Term | Simple meaning |
| --- | --- |
| 10DLC / DLT | Business registration for text messages (US / India) |
| Brand / campaign | Your registered company identity and the registered *purpose* of a message stream |
| Opt-in / opt-out | Explicit permission to text / the STOP mechanism |
| Throughput | Messages per second your registration is allowed to send |
| IaC | Infrastructure as code: config, numbers, and alerts set up from files, not clicks |

---

## 1. Decide first

- **Split system vs. marketing.** Separate numbers/campaigns/credentials for codes & alerts vs. promotions — trouble on one doesn't stop the other.
- **Estimate your busiest day.** It decides number type (local vs. toll-free vs. short code) and ramp speed.
- **Name one owner** for abuse reports, carrier registrations, and on-call.
- **Get legal review**: TCPA, CASL, India DLT/TRAI, and local consent/opt-out rules apply almost everywhere.

## 2. Cloud setup, so you can change fast

1. Separate **staging and production** — staging uses sandbox numbers only, never real customers.
2. **Everything as code**: queues, throughput settings, webhook config, alerts — reviewed in Git.
3. **Secrets manager** for provider API keys.
4. **A queue** between your app and the SMS send call. Nothing texts straight from a web request.
5. Your app calls *your own* "send SMS" service, not the provider directly — so switching providers is a one-place change.
6. **A second SMS provider/route**, pre-configured, ready as backup.
7. **Spending alerts and caps.**

## 3. The shared "do-not-contact" brain

- One consent record per person (when, how, wording).
- One opt-out list, checked before every send, shared with email/phone where the law or your promise requires it.
- An event log of every send, delivery, reply, and STOP.
- Every inbound webhook (replies, STOP, delivery receipts) signature-verified.

---

## 4. Redirector (Front Door) — build this before the sending pipeline itself

Put a redirector/reverse proxy in front of every public endpoint related to SMS, *before* wiring up the actual send pipeline, since replies and delivery receipts depend on it from day one.

Put a redirector in front of:

- Your **send API** (where internal apps submit SMS requests)
- **Inbound webhook receivers** (STOP/HELP replies, delivery receipts, filtered/blocked events)
- Your **own short link domain** (the one you use instead of a public shortener)
- Any **click-to-text or consent-capture web forms**

Checklist:

- [ ] HTTPS only, auto-renewing certs
- [ ] Rate limits and request-size limits on every endpoint
- [ ] A WAF for bots and common attacks
- [ ] Webhook routes: allow only the provider's published IPs where available, **and still verify every signature**
- [ ] Internal services never exposed directly to the internet
- [ ] Two or more redirector instances with health checks
- [ ] Your short-link domain stays fast and always up — slow or broken links get reported as spam
- [ ] Load balancer + WAF, or Nginx/Envoy/Caddy, defined in IaC

**Never:** route messages through rotating, shared, or "residential" proxies, or constantly rotate sending numbers to dodge carrier filters — carriers read number-hopping as abuse and will block the whole pattern, not just one number.

**Fast-rebuild notes for the redirector layer** (legitimate disaster recovery, not filter-evasion):

- Keep the redirector config as its own IaC module so you can redeploy the front door independently of your SMS send logic if it's compromised or needs scaling.
- Version your WAF/rate-limit rules so a fresh redirector inherits the same protections immediately.
- Keep an idle secondary redirector ready to promote via DNS/load-balancer switch if the primary fails, so inbound STOP/HELP replies are never silently dropped during a rebuild.

---

## 5. Pick provider(s)

Use a cloud messaging provider (Twilio, Telnyx, Vonage, Plivo, Sinch, or your cloud's own messaging service). Prefer one offering: registration help, delivery receipts, one-time-code protection, per-country routing, and signed webhooks. **Connect a second provider as backup.**

## 6. Choose the right sender type

| Sender | Best for | Notes |
| --- | --- | --- |
| **10-digit local number (10DLC)** (US) | Alerts, support, medium marketing | Must register brand + campaign; unregistered traffic is blocked or heavily filtered; speed depends on trust score |
| **Toll-free number** (US/Canada) | Alerts, support, small–medium volume | Verification required; takes days to weeks |
| **Short code** (5–6 digits) | High volume, urgent | Fastest sending; 8–12 weeks carrier approval; costs more |
| **Sender name** (e.g. "ACMEBANK") | Alerts in many countries | Not allowed in US/Canada; some countries require pre-registration |
| **Country-specific** | Everywhere outside the US | Rules vary — e.g. **India requires company, sender names, and every message template registered on DLT**; UK/EU and others have their own rules |

## 7. Registration steps (start these first — they take the longest)

1. Prepare: legal company name, tax ID, address, website, support contact, and a real privacy policy on the site.
2. Register the **brand** with your provider.
3. Register each **campaign** (purpose): who gets texts, how they opted in, sample messages, opt-out wording.
4. Buy or port numbers. Porting takes 1–4 weeks — **don't cancel the old service until it completes.**
5. Use **separate numbers/campaigns** for system vs. marketing messages.
6. Send only what you registered — a different kind of message than what's on file gets blocked.

**Fast-rebuild note:** keep your brand/campaign application details (company info, sample messages, opt-in wording) as a maintained template so registering a *new, legitimate* campaign or porting to a *new* provider is a paperwork exercise, not a research project. This is about shortening legitimate onboarding time — not about re-registering under a new identity after a campaign gets shut down for cause.

## 8. Consent and opt-out (legally required)

- Get clear permission before texting: a plain tick-box or text-in keyword naming your company, message type, frequency, "msg & data rates may apply," and how to stop. **Keep proof.**
- **Never** pre-tick the box, bundle consent into terms, or text a purchased list.
- The first message names your business; every message is clearly from you.
- **STOP** (also STOPALL, UNSUBSCRIBE, CANCEL, END, QUIT) must stop messages immediately with one confirmation reply. **HELP** must reply with contact details.
- Don't text between roughly 9pm and 8am recipient local time (some places are stricter).
- Add numbers to the shared do-not-contact list instantly.

## 9. Content and link rules (what carrier filters actually check)

- **No public link shorteners** (bit.ly, etc.) — use your own short domain on HTTPS.
- Keep links few, pointing to your own real, registered website.
- Be clear who you are; avoid urgent threats or "verify your account" wording.
- Avoid content that gets blocked or needs special approval: illegal items, payday loans, adult content, unlicensed gambling, tobacco/vape, cannabis, firearms, hate, "get rich quick," or debt-relief offers.
- **Never** rotate through many numbers to dodge filters — it's treated as abuse and risks the whole account.
- Keep wording consistent with your registered samples.

## 10. Build

- App → SMS queue → worker → provider, with idempotency keys and growing-delay retries, same pattern as email.
- **Throttle to your registered speed limit**; queue the rest.
- Use delivery receipt webhooks (sent/delivered/failed/filtered) and store them.
- **Protect one-time codes from fraud** ("SMS pumping" — attackers trigger codes to numbers they profit from, and you pay): country allow-lists, per-number and per-IP/device limits, CAPTCHA before sending, short code expiry, and the provider's fraud-guard feature. Alert on any spike.
- Handle inbound replies (STOP/HELP) via webhook; verify signatures.
- Account for long messages: non-standard characters (emoji) shorten the per-segment length and raise cost.

## 11. Ramp up and monitor

- Start with engaged recipients and low volume; raise slowly over 2–4 weeks.
- Watch: delivery rate (aim 95%+), filtered/blocked error codes, opt-out rate, cost per message, spikes by country.
- If a campaign shows many "filtered" results: pause, compare wording/links against what you registered, and contact the provider — don't just switch numbers and retry.

## 12. Fast, repeatable rebuilds (legitimate resilience, not filter evasion)

- **One IaC module per concern** (redirector, queue/throttle config, webhook handlers, fraud-guard rules) so a broken piece can be redeployed alone.
- **Pre-vetted backup numbers/routes** held with your second provider, ready to fail over to if your primary provider has an outage — not as a rotation pool to outrun filtering.
- **A documented new-campaign runbook** for genuinely new, legitimate message streams, so launching one is fast — separate from, and never a substitute for, fixing a campaign that got filtered for cause.
- **Key/credential rotation as a scheduled drill**, so rotating a compromised API key is routine, not a scramble.
- If a number gets labeled or a campaign gets throttled: the fix is content/consent/volume changes plus working with the provider and carrier — not replacing the number to start the trust clock over elsewhere.

## 13. Monitoring

Watch: delivery rate, filtered/blocked error codes, opt-out rate, cost per country, one-time-code volume spikes. Alert at **half** of each danger threshold.

## 14. Troubleshooting

| Problem | Likely cause | Fix |
| --- | --- | --- |
| "Filtered" or not-registered errors | Registration missing or message differs from sample | Finish registration; match registered wording; remove shorteners |
| Cost spike | Code-pumping fraud | Country allow-list, rate limits, CAPTCHA, fraud guard |
| Low delivery rate suddenly | Carrier-side filtering or volume spike | Check against registered samples; slow down; contact provider |

## 15. Go-live checklist

- [ ] Brand, campaigns, and numbers registered and approved
- [ ] Redirector live with WAF, rate limits, 2+ instances, verified webhook signatures
- [ ] Consent wording and proof saved; STOP/HELP tested end to end
- [ ] Own short-link domain in use; no public shorteners
- [ ] One-time-code fraud limits on (country allow-list, rate limits, CAPTCHA)
- [ ] Separate system vs. marketing numbers/campaigns
- [ ] Backup provider configured and tested
- [ ] Rebuild runbooks written for: redirector loss, stolen key, provider outage, filtered campaign

---

## Social engineering molding: Part 3 : for phone 

> Part of a 3-guide set (Email / SMS / Phone). Shared foundation pieces are repeated here in short form so this guide stands alone.

## The one idea behind everything

Carriers and phones ask three questions. Fail one and your calls show "Spam Likely" or get blocked.

| Question | Plain meaning | How phone answers it |
| --- | --- | --- |
| **1. Who are you?** | Proof you're really you | Caller verification (STIR/SHAKEN) |
| **2. Do you behave?** | People want your calls | Low hang-ups, sensible call volume |
| **3. Do you look honest?** | Nothing looks like a scam | Real caller name, no spoofing |

## Words you'll see

| Term | Simple meaning |
| --- | --- |
| STIR/SHAKEN | Caller-ID verification so your number shows as genuine |
| SIP trunk | A cloud phone line |
| CNAM | The caller name shown on the recipient's screen |
| IVR | Interactive voice response — "press 1 for support" menus |
| Toll fraud | Attackers using your account to call premium or foreign numbers |
| IaC | Infrastructure as code: call flows and config set up from files, not clicks |

---

## 1. Decide first

- **Split system vs. marketing calls.** Separate numbers for support/alerts vs. sales outreach — trouble on one doesn't stop the other.
- **Estimate your busiest hour.** It decides trunk capacity, number count, and agent/queue sizing.
- **Name one owner** for carrier registrations, number reputation, and on-call.
- **Get legal review**: TCPA and similar consent/recording/Do-Not-Call rules apply in most places.

## 2. Cloud setup, so you can change fast

1. Separate **staging and production** — staging uses sandbox numbers/test calls only.
2. **Everything as code**: call flows, trunk config, alerts — reviewed in Git.
3. **Secrets manager** for provider API keys.
4. **A queue** for outbound dial jobs, so pacing and retries are controlled centrally, not fired straight from the app.
5. Your app calls *your own* "place call" / call-flow service, not the provider directly — switching carriers is then a one-place change.
6. **A second carrier/trunk**, pre-configured, so one carrier's problem doesn't stop calling.
7. **Spending alerts and caps.**

## 3. The shared "do-not-contact" brain

- One consent record per person for marketing/auto-dialled calls.
- One opt-out list, checked before every outbound call.
- An event log of call attempts, outcomes, and "don't call me again" requests.
- Every webhook (call events from the provider) signature-verified.

---

## 4. Redirector (Front Door) — set this up before the call-flow service itself

Put a redirector/reverse proxy in front of every public endpoint related to calling, *before* building the actual IVR/call-flow logic behind it.

Put a redirector in front of:

- Your **call-control API** (where your app triggers outbound calls or configures flows)
- **Webhook receivers** for call events (ringing, answered, ended, failed)
- Any **web-based IVR builder or recording-playback endpoint** you expose
- **Click-to-call** web widgets, if you have them

Checklist:

- [ ] HTTPS only, auto-renewing certs
- [ ] Rate limits and request-size limits on every endpoint
- [ ] A WAF for bots and common attacks
- [ ] Webhook routes: allow only the provider's published IPs where available, **and still verify every signature**
- [ ] Internal call-flow services never exposed directly to the internet
- [ ] Two or more redirector instances with health checks
- [ ] Load balancer + WAF, or Nginx/Envoy/Caddy, defined in IaC

**Never:** route calls through anonymizing relays, or rotate through many caller-ID numbers to dodge spam-likely labels — carriers and labelling databases treat number-hopping itself as a strong spam signal.

**Fast-rebuild notes for the redirector layer** (legitimate disaster recovery, not label-evasion):

- Keep the redirector config as its own IaC module, separate from call-flow logic, so the front door can be redeployed independently if compromised or overloaded.
- Version your WAF/rate-limit rules so a fresh redirector inherits the same protections immediately.
- Keep an idle secondary redirector ready to promote via DNS/load-balancer switch, so call-event webhooks aren't silently dropped during a rebuild.

---

## 5. What you need

| Need | Example |
| --- | --- |
| Outbound calls | Sales, reminders, support callbacks |
| Inbound + menu (IVR) | "Press 1 for support" |
| Staff phone system | Desk phones, apps, extensions, transfer |
| Contact center | Queues, agents, recording, reports |

## 6. Pick provider and build style

| Option | When to use |
| --- | --- |
| **Cloud phone platform** (Twilio, Telnyx, Vonage, Plivo) with APIs | Custom call flows, fastest to change |
| **Cloud contact-center service** (Amazon Connect or similar) | Agents, queues, reports without building |
| **Own phone software** (Asterisk, FreeSWITCH) on cloud servers | Only if you need deep control; you own quality, security, uptime |

Connect lines by **SIP trunk**. Use **two carriers**, so one carrier's problem doesn't stop calling.

## 7. Numbers and caller trust — what actually stops "Spam Likely"

1. **Verify your business with the carrier/provider** so calls are signed as genuine (STIR/SHAKEN — "A-level" attestation means they vouch for you and your numbers).
2. **Use only numbers you own or are authorized to use.** Never fake or "neighbour-spoof" caller ID.
3. Set your **caller name (CNAM)** and, where offered, **branded calling** (name/logo on screen).
4. **Register your numbers with call-labelling companies** (via your provider, or Free Caller Registry, Hiya, TNS) and re-check regularly.
5. **Check number reputation** before use and weekly after; replace any number that gets labelled.
6. **Don't rotate numbers constantly** — use a small, steady set of numbers so they can build trust.
7. Make sure a number **works when called back**, with a clear greeting naming your business.
8. Move existing numbers by **porting** (1–4 weeks).

**Fast-rebuild note:** keep a documented, reusable process for verifying a *new, legitimate* number with STIR/SHAKEN and registering it with labelling companies, so onboarding additional genuine capacity is fast. This should never be used as a pool of fresh numbers to swap in after one gets labelled for cause — a labelled number means something about your calling pattern needs to change, not that you need a new number.

## 8. Calling rules

- **Marketing calls and robo-dialling need written consent** in many places (e.g. TCPA in the US). Check consent before each call.
- Clean calling lists against national and your own Do-Not-Call registries.
- Call only during allowed hours (roughly 8am–9pm recipient local time; some states stricter).
- **Recording laws differ** — some places need everyone's consent. Announce recording at the start; store recordings securely with a retention limit.
- Auto-dialers: keep **abandoned calls under 3%** and play a clear message when one happens.
- Taking card payments by phone: use the provider's secure payment pause so card numbers never reach recordings (PCI).
- **Emergency calling must work** for staff phones, with a registered address (e.g. E911 in the US) — test it.
- Always honour "do not call me again" instantly.

## 9. Build

- Number → provider → your call-flow service (menu, queues, transfer) running in your cloud.
- Provider reports call events by webhook (ringing, answered, ended, failed); verify signatures; store them.
- **Failover:** if your app is down, the number forwards to a backup number/voicemail; keep a second carrier ready.
- **Pace outbound calls** — avoid bursts of very short calls, which look like robo-dialling.
- Voicemail transcripts/recordings go to encrypted storage with access control.

## 10. Quality and monitoring

- Pick provider regions close to your callers.
- Watch: answer rate, call completion rate, call setup delay, dropped calls, echo/jitter/packet loss, spam-label status, cost spikes, and **unusual destinations** (toll fraud — attackers calling premium or foreign numbers on your account).
- Block high-risk countries you never legitimately call; set per-account spending caps.

## 11. Fast, repeatable rebuilds (legitimate resilience, not label evasion)

- **One IaC module per concern** (redirector, trunk config, call-flow logic, recording storage) so any one piece can be redeployed independently.
- **A second, pre-configured carrier/trunk** ready to take over traffic if the primary has an outage.
- **A documented new-number onboarding runbook** (verification, CNAM, labelling-company registration) for genuinely new, legitimate capacity — not a rotation pool.
- **Scheduled credential-rotation drills**, so swapping a compromised API key or trunk credential is routine.
- If a number gets labelled or calls start failing: the fix is reducing abandoned-call rate, tightening consent/targeting, and working the labelling-company dispute process — not cycling to a new number to restart the trust clock.

## 12. Troubleshooting

| Problem | Likely cause | Fix |
| --- | --- | --- |
| Calls show "Spam Likely" | Low trust, no verification | Verify business, register with labelling companies, use steady numbers, lower volume |
| Calls not connecting | Carrier issue or number problem | Switch to backup carrier; check logs and number status |
| Sudden cost spike to unfamiliar destinations | Toll fraud | Block high-risk countries, review account access, rotate compromised credentials |

## 13. Go-live checklist

- [ ] Business verified for caller ID; CNAM set
- [ ] Redirector live with WAF, rate limits, 2+ instances, verified webhook signatures
- [ ] Numbers registered with labelling companies; reputation checked
- [ ] Consent, calling-hour, and recording rules built into the call flow
- [ ] Emergency calling tested; failover carrier tested
- [ ] Backup trunk/carrier configured and tested
- [ ] Rebuild runbooks written for: redirector loss, stolen credential, carrier outage, fraud spike
