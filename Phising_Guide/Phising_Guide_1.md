# PHISHING: PRETEXT PSYCHOLOGY AND GUIDE 

## Pre-social engineering OSINT linkage 

> Every phase of a social engineering engagement — the pretext, the target selection, the thing that makes a lie land instead of bounce — is only as good as the information it's built from. OSINT isn't a prerequisite chapter you skim before the "real" work starts; it's the raw material the real work is made of. Nothing in the Phishing Guide or the HUMINT Guide works without it, and both of those guides say so themselves.

**The dependency, stated plainly**

Phishing Guide 1's own opening framing already names this: *"Phishing and social engineering is a field that depends on the adjacent OSINT field to take input."* HUMINT Guide's first two phases — Targeting and Spotting — aren't adjacent to OSINT, they *are* OSINT, just under a different name: requirement decomposition is impossible without knowing who plausibly has the access you need, and spotting sources (LinkedIn, conference programs, GitHub, alumni networks, published papers) are pulled directly from the exact same bucket chain as the OSINT Guide's Image/Username/Email/RealName pivots.

**Where each OSINT bucket actually lands downstream**

```
OSINT GUIDE (Parts 1-2)
  Person's name -> company/workplace    -> HUMINT Targeting: defines
                                             the source profile before
                                             a single person is spotted
  LinkedIn / employer / role             -> HUMINT Spotting Tier 1-3:
                                             the access-mapping itself
  Username/email breach correlation      -> HUMINT Assessment (MICE):
                                             a recent job change found
                                             via OSINT is literally the
                                             "loosened loyalty" signal
                                             MICE's Ideology lever
                                             depends on
  Chronemics / posting rhythm            -> HUMINT Development pacing:
                                             knowing someone's actual
                                             schedule tells you when a
                                             "coincidental" approach
                                             reads as natural, not
                                             engineered

DARK WEB OSINT GUIDE (Parts 1-4)
  Breach/leak correlation                -> Phishing pretext material:
                                             a credential or internal
                                             detail that already leaked
                                             makes a lure impossible to
                                             distinguish from a real
                                             account-compromise notice
  Persona correlation (PGP/wallet/
  stylometry/infrastructure overlap)     -> HUMINT Assessment: the exact
                                             same corroboration discipline,
                                             aimed at a suspect instead
                                             of a source

OSINT GUIDE: company/domain bucket
  theHarvester, Hunter.io, OpenCorporates -> Gophish target-list CSV
                                              (Users & Groups) directly;
                                              the Position field that
                                              drives department-themed
                                              pretext selection comes
                                              from here, not guesswork
```

**Why this determines pretext tier, not just target selection**

The Phishing Guide's own difficulty tiering makes the dependency explicit rather than implied: Tier 1 needs no OSINT at all — a generic lure, and it shows. Tier 2 needs enough OSINT to personalize a name and a plausible internal system. **Tier 3, the red-team-grade pretext, is defined entirely by its OSINT depth** — "pretext built from actual OSINT on the target org — real vendor names they use, a real recent internal event or project name, spoofed to look like a genuine reply in an existing thread." There is no version of a Tier 3 pretext that skips this step; the tier *is* the OSINT.

**What happens without it, concretely**

A pretext built without OSINT input reads exactly like the failure mode the Phishing Guide warns about directly: "deliberately bad grammar, an implausible prize, a sender domain that doesn't even try to look real" — because there's nothing real underneath it to anchor the lie. A HUMINT approach without prior OSINT means spotting the wrong person entirely — Part 1 of the HUMINT Guide names this exact mistake: "the most talkative person in a room is rarely the one with actual access; validate access level before investing development time," and validating access level is, again, an OSINT task that has to happen first.

**The actual sequencing, in practice**

```
1. OSINT GUIDE buckets establish WHO has the access you need
   (Targeting/Spotting)
2. DARK WEB OSINT GUIDE adds whatever's already exposed about them
   (credential reuse, persona correlation, prior leak appearances)
3. That combined picture determines the PRETEXT TIER you can
   credibly run — Tier 1 through Tier 3, per the Phishing Guide
4. Only then does actual contact begin — a Phishing campaign launch,
   or HUMINT's Development phase — built on information gathered
   before a single message was sent, not improvised during it
```

Same discipline as the rest of this series, just applied one layer earlier than usual: the tool you reach for during contact was never the hard part — the OSINT that made the contact believable in the first place is.

## PSYCHOLOGICAL FACTORS OF SOCIAL ENGINEERING 

**The Eight Psychological Levers Behind Social Engineering**

Nearly every social engineering attack — phishing, vishing, smishing, in-person pretexting — runs on the same small set of psychological levers. Most trace back to Robert Cialdini's research on influence and persuasion. The mechanism underneath all of them is the same: they push a target from slow, deliberate thinking into fast, reactive thinking, so the target responds to the *emotional tone* of a message before consciously evaluating its *content*. Real attacks rarely use one lever in isolation — stacking two or three is what actually drives compliance.

### Urgency

**What it is:** A closing window to act — "within 24 hours," "today only," "before your access is suspended."

**Why it works:** Time pressure is one of the most reliable ways to suppress deliberate thinking. When a decision feels time-limited, people default to fast, instinctive judgment rather than careful evaluation, because the brain treats "no time to think" as a reason to act rather than a reason to slow down.

**How to recognize it:** Ask whether the deadline is actually necessary for the stated purpose, or whether it exists purely to rush the decision. Legitimate time-sensitive requests (a real security incident, a real deadline) can usually be verified through a second channel without losing anything by doing so.

### Authority

**What it is:** The message appears to come from someone or something with legitimate power — IT, a manager, a government agency, a recognized brand.

**Why it works:** People are conditioned from an early age to defer to perceived authority with minimal friction, a tendency reinforced by workplaces that depend on hierarchy to function. Authority short-circuits the instinct to question a request, because questioning authority feels socially costly even when it's warranted.

**How to recognize it:** Authority claimed in a message is not authority verified. A genuine request from a real authority figure can always be confirmed independently — a callback to a known number, a message through a separate verified channel — without insulting anyone involved.

### Fear / Loss Aversion

**What it is:** A threatened loss — a compromised account, frozen funds, legal trouble, lost data.

**Why it works:** Humans are wired to weigh potential losses more heavily than equivalent gains (loss aversion is one of the most robust findings in behavioral economics). A message framed around an imminent loss activates a more urgent, anxious response than one offering an equivalent gain, making fear-framed pretexts disproportionately effective.

**How to recognize it:** Fear narrows attention onto the threat and away from scrutinizing the messenger. When a message makes you anxious, that's the moment to deliberately slow down rather than speed up — the anxiety is often the entire mechanism of the attack, not an honest side effect of real news.

### Curiosity

**What it is:** An unresolved information gap — "see who viewed your profile," an unexpected shared document, a delivery you don't remember ordering.

**Why it works:** The brain treats an open information gap as mildly aversive and wants it closed; this is sometimes called the "curiosity gap." Curiosity-based lures don't need to threaten or promise anything — they just need to make the target want to know more.

**How to recognize it:** Curiosity-driven messages are often vague by design, since specificity would let the target evaluate plausibility before clicking. A genuine notification usually gives enough detail to judge it without opening anything.

### Reward / Greed

**What it is:** A gain framed as being within easy reach — a bonus, a prize, a refund, a gift card.

**Why it works:** Opportunity triggers the same kind of urgency-driven, fast thinking as fear does, just from the positive side. The prospect of an unexpected gain reduces the instinct to scrutinize how plausible the offer actually is.

**How to recognize it:** Ask whether the reward matches the actual relationship — does this organization normally give out rewards this way, unprompted, through this channel? Most legitimate rewards don't require urgent action to "claim" them.

### Social Proof

**What it is:** The implication that the request is routine, expected, or that others have already engaged with it — "3 colleagues have already completed this," a forwarded-looking reply chain, a request that positions itself as continuing an existing relationship.

**Why it works:** People use others' behavior as a shortcut for what's appropriate or safe, especially under uncertainty. If a request looks like it's already underway or already normalized among peers, it lowers the instinct to be the one person who questions it.

**How to recognize it:** Social proof in a message is easy to fabricate and hard to verify from inside the message itself. If something is "already" happening, it can be confirmed by asking the people it allegedly involves, directly.

### Reciprocity / Rapport

**What it is:** A warm, friendly, or seemingly helpful tone — a sender who's apologetic about the inconvenience, who offers something first, or who simply seems likable and easy to talk to.

**Why it works:** Reciprocity is a deeply ingrained social norm — when someone does something for us, or simply treats us warmly, we feel an obligation to respond in kind. A friendly, rapport-building tone also lowers the guard people normally raise toward unfamiliar senders, because suspicion feels at odds with politeness.

**How to recognize it:** Friendliness is not evidence of legitimacy. A request's legitimacy rests on who the sender verifiably is and what they're asking for, not on how pleasant the interaction feels.

### Commitment / Consistency

**What it is:** Getting a small, low-stakes "yes" first — confirming a name, approving a minor prompt — which makes a larger follow-up request feel like a natural continuation rather than a new risk.

**Why it works:** People have a strong psychological drive to stay consistent with actions they've already taken. Once someone has gone along with a small step, declining the next one can feel inconsistent or awkward, even if the second step is far more consequential than the first.

**How to recognize it:** Each step in a request should be evaluated on its own merits, not on the momentum of the steps before it. A legitimate process doesn't depend on the target feeling obligated to finish something they started.

### How these stack together

The strongest pretexts combine two or three levers rather than relying on one — for example, authority plus urgency plus fear ("IT Security detected a login from an unrecognized device and will lock your account in 2 hours unless you verify now"). One lever alone is comparatively easy to shrug off; two or three compounding levers is what moves real click-through and compliance data.

### The underlying defense

