> The guide is in progress, so I would ask you to come back later <under construction> 

# PHISHING EMAIL / SOCIAL ENGINEERING INFRA BUILD GUIDE 

**Goal:** build email, text-message (SMS) and phone-call systems that run on the cloud, can be changed fast, and are **not flagged as spam or phishing**.

*Assumption: one cloud account (AWS, Google Cloud or Azure), everything defined as code, and a mix of system messages (receipts, codes) and marketing, for initial setup and fast shifting use*

## The one idea behind everything

Email providers, phone carriers and phones all ask the same three questions. Fail one and you get blocked or labelled "Spam Likely" or "Phishing".

| Question | Plain meaning | Email | SMS | Phone |
| --- | --- | --- | --- | --- |
| **1. Who are you?** | Proof you are really you | SPF, DKIM, DMARC | Registered brand + campaign | Caller verification (STIR/SHAKEN) |
| **2. Do you behave?** | People want your messages | Low complaints, clean list | Opt-in + STOP works | Low hang-ups, sensible call volume |
| **3. Do you look honest?** | Nothing looks like a scam | Honest links, steady branding | Same, no link shorteners | Real caller name, no spoofing |

## Words you will see

| Term | Simple meaning |
| --- | --- |
| **MTA** | The mail server that sends email |
| **IP address / IP reputation** | Your server's "address" and the trust score inbox providers give it |
| **DNS record** | A public note about your domain (who may send for it) |
| **SPF / DKIM / DMARC** | Three email ID checks: approved senders / tamper-proof signature / what to do if the checks fail |
| **Bounce / complaint** | Email could not be delivered / recipient clicked "report spam" |
| **Suppression list** | "Do not contact again" list |
| **Warm-up** | Slowly raising volume so providers learn to trust you |
| **10DLC / DLT** | Business registration for text messages (US / India) |
| **STIR/SHAKEN** | Caller-ID verification so your number shows as genuine |
| **SIP trunk** | A cloud phone line |
| **IaC** | "Infrastructure as code": servers set up from files, not by clicking |

---

# Part 1: Foundation (do once, used by all three channels)

## 1.1 Decide first

| Decision | Recommendation |
| --- | --- |
| Split by purpose | Keep **system messages** (receipts, codes, password resets) apart from **marketing**. Separate domains/numbers, credentials, queues. If marketing gets in trouble, system messages keep working. |
| Volume | Estimate the busiest day per channel. It decides plans, number types and warm-up speed. |
| Owner | Name one person for abuse reports, blocklists, registrations and on-call. |
| Rules | Consent records, opt-out and company address are required in almost every country (CAN-SPAM, GDPR, CASL, TCPA, India DLT/TRAI and local rules). Get legal review. |

## 1.2 Cloud setup (so you can change fast)

1. **Separate accounts** for production and staging. Staging must never reach real customers (use sandbox numbers and mail catchers like Mailpit).
2. **Everything as code** (Terraform): servers, DNS records, queues, alerts. Changes are reviewed in Git and applied in minutes. Restrict who can edit DNS.
3. **Secrets manager** for all API keys and passwords. One key per service per environment. Never put them in code.
4. **One queue per channel** (SQS, Pub/Sub or similar). Apps drop a request in the queue; workers send it. Nothing sends directly from a web request.
5. **Swap-ready design**: your apps call *your own* "send email / send SMS / place call" service, not the provider directly. Switching provider then means changing one place.
6. **A backup provider per channel**, already configured: a hot standby for email, a second SMS route, a second phone carrier.
7. **Spending alerts and limits** on every provider. A bug or attack can run up a large bill quickly.

## 1.3 The shared "do not contact" brain

One service, used by email, SMS and phone:

- **Consent record** for each person: when, how, which channel, what wording they agreed to.
- **One opt-out list.** If someone says STOP by text or unsubscribes by email, the other channels must respect it where the law or your promise requires.
- **Check it before every send or call.**
- **Event log**: every send, delivery, bounce, complaint, reply and call result, searchable for support ("did she get it?").
- **Webhook safety**: providers report events by calling your URL. Verify each request's signature, or attackers can fake events.

