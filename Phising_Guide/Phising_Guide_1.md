# PHISHING GUIDE 1 

`People are always the weakest links`

**WARNING:** This for Authorized and fully permitted red team engagement only. Using this without legal permission can land you in JAIL 
Owner does not stand accountable for anything 

`Phishing and social engineering is a field that depends on the adjacent OSINT field to take input and the malware field to deliver the actual payload.`

**Tools of the craft**
```
**Phishing platforms**: Gophish [alternatives] KingPhish, Sniperphish
**Tool to create phishing site**:evilginx2/evilnigix3 [alternatives] modlishka , muraena + necrobrowser 
**Site to create image beacons**:grabify.org (good)
**Steal location through phishing**: Seeker
```
```
I have given the guide for evilginx2/evilnigix3 in Red Team Guide 7; go check it out man! 
```

## Gophish guide 

**GoPhish — Full Practical Guide**

GoPhish is the standard open-source framework for running phishing simulations — internal security-awareness testing or an authorized red team's phishing vector. It's Go-based, single binary, and handles the entire lifecycle: templates, landing pages, target lists, sending, and per-recipient result tracking (opened/clicked/submitted-data/reported).

**1. Installation**
```bash
# grab the latest release (v0.12.1 as of writing)
wget https://github.com/gophish/gophish/releases/download/v0.12.1/gophish-v0.12.1-linux-64bit.zip
unzip gophish-v0.12.1-linux-64bit.zip -d ~/gophish
cd ~/gophish
chmod +x gophish

# lock down config.json — it holds DB credentials
chmod 640 config.json

./gophish
```
First run prints a one-time admin password to the terminal — copy it immediately, it's never shown again:
```
time="2026-08-02T13:17:12-04:00" level=info msg="Please login with the username admin and the password 4r7Xk2mP9qLw"
```
Log into the admin panel at https://127.0.0.1:3333 (self-signed cert on first run — accept it, or replace with a real cert per below). If you're on a remote VPS, tunnel it rather than exposing the admin port publicly:
```bash
ssh -L 3333:127.0.0.1:3333 user@your-vps
```
**2. config.json — the pieces that matter**
```json
{
  "admin_server": {
    "listen_url": "127.0.0.1:3333",
    "use_tls": true,
    "cert_path": "gophish_admin.crt",
    "key_path": "gophish_admin.key"
  },
  "phish_server": {
    "listen_url": "0.0.0.0:80",
    "use_tls": false,
    "cert_path": "example.crt",
    "key_path": "example.key"
  },
  "db_name": "sqlite3",
  "db_path": "gophish.db"
}
```
- admin_server.listen_url — bind this to 127.0.0.1 only, always. This is your control panel; it should never face the internet directly.
- phish_server.listen_url — this is the one that DOES need to face the internet (0.0.0.0), since it's what serves landing pages and tracking pixels to actual recipients.
- use_tls on phish_server` — get a real cert (Let's Encrypt) for this. A landing page cloning a login form over plain HTTP looks instantly wrong and also just isn't representative of a real phish.
- MySQL instead of SQLite — swap db_name to mysql and set db_path as user:pass@(host:port)/dbname?charset=utf8&parseTime=True&loc=UTC if you're running larger/concurrent campaigns; SQLite is fine for most single-operator engagements.

**3. Sending Profiles — how mail actually goes out**

GoPhish doesn't send mail itself, it relays through an SMTP server you configure. Sending Profiles → New Profile:
```
Name: <internal label>
From: IT Support <it-support@yourdomain-or-lookalike.com>
Host: smtp.yourrelay.com:587
Username / Password: <SMTP creds>
Ignore Certificate Errors: only if testing internally
```
Deliverability is the actual hard part, not the tool. Before launching anything real:

- Set up SPF, DKIM, and DMARC on your sending domain — without these, most corporate mail filters bin you instantly and your whole campaign's data is worthless
- Click Send Test Email and run it through mail-tester.com to check your spam score before targeting anyone real
- If you registered a lookalike domain for realism (infotekexpNess.com style, one character off the real domain) — make sure its DNS (A records, SPF/DKIM/DMARC) is fully configured before go-live, and get this written into your rules of engagement so nobody flags it as an actual attack mid-campaign

**4. Landing Pages**

Landing Pages → New Page. Two ways to build one:

- Import Site — point it at a real login page's URL and GoPhish clones the HTML for you (fastest way to get something convincing)
- Write your own HTML directly in the editor

Key toggles:
```
Capture Submitted Data: ON   — logs whatever the target types into forms
Capture Passwords: ON/OFF    — decide up front based on your ROE; many
                                 engagements deliberately capture username
                                 only, not password, to limit stored sensitive
                                 data
