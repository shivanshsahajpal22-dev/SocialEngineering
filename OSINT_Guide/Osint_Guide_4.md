# HUMint Guide

**A professional guide to** ~~**Gossiping**~~ **human source intelligence**

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

### The core cycle

```text
1. TARGETING   — define what info you need and who plausibly has it
2. SPOTTING    — identify specific people with that access
3. ASSESSMENT  — evaluate them: what would make them talk, what's
                 their actual access level, are they reliable
4. DEVELOPMENT — build rapport over time, usually starting with
                 completely innocuous, non-sensitive contact
5. ELICITATION — extract the info through ongoing casual conversation
                 or, in formal intelligence work, an actual pitch
6. HANDLING    — for an ongoing relationship, managing communication,
                 tasking, and verifying accuracy of what they report
7. TERMINATION — ending the relationship cleanly when no longer needed
                 or when the risk profile changes
```

Most practical use sits in steps 3–5 — assessment and elicitation are

where the actual skill lives.

---

### 1. Targeting — defining the requirement before finding the person

Targeting is the discipline of converting a vague intelligence need
into a specific, answerable question with a defined source profile.
Done badly, you end up talking to the wrong people and collecting
noise. Done well, you know exactly what you need, what kind of person
would have it, and what the minimum acceptable corroboration looks like
before you treat anything as fact.

```text
REQUIREMENT DECOMPOSITION

Raw need:     "understand competitor's product roadmap"
              ↓
Broken into   - What features are being built in the next two quarters?
answerable    - What is the rough engineering headcount allocated?
questions:    - Which technical bets have been deprioritized and why?
              - Who is making the roadmap decisions?
              ↓
Source        - Current mid-level engineers (know what's being built)
profile:      - Recently departed employees (know what was and wasn't
                working, less bound by loyalty)
              - Vendors / contractors with active engagement
                (breadth of view, often less guarded)
              - Conference speakers from that org
                (self-selected to talk publicly, topic = access signal)
```

**Requirement prioritization — PIR vs EEI**

```text
Priority Intelligence Requirements (PIR)
  → the top-level questions the operation is trying to answer;
    these drive everything else; if an elicitation doesn't serve
    a PIR it probably shouldn't be happening

Essential Elements of Information (EEI)
  → the specific data points that, when collected, answer a PIR;
    you use these as your internal checklist during development
    and elicitation — you know you're done when EEIs are covered,
    not when the conversation runs out of steam
```

**Common targeting mistakes to avoid**

```text
Collection creep    → drifting from the original PIR because the
                      conversation is going well; results in noise
                      and potential legal exposure for irrelevant
                      information you had no legitimate reason to seek

Assuming access     → the most talkative person in a room is rarely
                      the one with actual access; validate access
                      level before investing development time

Single-source       → treating one confirmed conversation as answered;
dependency            HUMINT sources are almost always incomplete and
                      need corroboration from at least one independent
                      signal before a PIR is considered answered
```

---

### 2. Spotting — finding the right person

Spotting is the process of identifying, from a population of
candidates, the specific individual most likely to have the access you
need and the most favorable conditions for interaction. It is
primarily an OSINT task that precedes any human contact.

**Access mapping**

```text
For any target organization, map the access tiers first:

Tier 1 — Strategic access
  C-suite, board, senior directors
  → know direction and decisions, rarely know operational detail
  → heavily guarded, low approach opportunity

Tier 2 — Operational access (highest value)
  Engineering managers, product leads, senior ICs, procurement
  → know both what is being done and how; usually more approachable
  → best target tier for most collection requirements

Tier 3 — Peripheral access
  Junior employees, interns, admin, support staff
  → often know surprisingly specific operational detail through
    proximity; much lower guard; useful for corroboration and
    for mapping who in Tier 2 to approach
```

**Spotting sources**