## 1.4 Proxy servers (the "front door" and the "exit door")

**In plain words:** a proxy is a middleman server. You need two kinds.

| Type | Where it sits | What it does for you |
|---|---|---|
| **Reverse proxy** ("front door") | In front of your own services | Receives internet traffic first, checks it, then passes it on |
| **Forward proxy** ("exit door") | Between your servers and the internet | Sends your outbound requests from fixed, known IP addresses |

### Front door (reverse proxy)

Put one in front of every public web endpoint:
- Your **send API** (where apps submit email/SMS/call requests)
- **Webhook receivers** (where providers report deliveries, bounces, replies and call events)
- Your **tracking domain** (`track.example.com`) and the **unsubscribe/preference pages**
- The **MTA-STS policy site** (`mta-sts.example.com`)

Checklist:
- [ ] HTTPS only, with auto-renewing certificates; redirect HTTP to HTTPS
- [ ] **Rate limits** and request-size limits on every endpoint
- [ ] A **web application firewall (WAF)** to block bots and common attacks
- [ ] Webhook endpoints: allow only the provider's published IPs where available, **and still verify every signature**
- [ ] Internal servers never exposed directly to the internet
- [ ] **Two or more copies** with health checks, so one failure doesn't take it down
- [ ] Tracking and unsubscribe links stay **fast and always up**. A broken unsubscribe link causes spam complaints.
- [ ] Use a cloud-managed load balancer + WAF, or Nginx/Envoy/Caddy defined in your infrastructure code

### Exit door (forward proxy)

Route your workers' outbound **API calls** (to email, SMS and phone providers) through a **fixed-IP exit**: a cloud NAT gateway with a static IP, or a small forward proxy.

Why:
- Some providers let you **restrict API keys to specific IPs**, so a stolen key is useless elsewhere
- You get **one place to log and firewall** outbound traffic
- Providers see consistent, known IPs

Run **two exits** for backup, and add both IPs to each provider's allow-list.

### Never do this

- **Do not send email, SMS or calls through rotating, shared, free or "residential" proxies**, and don't rotate IPs or numbers to dodge filters. Inbox providers and carriers read this as hiding, which is a strong spam/phishing signal and breaks provider terms.
- **Email must leave from the IP that has your PTR and SPF records.** Never route mail delivery through a proxy with a different IP.
- Never run an **open proxy**. Require authentication or keep it on a private network only.

### Keep it safe and watched

- Patch regularly, MFA on admin access, least-privilege permissions
- Don't log message bodies, tokens or full phone numbers (mask them)
- Watch: error rate, response time, blocked requests, certificate expiry
- Runbook: "proxy down" (switch to the second copy), "exit IP changed" (update every provider allow-list)

---

# Part 2: Email

## 2.1 Choose how to run it

| Path | What it is | Pros | Cons |
| --- | --- | --- | --- |
| **A. Cloud email service in your own account** (SES, Postmark, SendGrid, Mailgun, or similar) with **dedicated IPs** | Provider runs the mail servers; you own domains, settings and pipeline | Fastest to change, no port-25 problems, easier on the team | Less low-level control |
| **B. Your own mail servers on cloud machines** (Postfix, or KumoMTA at high volume) | You run the mail servers on rented cloud machines | Full control | You carry IP reputation, blocklists, and 24/7 care. Many clouds **block outgoing port 25**, so check first. |

Path A is recommended for "cloud + change fast". Everything in 2.2 to 2.10 applies to both. 2.3 is **extra work for Path B** (and a smaller task of choosing dedicated IPs in Path A).

## 2.2 Domains (the most important step)

Use a different subdomain for each purpose so one problem cannot hurt the others:

| Name | Use |
| --- | --- |
| `example.com` | Website and staff mailboxes. **Never send bulk mail from it.** |
| `mail.example.com` | System emails |
| `news.example.com` | Marketing |
| `bounce.mail.example.com` | Where "could not deliver" replies go |
| `track.example.com` | Your own link-tracking domain |

