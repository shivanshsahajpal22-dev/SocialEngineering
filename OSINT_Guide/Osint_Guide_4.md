# HUMint Guide 

**A professional guide to ~~Gossiping~~ human source intelligence**

> HUMINT is intelligence gathered through human interaction rather than
> technical means — the discipline OSINT, SIGINT, and every technical
> bucket in this series eventually feeds into or gets used alongside.
> The tradecraft itself is just structured conversation and applied
> psychology; what makes it legal or illegal is the pretext used and
> what you're trying to extract, not the underlying skill.

**WARNING:** Elicitation itself isn't illegal — it's just skilled
conversation. Pretexting crosses into crime depending on what you're
using it to obtain, and impersonating law enforcement, a government
official, or a licensed professional is illegal virtually everywhere,
independent of context. Authorized red team/CTI/journalism work only.
Know your jurisdiction's specific statutes before fieldwork, not after.

---

## The core cycle

```
1. TARGETING — define what info you need and who plausibly has it
2. SPOTTING — identify specific people with that access
3. ASSESSMENT — evaluate them: what would make them talk, what's
   their actual access level, are they reliable
4. DEVELOPMENT — build rapport over time, usually starting with
   completely innocuous, non-sensitive contact
5. ELICITATION / RECRUITMENT — extract the info, either through
   ongoing casual conversation or, in formal intelligence work,
   an actual "pitch" to become a recruited source
6. HANDLING — for an ongoing relationship, managing communication,
   tasking, and verifying what they tell you is accurate
7. TERMINATION — ending the relationship cleanly when it's no
   longer needed or the risk changes
```

Most practical use sits in steps 3-5 — assessment and elicitation are
where the actual skill lives.

---

## Assessment — what actually motivates someone to talk

**MICE** — the classic framework, taught across intelligence and
corporate security training alike:
```
Money      -> financial incentive, direct or indirect
Ideology   -> shared belief/cause, or grievance against their own org
Compromise -> coercion via something they don't want exposed
Ego        -> flattery, feeling important, being "the expert" someone
              came to specifically
```

**RASCLS** — a newer, broader companion pulled directly from Cialdini's
influence research:
```
Reciprocation -> give something small first, people feel obliged to give back
Authority     -> people defer to perceived expertise/rank
Scarcity      -> "only telling you this" framing increases perceived value
Commitment    -> small agreements now make bigger asks later feel consistent
Liking        -> people share more with someone they've bonded with
Social proof  -> "everyone on your team already told me X" lowers resistance
```

Most real elicitation is quietly running one or two of these against
someone in ordinary conversation, not a dramatic interrogation.

---

## Elicitation techniques — the actual mechanics

None of these look like an interrogation — that's the entire point.

```
Deliberate wrong statement -> say something slightly incorrect about
    their work, people have a strong reflex to correct you with
    accurate (often more detailed than needed) information
Assumed knowledge          -> act like you already know the sensitive
    part, so confirming it feels like agreeing, not disclosing
Bracketing                 -> throw out a broad guess range ("is it
    like a few hundred users or a few thousand?"), lets them "correct"
    you to the real number without ever stating a fact unprompted
Naive/dumb question        -> play uninformed, invite a full
    explanation from someone who enjoys explaining things
Quid pro quo               -> volunteer a real or plausible piece of
    your own info first, triggers reciprocity
Complaint bait             -> criticize something adjacent to their
    job, invites venting that often spills real detail
Productive silence         -> stop talking after their answer; most
    people are uncomfortable with silence and fill it with more
```

---

## Pretexting — the cover story that makes any of this possible

The fabricated identity/reason justifying why you're in the
conversation at all: a fellow conference attendee, a market researcher,
a journalist doing a "harmless" background piece, a new hire asking a
"dumb" question. Same underlying concept as a phishing/vishing pretext,
just delivered face-to-face or by voice instead of email.

---

## Where this is actually legitimate

```
Corporate competitive     -> conference conversations, informational
intelligence                 interviews, talking to former employees
                              or vendors within legal bounds; fine as
                              long as it doesn't involve fraud,
                              impersonation, or inducing an NDA breach

Journalism                -> source cultivation and elicitation are
                              core, protected tradecraft; recording-
                              consent laws still apply per jurisdiction
                              (one-party vs. two-party consent)

Authorized red team        -> vishing a helpdesk, badge-tailgating
engagements                   with a pretext, physical social
                              engineering as a scoped test — legal
                              ONLY because a signed contract and ROE
                              explicitly authorize impersonation for
                              that specific test window

Law enforcement /          -> formally regulated confidential-
intelligence agencies         informant and undercover programs,
                              entirely outside what an individual or
                              private company can lawfully run
```

---

## Where the legal line actually sits

```
Elicitation itself       -> not illegal; asking good questions in
                             normal conversation is just conversation

Pretexting to obtain      -> US: Telephone Records and Privacy
phone records                Protection Act (direct response to the
                             2006 HP pretexting scandal — impersonating
                             board members/journalists to get their
                             phone records) makes this a federal crime

Pretexting to obtain      -> US: Gramm-Leach-Bliley Act covers this
financial records            specifically

Impersonating law         -> illegal virtually everywhere, independent
enforcement/officials/       of what you're trying to extract
licensed professionals

Inducing an NDA breach    -> Economic Espionage Act exposure, for
for trade secrets            both parties involved
```

Same pattern as every other bucket in this series: the tradecraft is
legal and used constantly in benign contexts (sales, negotiation,
therapy, journalism all run the identical techniques) — what turns it
into a crime is the specific pretext and the specific target of the
extraction, not the skill itself. In a red team context, this is
exactly why the signed engagement scope has to explicitly authorize
social engineering as a test vector before any of it runs against a
real target.

---

## Worked example chain

```
Chain A — legitimate competitive-intelligence elicitation at a
conference, no pretext, no impersonation

1. Targeting: need to know a competitor's rough Q3 headcount growth
   in their engineering org
2. Spotting: identify an engineer from that company at an industry
   conference, badge visible, casually approachable at a session break
3. Assessment: Ego + Reciprocation — the person seems happy to talk
   shop, not guarded
4. Development: genuine conversation about the conference talk just
   attended, no fabricated identity, simple truthful small talk
5. Elicitation: bracketing technique — "must be tough hiring at the
   pace you're growing, feels like every team's doubled this year" —
   invites a natural correction/confirmation of actual growth rate
6. No recruitment, no ongoing relationship — single interaction,
   logged as a data point alongside other corroborating signals,
   never treated as a standalone confirmed fact
```

---

Same closing discipline as the rest of this guide series: know which
technique you're using and why it works on the person in front of you,
corroborate anything sensitive against at least one other source before
treating it as fact, and know exactly where the pretext you're using
stops being conversation and starts being a legal liability — before
you're in the room, not after.