```text
LinkedIn          → job titles, tenure, project descriptions;
                    endorsements signal actual skill areas, not just
                    claimed ones; recent job changes signal loosened
                    loyalty and increased willingness to discuss former
                    employer

Conference        → speakers self-select as people willing to discuss
programs            their work publicly; attendee lists (where exposed)
                    give access-mapped candidates before you arrive

GitHub / Stack    → technical staff often expose more operational
Overflow            detail in technical forums than in any professional
                    context; project names, tech stack choices, and
                    architecture questions are intelligence in
                    themselves

Alumni networks   → former employees are the highest-yield HUMINT
                    source category for competitive intelligence;
                    they're often willing to discuss a former employer
                    candidly and are outside any NDA enforcement
                    practical reach for general knowledge

Published papers  → for technical organizations; authorship and
/ patents           acknowledgements map internal team structure
                    and research direction simultaneously
```

**Approach opportunity mapping**

```text
Once you have candidates, map when and where natural contact is
plausible without a fabricated pretext:

High opportunity   → industry conferences, meetups, mutual connection
                     introduction, comment thread on their public post,
                     same professional association
Medium opportunity → cold LinkedIn message with genuine mutual topic,
                     response to their published content
Low opportunity    → cold approach with no natural common ground;
                     requires stronger pretext to be credible
```

---

### 3. Assessment — what actually motivates someone to talk

Assessment is the continuous process of building a profile of a
candidate source — their motivations, reliability, actual access level,
and risk tolerance — before and during development. It is never a
one-time checklist; it updates as contact progresses.

**MICE** — the classic framework

```text
Money      → financial incentive, direct or indirect; more relevant
              in formal intelligence recruitment than in CTI/red team
              contexts, but financial stress is a reliable loosener
              of professional caution in any context

Ideology   → shared belief or cause, or active grievance against
              their own organization; a person who believes their
              employer is doing something wrong will often talk to
              someone they perceive as aligned or as a check on that
              behavior; this includes both genuine whistleblower
              motivation and simple disgruntlement

Compromise → coercion via something they don't want exposed;
              strictly off-limits outside formal intelligence and
              law-enforcement contexts — in any commercial or red
              team context this is blackmail and is a serious crime

Ego        → flattery, feeling important, being the expert someone
              specifically sought out; the most reliable and
              universally applicable motivator; almost no one is
              immune to being genuinely valued
```

**RASCLS** — the influence-science companion

```text
Reciprocation → give something real or useful first; people have
                 a near-automatic obligation to give back; the
                 gift does not need to be large — sharing an
                 insight, making an introduction, or simply paying
                 genuine attention is often enough

Authority     → people defer to perceived expertise, rank, or
                 institutional affiliation; positioning yourself
                 as a credible professional in the same domain
                 lowers resistance significantly; this is why
                 a fellow engineer asking about another team's
                 stack gets a very different answer than a
                 stranger with no apparent context

Scarcity      → "I'm only asking a few people who would actually
                 know this" framing increases perceived value of
                 the interaction; people respond to being selected

Commitment    → small agreements early in a relationship make
                 larger asks later feel consistent with who they've
                 already shown themselves to be; the first "yes"
                 (to a harmless question, to meeting for coffee)
                 is the hardest and most important

Liking        → people share more with someone they've genuinely
                 bonded with; this is not manufactured warmth —
                 genuine curiosity about another person's work and
                 life produces real rapport in a way that performed
                 interest visibly does not

Social proof  → "everyone on your team I've spoken with described
                 it this way" lowers individual resistance by
                 reframing disclosure as already-normal group
                 behavior; use carefully and only when plausible —
                 a false social-proof claim that gets checked
                 destroys the relationship immediately
```

**Reliability assessment — separating access from willingness**

```text
A source who talks a lot but has no real access is worse than no
source — they fill your collection with confident noise.

Assess separately:

Access validity   → can they actually know what they're claiming?
  - Does their role plausibly give them visibility?
  - Are the details internally consistent with how that org works?
  - Do their technical specifics hold up against open-source
    corroboration?

Motivation check  → why are they talking to you?
  - Ego and reciprocation are stable, predictable motivators
  - Grievance is high-yield but introduces bias — they will
    emphasize failures and minimize their own role
  - Ideological alignment can produce motivated fabrication
    (telling you what they think you want to hear)

Reliability       → how do their claims hold up over time?
signals             - Voluntary admission of gaps ("I don't know
                      the exact number but it's in that range")
                      is a strong reliability signal
                    - Consistent detail across multiple conversations
                      without the story improving suspiciously
                    - Willingness to correct earlier statements
                      when new info changes their understanding
```