Across all eight, the actual counter isn't "look harder for red flags" so much as noticing the feeling itself — rushed, anxious, excited, obligated, curious — and treating that emotional spike as the signal to slow down and verify independently, rather than as a reason to act faster. That reaction is usually the entire mechanism the message was built to produce.

## Examples - Levers mapping (8*10*3 - 240 qulaity examples mapping)

### Email Examples 

### Phishing Email Classification by Psychological Lever

**Method:** Each email gets a **primary lever** (what the message leads with) and **supporting levers** (what reinforces it). Real attacks stack 2-3 levers, so the primary label is a judgment call. "Social Proof" follows the guide's definition, which includes requests that present themselves as routine or as continuing an existing relationship.

#### Summary

| Pillar | Count | Email #s |
|---|---|---|
| Fear / Loss Aversion | 31 | 3, 4, 6, 8, 11, 15, 16, 18a, 18b, 19, 28, 35, 36, 38, 39, 50, 55, 60, 61, 62, 63, 64, 65, 66, 68, 72, 73, 74, 76, 77a, 78 |
| Reward / Greed | 11 | 7, 9, 13, 51, 53, 54, 56, 58, 67, 79, 81 |
| Authority | 9 | 1, 5, 10, 21, 24, 25, 30, 32, 80 |
| Curiosity | 8 | 17, 23, 26, 33, 37, 40, 52, 77b |
| Urgency | 4 | 27, 69, 70, 75 |
| Social Proof | 4 | 22, 34, 59, 71 |
| Commitment / Consistency | 3 | 2, 14, 31 |
| Reciprocity / Rapport | 2 | 12, 57 |
| **Total (unique)** | **72** | |

---

### 1. URGENCY (4)

##### #27 - Mandatory Duo update
**Primary:** Urgency | **Also uses:** Authority, Fear
**Why:** The deadline is the next day and sits in the subject line. Missing it supposedly means deactivation and an in-person visit. It cites a "recent phishing incident" to seem legitimate.

~~~text
Date Sent: Saturday, May 24, 2025

Subject: ACTION REQUIRED: - Mandatory Duo Security Update Update Duo Before May 25 Deadline

U-M Information and Technology <[redacted]@umich.edu>
Sat, May 24, 5:24 PM

You received this email because you've been identified as someone who has outdated DUO Settings
Action Required: Update Duo settings by May 25
Greetings,

In response to a recent phishing incident, we are strengthening our authentication protocols to safeguard your account and university data. Our records show that your Duo two-factor authentication (2FA) settings have not yet been updated to meet these enhanced security requirements. This upgrade is mandatory..

To ensure uninterrupted access to your account, please complete the update by May 25, 2025. Failure to act will result in the deactivation of your Duo authentication, requiring an in-person visit to our office for reactivation.

How to Update Your Settings:
Click here: Update Duo Settings [hyperlink that leads to fake login screen]

Log in with your University of Michigan credentials.

Complete Duo Push authentication and wait for the confirmation screen.

Do not exit the page manually-it will close automatically once the update is complete.

Security Reminder:

This link is unique to your account. Do not share it with anyone.

Duo for iOS devices
Duo for Android devices
Thank you for your patience and commitment to keeping our systems and data secure.

How can we help you?
Contact the ITS Service Center:

Chat: chatsupport.it.umich.edu
Call: 734-764-HELP (764-4357)
~~~

##### #69 - RFQ from Yamabiko Corp
**Primary:** Urgency | **Also uses:** Reward, Rapport
**Why:** A "strict deadline" and 72-hour quote window push the victim to open the attachment quickly. The business opportunity is the bait.

~~~text
Dear John,

I am writing on behalf of Yamabiko Corporation to request a quote for an upcoming project. Given the critical timeline of our project, we are working within a strict deadline and require all quotes to be submitted within 72 hours.

We value quality and reliability and are looking to establish a long-term relationship with a supplier who can meet our project's demands within the specified timeframe.

Please provide a comprehensive quote, including pricing, delivery charges, and any other applicable fees for the products/services outlined in the attached RFQ.

Thank you for your prompt attention to this request. We look forward to your response.

Regards,

Hiro Tanaka
Procurement Officer
Signature

[footer trimmed: confidentiality disclaimer]
~~~

##### #70 - Netflix password expiring
**Primary:** Urgency | **Also uses:** Fear, Social Proof
**Why:** A 3-day countdown, plus "all customers" and "recent compromises," makes the reset feel routine and necessary.

~~~text
Password expiring soon

Hi John,

Your password is due to expire in 3 days.
Reset Password

Netflix are requesting all its customers perform a password reset due to a recent increase in account compromises.

- Your friends at Netflix

Questions? Visit the Help Centre

Netflix Productions
Communication

Settings | Terms of Use | Privacy | Help Centre

This message was emailed to john[.]doe@mybusiness[.]com by Netflix.
~~~

##### #75 - Critical device-management update
**Primary:** Urgency | **Also uses:** Authority, Fear
**Why:** The update must be done "by the end of today" or the victim is locked out tomorrow. It asks them to download and run a file.

~~~text
Dear all Contoso Corp employees,

Our IT team has identified a critical vulnerability in our device management software. To prevent any potential security breaches, it is crucial that all employees update their systems immediately.

Action Required:

Please follow these steps to install the necessary update:

    Click the link below to access the update portal.
    Use your company credentials to verify your identity.
    Download and run the update file to complete the installation.

Update Now

Please complete this update by the end of today. Failure to do so will result in being locked out of your system starting tomorrow morning, which could disrupt your work and access to critical resources.

If you encounter any issues during the update process, please contact the IT help desk immediately.

Thank you for your prompt attention to this matter.

Best regards,
IT Help Desk
Contoso Corp
~~~

---

### 2. AUTHORITY (9)