Redirect to: https://real-company-site.com  — sends the target somewhere
                                 legitimate-looking after they submit, so
                                 the simulation doesn't just dead-end
                                 suspiciously
```
5. Email Templates

Email Templates → New Template. GoPhish injects tracking automatically via template variables:
```html

Hi {{.FirstName}},

Your password will expire in 24 hours. Click here to reset it:
<a href="{{.URL}}">Reset Password</a>

{{.Tracker}}
```
- {{.URL}} — the unique per-recipient tracking link to your landing page
- {{.Tracker}} — an invisible 1x1 tracking pixel, records "email opened" even if they never click
- {{.FirstName}} / {{.LastName}} / {{.Position}} — pulled straight from your target group's CSV, personalizes at scale

You can also Import Email from a raw .eml to clone a real internal email's exact formatting/headers as your starting point.

**6. Users & Groups (your target list)**

Users & Groups → New Group, imported via CSV:
```csv
First Name,Last Name,Email,Position
Sarah,Johnson,s.johnson@targetcorp.com,Finance Manager
David,Chen,d.chen@targetcorp.com,IT Analyst
```
The Position field is genuinely useful for pretext targeting — a finance-themed lure to finance staff, an IT-themed lure to engineering, etc., tends to perform very differently and is worth segmenting groups by department rather than blasting one template at everyone.

**7. Campaigns — wiring it all together**

Campaigns → New Campaign:
```
Name: <internal label>
Email Template: <from step 5>
Landing Page: <from step 4>
URL: https://your-phish-domain.com   <- what recipients actually see/click
Launch Date / Send Emails By: schedule immediate or staggered sending
Sending Profile: <from step 3>
Groups: <from step 6>
```
Launch it, then watch the dashboard — it gives you a real-time timeline per recipient: Email Sent → Email Opened → Clicked Link → Submitted Data → (if you've set up IMAP monitoring) Reported.

**8. IMAP monitoring — catching who reported it**

This is the underrated feature most guides skip. Settings → IMAP lets GoPhish connect to a mailbox (the one your "Report Phishing" button forwards to) and automatically mark a recipient as "Email Reported" in the campaign timeline when their forwarded report arrives — genuinely useful for measuring the metric that actually matters most to a security-awareness program: not just who clicked, but who caught it and reported it.

**9. API — automating everything above**

GoPhish ships a full REST API (same one the web UI uses internally) — useful for scripted/repeatable campaign setup rather than clicking through the UI every time:
```bash
curl -s -k -H "Authorization: Bearer $API_KEY" \
  https://127.0.0.1:3333/api/campaigns/ | jq
```
Your API key is under Settings in the admin panel. Common use: scripting monthly recurring awareness campaigns, or pulling results programmatically into your own reporting pipeline (or GoReport, a companion open-source tool built specifically to turn GoPhish campaign data into polished client-facing reports).

**10. Docker option, if you'd rather not manage a bare VPS**
```bash
docker pull gophish/gophish
docker run -d -p 3333:3333 -p 80:80 --name gophish gophish/gophish
docker logs gophish   # grab the first-run admin password from here instead
```
11. Operational checklist before you actually launch
```
□ SPF/DKIM/DMARC configured and verified on sending domain
□ TLS cert on phish_server (not self-signed) if landing page needs to
  look legitimate
