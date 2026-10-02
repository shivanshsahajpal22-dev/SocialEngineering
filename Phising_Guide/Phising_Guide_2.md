# PHISHING: PRETEXT PSYCHOLOGY AND GUIDE Part 2 

## Examples - Levers mapping (SMS)

### SMS Examples

### Smishing Classification by Psychological Lever

**Method:** Same convention as the email section. Each message gets a **primary lever** (what it leads with) and **supporting levers** (what reinforces it). Real smishing stacks 2-3 levers inside 160 characters, so the primary label is a judgment call. From the raw collection I kept **20 per lever (160 total)**, picking the clearest example of each lever and avoiding near-duplicate templates.

**Cleanup applied:** phone numbers masked (last four digits shown as xxxx), URLs and email addresses defanged (`hxxp`, `[.]`), encoding errors fixed (Â£ shown as £), original spelling and abbreviations kept as sent.

#### Summary

| Pillar | Count | SMS #s |
|---|---|---|
| Urgency | 20 | S-01 to S-20 |
| Authority | 20 | S-21 to S-40 |
| Fear / Loss Aversion | 20 | S-41 to S-60 |
| Reward / Greed | 20 | S-61 to S-80 |
| Curiosity | 20 | S-81 to S-100 |
| Social Proof | 20 | S-101 to S-120 |
| Commitment / Consistency | 20 | S-121 to S-140 |
| Reciprocity / Rapport | 20 | S-141 to S-160 |
| **Total** | **160** | |

---

### 1. URGENCY (20)

**How it shows up in SMS:** Short fake deadlines ("valid 12hrs"), calendar cutoffs, and "2nd / final attempt" language. With 160 characters to work with, a bare deadline is the cheapest lever to add.

##### S-01 - "2nd attempt" with a dated deadline
**Primary:** Urgency | **Also uses:** Reward, Authority
**Why:** A calendar deadline ("b4 050703") plus "2nd attempt" says the window is closing. The per-minute cost is buried at the end.

~~~text
+44907151xxxx URGENT! This is the 2nd attempt to contact U!U have WON £1250 CALL 0907151xxxx b4 050703 T&CsBCM4235WC1N3XX. callcost 150ppm mobilesvary. max£7. 50
~~~

##### S-02 - "Still unclaimed" award
**Primary:** Urgency | **Also uses:** Reward, Fear
**Why:** "Still unclaimed" treats the prize as already yours and about to lapse. The closing date and claim code make it feel like a real record.

~~~text
Urgent Ur £500 guaranteed award is still unclaimed! Call 0906636xxxx NOW closingdate04/09/02 claimcode M39M51 £1.50pmmorefrommobile2Bremoved-MobyPOBox734LS27YF
~~~

##### S-03 - "Before the lines close"
**Primary:** Urgency | **Also uses:** Reward, Authority
**Why:** Invents a cutoff with no real mechanism behind it. Phone lines do not "close" for a prize draw.

~~~text
You have been specially selected to receive a 2000 pound award! Call 0871240xxxx BEFORE the lines close. Cost 10ppm. 16+. T&Cs apply. AG Promo
~~~

##### S-04 - Free flights, date cutoff
**Primary:** Urgency | **Also uses:** Reward, Social Proof
**Why:** Doubled "Urgent," a date cutoff, and a take-a-friend sweetener stack three nudges in one line.

~~~text
Urgent Urgent! We have 800 FREE flights to Europe to give away, call B4 10th Sept & take a friend 4 FREE. Call now to claim on 0905000xxxx. BA128NNFWFLY150ppm
~~~

##### S-05 - 12-hour claim window
**Primary:** Urgency | **Also uses:** Reward, Authority
**Why:** A 12-hour window is too short to research the sender, which is the point. The claim code adds an official feel.

~~~text
URGENT! We are trying to contact U. Todays draw shows that you have won a £800 prize GUARANTEED. Call 0905000xxxx from land line. Claim C52. Valid12hrs only
~~~

##### S-06 - "Last chance" vouchers
**Primary:** Urgency | **Also uses:** Reward, Commitment
**Why:** "Last chance" plus "now" compresses the decision. The recurring £3.00 charge only appears in the small print.

~~~text
Last chance 2 claim ur £150 worth of discount vouchers-Text YES to 85023 now!SavaMob-member offers mobile T Cs 0871789xxxx. £3.00 Sub. 16 . Remove txt X or STOP
~~~