**Psychological vulnerability indicators**

```text
These are not triggers for exploitation — they are signals that
affect how a source processes information and interaction, and
therefore how you should calibrate your approach:

High need for         → responds well to genuine interest and
recognition             being treated as an expert; do not fake
                        this — they will detect inauthentic flattery
                        faster than most

Active disgruntlement → productive for collection but introduces
                        bias; independently corroborate everything
                        a disgruntled source tells you

Recent life           → job change, professional setback, public
disruption              criticism from their employer; increases
                        openness but also increases emotional
                        volatility; approach with more care

Social isolation      → more willing to engage with an attentive
                        new contact; also more likely to form
                        dependency on the relationship, which
                        complicates termination later

Excessive openness    → a source who tells you things they clearly
(red flag)              shouldn't, early, without much prompting,
                        is either a fabricator, a dangle
                        (intelligence term for a deliberately
                        placed source), or has access that is
                        much lower than they present; treat
                        everything they provide as unconfirmed
                        until independently corroborated
```

**Updating assessment continuously**

```text
Assessment is not done once before first contact. Update after
every interaction:

- Did they know what they claimed to know?
- Did new open-source data corroborate or contradict what they said?
- Has their mood, situation, or access level changed?
- Are they becoming more guarded? (could signal they discussed
  the conversation with a colleague and were cautioned)
- Are they becoming less guarded than expected?
  (reason to probe whether they actually have the access
  they're presenting, or are performing access they don't have)
```

---

### 4. Development — building the relationship that makes elicitation possible

Development is the stage between first contact and active collection.
Its purpose is to move a candidate source from stranger to trusted
contact — someone who will answer questions candidly because the
relationship has established enough trust and mutual interest to make
that feel natural rather than suspicious.

**The baseline rule: development must be able to stand on its own**

```text
Every development interaction should have independent value to
the source. If the only reason you're having the conversation
is to eventually extract something, that will eventually show —
people detect instrumentalization over time even when they can't
name it. Genuine professional interest, useful information sharing,
and real reciprocity are not just tactics; they are what makes a
relationship durable enough to use.
```

**Development phases**

```text
Phase 1 — Innocuous contact
  First interactions contain nothing sensitive on either side.
  Goals: establish that you exist, are credible, and are worth
  engaging with again. The only ask at this stage is another
  conversation.

  Practical form: conference small talk, a LinkedIn comment on
  their post, a question about their published work that demonstrates
  you've actually read it, a mutual introduction from a trusted
  contact in common.

Phase 2 — Rapport building
  Shift from professional to slightly personal — shared frustrations
  about the industry, mutual professional interests, occasional
  non-work connection. This is where liking and reciprocation
  compound over time.

  Practical form: follow-up coffee or call, continued engagement
  with their public work, reciprocal sharing of genuinely useful
  information or introductions, remembering and referencing
  detail from earlier conversations.

Phase 3 — Access validation
  Probe, through natural conversation, whether the person's
  actual access matches the access tier you assessed them at.
  This is not interrogation — it's asking questions slightly
  adjacent to their role and seeing if they engage with specific
  operational detail or stay at a vague, general level.

  A source who gives consistently vague answers about things they
  should know in detail either doesn't have the access they presented
  or has been cautioned to be careful with this contact specifically.

Phase 4 — First substantive ask
  The first real question — framed as natural curiosity, not a
  formal request. The size of the ask should be calibrated to the
  depth of the relationship; asking too much too early breaks
  the development investment.

  Practically: frame as a problem you're trying to understand
  rather than a fact you're trying to extract. "I'm trying to
  figure out how organizations like yours typically handle X" is
  a much lower-resistance entry than "can you tell me how your
  team handles X."
```

**Rapport mechanics — what actually builds trust**