□ Admin panel bound to localhost only / behind SSH tunnel
□ Rules of engagement signed off — especially Capture Passwords setting,
  and whether a lookalike domain is explicitly authorized in writing
  (registering a domain one character off a real company's is the kind
  of thing that needs explicit sign-off, not an assumption)
□ IMAP reporting mailbox connected, if you want reported-rate data
□ Redirect-to page set so the simulation resolves cleanly instead of
  dead-ending on an obvious "gotcha" page
□ Test email run through mail-tester.com — confirm it's not landing in
  spam before you burn your one shot at a real target list
```
That covers the full lifecycle end to end — installation through reporting. 

`let extend the gophish using the GoReport`

## What it does

GoReport is a Python script (by Christopher Maddalena, originally `vysecurity/GoReport`, now maintained at `chrismaddalena/Goreport`, v3.1) that connects to your GoPhish API and turns raw campaign data into an actual polished report — CSV, Excel, or Word (.docx) — instead of you manually screenshotting the dashboard for a client deliverable.

## What it actually pulls and computes

```
- IP addresses, operating systems, browser types/versions of everyone
  who interacted with the campaign
- Geolocation lookups on those IPs
- Two DIFFERENT numbers people conflate: total EVENTS vs. unique
  RECIPIENTS. A campaign sent to 10 people could log 9 "Clicked Link"
  events while only 3 actual people clicked (someone clicking
  multiple times inflates the event count) — GoReport tracks both
  correctly so your report doesn't overstate the real click-through rate
- A per-recipient timeline: sent -> opened -> clicked -> submitted data
- Detailed Analysis sections specifically for anyone who at least clicked
```

## Setup

```bash
git clone https://github.com/chrismaddalena/GoReport.git
cd GoReport
pip install -r requirements.txt
```

Config file (`gophish.config`):
```ini
[GoPhish]
gp_host: https://localhost:3333
api_key: <your GoPhish API key from Settings>
geolocate_key: <GEOLOCATE_API_KEY>
```

If GoPhish is on a remote box and you're tunneling in:
```bash
ssh -L 8080:localhost:3333 user@your-gophish-server
```
then point `gp_host` at `https://localhost:8080` instead.

## Running it

```bash
python3 GoReport.py --id 26 --format excel
python3 GoReport.py --id 26,29-33,54 --format csv    # multiple campaigns at once, ranges supported
python3 GoReport.py --id 26 --format word
```

## The Word report specifically needs a template

Drop a `template.docx` into the GoReport directory before running `--format word` — GoReport fills it in rather than generating styling from scratch. You need to define a table style named exactly `GoReport` and set up your Heading 1/2 styles inside that template first; this is how you get a report that matches your own firm's branding instead of a generic-looking output.

## Worth knowing — it's not the only option anymore

The original repo (`chrismaddalena/GoReport`) hasn't seen much activity in a while, and a couple of forks/alternatives have since appeared covering the same gap:
- `2flying2/GoReport` — a maintained fork of the same tool
- `jamesm0rr1s/GoPhish-Phishing-Campaign-Reporting` — a different approach, builds reports straight from the two CSV exports GoPhish itself can generate, rather than hitting the API directly

Worth checking which one is actually maintained at the point you go looking, since this corner of the GoPhish ecosystem is community-tooling rather than something GoPhish ships and supports directly.

## INFRA REQUIRED: BEYOND THE GOPHISH