##### S-07 - Prize "from YESTERDAY"
**Primary:** Urgency | **Also uses:** Reward, Fear
**Why:** The prize has supposedly been waiting since yesterday and this is a second attempt, so any delay feels like it has already cost you something.

~~~text
URGENT This is our 2nd attempt to contact U. Your £900 prize from YESTERDAY is still awaiting collection. To claim CALL NOW 0906170xxxx. ACL03530150PM
~~~

##### S-08 - "Urgent message waiting"
**Primary:** Urgency | **Also uses:** Curiosity, Authority
**Why:** No prize and no threat, just an "urgent message" to handle "immediately." Not knowing what it says makes the call tempting.

~~~text
Please CALL 0871240xxxx immediately as there is an urgent message waiting for you
~~~

##### S-09 - "Final contact attempt," expiry date
**Primary:** Urgency | **Also uses:** Authority, Curiosity
**Why:** "Final contact attempt" and an expiry date imply a formal claims process about to close.

~~~text
IMPORTANT MESSAGE. This is a final contact attempt. You have important messages waiting out our customer claims dept. Expires 13/4/04. Call 0871750xxxx NOW!
~~~

##### S-10 - "Not to lose out"
**Primary:** Urgency | **Also uses:** Fear, Reward
**Why:** Pairs "URGENT collection" with "not to lose out," tying the deadline directly to loss.

~~~text
complimentary 4 STAR Ibiza Holiday or £10,000 cash needs your URGENT collection. 0906636xxxx NOW from Landline not to lose out! Box434SK38WP150PPM18+
~~~

##### S-11 - Radio presenter, prize "transferred to someone else"
**Primary:** Urgency | **Also uses:** Authority, Rapport, Fear
**Why:** A named presenter and a calendar deadline, with the threat the prize will be "transferred to someone else," turn a gift into something you can lose.

~~~text
Hi, this is Mandy Sullivan calling from HOTMIX FM...you are chosen to receive £5000.00 in our Easter Prize draw.....Please telephone 0904194xxxx to claim before 29/03/05 or your prize will be transferred to someone else....
~~~

##### S-12 - Unredeemed points, expiry date
**Primary:** Urgency | **Also uses:** Authority, Reward
**Why:** An account-style notice with an expiry date pushes you to act before the points vanish.

~~~text
Your 2004 account for 07XXXXXXXXX shows 786 unredeemed points. To claim call 0871918xxxx Identifier code: XXXXX Expires 26.03.05
~~~

##### S-13 - "Offer ends today"
**Primary:** Urgency | **Also uses:** Rapport, Authority
**Why:** A named agent and "offer ends today" turn a routine upgrade into a same-day decision.

~~~text
Please call Amanda with regard to renewing or upgrading your current T-Mobile handset free of charge. Offer ends today. Tel 0845 021 xxxx subject to T's and C's
~~~

##### S-14 - "Final try," dated award
**Primary:** Urgency | **Also uses:** Reward, Authority
**Why:** "Final try" with a stated award date reads like a last notice. The landline instruction steers the call to a costly line.

~~~text
URGENT! Your Mobile No. was awarded £2000 Bonus Caller Prize on 5/9/03 This is our final try to contact U! Call from Landline 0906401xxxx BOX42WR29C, 150PPM
~~~

##### S-15 - Repeated "NOW"
**Primary:** Urgency | **Also uses:** Reward
**Why:** "2nd time we have tried" plus a capitalised NOW leaves no room to wait. "Only 10p per min" downplays the cost.

~~~text
This is the 2nd time we have tried 2 contact u. U have won the 750 Pound prize. 2 claim is easy, call 0871872xxxx NOW! Only 10p per min. BT-national-rate
~~~

##### S-16 - "ASAP"
**Primary:** Urgency | **Also uses:** Authority, Reward
**Why:** "ASAP" plus the second-attempt claim. The masked number mimics a personalised notice.

~~~text
URGENT! Your mobile No *********** WON a £2,000 Bonus Caller Prize on 02/06/03! This is the 2nd attempt to reach YOU! Call 0906636xxxx ASAP! BOX97N7QP, 150ppm
~~~

##### S-17 - "Last weekend's draw," CALL NOW
**Primary:** Urgency | **Also uses:** Reward, Authority
**Why:** Refers to a draw you never entered, then adds "CALL NOW" so you do not stop to check.

~~~text
URGENT! Last weekend's draw shows that you have won £1000 cash or a Spanish holiday! CALL NOW 0905000xxxx to claim. T&C: RSTM, SW7 3SS. 150ppm
~~~