##### #1 - Fake professor, housing registration
**Primary:** Authority | **Also uses:** Commitment, Rapport
**Why:** Claims instruction from Student Affairs and Housing. The first ask is small (name and ID), leading to a form later. Sent from a Gmail address. (#20 is an exact duplicate of #1 and #2.)

~~~text
From: [redacted] <redacted@gmail.com>

Subject: Umich First Year Students Late Housing Registration

Hello,

I am  Professor [redacted] at the University of Michigan. I have been instructed by the Office of Students Affairs and the Michigan Housing Community to guide you in securing suitable housing accommodation here at Umich.

Kindly reply with your name and student ID along with a date and time convenient to you to discuss your information, which will determine how we will implement your housing.

I will be open to answer any questions you may have regarding your accommodation. After which you will proceed to sign the Undergraduate Housing Allotment Registration Form which I will send to you at the end of our discussion via mail.

Thank you for your attention to this matter, and I hope to hear from you soon.
~~~

##### #5 - Duo token out of sync
**Primary:** Authority | **Also uses:** Fear, Commitment
**Why:** Copies the real Weblogin/Duo branding and footer. Step-by-step instructions walk the victim into handing over a passcode.

~~~text
U-M Weblogin
DUO AUTHENTICATION

This is to inform you that the school database was currently upgraded and your Duo token got out of sync. Follow the steps below to sync your Duo two-factor authentication to the school database.

Step 1: Open the Duo Mobile app, In the Duo Mobile app goto michigan university and tap the key icon on the right side of the screen. A six-digit passcode will display, copy the passcode and proceed to step 2.

Step 2: Click the link below to Goto the school portal and enter the passcode to sync to the database.

[Link removed for safety - note that this link may use the text "weblogin.umich.edu" but that if you hover over the link you can see that it points to a fake login site, NOT the real weblogin!]

Failure to do this will result into error when trying to login the school website over time.

This page displays best when JavaScript is enabled in your web browser. JavaScript is required for two-factor authentication.
By your use of these resources, you agree to abide by Responsible Use of Information Resources (SPG 601.07), in addition to all relevant state and federal laws.

University of Michigan
© 2022 The Regents of the University of Michigan
~~~

##### #10 - NYU Abu Dhabi peer-review invite
**Primary:** Authority | **Also uses:** Reward, Rapport, Urgency
**Why:** Written "on behalf of" a Vice Provost in a formal solicitation style. The $3,000 honorarium and "reply within the weekend" reinforce it. (Borderline: could be Reward.)

~~~text
Dear Dr. [Name Redacted],

I am writing to you on behalf of our Vice Provost for Research Administration , Dr. [Name Redacted].

A proposal titled "Replication Of Electoral Registration and Turnout for Women: Experimental Evidence from Kenya" has been submitted in response to the solicitation. An initial research was conducted in march 2015. We would like to invite you to provide a review of the concluding research.

Would you please send a reply within the weekend to confirm your willingness to participate? We would ask for your completed review by 21 July.

We have attached a copy of the proposal and a template that you may use to facilitate your review. Comments from the referees provides extremely valuable guidance to the proposers. We would very much like to share your comments anonymously with the proposal's author. If you request that the contents of your review, not be shared with the proposer, we would honor your request, of course.

We sincerely appreciate your time, and would like to offer an honorarium of $3000. We have attached payment forms to this message. If you would be kind enough to return the signed completed form to me along with your reply, that would help to expedite the payment process.

Thank you, We very much appreciate your willingness to devote time to helping us with this important task.

Sincerely,

[Name Redacted]

NYU Abu Dhabi

West Administration Building/A3 (164A)

P.O. Box 129188Abu Dhabi, United Arab

Emirates
~~~

##### #21 - QR-code 2FA confirmation
**Primary:** Authority | **Also uses:** Fear, Social Proof
**Why:** Spoofed U-M logo and "required for all recipients." The text sits in an image to dodge filters, and the QR code hides the destination.

~~~text
{Note: For this phishing email, the text is an embedded image file that also includes a spoofed U-M logo and QR code.}

Due to recent security, This email is to confirm 2-factor authentication for all University of Michigan email recipients. You're hereby required to complete exercise with the mobile number you want your 2-Factor Authentication set to.

Scan QR Code to complete authentication.

{QR code}
~~~

##### #24 - Office of the Provost portal update
**Primary:** Authority | **Also uses:** Urgency
**Why:** A real-looking address and phone number lend credibility, but the sender domain is remax.pt. It asks for uniqname and password "as soon as possible."

~~~text
Date Sent: Sunday, October 19, 2025

Summary: The following phishing message linked to a Google doc (taken offline) that in turn linked to the phishing site shown in the images below.

From: Office of the Provost <redacted@remax.pt>

Subject: U-M Portal Enhancement

Dear University of Michigan Staff,

We're pleased to announce that the University of Michigan Portal has been upgraded to improve performance, security, and usability. Please take a moment to log in and complete the necessary update so you can access all features smoothly.

What you need to do:

Visit the portal.
Log in with your existing University of Michigan credentials (uniqname and Kerberos password)
Follow the on-screen instructions to complete the update (and update your profile if prompted)

Deadline: As soon as possible

If you experience any difficulty logging in or need assistance, please contact the ITS Service Desk

For additional inquiries you can reach the Office of the Provost at:

Phone: 734-764-9291
Email: provost-office@umich.edu
Address: 503 Thompson Street, Ann Arbor, MI 48109
~~~

##### #25 - Mandatory staff meeting (Zoom)
**Primary:** Authority | **Also uses:** Social Proof, Curiosity
**Why:** A "mandatory" notice from School Administration with a convincing Zoom invite. It asks for school credentials to "mark attendance."

~~~text
Date Sent: Monday, September 8, 2025

From: School Administration <admin-support@umich.edu> <admin-support@umich.edu> <admin-support@umich.edu>cc: <redacted@gmail.com>
Date: Mon, Sep 8, 2025 at 2:04 PM
Subject: Mandatory Staff Meeting: Important Policy Update
To: <redacted@umich.edu>

Mandatory Staff Meeting

Dear Staff Member,

Please plan to attend a mandatory staff meeting to discuss an important policy update. Your participation is essential to ensure everyone is informed of the upcoming changes.

Date: Tuesday, September 16, 2025

Time: 3:00 PM - 3:45 PM EDT

Location: Zoom (Virtual Meeting)

https://us05web.zoom.us/j/81234567890?pwd=nmki90jdhslkeuiopsllslsmooiwlls12opQJ.1

Meeting ID: 812 3456 7890
Passcode: 49087

Please add this event to your calendar. Mark your attendance by signing in with your school credentials.

Thank you,

School Administration
University of Michigan
E: admin@umich.edu
W: umich.edu
~~~

##### #30 - ITS routine account maintenance
**Primary:** Authority | **Also uses:** Social Proof
**Why:** Impersonates ITS and frames the request as routine. It asks the victim to approve a 2FA prompt, which hands the attacker access.

~~~text
Date Sent: Wednesday, May 14, 2025

This scam uses stolen credentials from individuals at universities to send phishing emails. This email is the first phase of the scam, which is to steal passwords on a fake login screen hosted on a Wix website.

Subject: Information Technology Services (ITS)

Dear Student and Staff ,
University of Michigan Information Technology Services (ITS) team will be performing routine account maintenance. To ensure uninterrupted access to university systems, please confirm your account details on the UMICH Portal at [redacted link].

complete the two-factor authentication process, allow the notification you receive on your phone via call or text message.

Thank you for helping keep the UMICH network secure.

-
ITS Help Desk
University of Michigan
Ann Arbor MI 48109-1432
its.umich.edu
~~~

##### #32 - President's 2026/27 event calendar
**Primary:** Authority | **Also uses:** Social Proof, Rapport
**Why:** Spoofs the president's name and title. The dull, routine content lowers suspicion. The signature block says "District Office," a template mismatch.

~~~text
From: Domenico Grasso < [redacted]@umich.edu>
Date: Tue,15 Sep 2026
Subject: Updated 2026/2027 Staff and Faculty Event Calendar
To: University Of Michigan <[redacted]@umich.edu>

Dear Staff and Faculty,

The 2026/2027 Staff & Faculty Event Calendar has now been finalized, updated and is available for review.The calendar brings together important dates and scheduled activities for the year, including:

    Staff and faculty events
    Department meetings and activities
    School-level meetings
    District-wide meetings and events
    Other scheduled staff commitments

Please take a moment to review the complete calendar so you can note the dates that apply to you and your department and plan accordingly.

Review the 2026 Staff & Faculty Event Calendar:
[Click here to view the complete calendar]

We encourage all staff members to review the calendar at their earliest convenience and keep it available for reference throughout the year.

Thank you for your continued commitment and support.
Domenico Grasso

Titles: President
Locations: District Office
Email: presoff@umich.edu
~~~

##### #80 - Supervisor shares confidential draft
**Primary:** Authority | **Also uses:** Urgency, Curiosity
**Why:** Appears to come from a boss and leadership team, with an end-of-day deadline before a board call. The sender domain is an external lookalike.

~~~text
Contoso Corp file share
supervisor@email-address[.]com shared a file with you
For John Doe
File
Board_Review_Draft_Confidential.docx
218 KB • Updated 02 Oct 2026
Message from supervisor@email-address[.]com
I need your quick input on a confidential draft I've been working on with the leadership team. Please don't circulate this yet. Can you review the document and leave your feedback by end of day. This is time sensitive as we need it before tomorrow's board call.
Open Document
~~~

---

### 3. FEAR / LOSS AVERSION (31)

##### #3 - "Login from another computer"
**Primary:** Fear | **Also uses:** Authority, Urgency
**Why:** Threatens loss of the account unless the victim clicks to prove it is still in use.

~~~text
This Email is to notify the student and Staff Of Michigan University that your email is being logged in from another computer. We will need you to confirm that your account is still in use  Click here  in order To keep your account active...

Michigan University

UMICH Help-desk

Copyright UMICH. All rights reserved
~~~

##### #4 - Pending messages, update to avoid deactivation
**Primary:** Fear | **Also uses:** Urgency, Curiosity, Authority
**Why:** Combines deactivation with a 10-day deletion deadline. "Pending messages" adds a curiosity hook.

~~~text
At 11/10/2021 4:22:41 p.m. [name redacted] @umich.edu have incoming messages pending.

You must update your account to avoid deactivation.

Click to update and restore pending emails

Messages will remain pending until proper action is taken.

Messages older than 10 days will be removed

umich.edu Team.
~~~

##### #6 - PayPal invoice, $219 Amazon Prime
**Primary:** Fear | **Also uses:** Urgency, Authority
**Why:** An unauthorized charge triggers panic and pushes the victim to call the scam number rather than check their account.

~~~text
(Please note that some details such as links and phone numbers have been removed for safety.)

umich.edu, here are your invoice details

Hello, @umich.edu
Here's your invoice
Timothy Lee Cotterill sent you an invoice for $219.00 USD
Due on receipt
View and Pay Invoice

Buy now. Pay over time.

Simply select PayPal Credit at checkout and enjoy No Interest if paid in full in 6 months. Subject to credit approval. See terms. US customers only.

Note from Timothy Lee Cotterill

Thank you for your Successful Purchase using PayPal for Amazon Prime 1yr Subscription. You paid $219. 00 USD which will be shown in your account within the next 24-48hrs. If you haven't made this transaction and do not Authorize it. Call or Reach us immediately at +1-888-xxx-xxxx

PayPal
Help & Contact | Security | Apps
Twitter    Instagram    Facebook    LinkedIn
~~~

##### #8 - Unauthorized mailbox login attempt
**Primary:** Fear | **Also uses:** Authority, Rapport
**Why:** "Account will be disabled" threat. The closing "thanks for helping us protect you" adds false rapport.

~~~text
Dear umich.edu mail user,

We've noticed an unauthorized login attempt to your mailbox. and your email account will be temporarily disabled by the System Mail administrator

CLICK HERE  To validate your account being disabled.

Thanks for helping us protect you.

University of Michigan Administrator
~~~

##### #11 - Email forwarding to a Gmail address
**Primary:** Fear | **Also uses:** Urgency, Authority
**Why:** Implies someone is reading your mail. The "Cancel Forwarding" button is the trap.

~~~text
IMPORTANT!!

All email sent to "redacted@umich.edu" will now be copied to "redacted@gmail.com".

If you haven't set up email forwarding, cancel below, If this was you, you don't need to do anything.

Cancel Forwarding

You received this email to let you know about important changes to your  umich.edu Account and services.
© 2023 Administrator, 1600 Amphitheatre Parkway
~~~

##### #15 - PayPal money request, $1,369.55
**Primary:** Fear | **Also uses:** Urgency, Authority
**Why:** A large unexpected charge, with "we will proceed" if the victim does not call.

~~~text
Creatrix inc has updated their money request
Updated request details

Amount requested

$1,369.55 USD

Note from Creatrix inc:

Don't recognize the seller? Please contact PayPal Support Team immediately at +1(888) 434-2883 (Toll Free). If you do not reach out, we will proceed with the transaction.

Transaction ID

U-3YP350079M3163310

Transaction date

December 5, 2024

Pay Now
Don't recognize this request?

Before paying, make sure you recognize this person. Don't engage with this request if you're unsure about it. PayPal won't contact you through a money request. Learn more about common security threats and how to spot them.

PayPal

Paypal invoice with Pay Now button
~~~

##### #16 - DocuSign + TOTAL AV receipt, $359.25
**Primary:** Fear | **Also uses:** Curiosity, Authority
**Why:** A fake charge pushes the victim to call a scam number. The DocuSign wrapper adds credibility.

~~~text
Email Text
Fakeali Locked sent you a document to review and sign.
REVIEW DOCUMENT

Fakeali Locked

sm.d.n.g.m.s.bfdndngn@gmail.com

Transaction Completed with DocuSign: fu7tp8PKOx.pdf

Payment Confirmation

TOTAL AV Premium

Dear Valued Customer,

We sincerely thank you for your payment of $359.25 towards your TOTAL AV Premium subscription. Your trust in us to safeguard your digital experience means a great deal. Below, you will find the details of your transaction:

Business Name: TOTAL AV Premium

Reference ID: 94XYZ8

Amount Paid: $359.25

Payment Date: November 26, 2024

Should you have any questions about your subscription or need help with the setup process, please don't hesitate to contact our support team. We're here to assist you.

Phone: +1 (866) 511-5248 / (866) 982-4913

Thank you for choosing TOTAL AV Premium!

Best regards,

TOTAL AV Support Team

(866) 613-3204

docusign invoice with link to scam document review
~~~

##### #18a - Sextortion email
**Primary:** Fear | **Also uses:** Urgency
**Why:** Threat of exposure with a one-day deadline. Personal details (address, photo) make it feel credible. This is extortion rather than credential phishing.

~~~text
[redacted first and last name],

I know that calling [redacted phone number] or visiting [redacted address] would be a effective way to have a word with you in case you don't cooperate. Don't even try to escape from this. You have no idea what I'm capable of in Ann Arbor.

I suggest you read this message carefully. Take a moment to chill, breathe, and analyze it thoroughly. We're talking about something serious here, and I don't play games. You don't know anything about me whereas I know you and right now, you are wondering how, correct?

Well, you've been a bit careless lately, clicking through those girlie videos and venturing into the darker corners of cyberspace. I actually installed a Malware on a porn website and you accessed it to watch(you get my drift). When you were watching videos, your smartphone started out working as a RDP (Remote Device) which allowed me total accessibility to your system. I can peep at everything on your display, switch on your cam and mic, and you wouldn't have a clue. Oh, and I have got access to all your emails, contacts, and social media accounts too.

Been keeping tabs on your pathetic existence for a while now. It is simply your misfortune that I noticed your misdemeanor. I gave in more days than I should have investigating into your life.

Extracted quite a bit of juicy info from your system. and I've seen it all. Yeah, Yeah, I've got footage of you doing embarrassing things in your room (nice setup, by the way). I then developed videos and screenshots where on one side of the screen, there's the videos you had been enjoying, and on the other half, its someone doing filthy things. With simply a single click, I can send this filth to every single of your contacts.

Your confusion is clear, but don't expect sympathy. As a family man, I am willing to wipe the slate clean, and allow you to move on with your regular life and forget you ever existed. I will offer you two alternatives.

Option One is to turn a blind eye to my e mail. Let me tell you what will happen if you select this option. Your video will get sent to your contacts. The video is straight fire, and I can't even fathom the humiliation you'll endure when your colleagues, friends, and fam check it out. But hey, that's life,ain't it? Don't be playing the victim here.

Wiser second option is to pay me, and be confidential about it. We'll call it my "confidentiality tip".let me tell you what happens if you opt this path. Your secret will remain your secret. I will wipe everything clean once you send payment. You'll transfer the payment via Bitcoins only. Pay attention, I'm telling you straight: 'We gotta make a deal'. I want you to know I'm coming at you with good intentions. My word is my bond.

Required Amount: USD 2000

BITCOIN ADDRESS: [redacted bitcoin address]

Once you pay up, you'llsleep like a baby. I keep my word.

Notice: You got one day to sort this out and I will only accept Bitcoins (I've a unique pixel in this email, and right now I know that you've read through this mail). My system will catch that Bitcoin payment and wipe out all the dirt I got on you. Don't even think about replying to this or negotiating,it's pointless. The email and wallet are custom-made for you, untraceable. If I notice that you've shared or discussed this message with anyone else, your shitty video will instantly start getting sent to your contacts. And don't even think about turning off your phone or resetting it to factory settings.It's pointless. I don't make mistakes, [first name redacted].

Beautiful neighborhood btw

[redacted photo of home of recipient - Google Maps image]

Honestly, those online tips about covering your camera aren't as useless as they seem. I am waiting for my payment..
~~~

##### #18b - HR "account deactivation" notice
**Primary:** Fear | **Also uses:** Authority, Urgency
**Why:** Loss of the account unless status is confirmed. "One-time submission" adds pressure.

~~~text
From: Human resources <[redacted]@umich.edu>

Sent: Wednesday, September 11, 2024 10:21 PM

Subject: NOTICE BY ADMIN

Your University of Michigan account is set to be deactivated due to retirement, graduation, or transfer. However, our records show that you are still an active member. To prevent deactivation, please confirm your status or let us know if you wish to close your account.

We recommend verifying your UMICH account as soon as possible to avoid any interruptions.

Verify HERE

Please note that this is a one-time submission.

Thank you
~~~

##### #19 - West Nile case on campus
**Primary:** Fear | **Also uses:** Curiosity, Authority
**Why:** A health scare with "click for details." The sender domain (utk.edu) does not match the institution.

~~~text
From: [redacted] <[redacted]@utk.edu>

Date: Mon, Sep 9, 2024 at 11:23 AM

Subject: Health Alert: West Nile Virus Case on Campus

Dear University of Michigan-Dearborn Staff,

We want to inform you that a member of our  University of Michigan-Dearborn community has been diagnosed with West Nile Virus (WNV). This highlights the importance of taking precautions against this mosquito-borne illness.

The affected individual is receiving medical care. Click For more details about the case

Key Information:

    West Nile Virus is spread by mosquitoes. While most people show mild or no symptoms, severe cases can cause serious health issues like encephalitis or meningitis.
    Symptoms may include fever, headaches, body aches, and in severe cases, confusion or muscle weakness.

How to Protect Yourself:

    Eliminate standing water to reduce mosquito breeding areas.
    Use insect repellent, especially during dawn and dusk.
    Wear long sleeves and pants to avoid mosquito bites.

University Actions:

    The university is increasing mosquito control efforts on campus and will provide updates as needed.

If you experience symptoms, please contact  University of Michigan-Dearborn health services immediately.

Thank you for helping keep our campus safe.

Sincerely,
[redacted] (she/her)
Special Collections Reference Health
University of Michigan-Dearborn
Health Care Center
~~~

##### #28 - COVID variant exposure
**Primary:** Fear | **Also uses:** Curiosity, Authority, Rapport
**Why:** Health fear plus curiosity about who was exposed. The request for "discretion" discourages checking with colleagues.

~~~text
Date Sent: Friday, May 16, 2025

Hello [recipient name]

I hope this message finds you well.

We're reaching out to inform you of a recent health development: a staff member who may have been in your vicinity has recently returned from international travel and tested positive for a newly identified COVID-19 variant.

While our community remains largely vaccinated, this particular variant has led us to implement enhanced precautionary measures. As such, we kindly ask that you review your recent interactions to assess any potential close contact.

You can do this by logging into the Staff Interaction Portal [hyperlink leads to fake U-M login screen] using your credentials to review your recent contact records.

If you believe you may have had contact with the individual, please reach out to us promptly. Otherwise, no action is necessary, and you may disregard this message.

We appreciate your discretion in keeping this information confidential to avoid unnecessary concern within the community.

Thank you for your understanding and cooperation. Should you have any questions or need further assistance, please don't hesitate to get in touch.

Warm regards,
Dr. [redacted name]
Health Care Center
University of Michigan
1109 Geddes Ave | Ann Arbor, MI 48109
Phone: 734-845-3211 | Fax: 734-845-2345
Email: healthservice@umich.edu
~~~

##### #35 - Google sign-in attempt blocked
**Primary:** Fear | **Also uses:** Authority
**Why:** Says someone used your password. The "blocked" reassurance still ends in a prompt to review activity.

~~~text
Sign-in attempt was blocked
john[.]doe@mybusiness[.]com
Someone just used your password to try to sign in to your account from a non-Google app. Google blocked them, but you should check what happened. Review your account activity to make sure no one else has access.
~~~

##### #36 - HR policy violation, "recorded evidence"
**Primary:** Fear | **Also uses:** Authority, Curiosity
**Why:** Threatens job and reputation. Shame discourages the victim from consulting anyone.

~~~text
Dear John Doe,

I hope this message finds you well.

This email is to bring to your attention an issue of concern. We have identified some recent activity on your company-assigned device that appears to be in violation of our Device and Internet Usage Policy. Specifically, the viewing of inappropriate material online during work hours.

View Recorded Evidence.

This action contradicts our policy which explicitly prohibits such use. Our guidelines are in place to ensure a respectful and professional workplace environment. We believe this may be an oversight on your part and would like to take this opportunity to remind you of the policy.

Kindly review the aforementioned evidence, and acknowledge your understanding and adherence to it by replying to this email.

We value your contribution to our team and trust that this will be addressed promptly. Please feel free to reach out if you have any questions or concerns.

Best regards,

The Contoso Corp HR Team

"Empowering People, Driving Success"
~~~

##### #38 - LastPass login from Singapore
**Primary:** Fear | **Also uses:** Authority, Urgency
**Why:** A password-vault compromise is high stakes. The "No, it wasn't me" button is the trap.

~~~text
Login attempt successful

Hello John,

Someone just used your master password to try to log in to your account from a device or location we didn't recognize. LastPass allowed this attempt, but you should take a closer look.

Was this you?

Account
john[.]doe@mybusiness[.]com

Time
Fri Oct 02 2026 17:56:41 GMT+0530 (India Standard Time)

Location
Ang Mo Kio, Singapore

IP address
159.192.13.147

Yes, it was me

Verify new device or location

No, it wasn't me

If it wasn't you, don't worry. If it wasn't you, click here to immediately block this request.

Security tip: Your master password should be unique to LastPass, but if you use it elsewhere, set new passwords for those accounts, as well.

© LastPass. All Rights Reserved.
This mandatory email was sent to john[.]doe@mybusiness[.]com: email opt-out settings have been ignored.
IP address of the person who requested this email: 159.192.13.147
~~~

##### #39 - Slack password reset
**Primary:** Fear | **Also uses:** Authority, Urgency
**Why:** "You asked for a reset" when you did not implies account takeover. The 24-hour link adds pressure.

~~~text
Reset your Slack workspace password

You asked us to send you a password reset link for auth.slack.com. This password reset request is for john[.]doe@mybusiness[.]com.

If you didn't authorise this password reset request, please notify the team at slack.

This password reset link will be valid for the next 24 hours and is tied to the email address: john[.]doe@mybusiness[.]com. If you have any issues, please contact your administrator for support.

Reset Password

Please let us know if you have any other questions or feedback.

Cheers,
The team at Slack

Made by Slack Technologies, Inc  •  Our Blog
500 Howard Street • San Francisco, CA 94105 • United States
~~~

##### #50 - Bank of America unusual activity
**Primary:** Fear | **Also uses:** Authority, Urgency, Rapport
**Why:** Frozen transactions create alarm. The apologetic tone and genuine-sounding "we'll never ask for your SSN" line add trust.

~~~text
Bank of America

We've detected unusual activity on your account

John Doe,

At Bank of America, we take the security of your account very seriously. During our recent routine monitoring, we detected some unusual activity on your account which doesn't align with your typical banking patterns.

For the security of your account, we have temporarily put a hold on any further transactions. We kindly ask that you review the recent activity on your account by logging in to verify if the transactions were made by you.

Login to your account

Thank you for your prompt attention to this matter. We apologize for any inconvenience this may cause and appreciate your understanding as we work to ensure the security of your account.

We'll never ask for your personal information such as SSN or ATM PIN in email messages. If you get an email that looks suspicious or you are not the intended recipient of this email, you should delete it immediately.

Please don't reply to this automatically generated service email.

Privacy Notice     Equal Housing Lender

Bank of America, N.A. Member FDIC
© 2026 Bank of America Corporation
~~~

##### #55 - Qantas unusual login
**Primary:** Fear | **Also uses:** Authority
**Why:** Threatens the account and points balance. Both the "wasn't me" and "was me" buttons likely lead to the lure.

~~~text
Dear John,
We noticed some unusual login activity on your account.

Time: Fri Oct 02 2026 18:04:33 GMT+0530 (India Standard Time)

Origin: Qantas

Location: Marina Bay, Singapore

Device / OS: Windows NT

Browser: Chrome

If you didn't login recently, or you believe an unauthorised person has accessed your Qantas Frequent Flyer account, please let us know
NO, THIS WASN'T ME     YES, THIS WAS ME

Need to speak with us? Contact the Frequent Flyer Service Centre on 13 11 31 or by emailing frequent_flyer@qantas.com.au.

International contact details are available here.
~~~

##### #60 - Dropbox inactivity closure
**Primary:** Fear | **Also uses:** Urgency, Authority
**Why:** Files and account to be "closed immediately," with "last chance" framing.

~~~text
Hello, John

We noticed that you haven't used your Dropbox account in the past two months, and we will close it immediately.

Please Sign In before we proceed further to erase your documents, this is your last chance to save your account.

Best regards,
The Dropbox Team
~~~

##### #61 - Facebook breach, 50 million accounts
**Primary:** Fear | **Also uses:** Social Proof, Authority, Urgency
**Why:** A huge number makes it feel real, and a named security officer adds authority. It echoes a real past incident.

~~~text
Facebook

Security Update

Hi John,

As you may have heard, our engineering team recently discovered a security issue affecting almost 50 million accounts. Unfortunately, we have reason to believe your account may have been affected.

As a precaution, we urge you to change your password immediately to prevent unauthorised access to your account.

Reset password

Learn More

People's privacy and security is incredibly important, and we're sorry this happened. It's why we've taken immediate action to secure these accounts and let users know what happened.

Sincerely,
Nathaniel Simon
Global Chief Security Officer

Facebook, Inc., 1 Facebook Way, Menlo Park, CA 94025
~~~

##### #62 - New login from Bangkok
**Primary:** Fear | **Also uses:** Curiosity
**Why:** Mild wording, but an unknown device and location create concern. "Safely disregard if it was you" makes clicking feel low-risk.

~~~text
John, we noticed a new login.

We noticed a login from a device you don't usually use.

Windows · Chrome · Bangkok, Thailand

Fri Oct 02 2026 18:10:11 GMT+0530 (India Standard Time) (PDT)

If this was you, you can safely disregard this email. If this wasn't you, you can secure your account here.

Learn more about keeping your account secure.
~~~

##### #63 - Quarantined emails
**Primary:** Fear | **Also uses:** Urgency, Curiosity, Authority
**Why:** Seven emails will be permanently deleted in 7 days, so the victim logs in to "rescue" them.

~~~text
RECEIVED EMAILS IN QUARANTINE

This is a notification to inform you that emails addressed to you have been placed in quarantine.

Emails quarantined: 7

Status: Pending Analysis

Please review your quarantine mailbox by following the below steps:

1. Sign into your quarantine mailbox

2. Click "Release" if the email is legitimate or "Block" if you suspect the email is spam or phishing

Emails in your quarantine mailbox will be permanently deleted within 7 days if action is not taken.

Kind Regards,
IT Support
~~~

##### #64 - GitHub OAuth app authorized
**Primary:** Fear | **Also uses:** Authority, Curiosity
**Why:** An unrecognized app with repo scopes suggests code compromise.

~~~text
Hey John!

A third-party OAuth application (AWS CodeBuild) with read:org and repo scopes was recently authorized to access your account.
Visit https://github.com/settings/connections/applications/41387c6857fcb0509a45 for more information.

To see this and other security events for your account, visit https://github.com/settings/security-log

If you run into problems, please contact support by visiting https://github.com/contact

Thanks,
The GitHub Team
~~~

##### #65 - Google Workspace MFA via QR
**Primary:** Fear | **Also uses:** Authority, Urgency
**Why:** The account is "temporarily suspended." The QR code hides the destination.

~~~text
Google Workspace Image

Your organization requires Multi-Factor Authentication.

Your account john[.]doe@mybusiness[.]com has been temporarily suspended. Please verify your email address by following the steps below to unlock your account.

    Open the camera app on your mobile device.
    Point the camera at the unique QR code below.
    When prompted, tap the notification to open the link and verify your email address.

QR Code.

You have received this important update about your Google Workspace account because you are the designated admin recipient for this alert type. You can turn off these alerts or change the email recipients in the System defined rules section of the Admin Console.

Google LLC 1600 Amphitheatre Parkway Mountain View, CA 94043
~~~

##### #66 - TikTok new-device code
**Primary:** Fear | **Also uses:** Authority, Urgency
**Why:** An unrequested verification code implies someone is trying to get in. The reset link is the lure.

~~~text
New Device Login
To verify your new device, enter this code in TikTok:

846752

Verification codes expire after 48 hours.

If you have not signed on using a new device, we recommend resetting your password immediately with the link below.

Reset Password

Tik Tok Support Team
TikTok Help Center: https://support.tiktok.com/

Have a question?
Check out our help center or contact us in the app using Settings > Report a Problem.

This is an automatically generated email. Replies to this email address aren't monitored.
This email was generated for john[.]doe@mybusiness[.]com

Privacy Policy
TikTok, 10100 Venice Bivd, Culver City, CA 90232
~~~

##### #68 - Amex card request confirmation
**Primary:** Fear | **Also uses:** Authority, Curiosity
**Why:** Someone supposedly requested a card. The "Something's Wrong" button invites a quick reaction.

~~~text
Visit American Express     Sign In
Can you please confirm this card request?
Either you or an authorized user requested a new card for your account. To help us keep your account safe, can you please confirm this request?
Confirm Request
Something's Wrong
As always, you can sign in to your account to verify your account info is accurate.

Thanks for choosing American Express
Simple. Flexible. Mobile.
Download the secure American Express Mobile app.
Important Information from American Express

Contact Us  |  Privacy  |  Help Prevent Fraud

This email was sent to john[.]doe@mybusiness[.]com and contains information directly related to your account with us, other services to which you have subscribed and/or any application you may have submitted.

The site may be unavailable during normal maintenance or due to unforeseen circumstances.

Products and services are offered by American Express Bank (Global), N.A., and American Express, N.A., Members FDIC.

American Express is a federally registered service mark. All rights reserved.
~~~

##### #72 - X copyright complaint
**Primary:** Fear | **Also uses:** Urgency, Authority
**Why:** Threatens page suspension, with a 48-hour appeal window.

~~~text
Logo

Content Infringement @Contoso Corp

Hi John,

We are writing to inform you that we have received a copyright complaint regarding content posted X. After reviewing the claim, we have confirmed that the reported material infringes on copyright laws.

The decision was made: Fri Oct 02 2026 18:19:34 GMT+0530 (India Standard Time)

Please review the details below to assist us in resolving the issue:

    Review the content in question and ensure it complies with our guidelines.
    Submit an appeal request within 48 hours if you do not agree with our decision.

Review Details

If we do not receive an appeal request from you within 48 hours, your page may be suspended for non-compliance with our policies.
We sent this email to @Contoso Corp.
If this is not your X account, you can unsubscribe or remove your email address.
~~~

##### #73 - SecureNotify, unused email deletion
**Primary:** Fear | **Also uses:** Urgency, Authority
**Why:** The account is marked for deletion unless verified within 24 hours. "Immediate action" is repeated throughout.

~~~text
Immediate Action Required: Verify Your Email Address

System Generated Notification
This is an automated message
sent to you by SecureNotify.

Immediate Action Required: Email Account Deactivation Notice
As part of your organization's efforts to manage and secure the flow of users as they come and go from the business, your IT department utilizes SecureNotify. This tool helps monitor and maintain the activity of email accounts to ensure optimal security and efficiency.

It has come to our attention that your email address appears to have been unused for an extended period. As a result, it has been marked for deletion. To avoid losing access to your account and any associated data, please verify your email account using the secure link below.
VERIFY YOUR EMAIL ACCOUNT
Failure to verify within 24 hours will result in your account being deactivated. If you have already completed this process, please disregard this message.
Thank you for your prompt action.

Best regards,
Contoso Corp IT Support via SecureNotify

Please do not reply to this email. Emails sent to this address will not be answered. This is an automated email sent by SecureNotify.

If you have any questions or need assistance, contact your IT Help Desk. Please note that failure to comply with the instructions provided in this email will result in the deactivation of your account. It is crucial that you take the necessary actions outlined in this message to avoid any disruptions to your access.
~~~

##### #74 - GoDaddy SQL injection, domain suspended
**Primary:** Fear | **Also uses:** Authority, Urgency
**Why:** The domain is already suspended and the victim is implicated in a botnet.

~~~text
Immediate Action Required: Validate Your Domain.

Need help? Contact us.
Customer Number: 3413936

Immediate Action Required: Security Breach Detected
We are contacting you regarding a critical security issue that has been identified within our network. Our monitoring systems have detected an SQL Injection vulnerability that has affected your domain registered with us. Subsequent investigations suggest that this vulnerability has led to unauthorized access, with your domain being used as part of a botnet operation involved in malicious activities.
Details of the Incident:

    Attack Vector: SQL Injection
    Compromise Indicators: Unusual outbound traffic, multiple unauthorized database queries
    Affected Organization: Contoso Corp

We have temporarily suspended your domain to prevent any further illegal activities. Your cooperation is crucial in ensuring the security and integrity of your services. Once the validation process is successfully completed, we aim to restore your domain as quickly as possible.
Required Action:

1. Please validate your domain. This is a critical step to regain control of your domain and restore normal operations.
2. Consider reviewing your database security measures and implementing enhanced security protocols.
Validate your domain
Thank you for your prompt attention to this critical security alert.

Best regards,
GoDaddy Security Team

Security Communication Notice: This message is part of our commitment to your digital security. GoDaddy sends communications like this only under urgent circumstances that require immediate attention and action to protect your account and personal data. Please ensure that the prescribed action is taken promptly to avoid your domain being shut down.
Please do not reply to this email. Emails sent to this address will not be answered.

Copyright 1999-2026 GoDaddy Operating Company, LLC. 2155 E. GoDaddy Way, Tempe, AZ 85284 USA. All rights reserved.
~~~

##### #76 - Microsoft account accessed
**Primary:** Fear | **Also uses:** Authority
**Why:** "Someone else accessed your account," with a demand for identity verification.

~~~text
Microsoft account
Security alert
We think that someone else has accessed the Microsoft account john[.]doe@mybusiness[.]com. When this happens, we require you to verify your identity with a security challenge and then change your password the next time you sign in.
If someone else has access to your account, they have your password and might be trying to access your personal information or send junk email.
If you haven't already recovered your account, we can help you do it now.
Recover account
Learn how to make your account more secure.
Thanks,
The Microsoft account team
Privacy Statement
Microsoft Corporation, One Microsoft Way, Redmond, WA 98052
~~~

##### #77a - Apple iCloud+ invoice, $89.99
**Primary:** Fear | **Also uses:** Authority, Curiosity
**Why:** An unexpected 12 TB subscription charge with a "click here if you don't recognize this" link.

~~~text
Apple

Tax Invoice

APPLE ID
john[.]doe@mybusiness[.]com

BILLED TO
Visa .... 9895
John Doe

DATE
02 Oct 2026

ORDER ID
MIO4Q81XZ8
DOCUMENT NO.
183781810780
iCloud+

A blue and white cloudDescription automatically generated

iCloud+ with 12 TB of Storage
Monthly
Renews 02 Oct 2026
$89.99

TOTAL
$89.99

Increased fraud alert. If you do not recognize this charge, click here.

If you have any questions about your bill, contact support. This email confirms payment for the iCloud+ plan listed above. You will be billed each plan period until you cancel by downgrading to the free storage plan from your iOS device, Mac or PC.

You may contact Apple for a full refund within 15 days of a monthly subscription upgrade or within 45 days of a yearly payment. Partial refunds are available where required by law.

Apple Pty Ltd.

Apple
Apple ID Summary • Purchase History • Terms of Sale • Privacy Policy
Copyright © 2026 Apple Pty Ltd.
All rights reserved
~~~

##### #78 - Drata account inactivity
**Primary:** Fear | **Also uses:** Authority, Urgency
**Why:** The account is scheduled for deletion. The footer mentions a competitor (Vanta), a mismatch tell.

~~~text
Automate and Accelerate

Dear John,

This is an automated notification from the Drata Security System. Your account has been flagged for inactivity. To maintain account security and data integrity, inactive accounts are scheduled for automatic deletion after a specified period.

Immediate Action Needed:

    Login to prevent your account from being deactivated and deleted using the link below.
    If you've forgotten your password, follow the prompts after entering your domain at the login screen

This is an automated message. Please do not reply.

Thank you,
Drata Security Team

LOGIN TO YOUR ACCOUNT
RECOVER YOUR ACCOUNT

Stay ahead with our latest security trends and updates. [Subscribe] for insights and exclusive offers. Protect your data, enhance compliance with Vanta.
~~~

---

### 4. REWARD / GREED (11)

##### #7 - "Data collection research" job, then check follow-up
**Primary:** Reward | **Also uses:** Authority, Commitment
**Why:** $250 weekly with no experience needed. The follow-up asks the victim to deposit a check, a classic overpayment fraud.

~~~text
Example 1: Initial contact email.

From: Professor [name redacted] uniqname@umich.edu

Date: Wed, Apr 13, 2022 at 10:36 PM

Subject: Data Collection Research

To: uniqname@umich.edu <uniqname@umich.edu>

Hello [recipient name]

This is an invitation to participate in an Interdisciplinary research project collecting data remotely and earn $250 weekly. It is an adaptable job that requires little to no prior experience not to mention its flexibility to fit into your regular schedule. Provide the information below to indicate interest and you'll receive a follow up email detailing specific.

Full Name:

Cell #:

Alternate email:

Regards

[professor's name]

Title of Professor

Area of Professorship

Departmental Title of the Professor

U-M School where professor teaches

University of Michigan

Example 2: A follow-up email attempting check fraud.

Attached herein is a check . Have both front and back of the check printed out, cut into a check size/shape and at the back of the check endorse by writing your

NAME

ACCOUNT NUMBER

FOR MOBILE DEPOSIT ONLY

 then YOUR SIGNATURE.

Once done, proceed to make a mobile deposit via your bank app on your cell phone..

Kindly send to us a screenshot of the confirmation of deposit when done for record purposes. Thanks

 Note: Print out the front and the back, then endorse it with your name. IT IS FOR MOBILE DEPOSIT ONLY.

[professor's name]

Title of Professor

Area of Professorship

Departmental Title of the Professor

U-M School where professor teaches

University of Michigan
~~~

##### #9 - Finance-aid part-time job, $500/week
**Primary:** Reward | **Also uses:** Authority, Social Proof
**Why:** Big pay for 1-2 hours of work. "Selected" and "verified" flatter the target.

~~~text
Hi,

You have been selected through the School finance aid for a job offer. This is a part time position that will only require 1-2hrs 3 days a week, no work experience or skill is required. You can make $500 weekly without affecting your regular activities and academics.

NOTE: This is for verified selected staff/students/Alumni of  University Of Michigan, international students are also welcome for this opportunity.

To apply/check your eligibility by visiting the Application Portal to apply.

Best Regards & Good Luck.
~~~

##### #13 - Free Yamaha baby grand piano
**Primary:** Reward | **Also uses:** Authority, Social Proof
**Why:** A valuable free item under Office of the President branding. Replies go to a personal Outlook address.

~~~text
Dear Student/Faculty/Staff,

One of our staff at UM Dr. Thomas Baird is downsizing and looking to give away her late dad's piano to a loving home. The Piano is a 2014 Yamaha Baby Grand used like new. You can write to her to indicate your interest on her private email Thomasbaird1881@outlook.com to arrange inspection and delivery or pickup with a moving company.

NB: Please write Dr. Thomas Baird with your personal email for a swift response.

Regards. 

Dr. Santa Ono

Office of the President

University of Michigan
~~~

##### #51 - HR sales job, $300K
**Primary:** Reward | **Also uses:** Authority, Rapport
**Why:** An implausibly large salary with an attached form to fill in.

~~~text
Dear John Doe,

Our company is hiring for a sales position, it's a $300K opportunity.

Ideal candidate must be responsible for:

    Resolving sellers issues, questions, concerns with effective, clear and professional written and oral communication.
    Building platform and business knowledge to better serve sellers.
    Demostrating excellent time-management skills and the ability to work independently.
    Contributing to a positive team environment.

Please fill the attached form. If this offer isn't a good fit for you, feel free to refer a friend! Or someone you know is looking for an exciting new career opportunity.

Learn more on the attached document.

Thanks!

The Human Resources Team

[footer trimmed: confidentiality disclaimer]
~~~

##### #53 - New Year bonus, 3.6% of salary
**Primary:** Reward | **Also uses:** Authority, Urgency, Rapport
**Why:** Unexpected pay that requires returning a form by end of week. The thank-you tone adds warmth.

~~~text
Dear John,

Happy New Year from Contoso Corp!

As we bid farewell to another successful year, we are excited to kick off 2026 with a token of our appreciation for your hard work and dedication.

Exclusive New Year Bonus for all Contoso Corp employees!

You will receive a one-time payment equivalent to 3.6% of your annualized salary.

How to Claim Your Bonus

All employees will need to fill out the attached New Year Bonus scheme document and return it to their manager; you will not receive this payment automatically as it is an out-of-cycle payment.

Deadline

Please return this form by the end of the week.

We are thrilled to be able to offer this bonus as a gesture of our gratitude for your exceptional performance and commitment.

Best Wishes for the New Year,
The Contoso Corp Senior Management Team
~~~

##### #54 - Incoming wire transfer, $257,000
**Primary:** Reward | **Also uses:** Urgency, Authority
**Why:** A large unexpected sum with an attached form to complete. "Secure the funds" adds loss framing.

~~~text
Dear John Doe,

Urgent: Confirmation Needed for Incoming Wire Transfer

You have just received an inbound wire transfer. In order to accept this transfer, confirmation is required within 14 days of 02 Oct 2026.

Transfer Details:

    Amount: USD$257,000
    Expected Arrival: 02 Oct 2026
    Origin: Western Union
    Description: INV5013-002

Immediate Action Required: To ensure the successful processing of this transfer, we need you to confirm the transaction details. This is a mandatory step to secure the funds.

What You Need to Do:

    Open the attached document titled "Wire Transfer Confirmation".
    Complete the wire transfer confirmation form.
    Send the filled-out form back to us as an attachment by replying to this email.

Attached Document: Wire Transfer Confirmation

Deadline: Please complete this process within 14 days of 02 Oct 2026.

We appreciate your prompt response to this matter.

Best Regards,

Pete Rush
Transfer Agent
~~~

##### #56 - Remote-work policy review + iPhone 15 draw
**Primary:** Reward | **Also uses:** Social Proof, Authority
**Why:** A prize for participating, with a QR code to "review the policy." All employees are invited.

~~~text
Dear John,

We're excited to announce that our updated Remote Working Policy is now ready for review. As part of our commitment to transparency and fostering a culture of open feedback, we invite each one of you to go through it and share your thoughts.

Give your feedback and go into the draw to win an iPhone 15!

All employees who participate in the review will be entered into a lucky draw to win an iPhone 15! This is our little way of saying thank you for your valuable time and input.

How to review?
It's simple. Use the attached QR code to access and review the policy.

QR Code

The winner will be announced at the end of next month!
~~~

##### #58 - "Your Benefits Are In!"
**Primary:** Reward | **Also uses:** Curiosity, Rapport
**Why:** Praise ("Nicely done!") plus an unspecified gain, with almost no detail.

~~~text
Your Benefits Are In!

Nicely done! Please take a moment to view your updated company benefits.

To view your new company benefits, click on the link below.

View benefits
~~~

##### #67 - Payment received, $8,580
**Primary:** Reward | **Also uses:** Curiosity, Urgency
**Why:** Unexpected money from an unknown payer, expiring in 14 days.

~~~text
Alice Winterfield paid invoice #0887112 - Winterfield Print LLC

Fri Oct 02 2026 18:15:58 GMT+0530 (India Standard Time)
+$8,580.00
Accept Money
Payment ID: 18764488974VEN
Expires 14 Days
~~~

##### #79 - Uber Eats $100 credit
**Primary:** Reward | **Also uses:** Urgency, Rapport
**Why:** Free credit expiring in 48 hours, with a warm "We've missed you!" and a QR code plus number verification.

~~~text
Uber Eats
We've missed you!
Scan the QR code. Verify your number. $100 will be credited to your Uber Eats account. Cha ching!

Having trouble using the QR code? Use this link.

John, we'd like to help you keep going with Uber Eats, so here's $100 that you can use on any Uber Eats Order!

Enjoy!

PROMO CODE:
EATS$100X154871

You have $100 in Uber Eats credit, but it expires in 48 hours.

The credit will apply automatically at checkout. A minimum spend of $25 per order (excluding fees) is required. Delivery Fee applies (excluding Pickup and Dine-In). A Service Fee also applies to delivery orders and is based on the order value before any discounts. Other fees may apply.

This offer is only for the intended recipient. You'll need to activate the link in the Uber Eats app before placing an order.

Use it before it disappears-your $100 credit expires in 48 hours.

Help Center
Unsubscribe
Terms
Privacy
Email Preferences

Uber BV
Mr. Treublaan 7
1097 DP Amsterdam
Uber.com
~~~

##### #81 - Payoneer payment, $9,100
**Primary:** Reward | **Also uses:** Urgency, Fear
**Why:** The money is "waiting" but will be returned in 24 hours unless the victim verifies. Greed is amplified by loss aversion.

~~~text
You have received a new payment to your Global Payment Service!

Amount     $9100.00
Payment ID     40114047

While you can continue to use your account, your Global Payment Service has not yet been verified and this payment will be returned to the payee unless verification is complete. Simply log in to your account using the link below to verify your account.
This is a time sensitive link. Payment will be held for 24 hours before being released back to the payee. The countdown begins at Fri Oct 02 2026 18:26:23 GMT+0530 (India Standard Time).
Note: If clicking Verify fails, please click the link below:
http://payouts.payoneer.com/Verify/Gateway.aspx?PD=9S1J4f39L9HDSM8Y
Verify
Once verified, your funds will become available instantly.

Sincerely,
Payoneer Verification & Approvals Department

[footer trimmed: confidentiality disclaimer]
~~~

---

### 5. CURIOSITY (8)

##### #17 - "You're invited!" from a compromised U-M account
**Primary:** Curiosity | **Also uses:** Social Proof, Commitment
**Why:** A vague subject from a real colleague's account. It leads to staged password, phone, and passcode pages. Only the subject and description were provided, so this is tentative.

~~~text
Date Sent: Tuesday, December 10, 2024

[This email below leads to a series of web pages/popups designed to steal your login credentials by asking you to:
1. Enter your password
2. Enter your telephone number, and
3. Enter a one-time passcode (which is created when the threat actor uses your password).

Do not respond and do not enter any login information into any forms other than the official U-M Weblogin.]

From: [name redacted] <[uniqname redacted]@umich.edu>

Date: Sun, Dec 8, 2024 at 4:47 PM

Subject: You're invited!

To:  [name redacted] <[uniqname redacted]@umich.edu>
~~~

##### #23 - Punchbowl potluck invite with EXE
**Primary:** Curiosity | **Also uses:** Social Proof, Rapport
**Why:** An unexpected invitation, possibly from a stranger. The "open on a laptop" note steers victims to run the file on a computer.

~~~text
Date Sent: Monday, December 1, 2025

Fake email invitations containing malware have been circulating. The subject line is the same, but with different names. The invitation link will initiate an EXE download. Recipients are then asked to open a malware file named RSVP_HONORINVITE.exe.

Notice the email body encourages you to open the invitation on a laptop or computer.

Subject:    PARTY WITH US, INVITATION FROM BARRY.
Date:    Mon, 1 Dec 2025 04:19:46 -0800
From:    Barry XXXXX <xxxxx@gmail.com>

Punchbowl

RSVP for our upcoming potluck! From Barry XXXXX.

Check the link below for more details.

NOTE: Some people have had trouble opening the invitation on phones or tablets. If the link doesn't load properly, please try opening it on a laptop or desktop computer-it should work just fine there!
~~~

##### #26 - "Notice concerning your Umich"
**Primary:** Curiosity | **Also uses:** Authority, Urgency
**Why:** Deliberately vague "items need your attention," so the victim cannot judge it without clicking.

~~~text
------ Example Phishing Email ------
From: <redacted>
Date: Tue, Aug 12, 2025 at 3:10 PM
Subject: Notice concerning your Umich
To:

You are being contacted by University of Michigan to notify you about your ID.

Currently there are items that require your attention login to umich.edu/um767t7e portal to complete the requested information that is listed on your account.
~~~

##### #33 - Contoso HR org chart
**Primary:** Curiosity | **Also uses:** Authority, Fear
**Why:** Management changes that "impact you" create an information gap about your own job.

~~~text
Contoso Corp HR shared an item
Unknown profile photo

Contoso Corp Human Resources (HR) has shared the following item:
Due to unforseen circumstances, changes have been made to the current management structure. Download the new org chart below to understand how these changes impact you.
Updated Company Org Chart - Contoso Corp.pdf
~~~

##### #37 - FedEx shipment notice
**Primary:** Curiosity | **Also uses:** Social Proof, Authority
**Why:** A delivery you may not remember ordering. Routine-looking business details make it seem legitimate.

~~~text
our shipment was tendered to FedEx Ground

Tracking # 700333134573

Ship date:
Pending
SHIPPING DEPT
WESTAMPTON, NJ 08060
US

Delivery progress bar
In transit

Scheduled delivery:
Pending
John
Doe

Shipment Facts

Tracking number:     700333134573
Invoice number:     3249-A745
Purchase order number:     10235331
Reference:     10235531W
Service type:     FedEx Express Delivery
Packaging type:     Package
Number of pieces:     1
Weight:     16.00 lb.

Preparing for Delivery

To help ensure successful delivery of your shipment, please review the below.

Won't be in?

You may be able to hold your delivery at a convenient FedEx World Service Center or FedEx Office location for pick up. Track your shipment to determine Hold at FedEx location availability.

Please do not respond to this message. This email was sent from an unattended mailbox.

All weights are estimated.
The shipment is scheduled for delivery on or before the scheduled delivery displayed above. FedEx does not determine money-back guarantee or delay claim requests based on the scheduled delivery. Please see the FedEx Service Guide for terms and conditions of service, including the FedEx Money-Back Guarantee, or contact your FedEx customer support representative.
To track the latest status of your shipment, click on the tracking number above.
Thank you for your business.
~~~

##### #40 - Teams message about annual leave
**Primary:** Curiosity | **Also uses:** Social Proof, Urgency
**Why:** A colleague wants to talk about leave. It is vague and personal.

~~~text
Hi John,

Your team-mates are trying to reach you in Microsoft Teams.

Daniel sent a message in chat

Can you please get in touch with me as soon as you get this message? It's about your annual leave. Thanks, Dan.

Message in Teams

Install Microsoft Teams now

iOS
Android

This email was sent from an unmonitored mailbox. Update your email preferences in Teams. Activity > Settings (Gear Icon) > Notifications.

© 2026 Microsoft Corporation, One Microsoft Way, Redmond WA 98052-7329
Read our privacy policy

Microsoft
~~~

##### #52 - Google shared folder "Office Holiday Party"
**Primary:** Curiosity | **Also uses:** Social Proof, Rapport
**Why:** Minimal text, with a relatable folder name as the bait.

~~~text
Google Notifications has invited you to view the following shared folder:
Office Holiday Party
~~~

##### #77b - Anonymous colleague feedback
**Primary:** Curiosity | **Also uses:** Fear, Authority
**Why:** The content is hidden "for privacy," so the victim must click to find out what was said about them.

~~~text
Hi John,

An anonymous colleague has submitted feedback concerning your recent workplace interactions. To protect privacy, the content of this feedback cannot be shared via standard email.

To view the feedback and respond confidentially, click below:

View Confidential Feedback

Your prompt attention demonstrates your commitment to a respectful workplace.

Sincerely,
Automated HR Solutions
Contoso Corp
~~~

---

### 6. SOCIAL PROOF (4)

##### #22 - Housing application follow-up (deposit received)
**Primary:** Social Proof | **Also uses:** Authority, Commitment, Rapport
**Why:** Presents itself as a normal next step in an existing process. It asks for name, U-M ID, and hall preference.

~~~text
Hi there, this email serves to confirm that your enrollment deposit has been received and that you have been sent a link to the Michigan Housing application. If so, we can start processing your room assignment and housing enrollment.

If you have received a link to the housing application form, please respond to this email with your name, U-M ID, preferred residence hall, and, if you have a roommate request, their name and U-M email is required.

With this information, we will be able to direct you to the relevant hall director of your chosen residential hall. The hall director, who will assist you in completing and submitting your housing application, will send you an email.

Please let us know in your response if you have not yet gotten a link to the Housing Application form in addition to the previously required information so that we may look into and address your concerns.

Finally, congratulations on your admission to the University of Michigan.We look forward to your prompt reply to this email.

Regards.
~~~

##### #34 - Jira ticket with requirements.docx
**Primary:** Social Proof | **Also uses:** Urgency, Authority
**Why:** Looks like a routine ticket assigned to you in an everyday workflow tool.

~~~text
JIRA-1536
Implement new system requirements
Issue Type:     Task
Assignee:     John Doe
Reporter:     David Johnson

We need to set up the new system ASAP.

@John Doe please review the complete list of requirements. This is what I have so far:

requirements.docx
Add Comment
~~~

##### #59 - Zoom group "Quarterly All Hands"
**Primary:** Social Proof | **Also uses:** Authority, Rapport
**Why:** A routine team invite implies others have already joined. "Confirm Account" is the credential step.

~~~text
Hello john[.]doe@mybusiness[.]com,

You're invited to join the "Quarterly All Hands" Zoom group. Click the button below to join the group as soon as is convenient.
Confirm Account

Questions? Please visit our Support Center.

Happy Zooming!

+1.888.778.4873
© 2026 Zoom - All Rights Reserved
~~~

##### #71 - Adobe Acrobat Sign, mutual NDA
**Primary:** Social Proof | **Also uses:** Authority, Curiosity
**Why:** A multi-party business workflow ("all parties receive a copy") implies others are already involved.

~~~text
Your signature is required on
Mutual NDA Contoso Corp & Nexora Group 02 Oct 2026
Review and sign

Please review and complete Mutual NDA - Contoso Corp & Nexora Group 02 Oct 2026

After you sign the Mutual NDA - Contoso Corp & Nexora Group 02 Oct 2026, all parties will receive a final PDF copy by email.
Do not forward this email: If you don't want to sign, you can delegate to someone else.
By proceeding, you agree that this agreement may be signed using electronic or handwritten signatures.
To ensure that you continue receiving our emails, please add Adobe Acrobat Sign to your address book or safe list.
Terms of Use | Report Abuse
~~~

---

### 7. COMMITMENT / CONSISTENCY (3)

##### #2 - "Re: Allington Housing Contract"
**Primary:** Commitment | **Also uses:** Authority, Social Proof, Rapport
**Why:** Claims you already signed, inside a fake reply thread. Filling in the next form feels like finishing what you started.

~~~text
From: [redacted] <[redacted]@gmail.com>

Subject: Re: Allington Housing Contract

Hello [redacted],

I hereby acknowledge that you have signed your M Housing Contract, which will now be delivered to the Regents of the University of Michigan.

After you have paid your housing rent, your contract will be made available in your housing application on your school portal when the housing allotment list is released.

In order for the Regent of the University of Michigan to process your M Housing invoice through the Undergraduate Room and Board Rates, I will forward to you the Undergraduate Housing Allotment Form to fill out.

Please ensure you fill out this form as soon as it gets to you to enable the regents to process your contract and implement your housing at Umich Residence Hall.

Congratulations on your admission to the University of Michigan. I look forward to having you on campus.

Professor [redacted]

Hall Director

University of Michigan
~~~

##### #14 - Accommodation offer + text-message "tasks"
**Primary:** Commitment | **Also uses:** Rapport, Reward, Authority
**Why:** Small staged asks (first task, then deposit, then a $719 Zelle payment) build on each other. The fake paycheck "paid first" adds obligation.

~~~text
Example 1: Initial contact email

From: [name redacted] uniqname@umich.edu
Subject: Umich Offer of Accommodations
To: uniqname@umich.edu

Hello

I am Professor [name redacted] of the University of Michigan. I have been instructed by the Office of Student Affairs and Accommodations to guide you in securing suitable accommodation for you here at Umich.

Kindly reply through your private mail; name and and Student ID along with a date and time convenient to you to discuss your information which will determine how we will implement your accommodations.

I will be open to answer any questions you may have regarding your accommodation. After which you will proceed to sign the Accommodation Allotment Registration Form which I will send to you at the end of our discussion via mail.

Thank you for your attention to this matter and I hope to hear from you soon.

[professor's name redacted]
University of Michigan

Example 2: Follow-up text messages in a scam asking a student to make office purchases

Your first task has been sent to your email address. Have you received it yet?

Are you done with your task?

Looks great! Your price details have been received, you will be contacted soon, can you make a Mobile deposit if your paycheck is sent to you?

Your paycheck has been sent. Your paycheck covers your first weekly pay and payment for office supplies. Have you received it yet?

You're to make a payment worth $719 to the zelle information below and send confirmation of payment completed.
~~~

##### #31 - Staged credential flow
**Primary:** Commitment | **Also uses:** Curiosity, Authority
**Why:** Each page asks for one small step. The lure text was not provided, so this is tentative.

~~~text
Date Sent: Wednesday, January 29, 2025

The email below leads to a series of pages designed to steal your login credentials and use the access to make direct deposit changes to your U-M account. The phishing prompts ask you to:

    Select your email provider to sign in to access a file.
    Enter your email address and password.
    Enter your telephone number.
    Enter a Duo SMS passcode, which is sent to your phone in real-time when the threat actor uses your password.

Do not respond and do not enter any login information. In general, do not log into any forms that do not present the official U-M Weblogin page.
~~~

---

### 8. RECIPROCITY / RAPPORT (2)

##### #12 - Fundraiser for a sick student
**Primary:** Rapport | **Also uses:** Social Proof
**Why:** Appeals to compassion and community ("student and a friend") with a donate-or-share request. Sympathy is not one of the 8 levers, so this is the closest fit.

~~~text
Hello

I thought you might be interested in supporting Christina to fight colorectal cancer by simply buying her a coffee

https://www.buymeacoffee.com/[redacted name]

Even a small donation could help reach her fundraising goal. And if you can't make a donation, it would be great if you could share to help spread the word.

She needs our support as a student of Umich and a friend. She is at the last stage of transitioning and all she wish is for her program to be funded to create more awareness for colorectal cancer

Thanks for taking a look!
~~~

##### #57 - HR "Return to Work Plan" survey
**Primary:** Rapport | **Also uses:** Authority, Social Proof, Commitment
**Why:** Flatters the reader ("you have a voice"). A low-stakes survey is the opening ask, delivered as an attached Word doc. (Borderline: could be Commitment.)

~~~text
Hello, John.

The HR department is currently creating a "Return to Work Plan" that will be shared with all employees once finalized. Please, use the attached word document to complete this survey so we can move forward with your ideas in mind.

The purpose of the survey is to hear your thoughts about returning to work, and to understand what concerns you might have so we can appropriately address them.

Thank you in advance for your participation. It's important that you have a voice and we hear your concerns in preparation for returning to our facility.

Best regards,

Human Resources Department
~~~

---

#### Notes on the dataset

- **#20 is a duplicate** of #1 and #2 and is not counted.
- **Numbering gaps:** there are two #18s (labeled 18a and 18b), two #77s (77a and 77b), and no #29 or #41-49.
- **#17 and #31** had no email body text, only descriptions, so their classification is tentative.
- **Borderline calls:** #10, #14, #57, and #69 could reasonably go under a different primary pillar.
- **Footers trimmed:** legal disclaimers on #51, #69, and #81 only.

`FOR PHONE CALL AND SMS TEXTS BREAKDOWN, CHECK OUT THE NEXT GUIDE`