```text
Active listening     → most people are listened to very little
                       in professional contexts; someone who
                       demonstrates they actually heard what was
                       said (referencing earlier detail, asking
                       follow-up questions that only make sense if
                       you were paying attention) stands out
                       sharply and is trusted more quickly

Mirroring            → subtly matching communication style, pace,
                       and vocabulary reduces perceived distance
                       without being detectable; do not mirror
                       affect or emotion — only style and pace

Labeling             → naming what someone appears to be feeling
                       or thinking ("it sounds like that project
                       was more complicated than it looked from
                       the outside") without evaluating it creates
                       strong rapport; people feel understood

Calibrated questions → open questions that invite the source to
                       define terms and share context rather than
                       confirm or deny: "how does that typically
                       work?" rather than "does it work like X?"

Minimal              → brief, neutral responses ("that makes
encouragers            sense," "right," "go on") that signal
                       continued interest without redirecting
                       or interrupting; keeps the source talking
                       in their own direction
```

**Development pace failures**

```text
Too fast   → asking for sensitive information before the relationship
              justifies it; source becomes guarded and the development
              investment is lost; very difficult to recover

Too slow   → development that never progresses to collection is
              just relationship maintenance with no intelligence
              value; also increases the source's personal
              investment in the relationship, which complicates
              eventual termination

Inconsistent → irregular contact after heavy early investment
contact        signals to the source that they were being used;
               if operational requirements require a pause,
               low-effort maintenance contact
               (sharing an article, a brief check-in) preserves
               the relationship at low cost
```

---

### 5. Elicitation — the actual mechanics of extraction

None of these look like an interrogation — that is the entire point.

**Core techniques**

```text
Deliberate wrong    → state something slightly incorrect about
statement             their work or domain; people have a near-
                      automatic reflex to correct inaccuracy,
                      and the correction is usually more detailed
                      and specific than any direct answer would
                      have been; works especially well against
                      technical experts and people with high
                      need for accuracy

Assumed knowledge   → speak as though you already know the
                      sensitive piece, so confirming it feels
                      like agreeing rather than disclosing;
                      "given that you've already moved to X
                      architecture, how are you handling Y?"
                      is far lower resistance than asking
                      whether they use X at all

Bracketing          → offer a range that bounds the real answer
                      and invite correction: "is it more like
                      a team of ten or closer to fifty?";
                      the source corrects you to the real
                      number without ever having decided
                      to disclose a fact unprompted

Naive question      → play genuinely uninformed and invite a
                      full explanation; works reliably against
                      people who enjoy explaining things (a
                      large percentage of technical professionals);
                      the expert explaining basics often
                      explains more than basics

Quid pro quo        → share a real or plausible piece of your
                      own information first; triggers reciprocity
                      reliably; the information you share should
                      be genuinely useful to them, not obviously
                      low-value trading-down

Complaint bait      → criticize something adjacent to their
                      work — the industry, a competitor, a
                      common tool; invites venting, and venting
                      about professional frustrations almost
                      always spills operational specifics

Productive silence  → stop talking immediately after their
                      answer and hold the silence; most people
                      are deeply uncomfortable with conversational
                      silence and fill it unprompted; what they
                      choose to fill it with is often the most
                      revealing part of the exchange

Flattery toward     → explicitly position them as the rare
expertise             person who actually understands this
                      domain; ego motivation operationalized
                      directly; the expert who feels recognized
                      gives a longer, more detailed answer

Third-party         → "a colleague of mine was trying to
attribution           understand X" distances the question from
                      you personally, reduces the sense that
                      you're extracting something, and often
                      lowers resistance to specifics

Topic bridging      → approach the sensitive topic indirectly
                      through a series of increasingly proximate
                      questions; each step feels natural coming
                      from the last, and the source arrives at
                      the sensitive territory without a single
                      jarring moment of "why are you asking
                      that?"
```

**Sequencing elicitation in a single conversation**

```text
Opening             → establish or reestablish rapport;
                      something genuine and non-instrumental;
                      reference a previous conversation detail
                      if one exists

Warm-up questions   → ask about things you already know the
                      answer to; this lets you calibrate
                      whether they're being accurate and
                      establishes a conversational rhythm
                      before the substantive questions begin

Core collection     → the actual EEI questions, disguised as
                      natural conversation using the techniques
                      above; never ask more than two or three
                      substantive questions in a single
                      conversation — more than that pattern-
                      matches to interrogation

Graceful exit       → end before the source feels drained or
                      suspicious; leave them feeling the
                      interaction was valuable for them, not
                      just for you; this is what makes the
                      next interaction possible
```