**Domain hygiene (affects phishing checks directly):**

- Buy domains **4+ weeks before launch**. Brand-new domains look suspicious.
- Put a **real website** on the main domain: company name, address, contact, privacy policy.
- Registrar: lock on, MFA on, limited access, DNSSEC if available.
- No look-alikes of other brands; avoid cheap or abused domain endings.
- **Protect unused domains:** SPF `v=spf1 -all`, null MX, DMARC `p=reject`, so nobody can fake them.

## 2.3 Servers and IP addresses

- Use **dedicated IPs** (not shared): 1 for system mail, 1-2 for marketing, 1 spare.
- **Check each IP is clean** on Spamhaus, Barracuda, SORBS and MXToolbox before using it. Reject recycled IPs with a bad history.
- **Path B only:** confirm outgoing port 25 works (`nc -vz gmail-smtp-in.l.google.com 25`); run at least 2 mail servers; keep a firewall that allows inbound port 25 only on the bounce-receiving server and submission only from your apps; install fail-ban tools; sign every message and **refuse to send anything unsigned**; never be an "open relay" (a server that sends for strangers); keep clocks synced.
- **Reverse DNS (PTR):** each IP must point back to a name (e.g. `mta1.mail.example.com`), and that name must point to the same IP and match the name the server announces. All three must match.
- IPv6 is optional; start with IPv4 only.

## 2.4 Identity records (your email "ID card")

| Record | In plain words | Key rules |
| --- | --- | --- |
| **SPF** | Lists which servers may send for your domain | Ends with `-all` once confident. Max 10 lookups (list IPs directly to save lookups). Set on the bounce domain too. |
| **DKIM** | A digital signature proving the email was not altered | **2048-bit** key. One new key name per rotation. Replace keys every 6-12 months. Signing domain must match the From domain. |
| **DMARC** | Tells receivers what to do when SPF/DKIM fail, and sends you reports | Roll out in steps (below). |
| **Custom bounce address (Return-Path)** | Your own domain on the hidden "return" address | Needed so SPF *matches* your visible From domain ("alignment"). |
| **MX records** | Where replies and abuse reports arrive | Every domain you send from must be able to receive. |
| **PTR** | See 2.3 | Set at the IP owner. |
| **MTA-STS + TLS-RPT** | Forces encrypted delivery *to you*, and reports failures | Start in "testing" mode. |
| **BIMI** (optional) | Shows your logo in inboxes | Needs DMARC at quarantine/reject. |

**DMARC rollout:** (1) `p=none` for 2-4 weeks and read the reports (parsedmarc, dmarcian, Postmark digest). (2) Fix every legitimate sender that fails (staff mail, CRM, helpdesk, billing). (3) Move to `quarantine` at 25%, then 50%, then 100%. (4) Move to `reject`. This stops criminals phishing in your name and builds trust.

## 2.5 Handling bounces and complaints

1. Give every message its **own return address** (e.g. `bounce+ID@bounce.mail.example.com`), so each bounce maps to one email.
2. Send incoming bounces to a small program that reads the standard bounce reports (DSN) and spam-complaint reports (ARF).
3. Rules: **"user unknown" (5.1.1) goes on the do-not-contact list immediately.** Temporary failures retry, then suppress after repeats. **Any complaint is suppressed forever.**
4. Create and watch `abuse@`, `postmaster@`, `dmarc@`, `tlsrpt@`. Answer abuse reports within 24 hours.
5. Register with **Google Postmaster Tools**, **Microsoft SNDS + JMRP**, and the **Yahoo complaint feedback loop**.

## 2.6 The sending pipeline

App -> queue -> worker -> email service -> internet. Events (delivered, bounced, complained) flow back into the shared brain.

- **Idempotency keys** so a retry never sends twice; **retries with growing delays**; a "dead-letter" queue for messages that keep failing.
- **Check the do-not-contact list before every send.**
- **Rate limits** per customer/app and per receiving provider; **auto-pause** if bounces or complaints spike (a stolen API key can ruin your reputation in hours).
- **Template versions** with test renders.
- Never log full bodies or personal data unnecessarily.