**domain again and categorisation**
```
- Register your sending/landing domain 60+ days before launch if at
  all possible. Brand-new domains get auto-flagged as suspicious by
  nearly every mail filter and web categorization engine
- Proactively submit the domain for categorization to the major
  filtering vendors BEFORE launch: Cisco Talos, Palo Alto URL
  filtering, Fortinet FortiGuard, Symantec/Broadcom, McAfee
  TrustedSource — getting it bucketed as "business/general" ahead of
  time avoids it showing up as "newly registered/uncategorized",
  which is itself a red flag many corporate filters block on
- Check the domain and hosting IP's existing reputation before use:
  MXToolbox blacklist check, Talos Intelligence reputation lookup —
  a VPS IP block with prior abuse history will sink you before you
  even start
```
**Mail delivery infrastructure**
```
- SPF, DKIM, DMARC fully configured and verified (mail-tester.com)
- Consider sending through a reputable relay (SendGrid, Mailgun, a
  dedicated ESP) rather than direct-from-VPS — fresh VPS IP ranges
  get blacklisted constantly, established relay IP reputation is a
  real deliverability advantage
- Valid TLS cert on the phishing/landing server (Let's Encrypt is
  fine, self-signed will visibly break the illusion instantly)
```
**Work around in case mail delivery agent flags you**
```
- Disclose the use case to the provider's compliance/support team
  ahead of time for larger providers — some will whitelist an account
  once they understand it's an authorized security assessment
- Use a provider that explicitly markets to the security-awareness/
  phishing-simulation space rather than a general-purpose ESP
- Keep template content professional-looking rather than maximally
  aggressive on your first campaign with a new provider/account,
  since a brand-new sending account with zero history AND
  aggressive-looking content is the exact combination that gets
  auto-flagged fastest
```

**Redirector in front of Gophish sever**
```
- Put a lightweight redirector (nginx reverse proxy, or a dedicated
  redirector tool) between the public-facing domain and your actual
  GoPhish server, so the real server IP never appears directly in any
  header, DNS record, or crawl a defender might run
- This also lets you filter/log incoming requests before they even
  reach GoPhish — useful for spotting when a security team starts
  actively probing your infrastructure mid-campaign
```
**The server (VPS) - where Gophish/Evilginx**

you thinking to run this locally right ? you cannot be that bad at opsec (risking real ip) 🤨, cause i was in the start 🥲
```
DigitalOcean, Linode, Vultr    -> cheap, fast to spin up, most common
                                   choice for this exact use case in the
                                   industry, $5-10/month tier is plenty
AWS EC2 / Azure / GCP           -> more expensive, more setup overhead,
                                   but IP ranges from major cloud
                                   providers tend to carry BETTER baseline
                                   reputation with mail/web filters than
                                   budget VPS ranges, since they're used
                                   for so much legitimate traffic too
```
**here is the full infrastructure**
```
Domain DNS -> Redirector (public-facing) -> GoPhish (hidden, real work happens here)
```
--- 

# Writing Effective Phishing Pretexts — The Companion Piece

The tool setup from the last guide is the easy 20%. What actually determines whether a campaign produces useful training data — versus everyone ignoring it or, worse, everyone panicking and reporting IT for something obviously fake — is the pretext itself. Here's the actual craft.

## 1. The psychology underneath every pretext that works

Every effective lure pulls at least one of these levers, straight from Cialdini's influence principles — real phishing (and real simulations) are built on them, consciously or not:

```
Urgency          "expires in 24 hours" / "action required today"
                 -> short-circuits careful reading, most reliable lever
Authority         appears to come from IT, HR, a C-level exec, or a
                 recognized external brand (Microsoft, DocuSign, the bank)
Fear              "your account has been compromised" / "suspicious
                 login detected" — triggers an anxious click-first-
                 think-later response
Curiosity         "see who viewed your profile" / an unexpected
                 delivery notification for something they don't
                 remember ordering
Reciprocity/       "you've been selected for a bonus" / "HR needs your
Reward             updated info to process your raise" — greed and
                 self-interest are just as reliable as fear
Social proof       "3 colleagues have already completed this" / a
                 spoofed reply-chain making it look like others already
                 engaged
```

**Why this matters for template-writing specifically:** the strongest real-world and simulation pretexts stack two of these, not one — "IT Security (authority) detected a login from an unrecognized device and will lock your account in 2 hours (urgency + fear) unless you verify now." One lever alone is easy to ignore; two compounding levers is what actually drives click-through in real data.

## 2. The pretext categories that consistently perform, by theme