**Reading real-time resistance signals**

```text
Answer shortens     → questions are landing too directly; pivot
significantly         to an adjacent topic and return later

Topic change        → the source redirects away from an area;
by source             note it and do not push; come back via
                      a different approach in a later conversation

Verification        → "why do you ask?" or "who else have you
question              been talking to?"; have a prepared,
                      credible answer ready before any
                      conversation begins; a hesitant or
                      inconsistent answer here ends the
                      relationship and potentially the operation

Increased           → sudden precision about what they do and
qualification         don't know; often signals they've become
                      aware they may have said more than intended
                      in a previous exchange; back off and let
                      the relationship rest

Increased           → the opposite problem; a source who
openness              becomes dramatically more forthcoming
                      should be assessed for access validity
                      and for whether they're a dangle
```

---

### 6. Handling — managing an ongoing source relationship

Handling applies when the source is not a single interaction but an
ongoing relationship that will be contacted multiple times for
continued collection. It introduces management complexity that
single-elicitation contacts do not have.

**Communication security and contact protocols**

```text
Establish from the start how contact will happen and stick to it.
Changing communication channels mid-relationship is a reliability
signal to the source that something has changed operationally —
and an attentive source will notice.

Channel selection should be driven by the source's normal behavior,
not by what is most convenient for you. A source who never uses
Signal in their personal life will behave differently — more
formally, more cautiously — if you insist on it.

Frequency calibration: enough to maintain the relationship as a
living thing, not so much that contact itself becomes a data point
that could be noticed by the source's colleagues or employer.
```

**Tasking — what to ask for and in what order**

```text
Never give a source a list of questions — this is the single
fastest way to convert a natural conversation into something
that feels like a debrief, and a source who feels they're being
debriefed will either become guarded or begin performing.

Instead: identify the one or two EEIs most important for the
current collection period. Introduce them as natural topics,
not requests. Save other EEIs for subsequent contacts.

Resist the temptation to over-collect from a productive source.
A source who is giving you high-quality information is a long-
term asset; exhausting them in a few intense sessions destroys
that. Pace collection to the depth of information and the
source's continued willingness to engage.
```

**Verification — never take a single source at face value**

```text
Every substantive piece of information from a source should be
treated as a lead to be confirmed, not a fact to be reported.

Verification methods:
  Cross-source         → does a second, independent source
                         describe the same situation the
                         same way?
  Open-source check    → does publicly available information
                         (job postings, press releases, filings,
                         technical forums) corroborate or
                         contradict the claim?
  Internal consistency → does what they told you today match
                         what they told you three months ago?
                         Fabricators and low-access sources
                         gradually diverge from themselves.
  Technical plausibility → for technical claims, does what they
                           describe actually work the way they
                           say it does? Subject-matter expertise
                           on your side is the fastest fabrication
                           detector in technical domains.

Report handling:
  Grade every piece of information on two axes independently:
  - Source reliability (A-F: known reliable → unknown → unreliable)
  - Information credibility (1-6: confirmed → probably true →
    unconfirmed → doubtful → improbable → cannot be judged)
  The NATO intelligence grading standard uses exactly this matrix.
  A single piece from an A-source can still be graded 3 if it
  cannot be independently confirmed.
```

**Welfare and relationship maintenance**

```text
A source who feels used will eventually stop being one, either
by withdrawing or by becoming unreliable in ways that are hard
to detect. Maintenance means:

  - Continuing to provide genuine value to them, not just
    extracting; the relationship should feel reciprocal
  - Checking in without always having a collection purpose;
    a contact who only appears when they want something is
    recognizable as such
  - Noticing when their situation has changed (new role,
    professional difficulty, personal stress) and responding
    to that as a human interaction, not just assessing it
    for collection implications
```

---