## 2.7 What every email must contain

- A valid From on a domain that passes checks; **same sender name every time**
- `Message-ID` on your domain, `Date`, and **both plain-text and HTML** versions
- Reply-To on the **same domain** unless there is a real reason
- **One-click unsubscribe headers** (`List-Unsubscribe` and `List-Unsubscribe-Post`) on all marketing mail, honoured within 48 hours (instantly is better)
- Footer: company name, physical address, why they received it

## 2.8 Not being flagged as phishing (checklist)

**ID and servers**

- [ ] SPF, DKIM and DMARC all **pass and match** the visible From domain
- [ ] A record, PTR and server name match on every IP
- [ ] Encryption (TLS) on all connections
- [ ] IPs not on blocklists, checked daily
- [ ] Registered with Google, Microsoft and Yahoo tools (2.5)

**Links (the biggest phishing signal)**

- [ ] Link text and real destination are the **same domain**
- [ ] **No link shorteners**, free-hosting or redirect-heavy sites
- [ ] Your **own tracking domain** over HTTPS, with few redirects
- [ ] Every linked domain is old enough, has HTTPS and real content, and is **not** on Google Safe Browsing, Spamhaus DBL, SURBL or URIBL. Check before each campaign.
- [ ] Avoid raw IP links and many different domains in one email

**Content and identity**

- [ ] Never pretend to be another brand or person (no "PayPal" or CEO display names)
- [ ] Avoid scam wording ("verify within 24 hours", threats, "confirm your login"). For real security notices, tell people to open the website themselves.
- [ ] No HTML, ZIP, ISO or EXE attachments; avoid attachments if a link works; no QR-only emails
- [ ] Not image-only; no hidden text, giant fonts, ALL-CAPS subjects, fake "Re:/Fwd:"
- [ ] Same look, name and footer every time. Inconsistency looks like spoofing.

**Reputation**

- [ ] **Permission-only lists** (double opt-in is best). Never buy or scrape lists.
- [ ] Bounces under 2%, complaints under 0.1% (never reach 0.3%)
- [ ] Stop mailing people who ignored you for 6 months
- [ ] Check addresses at sign-up (spelling, can receive mail, not disposable) to avoid spam traps
- [ ] Steady volume. Sudden spikes trigger blocks.

**Account security**

- [ ] MFA on registrar, DNS, cloud, email service and admin tools
- [ ] DNS managed as code; delete unused records (stops takeover of forgotten subdomains)
- [ ] Outbound content scan to catch hacked templates; rate limits on all send access
- [ ] Watch for look-alike domains registered against your brand

## 2.9 Test before launch

1. Send to Gmail, Outlook, Yahoo and iCloud test accounts. Use "Show original" and check **SPF, DKIM and DMARC all say pass**.
2. Score with mail-tester.com (aim 9+/10); run an inbox-placement test (GlockApps or similar).
3. Check all records with MXToolbox and `dig` from outside.
4. Test unsubscribe, bounces (send to a fake address) and complaints (mark as spam) end to end.
5. Confirm staging cannot reach real addresses.

## 2.10 Warm-up (mandatory for new IPs and domains)

Start with your **most engaged people** only. Warm up each big provider separately; Outlook is the slowest to trust you. Start with system mail, then add marketing.

| When | Emails per day, per IP |
| --- | --- |
| Days 1-3 | 50-200 |
| Days 4-7 | 500-1,000 |
| Week 2 | 2,000-5,000 |
| Week 3 | 10,000-25,000 |
| Week 4 | 50,000-100,000 |
| Weeks 5-8 | Double only while numbers stay clean |

**Pause if** a big provider starts delaying you, complaints pass 0.1%, bounces pass 2%, or any blocklist lists you.

---

# Part 3: SMS (text messages)

## 3.1 Pick provider(s)