```
┌──────────────────────┬────────────────────────────────────────────┐
│ Category               │ Why it works / example angle                 │
├──────────────────────┼────────────────────────────────────────────┤
│ IT/password reset       │ Universal — everyone has had a real           │
│                        │ password reset email, so the format is         │
│                        │ already familiar and unremarkable               │
├──────────────────────┼────────────────────────────────────────────┤
│ Shared document          │ "John shared a document with you" (Google      │
│ (SharePoint/Drive/       │ Drive, OneDrive, DocuSign-style) — plausible    │
│ DocuSign)                │ in nearly any org, low suspicion trigger        │
├──────────────────────┼────────────────────────────────────────────┤
│ HR/benefits              │ Open enrollment, payroll discrepancy, W-2/tax   │
│                        │ document notices — high engagement because       │
│                        │ money and benefits are personally relevant        │
├──────────────────────┼────────────────────────────────────────────┤
│ Executive impersonation   │ "CEO" emailing a finance employee asking for   │
│ (CEO fraud/BEC)          │ an urgent wire transfer or gift card purchase —  │
│                        │ tests whether staff verify out-of-band before      │
│                        │ acting on authority alone                          │
├──────────────────────┼────────────────────────────────────────────┤
│ Shipping/delivery         │ "Delivery exception, action needed" — works     │
│                        │ especially well timed near holidays when          │
│                        │ package volume is genuinely high                   │
├──────────────────────┼────────────────────────────────────────────┤
│ Calendar/meeting invite   │ A fake Teams/Zoom/Calendar invite with a         │
│                        │ malicious "join link" — exploits the reflex of     │
│                        │ clicking meeting links without a second thought    │
├──────────────────────┼────────────────────────────────────────────┤
│ Security alert (ironic,   │ "Unusual sign-in activity detected" — spoofing   │
│ but genuinely effective)  │ the exact kind of email a REAL security team     │
│                        │ would send, which is exactly why it works so well  │
└──────────────────────┴────────────────────────────────────────────┘
```

## 3. What makes a template read as real vs. obviously fake

**Technical realism checklist:**
```
□ Match the target org's actual visual branding — pull real logo, real
  footer/signature format, real color scheme if you're impersonating
  an internal system
□ Match internal tone/formality — an org that writes casually
  ("Hey team!") gets a casual pretext; a formal org gets formal
□ Sender display name should look like a real person or real system
  name your target org actually uses (check what their real IT
  helpdesk emails actually say "From:" — often something specific
  like "IT Service Desk" not generic "Support")
□ Avoid the tells that make people say "oh that's obviously the
  phishing test" — no generic stock phishing language, no
  suspiciously perfect grammar paired with suspiciously vague
  content, no giant red "URGENT!!!" styling that reads as parody
  rather than a real corporate email
□ One clear, single call-to-action — real corporate emails are
  usually not cluttered with five different links, and neither
  should your template be; a single obvious button/link reads as
  more legitimate than a wall of text with multiple links
```