### 7. Termination — ending the relationship cleanly

Termination is the planned, deliberate end of an active source
relationship. It is the most neglected phase of the cycle and the
source of most long-term problems when done poorly.

**Why termination matters**

```text
A source who feels abruptly dropped after a period of close
contact is a risk:

  - They may seek to understand why contact stopped, asking
    questions that expose the operation retroactively
  - They may feel betrayed and actively discuss the
    relationship with colleagues or, in extreme cases,
    with an adversary
  - They may contact you at an operationally inconvenient
    time expecting a relationship that no longer exists
    from your side
```

**Reasons for termination**

```text
Collection complete   → the PIR is answered; no further
                        intelligence requirement justifies
                        the operational overhead

Access change         → the source has changed roles and
                        no longer has access to what you
                        needed; continuing the relationship
                        has cost without collection benefit

Reliability failure   → the source has been found to be
                        fabricating, has dramatically
                        inconsistent accounts, or open-source
                        information consistently contradicts
                        their reporting

Security concern      → evidence that the source has
                        discussed your contact with a
                        colleague, that the relationship
                        has come to a third party's attention,
                        or that the source is themselves a
                        dangle; terminate cleanly and quickly

Risk change           → the legal or operational risk of
                        the relationship has increased beyond
                        what the collection value justifies
```

**How to terminate cleanly**

```text
Gradual reduction     → the lowest-risk method when time
                        permits; reduce contact frequency
                        and intensity over several cycles
                        rather than going dark; the source
                        perceives the relationship winding
                        down naturally rather than being
                        cut off

Natural transition    → engineer a plausible real-world
                        reason for reduced contact: a role
                        change, a project ending, a geographic
                        move; this requires the transition
                        to be believable given the pretext
                        used throughout the relationship

Direct close          → for formal intelligence relationships
                        (less common in CTI/red team contexts);
                        explicit conversation that the
                        relationship is ending, often with
                        a cover story for why; cleanest for
                        the source's understanding but requires
                        the most operational maturity to
                        execute without creating suspicion

Going dark            → simply stopping contact; highest
                        risk of the four options; justified
                        only when a security concern requires
                        immediate termination and gradual
                        reduction is not operationally
                        possible; if you go dark, expect
                        the source to attempt re-contact
                        and have a plan for how that
                        attempt will be handled
```

**Post-termination**

```text
- Document the relationship, collection yield, and termination
  reason before any operational memory fades
- If the source's access or situation changes in the future,
  the documented relationship history is the starting point
  for re-engagement if that ever becomes relevant
- Monitor for any signs that the source has discussed the
  relationship post-termination; most won't, but the ones
  who feel they were used poorly sometimes do
```

---

### Pretexting — the cover story that makes any of this possible

The fabricated identity or reason justifying why you're in the
conversation at all: a fellow conference attendee, a market researcher,
a journalist doing a background piece, a new hire asking a naive
question. Same underlying concept as a phishing or vishing pretext,
delivered face-to-face or by voice instead of email.

**Pretext design principles**

```text
Proximity to truth    → the best pretext is one that requires
                        the fewest fabricated details; every
                        fabricated detail is a potential failure
                        point; a pretext grounded in your real
                        professional background is far more
                        durable under questioning than a
                        constructed persona with no real basis

Consistency          → every detail of the pretext must be
                        internally consistent across all
                        interactions; a source who compares
                        notes with a colleague, or who simply
                        has a good memory, will catch
                        inconsistencies that build over time

Depth preparation    → prepare for questions two levels deeper
                        than you expect to be asked; a market
                        researcher pretext requires you to know
                        what research firm you're notionally
                        from, who you work with there, what
                        recent work you've done, and how you
                        found this particular source; a pretext
                        that collapses at the second follow-up
                        question is not a pretext, it's a story

Graceful handling    → know in advance how you will respond to
of exposure            "actually, I looked you up and couldn't
                        find you" or "I mentioned you to X and
                        they said they didn't know you"; have
                        a calibrated response ready; a pretext
                        that requires the conversation to never
                        be checked is operationally fragile
```

---

### Where this is actually legitimate