Use a cloud messaging provider (Twilio, Telnyx, Vonage, Plivo, Sinch, or your cloud's own messaging service). Prefer one that offers: number registration help, delivery receipts, one-time-code protection, per-country routing, and an API with signed webhooks. **Connect a second provider as backup.**

## 3.2 Choose the right sender type

| Sender | Best for | Notes |
| --- | --- | --- |
| **10-digit local number (10DLC)** (US) | Customer alerts, support, medium marketing | **Must register your brand and campaign.** Unregistered traffic is blocked or heavily filtered. Speed depends on your trust score. |
| **Toll-free number** (US/Canada) | Alerts, support, small to medium volume | **Verification required.** Takes days to weeks. |
| **Short code** (5-6 digits) | High volume, urgent | Fastest sending, **8-12 weeks** carrier approval, costs more. |
| **Sender name** (e.g. "ACMEBANK") | Alerts in many countries | Not allowed in US/Canada. Some countries require pre-registration. |
| **Country-specific** | Everything outside the US | Rules vary (e.g. **India: company, sender names and every message template must be registered on DLT**; UK, EU and others have their own). Check each country you send to. |

## 3.3 Registration steps (start early, they take time)

1. Prepare: legal company name, tax ID, address, website, support contact, and a real **privacy policy** on the site.
2. Register the **brand** with your provider.
3. Register each **campaign** (purpose): describe who gets texts, how they opted in, **sample messages** and opt-out wording.
4. Buy or move (port) numbers. Porting takes 1-4 weeks; **don't cancel the old service until it completes.**
5. Use **separate numbers/campaigns** for system messages and marketing.
6. Send only what you registered. A different kind of message than registered gets blocked.

## 3.4 Consent and opt-out (legally required)

- Get **clear permission before texting**: a plain tick-box or text-in keyword, with wording that names your company, message type, frequency, "msg & data rates may apply", and how to stop. **Keep proof.**
- **Never** pre-tick the box, bundle consent into terms, or text a purchased list.
- The first message names your business; every message is clearly from you.
- **STOP** (also STOPALL, UNSUBSCRIBE, CANCEL, END, QUIT) must stop messages immediately and send one confirmation. **HELP** must reply with contact details.
- Do not text between about **9pm and 8am recipient local time** (some places are stricter).
- Add numbers to the shared do-not-contact list instantly.

## 3.5 Content and link rules (to avoid carrier filtering)

- **No public link shorteners** (bit.ly etc.). Use your own short domain on HTTPS.
- Keep links few, to your own real domain, matching your registered website.
- Be clear who you are; no urgent threats or "verify your account" scare wording.
- Do not send banned content: illegal items, **payday loans, adult, gambling (where not licensed), tobacco/vape, cannabis, firearms, hate**, and "get rich quick" or debt-relief offers. These get blocked or need special approval.
- **Never** rotate through many numbers to dodge filters. It is treated as abuse.
- Keep message wording consistent with your registered samples.

## 3.6 Build

- App -> SMS queue -> worker -> provider. Use idempotency keys and retries as in email.
- **Throttle** to your registered speed limit; queue extras.
- Use **delivery receipts** (webhooks) to track sent, delivered, failed, filtered. Store them.
- **Protect one-time codes from fraud.** Attackers trigger codes to numbers they profit from ("SMS pumping"), and you pay. Use: a country allow-list, limits per phone number and per IP/device, CAPTCHA before sending, short code expiry, and a provider's fraud-guard feature. Alert on any spike.
- Handle incoming replies (STOP/HELP) with a webhook; check signatures.
- Handle long messages: non-standard characters (emoji) shorten the length per segment and raise cost.

## 3.7 Ramp up and monitor

- Start with engaged recipients and low volume, then raise slowly over 2-4 weeks.
- Watch: **delivery rate** (aim 95%+), filtered/blocked error codes, opt-out rate, cost per message, spikes by country.
- If a campaign shows many "filtered" results: pause, compare wording and links with what you registered, and contact the provider.

---

# Part 4: Phone (voice calls)

## 4.1 What you need

| Need | Example |
| --- | --- |
| **Outbound calls** | Sales, reminders, support callbacks |
| **Inbound + menu (IVR)** | "Press 1 for support" |
| **Staff phone system** | Desk phones, apps, extensions, call transfer |
| **Contact center** | Queues, agents, recording, reports |

## 4.2 Pick provider and build style

| Option | When to use |
| --- | --- |
| **Cloud phone platform** (Twilio, Telnyx, Vonage, Plivo or similar) with APIs | Custom call flows, fastest to change |
| **Cloud contact-center service** (Amazon Connect or similar) | Agents, queues, reports without building |
| **Own phone software** (Asterisk, FreeSWITCH) on cloud servers | Only if you need deep control; you carry quality, security and uptime |

Connect phone lines by **SIP trunk** (cloud phone line). Use **two carriers**, so one carrier problem does not stop calling.

## 4.3 Numbers and caller trust (stops "Spam Likely")

1. **Verify your business with the carrier/provider** so your calls are signed as genuine (**STIR/SHAKEN**; "A-level" means they know you and your numbers).
2. **Use only numbers you own** or are authorised to use. Never fake or "neighbour-spoof" caller ID.
3. Set your **caller name (CNAM)** and, where offered, **branded calling** (name/logo on screen).
4. **Register your numbers with call-labelling companies** (via your provider, or programs such as Free Caller Registry, Hiya, TNS) and re-check regularly.
5. **Check number reputation** before use and weekly after; replace labelled numbers.
6. Don't rotate numbers constantly. Use a **small set of steady numbers**.
7. Make sure the number **works when called back**, with a clear greeting naming your business.
8. Move existing numbers by **porting** (1-4 weeks).

## 4.4 Calling rules

- **Marketing calls and robo-dialling need written consent** in many places (e.g. TCPA in the US). Check consent before each call.
- Clean calling lists against **Do-Not-Call registries** (national and your own).
- Call only during **allowed hours** (about 8am-9pm recipient local time; some states stricter).
- **Recording laws differ.** Some places need everyone's consent. Announce recording at the start and store recordings securely with a retention limit.
- Auto-dialers: keep **abandoned calls under 3%** and play a clear message.
- Taking card payments by phone: use a provider's secure payment pause so card numbers never reach your recordings (PCI).
- **Emergency calling must work** for staff phones, with a registered address (e.g. E911 in the US). Test it.
- Always honour "do not call me again" instantly.

## 4.5 Build

- Number -> provider -> your **call-flow service** (menu, queues, transfer) running in your cloud.
- Provider reports call events by webhook (ringing, answered, ended, failed); verify signatures; store them.
- **Failover:** if your app is down, the number forwards to a backup number/voicemail; keep a second carrier ready.
- **Pace outbound calls**; avoid bursts of very short calls (looks like a robo-caller).
- Voicemail transcripts and recordings go to encrypted storage with access control.

## 4.6 Quality and monitoring

- Pick provider regions **close to callers**.
- Watch: **answer rate**, call completion rate, call setup delay, dropped calls, echo/choppy audio (jitter, packet loss), spam-label status, cost spikes and **unusual destinations** (toll fraud: attackers use your account to call premium or foreign numbers).
- Block high-risk countries you never call; set per-account spending caps.

---

# Part 5: Security and compliance (all channels)

- MFA everywhere (cloud, registrar, DNS, every provider dashboard)
- Least-privilege access; remove ex-staff the same day
- All keys in the secrets manager; rotate regularly
- Encrypt data at rest and in transit; keep personal data minimal and delete on schedule
- Record consent, opt-outs, registrations and call recordings with retention rules
- Verify every webhook signature
- Per-app limits on sending, texting and calling, with auto-suspend on odd behaviour
- Pen-test or at least review the injection endpoints; patch regularly
- Written incident plan: stolen key, blocklisted IP, labelled number, filtered SMS campaign

---

# Part 6: Monitoring and operations

| Channel | Watch |
| --- | --- |
| **Email** | Delivered/delayed/bounced by provider; complaints; blocklists; Google Postmaster + SNDS; DMARC and TLS reports; certificate, key and domain expiry; **sudden drops in volume** (a silent block looks like an empty queue) |
| **SMS** | Delivery rate; filtered/blocked errors; opt-outs; cost per country; one-time-code spikes |
| **Phone** | Answer and completion rates; call quality; spam labels; unusual destinations; spend |
| **Platform** | Queue size, worker errors, provider outages, cloud cost, API error rates |

- Alert at **half** of each danger limit, not at the limit.
- Run **two of everything** (servers, providers, regions as volume grows). Drain before maintenance.
- Keep **runbooks** (step-by-step fix lists) for: blocklisted email IP, big provider slowing you, stolen credential, DKIM key swap, SMS campaign filtered, number labelled spam, carrier outage, fraud spike.
- Every quarter: test the backup providers, restore from backup, review consent records, and re-check registrations.

---

# Part 7: Rollout order

| Weeks | Do |
| --- | --- |
| **0-1** | Decisions (1.1), cloud accounts, code repo, secrets, queues. Buy domains. Start SMS brand/campaign registrations and number purchases (they take longest). |
| **1-2** | Email: domains, IPs, SPF/DKIM/DMARC (`p=none`), bounce handling, suppression list. |
| **2-3** | Shared consent/opt-out service, event log, send-services for each channel, staging tests. |
| **3-4** | Phone: provider, trunk, numbers, caller verification and labelling, call flows, emergency calling test. |
| **4-5** | Testing (2.9), seed-account checks, SMS test messages, test calls from real phones. |
| **5-12** | Warm-up for email, SMS and calls with engaged contacts. DMARC to quarantine, then reject. |
| **Ongoing** | Monitoring, key rotation, quarterly drills. |

---

# Troubleshooting

| Problem | Likely cause | Fix |
| --- | --- | --- |
| Email goes to spam but checks pass | Reputation or content | Check complaints, list quality, links, engagement; run seed tests |
| `dmarc=fail` | Domains do not match | Make signing and return-path domains match the From domain |
| SPF error | More than 10 lookups | Remove includes; list IPs directly |
| Gmail "suspicious" warning | Link/domain reputation, spoof-like look, new domain | Check links against Safe Browsing/DBL; no shorteners; fix display name; age domain |
| Outlook blocks your IP | IP reputation | Register SNDS; send delisting request; slow down |
| High bounces | Old or unverified list | Verify list, suppress, pause |
| SMS "filtered" or not registered errors | Registration missing or message differs | Finish registration; match registered samples; remove shorteners |
| SMS cost spike | Code-pumping fraud | Country allow-list, rate limits, CAPTCHA, fraud guard |
| Calls show "Spam Likely" | Low trust, no verification | Verify business, register numbers with labelling companies, steady numbers, lower volume |
| Calls not connecting | Carrier issue or number problem | Switch to backup carrier; check logs and number status |

---

# Go-live checklist

**Foundation**

- [ ] Prod/staging separated; all setup in code
- [ ] Secrets in manager; spending alerts on every provider
- [ ] Shared consent and do-not-contact service live; webhooks verified
- [ ] Backup provider ready for email, SMS and phone

**Email**

- [ ] Domains aged; website live; registrar locked; MFA on
- [ ] IPs clean; A, PTR and server name match
- [ ] SPF, DKIM (2048-bit) and DMARC published and passing
- [ ] Bounce and complaint handling writes to the do-not-contact list
- [ ] Unsubscribe headers and footer in place
- [ ] Google, Microsoft and Yahoo tools registered
- [ ] Warm-up scheduled; DMARC tightening scheduled

**SMS**

- [ ] Brand, campaigns and numbers registered and approved
- [ ] Consent wording and proof saved; STOP/HELP tested
- [ ] Own link domain; no shorteners; one-time-code fraud limits on

**Phone**

- [ ] Business verified for caller ID; caller name set
- [ ] Numbers registered with label companies; reputation clean
- [ ] Consent, calling-hour and recording rules built in
- [ ] Emergency calling tested; failover carrier tested

**Operations**

- [ ] Dashboards and alerts live for every channel
- [ ] Runbooks written; owner named