##### S-18 - Fake "Forwarded from" header
**Primary:** Urgency | **Also uses:** Curiosity, Authority
**Why:** A fake forwarding header looks like a system relay. "Immediately" plus "urgent message" do the work.

~~~text
<Forwarded from 44871240xxxx>Please CALL 0871240xxxx immediately as there is an urgent message waiting for you.
~~~

##### S-19 - Personal name, "final notice"
**Primary:** Urgency | **Also uses:** Authority, Fear, Reward
**Why:** A personal name and "final notice" mirror a debt or collection letter.

~~~text
Dear Dave this is your final notice to collect your 4* Tenerife Holiday or #5000 CASH award! Call 0906174xxxx from landline. TCs SAE Box326 CW25WX 150ppm
~~~

##### S-20 - Interflora "before Midnight tomorrow"
**Primary:** Urgency | **Also uses:** Authority
**Why:** This is ordinary marketing rather than a scam, but it is a clean example of the deadline lever. A real deadline can be checked on the retailer's own site; a fake one cannot.

~~~text
INTERFLORA - It's not too late to order Interflora flowers for christmas call 0800 505xxx to place your order before Midnight tomorrow.
~~~

---

### 2. AUTHORITY (20)

**How it shows up in SMS:** The sender poses as a carrier (O2, Vodafone, Orange, T-Mobile), a "customer service" department, a delivery firm, or a corporate brand. With no logo or domain to check, the brand name and formal tone carry the whole claim.

##### S-21 - Failed-delivery "customer service announcement"
**Primary:** Authority | **Also uses:** Curiosity, Fear
**Why:** A delivery-failure notice from an unnamed "customer service," with a reference number for a paperwork feel. The same shape still drives parcel-scam texts today.

~~~text
Customer service announcement. We recently tried to make a delivery to you but were unable to do so, please call 0709029xxxx to re-schedule. Ref:9307622
~~~

##### S-22 - "New Years delivery waiting"
**Primary:** Authority | **Also uses:** Curiosity, Urgency
**Why:** A delivery you never ordered, announced by "customer service" with no company name.

~~~text
Customer service annoncement. You have a New Years delivery waiting for you. Please call 0704674xxxx now to arrange delivery
~~~

##### S-23 - Generic "important announcement"
**Primary:** Authority | **Also uses:** Curiosity, Urgency
**Why:** An official-sounding "announcement" with a freephone number says nothing about content, so the tone does all the work.

~~~text
You have an important customer service announcement. Call FREEPHONE 0800 542 xxxx now!
~~~

##### S-24 - "Customer service representative," office hours
**Primary:** Authority | **Also uses:** Reward, Urgency
**Why:** Stated office hours and a "representative" read like a call-centre process before the prize pitch lands.

~~~text
Please call our customer service representative on FREEPHONE 0808 145 xxxx between 9am-11pm as you have WON a guaranteed £1000 cash or £5000 prize!
~~~

##### S-25 - Formal letter-style bonus prize
**Primary:** Authority | **Also uses:** Reward
**Why:** Letter-style phrasing ("I am pleased to advise you," "following recent review") imitates a bank or carrier notice.

~~~text
As a valued customer, I am pleased to advise you that following recent review of your Mob No. you are awarded with a £1500 Bonus Prize, call 0906636xxxx
~~~

##### S-26 - Vodafone "numbers ending in"
**Primary:** Authority | **Also uses:** Reward, Social Proof
**Why:** A carrier brand plus "numbers ending in" and a claim code looks like a system-run draw.

~~~text
Todays Voda numbers ending 7548 are selected to receive a $350 award. If you have a match please call 0871230xxxx quoting claim code 4041 standard rates app
~~~

##### S-27 - "Our computer has picked YOU"
**Primary:** Authority | **Also uses:** Reward
**Why:** Names the carrier and says a computer made the pick, so the selection feels automated and impartial.

~~~text
YOU HAVE WON! As a valued Vodafone customer our computer has picked YOU to win a £150 prize. To collect is easy. Just call 0906174xxxx
~~~

##### S-28 - T-Mobile loyalty upgrade
**Primary:** Authority | **Also uses:** Reward, Urgency
**Why:** Loyalty framing from the carrier. The "Offer ends" line adds a deadline.

~~~text
T-Mobile customer you may now claim your FREE CAMERA PHONE upgrade & a pay & go sim card for your loyalty. Call on 0845 021 xxxx.Offer ends 28thFeb.T&C's apply
~~~