**The specific mistake that ruins simulation data:** writing a template so obviously "phishy" (deliberately bad grammar, an implausible prize, a sender domain that doesn't even try to look real) that your click-rate metric ends up measuring "who wasn't paying attention at all" rather than "who would fall for something a real attacker would actually send." If your goal is genuine risk measurement, the template needs to be as good as what a real attacker targeting this specific org would write — not a caricature.

## 4. Difficulty tiering — don't run one template, run a curriculum

```
Tier 1 (baseline, obvious-ish)
    -> generic "password expiring" lure, no personalization at all
    -> establishes your organization's baseline click-rate floor

Tier 2 (moderate)
    -> personalized with {{.FirstName}}/{{.Position}}, plausible
       internal system name, matches real branding
    -> measures response to a "decent" real-world-quality attempt

Tier 3 (advanced/red-team grade)
    -> pretext built from actual OSINT on the target org — real
       vendor names they use, a real recent internal event/project
       name if you have it, spoofed to look like a genuine reply
       in an existing thread
    -> this tier is where you find out what your actual most-skilled
       adversary-emulation exposure looks like, not just general
       awareness
```

**Investigative logic:** running only Tier 1 forever makes your metrics look great and tells you nothing useful — the whole point of tiering up over successive campaigns is tracking whether awareness training is actually improving resilience against increasingly realistic attempts, not just measuring susceptibility to the same obvious lure repeatedly.

## 5. Timing and context — free realism you don't have to invent

Real phishing campaigns ride real calendar events, and simulations should too:
```
Tax season (Jan-Apr)     -> W-2/tax document lures perform exceptionally well
Open enrollment           -> benefits/HR themed lures
Black Friday/holidays      -> shipping/delivery, gift card, "your order" lures
End of fiscal quarter       -> finance/invoice/expense report lures
New employee onboarding      -> "complete your onboarding paperwork" lures
                              targeted at recently-hired staff specifically
Software rollout/patch        -> "mandatory security update, click to install"
   announcements timed to      timed right after your org actually announced
   a real internal rollout      a real rollout — devastatingly effective,
                               also ethically the most sensitive, see below
```

## 6. The proportionality line — themes to avoid regardless of effectiveness

This isn't a legal disclaimer, it's genuinely standard industry practice (SANS, Proofpoint, and most reputable awareness vendors explicitly build this into their guidelines) because a badly-chosen pretext actively damages trust in your security team and can cause real harm:

```
Avoid:
├── Fake termination/layoff notices          — causes genuine distress,
│                                               damages trust in HR comms
├── Fake positive medical test results         — same reasoning, exploits
│                                               real fear inappropriately
├── Fake bonus/raise notifications              — technically effective,
│                                               but creates a "the security
│                                               team lied to get me excited
│                                               then punished me" resentment
│                                               dynamic that undermines the
│                                               whole program's goodwill
├── Anything impersonating a specific named       — reputational/legal risk,
│   real individual in a way that could            get explicit sign-off if
│   embarrass or implicate them personally          impersonating a real exec
└── External crisis-themed lures (impersonating   — exploits genuine public
    disaster relief orgs, real breaking news         anxiety, considered bad
    events) during an actual live crisis              practice even when
                                                       technically effective
```

**Why this matters practically, not just ethically:** a security awareness program that burns employee trust with a needlessly cruel pretext gets less cooperation on every FUTURE initiative, and often generates a formal HR complaint that becomes a bigger problem than any click-rate metric was worth. The effective-but-not-manipulative line is where the real skill in this craft actually is.

## 7. Two worked template examples, annotated

**Tier 2 — IT security alert (moderate difficulty)**
```html
Subject: Action Required: Unusual sign-in activity on your account

Hi {{.FirstName}},

We detected a sign-in to your account from a new device on
{{.CurrentDate}} in a location we don't recognize.

If this wasn't you, please verify your account immediately to
prevent unauthorized access:

[Verify My Account]({{.URL}})

If you don't recognize this activity, we recommend verifying within
the next 2 hours.

IT Security Team
{{.Tracker}}
```
**Why this works:** authority (IT Security) + fear (unauthorized access) + urgency (2 hour window), matches a format almost every employee has genuinely received from a real service before, single clear CTA, no over-the-top styling.

**Tier 3 — shared document, red-team grade (advanced)**
```html
Subject: {{.Position}} - Q3 Budget Review.xlsx shared with you

{{.FirstName}},

Per our conversation, here's the file for review before Thursday's
meeting. Let me know if the numbers on your side match what Finance
sent over.

[Open Document]({{.URL}})

Thanks,
Sarah

{{.Tracker}}
```
**Why this is harder to catch:** no urgency language at all, no fear/authority pressure — it reads as a completely mundane internal exchange, references a plausible real meeting cadence, and relies purely on it looking unremarkable rather than pressuring a reaction. This is closer to how a patient real attacker who's done some internal reconnaissance actually operates, and it's specifically the style that tends to catch even security-aware staff off guard, since there's no obvious "red flag" to train people to spot.

---

Both of these plug directly into the GoPhish template editor from the last guide — same `{{.FirstName}}`/`{{.URL}}`/`{{.Tracker}}` variables, same landing page/sending profile wiring.

---
## Bypassing the modern secure email gateways

---
## Some real attacks people fell for
```
```