```text
Corporate            → conference conversations, informational
competitive            interviews, talking to former employees or
intelligence           vendors within legal bounds; fine as long
                       as it doesn't involve fraud, impersonation,
                       or inducing an NDA breach

Journalism           → source cultivation and elicitation are
                       core, protected tradecraft; recording-
                       consent laws still apply per jurisdiction
                       (one-party vs. two-party consent)

Authorized red       → vishing a helpdesk, badge-tailgating
team engagements       with a pretext, physical social engineering
                       as a scoped test — legal ONLY because a
                       signed contract and ROE explicitly authorize
                       impersonation for that specific test window

Law enforcement /    → formally regulated confidential-informant
intelligence           and undercover programs, entirely outside
agencies               what an individual or private company can
                       lawfully run
```

---

### Where the legal line actually sits

```text
Elicitation itself      → not illegal; asking good questions in
                           normal conversation is just conversation

Pretexting to obtain    → US: Telephone Records and Privacy
phone records             Protection Act makes this a federal crime

Pretexting to obtain    → US: Gramm-Leach-Bliley Act covers this
financial records         specifically

Impersonating law       → illegal virtually everywhere, independent
enforcement / officials   of what you're trying to extract

Inducing an NDA         → Economic Espionage Act exposure for both
breach for trade          parties involved
secrets
```

---

### Worked example chains

**Chain A — competitive intelligence at a conference**

```text
1. Targeting:  need to understand competitor's engineering headcount
               growth in Q3; decomposed into two EEIs: total team
               size and whether any new specialty teams were formed

2. Spotting:   identified a senior engineer from the target org at
               an industry conference via the speaker list; public
               talk topic directly maps to the access tier needed

3. Assessment: Ego (speaker, comfortable presenting publicly) +
               Reciprocation; no active disgruntlement signals;
               access tier confirmed as Tier 2 operational

4. Development: attended their talk, referenced a specific technical
                point during the post-talk Q&A (demonstrated genuine
                engagement); introduced during the break by a mutual
                contact; brief follow-up conversation about the talk,
                no sensitive content; exchanged contact details

5. Elicitation: in a follow-up call two weeks later, led with a
                genuine technical discussion; introduced bracketing:
                "must be an interesting time to be scaling — feels
                like every org in this space has at least doubled
                their backend team in the last year"; source corrected
                the framing with specific context about their own
                growth pattern; EEI 1 answered without a direct
                question; topic-bridged to team structure naturally

6. No recruitment, no ongoing handling — single-elicitation
   collection; data logged alongside open-source corroboration;
   not treated as confirmed until cross-referenced against
   two independent signals
```

**Chain B — red team vishing engagement**

```text
1. Targeting:  determine whether a helpdesk operator will reset
               an MFA credential without following the verification
               protocol defined in the client's own policy

2. Spotting:   no specific individual targeted; the population is
               the helpdesk team as a whole; the specific operator
               encountered is the sample

3. Assessment: authority pretext (IT security team calling about
               a reported incident) maps cleanly to how helpdesk
               operators are trained to prioritize inbound requests

4. Pretext:    IT security team member responding to an alert about
               suspicious activity on the target account; framing
               creates time pressure and authority simultaneously
               within the scope of the signed engagement letter

5. Elicitation: urgency framing ("we're seeing something on our
                end and need to verify the account state before
                it escalates") combined with assumed knowledge
                ("the ticket shows the MFA device was flagged —
                has the user reported anything?") to reduce the
                operator's sense that they are being asked to do
                something unusual

6. Termination: call ends naturally once the test objective is
                met or failed; full documentation of technique,
                response, and outcome for the client report
```

---

### Core discipline that runs across all seven phases

```text
Know which technique you're running and why it works on this
specific person — not just "I'm doing HUMINT." Technique selection
without target assessment is just noise.

Corroborate everything sensitive against at least one independent
source before treating it as fact — HUMINT is leads, not verdicts.

Know exactly where your pretext stops being conversation and
starts being a legal liability — before you're in the room, not
after.

Keep records. Not for surveillance — for operational continuity;
the details you remember accurately six months later without notes
are the details that didn't matter.
```