##### S-29 - Same template, different brand
**Primary:** Authority | **Also uses:** Reward, Urgency
**Why:** The same carrier-loyalty template with a different brand name ("UpgrdCentre Orange") shows how freely the brand slot is swapped.

~~~text
UpgrdCentre Orange customer, you may now claim your FREE CAMERA PHONE upgrade for your loyalty. Call now on 0207 153 xxxx. Offer ends 26th July. T&C's apply. Opt-out available
~~~

##### S-30 - Carrier reminder asking for address
**Primary:** Authority | **Also uses:** Reward
**Why:** A carrier-style "reminder" that asks for name, house number, and postcode. This is data harvesting disguised as account admin, and the closest match here to modern phishing.

~~~text
REMINDER FROM O2: To get 2.50 pounds free call credit and details of great offers pls reply 2 this text with your valid name, house no and postcode
~~~

##### S-31 - "Specially trained advisors"
**Primary:** Authority | **Also uses:** Reward, Rapport
**Why:** "Specially trained advisors" and a short dial code sound like a real carrier service.

~~~text
Welcome to Select, an O2 service with added benefits. You can now call our specially trained advisors FREE from your mobile by dialling 402.
~~~

##### S-32 - "Account Statement" with identifier code
**Primary:** Authority | **Also uses:** Reward, Urgency
**Why:** A statement-style message with an "Identifier Code" feels like a traceable bank or loyalty mailing.

~~~text
PRIVATE! Your 2004 Account Statement for 0774267xxxx shows 786 unredeemed Bonus Points. To claim call 0871918xxxx Identifier Code: 45239 Expires
~~~

##### S-33 - Service login details
**Primary:** Authority | **Also uses:** Curiosity
**Why:** A website plus a login code reads like an account-access notice. It trains the reader to type into a page they were pointed to.

~~~text
SMS SERVICES. for your inclusive text credits, pls goto www.comuk[.]net login= 3qxj9 unsubscribe with STOP, no extra charge. help 0870284xxxx.COMUK. 220-CM2 9AE
~~~

##### S-34 - "Monthly password"
**Primary:** Authority | **Also uses:** Curiosity
**Why:** A "monthly password" notice looks like automated system output, which people tend not to question.

~~~text
Monthly password for wap. mobsi[.]com is 391784. Use your wap phone not PC.
~~~

##### S-35 - Billing-system language
**Primary:** Authority | **Also uses:** Fear
**Why:** Operations-speak ("resent as previous attempt failed due to network error") and a support address read like a billing system, not a sales message.

~~~text
This msg is for your mobile content order It has been resent as previous attempt failed due to network error Queries to customersqueries@netvision[.]uk[.]com
~~~

##### S-36 - Corporate brand "Message center"
**Primary:** Authority | **Also uses:** Reward, Curiosity
**Why:** A recognisable corporate brand and a recruiting-style line ("Apply for your future") borrow enterprise credibility.

~~~text
Bloomberg -Message center +44779770xxxx Why wait? Apply for your future hxxp://careers[.]bloomberg[.]com
~~~

##### S-37 - "SIM subscriber"
**Primary:** Authority | **Also uses:** Reward, Urgency
**Why:** Addresses you as a carrier customer ("SIM subscriber"), then promises delivery "to your door." The EXP date adds a deadline.

~~~text
As a SIM subscriber, you are selected to receive a Bonus! Get it delivered to your door, Txt the word OK to No: 88600 to claim. 150p/msg, EXP. 30Apr
~~~

##### S-38 - Short-code "service could not be delivered"
**Primary:** Authority | **Also uses:** Fear
**Why:** Reads like a billing-system error from a short code. The "fix" is to spend more.

~~~text
Free msg. Sorry, a service you ordered from 81303 could not be delivered as you do not have sufficient credit. Please top up to receive the service.
~~~

##### S-39 - Voucher with "customer care"
**Primary:** Authority | **Also uses:** Reward
**Why:** Looks like a routine telecom or retail voucher update, complete with a "customer care" line.

~~~text
Your B4U voucher w/c 27/03 is MARSMS. Log onto www.B4Utele[.]com for discount credit. To opt out reply stop. Customer care call 0871716xxxx
~~~

##### S-40 - Bare system message
**Primary:** Authority | **Also uses:** Curiosity
**Why:** A bare system-style message (user ID, removal instructions, customer services) with no content, so recipients assume it belongs to a service they use.

~~~text
Your unique user ID is 1172. For removal send STOP to 87239 customer services 0870803xxxx
~~~

