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

---

### 3. FEAR / LOSS AVERSION (20)

**How it shows up in SMS:** Outright threats are rare in this collection. Fear mostly arrives as a charge you did not expect, a subscription you cannot easily leave, a prize or service about to go to someone else, or money you are told you are already owed. The recipient is pushed to act to stop a loss rather than to gain something, and the "fix" is usually a reply or a call back to the sender.

##### S-41 - "Billed by mistake" refund pretext
**Primary:** Fear | **Also uses:** Authority, Reward
**Why:** Opens with a wrong charge, which is a loss, then offers the cure. A refund is a reason to call, and the "free from a BT landline" wording leaves open that a mobile call may cost.

~~~text
FREE MSG:We billed your mobile number by mistake from shortcode 83332.Please call 0808126xxxx to have charges refunded.This call will be free from a BT landline
~~~

##### S-42 - Bare billing line
**Primary:** Fear | **Also uses:** Authority, Curiosity
**Why:** A one-line statement for something you do not remember buying. The small amount keeps it from feeling worth a dispute, and a company name with a PO box makes it read like a ledger entry.

~~~text
Kit Strip - you have been billed 150p. Netcollex Ltd. PO Box 1013 IG11 OJA
~~~

##### S-43 - Purchase you never made
**Primary:** Fear | **Also uses:** Authority, Commitment
**Why:** Confirms a purchase and a charge in the same breath, then pivots to "think you can do better?" so the reader is nudged into taking part rather than disputing the charge.

~~~text
LookAtMe!: Thanks for your purchase of a video clip from LookAtMe!, you've been charged 35p. Think you can do better? Why not send a video in a MMSto 32323.
~~~

##### S-44 - "Subscription renewed" charge
**Primary:** Fear | **Also uses:** Authority, Commitment
**Why:** "Renewed" implies an agreement already exists. The *BILLING MSG* tag mimics system output, and the "10 more polys" line pushes the reader to the site to get value for money already taken.

~~~text
Ur TONEXS subscription has been renewed and you have been charged £4.50. You can choose 10 more polys this month. www.clubzed[.]co[.]uk *BILLING MSG*
~~~

##### S-45 - Ringtone order confirmation
**Primary:** Fear | **Also uses:** Authority, Commitment
**Why:** An order confirmation for something never ordered, with a reference number for a paperwork feel. The "call customer services" fix points to a 090 number, which is premium-rate in the UK.

~~~text
Thanks for your ringtone order, ref number K718. Your mobile will be charged £4.50. Should your tone not arrive please call customer services on 0906506xxxx
~~~

##### S-46 - Weekly charge, cancel by phone
**Primary:** Fear | **Also uses:** Authority, Commitment
**Why:** Moves from a one-off charge to a recurring weekly one, and the only stated way out is a phone call. The cancellation route is the paid route.

~~~text
Thanks for your Ringtone Order, Reference T91. You will be charged GBP 4 per week. You can unsubscribe at anytime by calling customer services on 0905703xxxx
~~~

##### S-47 - Numbered "RECPT" notice
**Primary:** Fear | **Also uses:** Authority, Curiosity
**Why:** The "1/3" numbering implies a receipt trail and more messages to come. It gives no instructions at all, so the reader has to go looking for the sender to sort it out.

~~~text
RECPT 1/3. You have ordered a Ringtone. Your order is being processed...
~~~

##### S-48 - "Already paid for"
**Primary:** Fear | **Also uses:** Authority, Commitment
**Why:** Frames inaction as waste ("already paid for"), which is loss aversion in miniature. The link is the only action offered.

~~~text
Reminder: You have not downloaded the content you have already paid for. Goto hxxp://doit[.]mymoby[.]tv/ to collect your content.
~~~

##### S-49 - Confirm by replying YES or NO
**Primary:** Fear | **Also uses:** Commitment, Authority
**Why:** Presents a monthly charge as pending and invites you to stop it by replying. Any reply, even NO, tells the sender the number is live and gets the reader engaging with them.

~~~text
Thanks for your subscription to Ringtone UK your mobile will be charged £5/month Please confirm by replying YES or NO. If you reply NO you will not be charged
~~~

##### S-50 - "Can not unsubscribe yet"
**Primary:** Fear | **Also uses:** Authority, Commitment
**Why:** Blocks the exit with a 54-week minimum term, which creates the fear of being locked in. The only suggested next step is another reply to the sender.

~~~text
Sorry! U can not unsubscribe yet. THE MOB offer package has a min term of 54 weeks> pls resubmit request after expiry. Reply THEMOB HELP 4 more info
~~~

##### S-51 - Subscribed "until you send STOP"
**Primary:** Fear | **Also uses:** Authority, Commitment
**Why:** States a subscription and a recurring cost as already in force. The stop instruction is present, but the framing is that charges continue until you act.

~~~text
U are subscribed to the best Mobile Content Service in the UK for £3 per ten days until you send STOP to 83435. Helpline 0870609xxxx.
~~~

##### S-52 - Cost buried in part 2 of 3
**Primary:** Fear | **Also uses:** Authority, Commitment
**Why:** The price appears only in the middle message of a series. A reader who never saw part 1 sees a weekly cost and an instruction to "stop further" tones, which implies they are already enrolled.

~~~text
You can stop further club tones by replying "STOP MIX" See my-tone[.]com/enjoy[.]html for terms. Club tones cost GBP4.50/week. MFL, PO Box 1146 MK45 2WT (2/3)
~~~

##### S-53 - "Subs has now expired"
**Primary:** Fear | **Also uses:** Commitment, Authority
**Why:** Treats a lapse as a loss of something you supposedly had, then sells the fix at a weekly price. The relationship it refers to is invented.

~~~text
Tone Club: Your subs has now expired 2 re-sub reply MONOC 4 monos or POLYC 4 polys 1 weekly @ 150p per week Txt STOP 2 stop This msg free Stream 087121202xxxx
~~~

##### S-54 - Outbid by a named rival
**Primary:** Fear | **Also uses:** Social Proof, Commitment
**Why:** A named rival and a lost item make this a clean loss-aversion example. "2 bid again" invites re-engagement, while "end bid notifications" is the escape hatch.

~~~text
U were outbid by simonwatson5120 on the Shinco DVD Plyr. 2 bid again, visit sms[.]ac/smsrewards 2 end bid notifications, reply END OUT
~~~

##### S-55 - "Your prize will go to another customer"
**Primary:** Fear | **Also uses:** Urgency, Social Proof
**Why:** Clearly the tail of a longer message, yet the fear still works without the setup: the prize is yours until you hesitate, then it goes to someone else. "Please call back if busy" implies many other callers.

~~~text
If you don't, your prize will go to another customer. T&C at www.t-c[.]biz 18+ 150p/min Polo Ltd Suite 373 London W1J 6HL Please call back if busy
~~~

##### S-56 - "Missed call alert"
**Primary:** Fear | **Also uses:** Curiosity, Authority
**Why:** No threat is stated. The fear is of missing something important, and since the callers "left no message," the only way to find out is to call back.

~~~text
Missed call alert. These numbers called but left no message. 0700800xxxx
~~~

##### S-57 - Accident compensation "entitled to"
**Primary:** Fear | **Also uses:** Reward, Authority
**Why:** "Our records indicate" claims a database, and "the Accident you had" presumes an event. Framing the money as owed rather than won makes ignoring it feel like losing it. The same shape survives today as accident-claim spam.

~~~text
FREEMSG: Our records indicate you may be entitled to 3750 pounds for the Accident you had. To claim for free reply with YES to this msg. To opt out text STOP
~~~

##### S-58 - "You are being ripped off!"
**Primary:** Fear | **Also uses:** Reward
**Why:** Opens with an accusation that you are overpaying, then casts the sender as the rescue. The "six downloads for only 3" line supplies the cheaper alternative.

~~~text
You are being ripped off! Get your mobile content from www.clubmoby[.]com call 0871750xxxx poly/true/Pix/Ringtones/Games six downloads for only 3
~~~

##### S-59 - "Refused a loan?"
**Primary:** Fear | **Also uses:** Reward, Rapport
**Why:** Stacks rhetorical questions about past rejection so the reader recognises themselves, then promises relief ("we will!"). It targets people already under financial stress.

~~~text
Refused a loan? Secured or Unsecured? Can't get credit? Call free now 0800 195 xxxx or text back 'help' & we will!
~~~

##### S-60 - A scam warning
**Primary:** Fear | **Also uses:** Rapport
**Why:** This is not a scam but a recipient's warning about one, kept here because it shows fear used defensively. It also records the bait pattern: a friendly opener from a normal-looking number, then premium-rate follow-ups.

~~~text
Hi ya babe x u 4goten bout me?' scammers getting smart..Though this is a regular vodafone no, if you respond you get further prem rate msg/subscription. Other nos used also. Beware!
~~~

---

### 4. REWARD / GREED (20)

**How it shows up in SMS:** Prize draws, "guaranteed" awards, free gadgets and upgrade offers. The prize is large and specific, the effort to claim is tiny (one call or one text), and the real cost, usually a premium-rate call or a recurring subscription, sits in the small print. Many of these also say "you have won" when what you actually get is entry into something.

##### S-61 - "Over £1million to give away"
**Primary:** Reward | **Also uses:** Social Proof
**Why:** The headline sum is a pool, not a prize, so there is no fixed figure to check. "Over £1million to give away" implies plenty for everyone, which makes your own win feel likely. The repeat-charge note ("ppt150x3") comes last.

~~~text
money!!! you r a lucky winner ! 2 claim your prize text money 2 88600 over £1million to give away ! ppt150x3+normal text rate box403 w1t1jy
~~~

##### S-62 - "SIX chances to win CASH"
**Primary:** Reward | **Also uses:** Commitment
**Why:** Multiple chances and a wide prize range (100 to 20,000) make winning feel probable and the upside large. The daily charge for six days appears only in the middle of the small print.

~~~text
SIX chances to win CASH! From 100 to 20,000 pounds txt> CSH11 and send to 87575. Cost 150p/day, 6days, 16+ TsandCs apply Reply HL 4 info
~~~

##### S-63 - Tiered prize list
**Primary:** Reward | **Also uses:** Urgency, Curiosity
**Why:** A ladder from 10K down to a £100 voucher makes "winning something" feel close to certain. "IS IT YOU" adds a prompt to find out.

~~~text
Shop till u Drop, IS IT YOU, either 10K, 5K, £500 Cash or £100 Travel voucher, Call now, 0906401xxxx. NTT PO Box CR01327BT fixedline Cost 150ppm mobile vary
~~~

##### S-64 - Free week in a £100,000 jackpot
**Primary:** Reward | **Also uses:** Urgency, Commitment
**Why:** What you have "won" is a free week of membership, which is access to a chance. The six-figure jackpot in the same sentence does the emotional work.

~~~text
URGENT! You have won a 1 week FREE membership in our £100,000 Prize Jackpot! Txt the word: CLAIM to No: 81010 T&C www.dbuk[.]net LCCLTD POBOX 4403LDNW1A7RW18
~~~

##### S-65 - Newsletter-style cash giveaway
**Primary:** Reward | **Also uses:** Urgency, Curiosity
**Why:** Written like an email newsletter preview rather than a text. "Biggest and best EVER" is pure superlative, "this weekend" gives it a time anchor, and the cut-off ("These..") leaves the details behind a click.

~~~text
Cashbin[.]co[.]uk (Get lots of cash this weekend!) www.cashbin[.]co[.]uk Dear Welcome to the weekend We have got our biggest and best EVER cash give away!! These..
~~~

##### S-66 - Made-up "cash-balance"
**Primary:** Reward | **Also uses:** Commitment, Authority
**Why:** A made-up balance makes the money feel like it is already yours, and "maximize ur cash-in" frames a paid reply as an investment. The company address and customer-care line add a ledger feel.

~~~text
Ur cash-balance is currently 500 pounds - to maximize ur cash-in now send CASH to 86688 only 150p/msg. CC: 0871872xxxx PO BOX 114/14 TCR/W1
~~~

##### S-67 - Stock tip with "UP OVER 300%"
**Primary:** Reward | **Also uses:** Authority, Social Proof
**Why:** A pump-and-dump style pitch: a ticker symbol, a claimed 300% gain and "for our members" exclusivity. The greed is for a gain you could still catch. Like S-65, this is an email-style alert that landed in the SMS collection.

~~~text
Dorothy@kiefer[.]com (Bank of Granite issues Strong-Buy) EXPLOSIVE PICK FOR OUR MEMBERS *****UP OVER 300% *********** Nasdaq Symbol CDGT That is a $5.00 per..
~~~

##### S-68 - Bundle deal with a free handset
**Primary:** Reward | **Also uses:** Urgency, Commitment
**Why:** Minutes, texts and a new handset for a low weekly price is framed as obvious value. "Delivery Tomorrow" makes it concrete and close.

~~~text
Do you want 750 anytime any network mins 150 text and a NEW video phone for only five pounds per week call 0800077xxxx now or reply for delivery Tomorrow
~~~

##### S-69 - Named gadget, delivery window
**Primary:** Reward | **Also uses:** Authority
**Why:** A specific product ("SiPix Digital Camera") and a delivery window make the prize feel physical. The "from landline" instruction steers the call to a particular line type.

~~~text
Camera - You are awarded a SiPix Digital Camera! call 0906122xxxx fromm landline. Delivery within 8 days
~~~

##### S-70 - "Guaranteed," pick-one prize
**Primary:** Reward | **Also uses:** Authority
**Why:** "Guaranteed" plus a pick-one list (phone, iPod or £500) means every branch is a win. The per-message cost is only in the trailing small print.

~~~text
You are guaranteed the latest Nokia Phone, a 40GB iPod MP3 player or a £500 prize! Txt word: COLLECT to No: 83355! IBHltd LdnW15H 150p/Mtmsgrcvd18+
~~~

##### S-71 - "Entitled" to a free upgrade
**Primary:** Reward | **Also uses:** Authority
**Why:** Opens with a question that fits most phone owners ("11 months or more?") and then says "entitled" to a free upgrade. A template that recurs many times in the collection, with the freephone number changed between copies.

~~~text
Had your mobile 11 months or more? U R entitled to Update to the latest colour mobiles with camera for Free! Call The Mobile Update Co FREE on 0800298xxxx
~~~

##### S-72 - "You have won" an auction
**Primary:** Reward | **Also uses:** Commitment, Urgency
**Why:** "You have won" is followed straight away by "to take part," which means what you really get is entry into an auction. Nearly all of the reward lever is in the first sentence.

~~~text
You have won a Nokia 7250i. This is what you get when you win our FREE auction. To take part send Nokia to 86021 now. HG/Suite342/2Lands Row/W1JHL 16+
~~~

##### S-73 - Cruise "or" cash
**Primary:** Reward | **Also uses:** Authority
**Why:** "UR GOING" states the trip as settled, and the cash alternative means the prize still looks worth claiming. A "live operator" adds a human touch.

~~~text
UR GOING 2 BAHAMAS! CallFREEFONE 0808156xxxx and speak to a live operator to claim either Bahamas cruise of£2000 CASH 18+only. To opt out txt X to 0778620xxxx
~~~

##### S-74 - "FOR NOTHING"
**Primary:** Reward | **Also uses:** Social Proof, Authority
**Why:** "FOR NOTHING" plus a stated value (£350) gives a freebie a price tag. "1 of 250" suggests a real programme with a finite number of places.

~~~text
You have been selected to stay in 1 of 250 top British hotels - FOR NOTHING! Holiday Worth £350! To Claim, Call London 0207206xxxx. Bx 526, SW73SS
~~~

##### S-75 - Valentine's weekend bundle
**Primary:** Reward | **Also uses:** Urgency, Rapport
**Why:** A seasonal hook, an itemised bundle (flight, hotel, extra prize) and "guaranteed." "Collect" wording implies it is already yours.

~~~text
Collect your VALENTINE'S weekend to PARIS inc Flight & Hotel + £200 Prize guaranteed! Text: PARIS to No: 69101. www.rtf.sphosting[.]com
~~~

##### S-76 - CD vouchers, no small print
**Primary:** Reward | **Also uses:** Commitment, Authority
**Why:** Two prizes plus a "free entry" draw, framed as already earned ("ur awarded"). This copy omits the cost terms that other copies of the same message carry.

~~~text
Congratulations ur awarded 500 of CD vouchers or 125gift guaranteed & Free entry 2 100 wkly draw txt MUSIC to 87066
~~~

##### S-77 - "Next meal on us"
**Primary:** Reward | **Also uses:** Authority, Rapport
**Why:** "Dear Voucher holder" assumes an existing relationship, and "on us" frames it as a gift. It moves the reader to a PC and a link, which makes it the closest match here to modern link-based phishing.

~~~text
Dear Voucher holder Have your next meal on us. Use the following link on your pc 2 enjoy a 2 4 1 dining experiencehxxp://www.vouch4me[.]com/etlp/dining[.]asp
~~~

##### S-78 - Free texts "credited"
**Primary:** Reward | **Also uses:** Authority, Commitment
**Why:** "Has been credited" implies the value is already in an account, and all that is left is to "activate" it, which sounds like routine admin.

~~~text
Your account has been credited with 500 FREE Text Messages. To activate, just txt the word: CREDIT to No: 80488 T&Cs www.80488[.]biz
~~~

##### S-79 - Free ringtone "waiting to be collected"
**Primary:** Reward | **Also uses:** Authority, Commitment
**Why:** "Waiting to be collected" gives a sense of ownership, and a "password" to "verify" sounds like a security step. The weekly charge ("450Ppw") is squeezed into the last line.

~~~text
Your free ringtone is waiting to be collected. Simply text the password "MIX" to 85069 to verify. Get Usher and Britney. FML, PO Box 5249, MK17 92H. 450Ppw 16
~~~

##### S-80 - Free product trial, address and DOB
**Primary:** Reward | **Also uses:** Rapport, Urgency
**Why:** A perk (a free product trial) in a casual voice ("Ta r"), then a request for address and date of birth "asap." The reward is a pretext for personal data, which is closer to modern identity-harvesting phishing than most of this collection.

~~~text
Hello. We need some posh birds and chaps to user trial prods for champneys. Can i put you down? I need your address and dob asap. Ta r
~~~
### 5. CURIOSITY (20)

**How it shows up in SMS:** Curiosity pretexts exploit information gaps, driving recipients to act simply to satisfy an unanswered question[cite: 2]. Rather than dangling explicit monetary gains or making high-pressure threats, these messages rely on incomplete details—unnamed contacts, mysterious voicemail or picture notifications, secret admirers, or misdirected personal notes[cite: 2]. The victim responds or clicks a link to resolve the ambiguity[cite: 2].

##### S-81 - Bare voicemail notification
**Primary:** Curiosity | **Also uses:** Urgency
**Why:** Strips away all context except the existence of an unread voice message[cite: 2]. By withholding the caller's identity and subject, it triggers an impulse to resolve the missing information[cite: 2].

```text
You have 1 new voicemail. Please call 0871918xxxx.
```

##### S-82 - "Secret Admirer" reveal hook
**Primary:** Curiosity | **Also uses:** Rapport, Reward
**Why:** Plays on ego and social curiosity by teasing an anonymous admirer[cite: 2]. The identity is locked behind a premium-rate line[cite: 2].

```text
U have a secret admirer. REVEAL who thinks U R So special. Call 0906517xxxx. To opt out Cust care 0782123xxxx
```

##### S-83 - Unrequested picture link
**Primary:** Curiosity | **Also uses:** Social Proof
**Why:** Hints that a photo involving or directed at the recipient is available online[cite: 2], driving an immediate click to inspect the URL[cite: 2].

```text
A link to your picture has been sent. You can also use hxxp://alto18[.]co[.]uk/wave/wave.asp?o=44345
```

##### S-84 - Vague "Today is your lucky day!" teaser
**Primary:** Curiosity | **Also uses:** Authority, Reward
**Why:** Merges network carrier branding with an intentionally vague promise[cite: 2], forcing the recipient to visit an external domain to discover what they supposedly won[cite: 2].

```text
IMPORTANT INFORMATION 4 ORANGE USER 0796XXXXXX. TODAY IS UR LUCKY DAY!2 FIND OUT WHY LOG ONTO hxxp://www[.]urawinner[.]com
```

##### S-85 - Unread mailbox messages and matches
**Primary:** Curiosity | **Also uses:** Authority, Urgency
**Why:** Simulates an automated messaging system reporting unread social matches and notes[cite: 2], tapping into curiosity and FOMO[cite: 2].

```text
<Forwarded 21870000 from>Hi - this is your Mailbox Messaging SMS alert. You have 4 messages. You have 21 matches. Please call back on 0905624xxxx to retrieve your messages and matches
```

##### S-86 - "Dating Service" hook from an acquaintance
**Primary:** Curiosity | **Also uses:** Social Proof, Rapport
**Why:** Claims that someone the victim knows personally entered their contact details into a dating site[cite: 2], creating an intriguing interpersonal puzzle[cite: 2].

```text
Someone has contacted our dating service and entered your phone because they fancy you! To find out who it is call from a landline 0911103xxxx . PoBox12n146tf150p
```

##### S-87 - Cryptic branding campaign ("Are you unique enough?")
**Primary:** Curiosity | **Also uses:** Commitment
**Why:** Uses a philosophical teaser question with no context or product explanation[cite: 2], tempting recipients to visit the site out of sheer intrigue[cite: 2].

```text
Are you unique enough? Find out from 30th August. www[.]areyouunique[.]co[.]uk
```

##### S-88 - Minimalist message notice
**Primary:** Curiosity | **Also uses:** Urgency
**Why:** A bare notification with zero specifics[cite: 2]. The lack of details forces the recipient to call to bridge the information gap[cite: 2].

```text
You have 1 new message. Please call 0871873xxxx.
```

##### S-89 - Unsolicited social network friend invite
**Primary:** Curiosity | **Also uses:** Rapport, Social Proof
**Why:** Uses a specific profile name and age to make an invitation feel genuine[cite: 2], prompting the recipient to check who is attempting to connect[cite: 2].

```text
-PLS STOP bootydelious (32/F) is inviting you to be her friend. Reply YES-434 or NO-434 See her: www[.]SMS[.]ac/u/bootydelious STOP? Send STOP FRND to 62468
```

##### S-90 - "Guess what! Somebody secretly fancies you"
**Primary:** Curiosity | **Also uses:** Rapport
**Why:** Starts with an informal conversational hook ("Guess what!")[cite: 2] paired with an anonymous crush lure to compel a premium call back[cite: 2].

```text
Guess what! Somebody you know secretly fancies you! Wanna find out who it is? Give us a call on 0906539xxxx From Landline DATEBox1282EssexCM61XN 150p/min 18
```

##### S-91 - Personal picture teaser
**Primary:** Curiosity | **Also uses:** Rapport
**Why:** Mixes informal conversational phrasing ("Feelin kinda lnly") with visual curiosity ("wanna c my pic?")[cite: 2] to drive a paid response[cite: 2].

```text
FreeMsg:Feelin kinda lnly hope u like 2 keep me company! Jst got a cam moby wanna c my pic?Txt or reply DATE to 82242 Msg150p 2rcv Hlp 0871231xxxx stop to 82242
```

##### S-92 - Open-ended email audio gateway alert
**Primary:** Curiosity | **Also uses:** Authority
**Why:** Formatted as an automated gateway alert for an incoming voice email[cite: 2]. The recipient's curiosity is piqued by an unknown sender name[cite: 2].

```text
Email AlertFrom: Jeri StewartSize: 2KBSubject: Low-cost prescripiton drvgsTo listen to email call 123
```

##### S-93 - Personal fortune / horoscope hook
**Primary:** Curiosity | **Also uses:** Reward
**Why:** Asks self-reflective questions regarding love and career prospects[cite: 2], exploiting personal curiosity to solicit an SMS reply[cite: 2].

```text
Will u meet ur dream partner soon? Is ur career off 2 a flyng start? 2 find out free, txt HORO followed by ur star sign, e. g. HORO ARIES
```

##### S-94 - Cryptic lifestyle survey
**Primary:** Curiosity | **Also uses:** Rapport, Commitment
**Why:** Poses an open-ended question about personal goals before requesting personal information (name and age)[cite: 2].

```text
Ever thought about living a good life with a perfect partner? Just txt back NAME and AGE to join the mobile community. (100p/SMS)
```

##### S-95 - Teaser for upcoming media content
**Primary:** Curiosity | **Also uses:** Reward, Authority
**Why:** Builds anticipation for exclusive "free downloads" coming in a subsequent message[cite: 2], conditioning the user to accept incoming WAP push links[cite: 2].

```text
TheMob>Yo yo yo-Here comes a new selection of hot downloads for our members to get for FREE! Just click & open the next link sent to ur fone...
```

##### S-96 - Unrecognized ongoing conversation hook
**Primary:** Curiosity | **Also uses:** Fear, Rapport
**Why:** Implies a prior intimate relationship or missed communication ("no word back!")[cite: 2], inducing confusion that prompts a reply to clarify[cite: 2].

```text
FreeMsg Hey there darling it's been 3 week's now and no word back! I'd like some fun you up for it still? Tb ok! XxX std chgs to send, £1.50 to rcv
```

##### S-97 - Social platform greeting alert
**Primary:** Curiosity | **Also uses:** Social Proof, Rapport
**Why:** Mimics a notification from a social network showing an incoming friendly note[cite: 2], tempting the recipient to engage[cite: 2].

```text
SMS[.]ac sun0819 posts HELLO:"You seem cool, wanted to say hi. HI!!!" Stop? Send STOP to 62468
```

##### S-98 - Blind date profile recommendation
**Primary:** Curiosity | **Also uses:** Social Proof
**Why:** Displays a specific username and location ("21/m from Aberdeen")[cite: 2], using targeted profile details to spur curiosity clicks[cite: 2].

```text
SMS[.]ac Blind Date 4U!: Rodds1 is 21/m from Aberdeen, United Kingdom. Check Him out hxxp://img[.]sms[.]ac/W/icmb3cktz8r7!-4 no Blind Dates send HIDE
```

##### S-99 - Unrequested order download notification
**Primary:** Curiosity | **Also uses:** Authority, Fear
**Why:** Informs the user of an incoming media order they never placed[cite: 2], leveraging curiosity and minor concern to drive traffic to a WAP portal[cite: 2].

```text
BangBabes Ur order is on the way. U SHOULD receive a Service Msg 2 download UR content. If U do not, GoTo wap[.]bangb[.]tv on UR mobile internet/service menu
```

##### S-100 - Misdirected personal illicit invitation
**Primary:** Curiosity | **Also uses:** Rapport, Reward
**Why:** Disguised as a private, misdirected invite ("Hubby at meetins all day")[cite: 2], exploiting taboo intrigue to prompt a call[cite: 2].

```text
Hi its LUCY Hubby at meetins all day Fri & I will B alone at hotel U fancy cumin over? Pls leave msg 2day 0909972xxxx Lucy x Calls£1/minMobsmoreLKPOBOX177HP51FL
```

### 6. SOCIAL PROOF (20)[cite: 1]

**How it shows up in SMS:** Social proof relies on group activity ("1000's flirting NOW"), claimed local popular demand ("lots of new people registered in YOUR AREA"), social network invites, or named individual winners ("Mr. T. Foley won an iPod!") to make joining, calling, or replying feel validated, safe, or socially expected[cite: 1]. Pretending someone from the victim's social circle is involved ("someone you know fancies you") is the most common smishing shortcut for this lever[cite: 1].

##### S-101 - Named public winner showcase
**Primary:** Social Proof | **Also uses:** Reward, Curiosity
**Why:** Naming a specific individual ("Mr. T. Foley") who allegedly won creates believable proof that real people are winning, encouraging others to stay tuned or visit the site[cite: 1].

```text
WIN: We have a winner! Mr. T. Foley won an iPod! More exciting prizes soon, so keep an eye on ur mobile or visit www.win-82050[.]co[.]uk
```

##### S-102 - "1000's flirting NOW!"
**Primary:** Social Proof | **Also uses:** Curiosity, Reward
**Why:** High volume numbers ("1000's") create a bandwagon effect, making participation feel popular and widely accepted[cite: 1].

```text
1000's flirting NOW! Txt GIRL or BLOKE & ur NAME & AGE, eg GIRL ZOE 18 to 8007 to join and get chatting!
```

##### S-103 - "Lots of new people registered in YOUR AREA"
**Primary:** Social Proof | **Also uses:** Curiosity, Rapport
**Why:** Claims local momentum and group participation to normalize joining the service[cite: 1].

```text
We have new local dates in your area - Lots of new people registered in YOUR AREA. Reply DATE to start now! 18 only www.flirtparty[.]us REPLYS150
```

##### S-104 - "More people are... in your area now"
**Primary:** Social Proof | **Also uses:** Curiosity, Commitment
**Why:** Suggests an existing local group of "like minded guys" who are already participating, nudging the reader to fit in[cite: 1].

```text
More people are dogging in your area now. Call 0909020xxxx and join like minded guys. Why not arrange 1 yourself. There's 1 this evening. A£1.50 minAPN LS278BB
```

##### S-105 - "Someone you know" contact claim
**Primary:** Social Proof | **Also uses:** Curiosity, Rapport
**Why:** Leverages existing social connections by asserting that an actual acquaintance initiated the contact[cite: 1].

```text
You are being contacted by our dating service by someone you know! To find out who it is, call from a land line 0905000xxxx. PoBox45W2TG150P
```

##### S-106 - "Secret admirer" validation
**Primary:** Social Proof | **Also uses:** Curiosity, Rapport
**Why:** Uses implicit social validation (the idea that someone already finds the target special) to provoke curiosity and action[cite: 1].

```text
U have a secret admirer. REVEAL who thinks U R So special. Call 0906517xxxx. To opt out Cust care 0782123xxxx
```

##### S-107 - Social network friend invitation
**Primary:** Social Proof | **Also uses:** Curiosity, Authority
**Why:** Mimics a social network notification where a named user (`bootydelious`) is waiting for friend approval[cite: 1].

```text
-PLS STOP bootydelious (32/F) is inviting you to be her friend. Reply YES-434 or NO-434 See her: www.SMS[.]ac/u/bootydelious STOP? Send STOP FRND to 62468
```

##### S-108 - "Join the mobile community"
**Primary:** Social Proof | **Also uses:** Rapport, Commitment
**Why:** Frames participation as joining a community of peers rather than purchasing a service

```text
Ever thought about living a good life with a perfect partner? Just txt back NAME and AGE to join the mobile community. (100p/SMS)
```

##### S-109 - "Folks waiting for company"
**Primary:** Social Proof | **Also uses:** Rapport, Curiosity
**Why:** Implies an active community of waiting participants ("folks"), making the user feel welcomed into a pre-existing social group[cite: 1].

```text
How about getting in touch with folks waiting for company? Just txt back your NAME and AGE to opt in! Enjoy the community (150p/SMS)
```

##### S-110 - "World's most discreet text dating service"
**Primary:** Social Proof | **Also uses:** Curiosity, Rapport
**Why:** Reassures the target by framing the platform as an established global service where others meet safely

```text
Talk sexy!! Make new friends or fall in love in the worlds most discreet text dating service. Just text VIP to 83110 and see who you could meet.
```

##### S-111 - Local singles matching prompt
**Primary:** Social Proof | **Also uses:** Rapport, Curiosity
**Why:** Implies local demand ("sexy singles in yr area") to encourage instant participation

```text
Summers finally here! Fancy a chat or flirt with sexy singles in yr area? To get MATCHED up just reply SUMMER now. Free 2 Join. OptOut txt STOP Help0871474xxxx
```

##### S-112 - Specific profile highlight
**Primary:** Social Proof | **Also uses:** Curiosity, Rapport
**Why:** Displays a real-sounding user profile (`Rodds1 is 21/m from Aberdeen`) to prove that real members actively use the network.

```text
SMS. ac Blind Date 4U!: Rodds1 is 21/m from Aberdeen, United Kingdom. Check Him out hxxp://img[.] sms[.] ac/W/icmb3cktz8r7!-4 no Blind Dates send HIDE
```

##### S-113 - Secret acquaintance interest
**Primary:** Social Proof | **Also uses:** Curiosity, Rapport
**Why:** Claims personal acquaintance interest ("Somebody you know secretly fancies you") to exploit social peer dynamics[cite: 1].

```text
Guess what! Somebody you know secretly fancies you! Wanna find out who it is? Give us a call on 0906539xxxx From Landline DATEBox1282EssexCM61XN 150p/min 18
```

##### S-114 - Peer-initiated service referral
**Primary:** Social Proof | **Also uses:** Curiosity, Rapport
**Why:** Claims that a personal acquaintance specifically requested the service to make contact with the recipient[cite: 1].

```text
Someone U know has asked our dating service 2 contact you! Cant Guess who? CALL 0905809xxxx NOW all will be revealed. PO BOX385 M6 6WU
```

##### S-115 - Speedchat active platform
**Primary:** Social Proof | **Also uses:** Curiosity, Commitment
**Why:** Presents an active environment where users can dynamically swap chat partners, signaling a large live audience[cite: 1].

```text
Bored of speed dating? Try SPEEDCHAT, txt SPEEDCHAT to 80155, if you don't like em txt SWAP and get a new chatter! Chat80155 POBox36504W45WQ 150p/msg rcd 16
```

##### S-116 - Postcode-based local network search
**Primary:** Social Proof | **Also uses:** Curiosity, Rapport
**Why:** Encourages local area matching, giving the impression of an active local user base

```text
New TEXTBUDDY Chat 2 horny guys in ur area 4 just 25p Free 2 receive Search postcode or at gaytextbuddy[.]com. TXT ONE name to 89693. 0871550xxxx rpl Stop 2 cnl
```

##### S-117 - "UK's largest network" claim
**Primary:** Social Proof | **Also uses:** Authority, Curiosity
**Why:** Claims market leadership ("UK's largest") so potential users assume safety and widespread participation

```text
Want 2 get laid tonight? Want real Dogging locations sent direct 2 ur mob? Join the UK's largest Dogging Network bt Txting GRAVEL to 69888! Nt. ec2a. 31p.msg@150p
```

##### S-118 - Simulated ongoing social relationship
**Primary:** Social Proof | **Also uses:** Rapport, Curiosity
**Why:** Pretends an ongoing dialogue ("3 week's now and no word back!"), exploiting personal familiarity to force a response.

```text
FreeMsg Hey there darling it's been 3 week's now and no word back! I'd like some fun you up for it still? Tb ok! XxX std chgs to send, £1.50 to rcv
```

##### S-119 - User panel recruitment ("posh birds and chaps")
**Primary:** Social Proof | **Also uses:** Authority, Rapport
**Why:** Uses casual peer-group language to recruit participants for product trials under the guise of an exclusive user group.

```text
Hello. We need some posh birds and chaps to user trial prods for champneys. Can i put you down? I need your address and dob asap. Ta r
```

##### S-120 - Public winner announcement ("Mr. T. Tilo")
**Primary:** Social Proof | **Also uses:** Reward, Curiosity
**Why:** Highlights individual win verification ("Mr. T. Tilo won an iPod!") to reinforce the legitimacy of the prize draw.

```text
WIN: We have a winner! Mr. T. Tilo won an iPod! More exciting prizes soon, so keep an eye on ur mobile or visit www.win-82050[.]co[.]uk
```

### 7. COMMITMENT / CONSISTENCY (20)

**How it shows up in SMS:** Low-friction micro-commitments—such as answering an insultingly simple trivia question, replying with basic profile info ("NAME & AGE"), texting "YES" or "CREDIT" to activate an offer, or confirming a previous action (e.g., "for taking part in our survey"). Once the recipient takes that first trivial step, psychological consistency kicks in, making them far more likely to follow through with subsequent questions, recurring subscriptions, or fee-bearing responses.

##### S-121 - Trivial trivia question to enter draw
**Primary:** Commitment | **Also uses:** Reward, Curiosity
**Why:** Asks an trivially simple question ("which country the Algarve is in?") to get the target to take an initial interactive action. Sending the answer establishes participation in a paid service.

~~~text
Sunshine Quiz Wkly Q! Win a top Sony DVD player if u know which country the Algarve is in? Txt ansr to 82277. £1.50 SP:Tyrone
~~~

##### S-122 - Multi-choice TV quiz entry
**Primary:** Commitment | **Also uses:** Reward, Urgency
**Why:** A simple multiple-choice question lowers the friction to respond. Selecting a letter ("D E or F") feels effortless, committing the user into an ongoing premium text game.

~~~text
EASTENDERS TV Quiz. What FLOWER does DOT compare herself to? D= VIOLET E= TULIP F= LILY txt D E or F to 84025 NOW 4 chance 2 WIN £100 Cash WKENT/150P16+
~~~

##### S-123 - Accumulated "balance" quiz progression
**Primary:** Commitment | **Also uses:** Reward, Social Proof
**Why:** Reinforces an ongoing state ("Ur balance is now £500") and presents the "next question" to leverage momentum and sunk-cost feelings.

~~~text
Ur balance is now £500. Ur next question is: Who sang 'Uptown Girl' in the 80's ? 2 answer txt ur ANSWER to 83600. Good luck!
~~~

##### S-124 - Pop-culture question for draw entry
**Primary:** Commitment | **Also uses:** Authority, Reward
**Why:** Frames the prize draw entry as conditional upon providing a trivial answer ("Elvis Presleys Birthday"), turning a passive draw into an active commitment step.

~~~text
Dear Subscriber ur draw 4 £100 gift voucher will b entered on receipt of a correct ans. When was Elvis Presleys Birthday? TXT answer to 80062
~~~

##### S-125 - Registered subscriber chart question
**Primary:** Commitment | **Also uses:** Authority, Reward
**Why:** Addresses the recipient as a "registered optin subscriber" to prime consistency, then prompts for a single answer to complete entry into the draw.

~~~text
As a registered optin subscriber ur draw 4 £100 gift voucher will be entered on receipt of a correct ans to 80062 Whats No1 in the BBC charts
~~~

##### S-126 - Survey completion reward activation
**Primary:** Commitment | **Also uses:** Reciprocity, Reward
**Why:** References a prior action ("taking part in our mobile survey yesterday") to invoke consistency before asking for a text reply to claim the reward.

~~~text
For taking part in our mobile survey yesterday! You can now have 500 texts 2 use however you wish. 2 get txts just send TXT to 80160 T&C www.txt43.com 1.50p
~~~

##### S-127 - Post-vote engagement hook
**Primary:** Commitment | **Also uses:** Reciprocity, Curiosity
**Why:** Leverages a prior micro-action ("Thanks for the Vote") to pivot immediately into another low-friction request ("reply with SING now").

~~~text
Thanks for the Vote. Now sing along with the stars with Karaoke on your mobile. For a FREE link just reply with SING now.
~~~

##### S-128 - Multi-step quiz commitment
**Primary:** Commitment | **Also uses:** Reward, Authority
**Why:** Asks the user to complete "4 easy questions," framing the path to a £500 voucher as a small, manageable sequence of tasks.

~~~text
HMV BONUS SPECIAL 500 pounds of genuine HMV vouchers to be won. Just answer 4 easy questions. Play Now! Send HMV to 86688 More info:www.100percent-real.com
~~~

##### S-129 - Book launch multi-question quiz
**Primary:** Commitment | **Also uses:** Reward, Curiosity
**Why:** Hooks fans of a high-profile release into a 5-question commitment ladder, trading simple responses for a promised chance at early access.

~~~text
Win the newest “Harry Potter and the Order of the Phoenix (Book 5) reply HARRY, answer 5 questions - chance to be the first among readers!
~~~

##### S-130 - Self-identity appeal ("Think ur smart?")
**Primary:** Commitment | **Also uses:** Reward, Social Proof
**Why:** Challenges the recipient's self-image ("Think ur smart ?"), prompting them to prove it by texting "PLAY" and subscribing to a weekly quiz fee.

~~~text
Think ur smart ? Win £200 this week in our weekly quiz, text PLAY to 85222 now!T&Cs WinnersClub PO BOX 84, M26 3UZ. 16+. GBP1.50/week
~~~

##### S-131 - Offer activation via keyword
**Primary:** Commitment | **Also uses:** Reward, Authority
**Why:** Tells the user their account has already been credited, requiring a single keyword reply ("CREDIT") to finish activating the benefit.

~~~text
Your account has been credited with 500 FREE Text Messages. To activate, just txt the word: CREDIT to No: 80488 T&Cs www.80488.biz
~~~

##### S-132 - One-word "OK" activation
**Primary:** Commitment | **Also uses:** Reward
**Why:** Minimizes friction to the absolute lowest barrier—replying with a simple "ok"—to get the victim to initiate contact with a shortcode.

~~~text
500 free text msgs. Just text ok to 80488 and we'll credit your account
~~~

##### S-133 - Incomplete message fragment requiring action
**Primary:** Commitment | **Also uses:** Urgency, Reward
**Why:** Presents a truncated instruction ending with a calendar boundary, forcing the user to supply the missing response to secure claimed credits.

~~~text
it to 80488. Your 500 free text messages are valid until 31 December 2005.
~~~

##### S-134 - Community membership via profile input
**Primary:** Commitment | **Also uses:** Social Proof, Rapport
**Why:** Asking for basic personal details ("NAME and AGE") acts as a registration step that formally commits the target to the paid community network.

~~~text
Ever thought about living a good life with a perfect partner? Just txt back NAME and AGE to join the mobile community. (100p/SMS)
~~~

##### S-135 - Onboarding opt-in prompt
**Primary:** Commitment | **Also uses:** Rapport, Social Proof
**Why:** Uses a two-parameter reply ("NAME and AGE") as an explicit opt-in mechanism, establishing ongoing participation charged per SMS.

~~~text
How about getting in touch with folks waiting for company? Just txt back your NAME and AGE to opt in! Enjoy the community (150p/SMS)
~~~

##### S-136 - Low-cost trial entry with personal details
**Primary:** Commitment | **Also uses:** Rapport, Reward
**Why:** Pairs an initial low price point ("Join 4 just 10p") with a profile completion step ("REPLY with NAME & AGE") to lock in the subscription.

~~~text
Text & meet someone sexy today. U can find a date or even flirt its up to U. Join 4 just 10p. REPLY with NAME & AGE eg Sam 25. 18 -msg recd@thirtyeight pence
~~~

##### S-137 - Segment selection and profile onboarding
**Primary:** Commitment | **Also uses:** Social Proof, Curiosity
**Why:** Forces the user to make a explicit self-categorization choice ("GIRL or BLOKE") along with profile details to complete registration into a paid chat line.

~~~text
1000's flirting NOW! Txt GIRL or BLOKE & ur NAME & AGE, eg GIRL ZOE 18 to 8007 to join and get chatting!
~~~

##### S-138 - Free auction entry prompt
**Primary:** Commitment | **Also uses:** Reward, Authority
**Why:** Lowers resistance by declaring the auction "FREE 2 join & take part," getting the target to text "NOKIA" and enter the bidding funnel.

~~~text
SMS AUCTION - A BRAND NEW Nokia 7250 is up 4 auction today! Auction is FREE 2 join & take part! Txt NOKIA to 86021 now! HG/Suite342/2Lands Row/W1J6HL
~~~

##### S-139 - Multi-step competition entry flow
**Primary:** Commitment | **Also uses:** Reward, Urgency
**Why:** Promises free entry but requires texting a keyword ("FA") to receive a follow-up question, establishing a multi-turn engagement flow.

~~~text
Free entry in 2 a wkly comp to win FA Cup final tkts 21st May 2005. Text FA to 87121 to receive entry question(std txt rate)T&C's apply 08452810075over18's
~~~

##### S-140 - Carrier trial opt-in reply
**Primary:** Commitment | **Also uses:** Authority, Reward
**Why:** Uses carrier branding to offer a free 1-month trial, asking for a single "YES" reply to bind the user to terms and ongoing access.

~~~text
Hello from Orange. For 1 month's free access to games, news and sport, plus 10 free texts and 20 photo messages, reply YES. Terms apply: www.orange.co.uk/ow
~~~

### 8. RECIPROCITY / RAPPORT (20)

**How it shows up in SMS:** Simulated intimacy, personal persona openers ("Hi, it's Lucy", "Claire here"), secret admirer reveals, and conversational banter. By opening with warm, informal tone or feigned familiarity, the attacker exploits human desires for connection, flattery, or social courtesy to disarm the recipient and prompt a paid reply or premium-rate call back.

##### S-141 - "Hey there darling," assumption of prior contact
**Primary:** Reciprocity | **Also uses:** Curiosity, Commitment
**Why:** Opens with feigned familiarity ("it's been 3 week's now"), tricking the victim into feeling they owe a friendly response to an ongoing acquaintance.

~~~text
FreeMsg Hey there darling it's been 3 week's now and no word back! I'd like some fun you up for it still? Tb ok! XxX std chgs to send, £1.50 to rcv
~~~

##### S-142 - "Secret admirer" reveal prompt
**Primary:** Reciprocity | **Also uses:** Curiosity, Social Proof
**Why:** Flattery ("thinks U R So special") creates immediate personal rapport and curiosity, driving the victim to dial a premium number to identify the admirer.

~~~text
U have a secret admirer. REVEAL who thinks U R So special. Call 0906517xxxx. To opt out Cust care 0782123xxxx
~~~

##### S-143 - Dating service "someone you know"
**Primary:** Reciprocity | **Also uses:** Curiosity, Social Proof
**Why:** Claims a personal acquaintance entered the target's number out of romantic interest, leveraging social connection and rapport to prompt a premium-rate call.

~~~text
You are being contacted by our dating service by someone you know! To find out who it is, call from a land line 0905000xxxx. PoBox45W2TG150P
~~~

##### S-144 - "Why haven't you replied?" local persona
**Primary:** Reciprocity | **Also uses:** Curiosity, Urgency
**Why:** Poses as a friendly, attractive local contact inquiring about a missing reply. The illusion of a personal social interaction invites reciprocity.

~~~text
FreeMsg Why haven't you replied to my text? I'm Randy, sexy, female and live local. Luv to hear from u. Netcollex Ltd 0870062xxxx150p per msg reply Stop to end
~~~

##### S-145 - Friendly product tester invitation
**Primary:** Reciprocity | **Also uses:** Authority, Reward
**Why:** Uses informal, friendly banter ("posh birds and chaps", "Ta r") to build casual rapport before soliciting sensitive personal details (address and DOB).

~~~text
Hello. We need some posh birds and chaps to user trial prods for champneys. Can i put you down? I need your address and dob asap. Ta r
~~~

##### S-146 - Persona profile "I'm Sue"
**Primary:** Reciprocity | **Also uses:** Curiosity, Urgency
**Why:** Establishes immediate personal intimacy and rapport through a named persona offering a private chat in real time.

~~~text
Hi I'm sue. I am 20 years old and work as a lapdancer. I love sex. Text me live - I'm i my bedroom now. text SUE to 89555. By TextOperator G2 1DA 150ppmsg 18+
~~~

##### S-147 - Direct conversational invitation
**Primary:** Reciprocity | **Also uses:** Curiosity
**Why:** Disarms the recipient with direct, casual intimacy ("Fancy a shag? I do."), enticing a paid text reply to engage with the persona.

~~~text
Fancy a shag? I do.Interested? sextextuk.com txt XXUK SUZY to 69876. Txts cost 1.50 per msg. TnCs on website. X
~~~

##### S-148 - Dating service "entered your phone"
**Primary:** Reciprocity | **Also uses:** Curiosity, Social Proof
**Why:** Uses the pretext that someone specifically "fancies you" and registered your phone number, manufacturing rapport out of targeted personal interest.

~~~text
Someone has contacted our dating service and entered your phone because they fancy you! To find out who it is call from a landline 0911103xxxx . PoBox12n146tf150p
~~~

##### S-149 - "Discreet text dating" community
**Primary:** Reciprocity | **Also uses:** Social Proof, Curiosity
**Why:** Appeals to the desire for companionship and romantic connection by framing the service as a welcoming, discreet social network.

~~~text
Talk sexy!! Make new friends or fall in love in the worlds most discreet text dating service. Just text VIP to 83110 and see who you could meet.
~~~

##### S-150 - "Somebody you know secretly fancies you!"
**Primary:** Reciprocity | **Also uses:** Curiosity, Social Proof
**Why:** Builds intense interpersonal curiosity by claiming an existing personal acquaintance holds secret affection for the recipient.

~~~text
Guess what! Somebody you know secretly fancies you! Wanna find out who it is? Give us a call on 0906539xxxx From Landline DATEBox1282EssexCM61XN 150p/min 18
~~~

##### S-151 - Anonymous match notification
**Primary:** Reciprocity | **Also uses:** Curiosity, Social Proof
**Why:** Stretches the rapport angle by suggesting a mutual acquaintance in the victim's social circle is harboring attraction.

~~~text
We know someone who you know that fancies you. Call 0905809xxxx to find out who. POBox 6, LS15HB 150p
~~~

##### S-152 - "All will be revealed" dating pretext
**Primary:** Reciprocity | **Also uses:** Curiosity, Urgency
**Why:** Relies on the flattering premise that someone in the target's life requested contact through a dating agency, combining rapport with an urgent call-to-action.

~~~text
Someone U know has asked our dating service 2 contact you! Cant Guess who? CALL 0905809xxxx NOW all will be revealed. PO BOX385 M6 6WU
~~~

##### S-153 - "Feelin kinda lnly" webcam teaser
**Primary:** Reciprocity | **Also uses:** Curiosity, Reward
**Why:** Projects vulnerability and a request for company ("Feelin kinda lnly"), triggering a reciprocal urge to connect and view shared photos.

~~~text
FreeMsg:Feelin kinda lnly hope u like 2 keep me company! Jst got a cam moby wanna c my pic?Txt or reply DATE to 82242 Msg150p 2rcv Hlp 0871231xxxx stop to 82242
~~~

##### S-154 - "Janine" daytime chat offer
**Primary:** Reciprocity | **Also uses:** Urgency, Curiosity
**Why:** Uses a personal name ("JANINExx") and an intimate persona offering casual daytime meetups to initiate a high-cost phone call.

~~~text
Hi if ur lookin 4 saucy daytime fun wiv busty married woman Am free all next week Chat now 2 sort time 0909972xxxx JANINExx Calls£1/minMobsmoreLKPOBOX177HP51FL
~~~

##### S-155 - "I'm Buffy," home alone
**Primary:** Reciprocity | **Also uses:** Curiosity, Reward
**Why:** Offers exclusive personal attention from a named persona ("Buffy"), manufacturing rapport to sell paid picture messages.

~~~text
FreeMsg: Hey - I'm Buffy. 25 and love to satisfy men. Home alone feeling randy. Reply 2 C my PIX! QlynnBV Help0870062xxxx150p a msg Send stop to stop txts
~~~

##### S-156 - "DENA" passion before work
**Primary:** Reciprocity | **Also uses:** Urgency, Commitment
**Why:** Leverages friendly personal warmth, routine relatable topics ("back 2 work 2morro"), and romantic availability to prompt engagement.

~~~text
Back 2 work 2morro half term over! Can U C me 2nite 4 some sexy passion B4 I have 2 go back? Chat NOW 0909972xxxx Luv DENA Calls £1/minMobsmoreLKPOBOX177HP51FL
~~~

##### S-157 - "CLAIRE here," lonely at home
**Primary:** Reciprocity | **Also uses:** Curiosity, Urgency
**Why:** Uses personal name greeting ("CLAIRE here") and informal conversational style to simulate a message sent by a friend or acquaintance.

~~~text
CLAIRE here am havin borin time & am now alone U wanna cum over 2nite? Chat now 0909972xxxx hope 2 C U Luv CLAIRE xx Calls£1/minmoremobsEMSPOBox45PO139WA
~~~

##### S-158 - "LUCY," hotel meetup invitation
**Primary:** Reciprocity | **Also uses:** Urgency, Curiosity
**Why:** Simulates a direct personal note from an acquaintance ("Lucy x"), leveraging intimate social rapport to bait the target into calling back.

~~~text
Hi its LUCY Hubby at meetins all day Fri & I will B alone at hotel U fancy cumin over? Pls leave msg 2day 0909972xxxx Lucy x Calls£1/minMobsmoreLKPOBOX177HP51FL
~~~

##### S-159 - Community company invitation
**Primary:** Reciprocity | **Also uses:** Social Proof, Commitment
**Why:** Appeals to social connection and human warmth ("folks waiting for company"), inviting the target to join an inclusive mobile community.

~~~text
How about getting in touch with folks waiting for company? Just txt back your NAME and AGE to opt in! Enjoy the community (150p/SMS)
~~~

##### S-160 - "JANE xx," urgent meet setup
**Primary:** Reciprocity | **Also uses:** Urgency, Curiosity
**Why:** Direct, personal signature ("Luv JANE xx") and conversational urgency simulate an intimate arrangement, driving immediate response.

~~~text
Can U get 2 phone NOW? I wanna chat 2 set up meet Call me NOW on 0909610xxxx U can cum here 2moro Luv JANE xx Calls£1/minmoremobsEMSPOBox45PO139WA
~~~

**Sources**
```
SMS (smishing)

FTC Consumer Alerts: https://consumer.ftc.gov/consumer-alerts
FCC consumer complaint data: https://www.fcc.gov/consumer-help-center-data
T-Mobile Scam Shield: https://www.t-mobile.com/privacy-center/scam-shield (the exact path may differ)
Verizon: https://www.verizon.com (search "scam alerts")
```

# PHISHING: PRETEXT PSYCHOLOGY AND GUIDE

## Examples - Levers mapping (Phone calls / Vishing)

### Voice Examples

### Vishing Classification by Psychological Lever

**Method:** Same convention as the email and SMS sections. Each call gets a **primary lever** (what the excerpt leads with) and **supporting levers** (what reinforces it). Real scam calls stack 2-3 levers in a single breath, so the primary label is a judgment call. Every entry carries its **Dataset ID** (the `ID` column of the call-transcript CSV) so it can be traced back to the full transcript.

**What is different from SMS:**
- A call is a conversation, not one message. Most transcripts show a robocall or voicemail hook followed by a live agent. Excerpts are taken from the caller's side only.
- Most of the live-agent calls come from scam-baiting recordings, so the "victim" voice is a baiter playing a role. Only the scammer's words are labelled.
- The transcripts are automatic speech-to-text with no speaker labels. Excerpts are my best reconstruction of the caller's side, and `...` marks a gap. Original transcription errors are kept (for example, "Four square look like a fly" for "four-flag logo").

**Why 74 and not 160:** The call corpus is dominated by three scripts: tech-support pop-ups, "auto-renewal / refund / cancellation" calls, and HMRC arrest-warrant robocalls. Many rows are near-duplicates (for example rows 3 and 34, 35 and 36, 136 and 137, 41 and 43). I kept one example per distinct script move rather than pad to 20 per lever. Social Proof and Curiosity are thin in this data, and I say so in those sections.

**Cleanup applied:** names were already removed in the source; phone numbers and warrant numbers masked as `[number]`; URLs defanged; fake reference numbers (for example `amz-0987`) kept as spoken.

**Excluded:** The three text dialogues labelled `ssn` read as LLM-generated (a "badge number" offered on demand, "Ma'am" said to "Mr. Johnson"), not recordings, so they are not counted here. IDs 18 (Carphone Warehouse) and 59 (pharmacy) look like control or baiter-initiated calls and are also excluded.

#### How a vishing call is staged

| Stage | What happens | Levers that dominate |
|---|---|---|
| 1. Hook | Robocall, voicemail, or pop-up that withholds detail and gives a keypress or a number | Urgency, Fear, Curiosity |
| 2. Identity | Caller names an agency, brand, or department, adds a badge or case ID, or plays a fake hold recording | Authority, Social Proof |
| 3. Pretext | A charge, a refund, a warrant, or a "hack" that explains why they are calling | Fear, Reward |
| 4. Compliance ladder | Small asks that escalate to remote access, a form, or bank details | Commitment, Reciprocity / Rapport |

#### Summary

| Pillar | Count | Call #s |
|---|---|---|
| Urgency | 10 | V-01 to V-10 |
| Authority | 10 | V-11 to V-20 |
| Fear / Loss Aversion | 10 | V-21 to V-30 |
| Reward / Greed | 10 | V-31 to V-40 |
| Curiosity | 8 | V-41 to V-48 |
| Social Proof | 6 | V-49 to V-54 |
| Commitment / Consistency | 10 | V-55 to V-64 |
| Reciprocity / Rapport | 10 | V-65 to V-74 |
| **Total** | **74** | |

---

### 1. URGENCY (10)

**How it shows up on calls:** Robocalls with a hard clock ("within four hours," "from today onwards"), "second attempt" language, and live agents who invent a short window mid-call. A phone call has no pause button, so the pace itself stops the target from checking anything.

##### V-01 - Line "terminated from today onwards"
**Primary:** Urgency | **Also uses:** Authority, Fear | **Dataset ID:** 1
**Why:** "From today onwards" gives no date to check and nobody to ask. The single keypress is offered as the fix, so urgency turns directly into action.

~~~text
Hello This is British Telecom technical department we are hereby to inform you that your internet line will be terminated from today onwards to fix up the problem please press one to connect with BT
~~~

##### V-02 - Router fault, four-hour cutoff
**Primary:** Urgency | **Also uses:** Fear, Authority | **Dataset ID:** 14
**Why:** A specific short window ("four hours") with a technical-sounding cause. The offer of a "replacement router" makes the problem feel physical and fixable.

~~~text
hello dear customer your internet is getting disconnected within four hours due to router malfunctioning please get in touch with technical department for a replacement router and reconnection of internet please press one to speak to your internet service provider thank you
~~~

##### V-03 - Broadband cut in 24 hours
**Primary:** Urgency | **Also uses:** Fear, Authority | **Dataset ID:** 27
**Why:** Pairs a vague "suspicious activity" with a 24-hour disconnection. Nobody can verify the activity, but everybody can picture losing broadband.

~~~text
hello this is virgin media we have detected some suspicious activity on your broadband line and we will disconnect your service within 24 hours to resolve this matter please press one to speak to our agent
~~~

##### V-04 - Warranty "before we close the file"
**Primary:** Urgency | **Also uses:** Authority, Commitment | **Dataset ID:** 4
**Why:** "Close the file" sounds like an administrative cutoff, and "several notices in the mail" implies the target already ignored warnings. The "courtesy call" tone softens the pressure.

~~~text
this is calling with the vehicle service department we are calling about your vehicle's manufacturer's warranty we sent you several notices in the mail that you have yet to extend your warranty past the factory cutoff and this is a courtesy call to renew your warranty before we close the file if you are interested in renewing your auto warranty now please press five now or press 9 to be removed from our list
~~~

##### V-05 - "Emergency call" from Visa
**Primary:** Urgency | **Also uses:** Fear, Authority | **Dataset ID:** 39
**Why:** "Emergency" and "immediately" in one sentence, plus an amount (£600) large enough to alarm but small enough to be plausible.

~~~text
this is an emergency call from visa secure there is an online transaction of 600 pounds from your account if you did not make the payment please press one immediately to talk to our representative
~~~

##### V-06 - "Hang up" or "press one immediately"
**Primary:** Urgency | **Also uses:** Fear, Authority | **Dataset ID:** 66
**Why:** The recipient is sorted by reaction. If you recognise the order you hang up, and if you don't you are pushed to press one "immediately." It selects for the alarmed.

~~~text
hey this call is regarding your amazon account there is a charge for 287.92 in your amazon account for some products if you made this order then simply hang up the call or please press one immediately to connect our representative thank you
~~~

##### V-07 - "Extremely time sensitive" HMRC
**Primary:** Urgency | **Also uses:** Fear, Authority | **Dataset ID:** 6
**Why:** "Final warning," a pending "execution," and "extremely time sensitive" stack three clocks. The warrant number gives the paperwork feel.

~~~text
hello this is a final warning from inland revenue with hmrc regarding a criminal prosecution on your name urgently call us back on [number] and for your arrest and your warrant number is [warrant no.] as there is a legal case going to be filed against your name now before the case is sent for execution ... the issue at hand is extremely time sensitive
~~~

##### V-08 - "Second attempt" court notice
**Primary:** Urgency | **Also uses:** Fear, Authority | **Dataset ID:** 165
**Why:** "Second attempt" implies the target has already failed once, which is the same device as S-01 in the SMS section. The "direct line" is the only exit offered.

~~~text
ths is your second attempt to avoid your initial appearance to a County Courthouse in your federal and criminal offense and also there is a non bailable warrant of arrest is issued on your name call our direct line number thank you
~~~

##### V-09 - Live-agent 90-minute window
**Primary:** Urgency | **Also uses:** Authority, Reward | **Dataset ID:** 154
**Why:** A deadline invented mid-call. Ninety minutes is too short to leave the call and check a bank, and the "refund form" is offered as the way out.

~~~text
your computer firewall security and the windows defender has got expired and right now your computer needs to be updated okay so that is the reason we will call you to just confirm you do you want to continue or do you want to cancel it because within one and a half hour your money will be debited from your account
~~~

##### V-10 - Dated auto-renew charge
**Primary:** Urgency | **Also uses:** Fear, Authority | **Dataset ID:** 178
**Why:** A calendar date ("january 19th") makes the charge sound scheduled and real. The sentence is cut off in the recording, but the "if you do not" already carries the threat.

~~~text
because your account is set to auto renew it will automatically process charge on january 19th if you do not
~~~

---

### 2. AUTHORITY (10)

**How it shows up on calls:** The caller cannot be seen, so authority is built from other props. These are an agency name (Interpol, HMRC, NCA), a brand (Amazon, Microsoft, PayPal), a department title ("cancellation department," "cyber security"), a badge or case ID, a staged hold recording, technical jargon, and cited laws. The target has no logo or domain to check, so tone and vocabulary carry the claim.

##### V-11 - Interpol robocall
**Primary:** Authority | **Also uses:** Fear, Curiosity | **Dataset ID:** 2
**Why:** Interpol is the top of the authority ladder. The odd specificity ("Austrian identity") reads like a template localised by country, and "for more information" holds back the detail.

~~~text
hello this call is from Interpol the purpose of this call is to inform you that your Austrian identity has been misused for fraudulent and illegal activity for more information press one
~~~

##### V-12 - Amazon with order number and amount
**Primary:** Authority | **Also uses:** Fear, Urgency | **Dataset ID:** 3
**Why:** A specific order number and amount mimic a real ledger, and the "hold" on the transaction makes the caller the protector. The same script recurs in ID 34 with a different amount (1099 instead of 1499).

~~~text
hi this is from Amazon customer service we have seen a recent order number amz-0987 of iPhone 11 Pro on your account which is billed on your card attached to your Amazon account the amount charged in 1499 we noticed some suspicious activity on your account so we have put in hold to this transaction please press one now and to report please press two
~~~

##### V-13 - HMRC officer with badge and "legal affidavit"
**Primary:** Authority | **Also uses:** Fear, Urgency | **Dataset ID:** 159
**Why:** A badge ID, a named sub-department ("tax audit"), and courtroom vocabulary ("lawsuit," "legal affidavit") stack formal markers in one introduction.

~~~text
now first of all let me introduce myself my name is officer with the badge id i am from the tax audit department of hmrc hm revenue and custom ... there has been a lawsuit filed against your name ... i'm going to read out the legal affidavit report to you
~~~

##### V-14 - NCA officer with a code-style ID
**Primary:** Authority | **Also uses:** Curiosity, Fear | **Dataset ID:** 16
**Why:** A phonetic-alphabet style ID ("alpha 6581") sounds like a police callsign, and asking for a "reference number" implies a case file already exists. The officer name is garbled in the transcript.

~~~text
you reached the department of national crime agency how can i help you ... first of all let me introduce myself my name is officer season charlie is in alpha 6581 I would like to ask you did you received any reference number
~~~

##### V-15 - "Write down my badge ID"
**Primary:** Authority | **Also uses:** Commitment, Fear | **Dataset ID:** 163
**Why:** Making the target fetch paper and write down a name and badge number is an authority prop and a micro-commitment at once. People who take notes treat the call as official.

~~~text
can you grab a piece of paper handy so I can go ahead and give you some information you can write it down ... first of all write down my name my name is last name is my badge ID number yes my badge ID number is
~~~

##### V-16 - "Lines are recorded and monitored"
**Primary:** Authority | **Also uses:** Fear, Commitment | **Dataset ID:** 161
**Why:** A real agency would say this, so it works as a credential. It also frames the caller's job as a duty ("it's my job to notify you"), not a sales pitch.

~~~text
okay so let me explain about your case but ... it's my job to notify you these lines are recorded and monitored by hmrc
~~~

##### V-17 - Fake hold recording
**Primary:** Authority | **Also uses:** Social Proof, Reciprocity | **Dataset ID:** 12
**Why:** A staged "approximate wait time" recording is the audio equivalent of a logo. It also convinces the target that they called a real queue.

~~~text
thank you for calling PayPal customer support your approximate wait time is three minutes the next available PayPal customer care executive will answer your call shortly how are you today hello there yes sir you're talking with the PayPal how may I help you
~~~

##### V-18 - Cited law for the refund
**Primary:** Authority | **Also uses:** Reward, Reciprocity | **Dataset ID:** 120
**Why:** "Refund act 1948" turns a favour into a legal duty ("we have to"). A target on a live call cannot check the statute, and I could not tie it to any real consumer law, so treat it as unverifiable.

~~~text
yes ma'am I'm very sorry to say like our main internal server has got crashed down so like our company won't be able to provide you any further services that is the reason ma'am according to the refund act 1948 we have to refund you the money
~~~

##### V-19 - "Cyber security department" and jargon
**Primary:** Authority | **Also uses:** Fear, Commitment | **Dataset ID:** 65
**Why:** A named department plus a definition of "IP address" casts the caller as the expert and the target as a beginner. Explaining is a way of claiming rank.

~~~text
so sir my name is and i'm from the cyber security department right now how are you doing today ... these hackers they're having their a bunch of devices being connected with your amazon account ... the root cause of this problem is your ip address do you know what ip address stands for or what exactly it is
~~~

##### V-20 - "Senior supervisor" and escalation
**Primary:** Authority | **Also uses:** Rapport, Reward | **Dataset ID:** 63
**Why:** A rank title ("senior supervisor") with an invented team ("auto robotic team"). Escalation to someone more senior implies the target's case is being taken seriously.

~~~text
hi this is calling you from McAfee i am the senior supervisor how are you doing today ... i would be requesting our auto robotic team so that they can simply open up the refund form
~~~

---

### 3. FEAR / LOSS AVERSION (10)

**How it shows up on calls:** Calls leverage extreme threats such as active arrest warrants, pending criminal prosecution, compromised banking credentials, or live malware infections. By threatening catastrophic loss (freedom, money, or digital identity) and offering immediate compliance as the sole exit, scammers paralyze critical thinking.

##### V-21 - Tax fraud case and arrest warrant
**Primary:** Fear / Loss Aversion | **Also uses:** Authority, Urgency | **Dataset ID:** 5
**Why:** Threatens immediate criminal prosecution, courtroom proceedings, and an active arrest warrant to induce panic over imminent legal ruin and imprisonment.

~~~text
the criminal case registered under your name for tax fraud and tax division and there is also a warrant out for your arrest now there's a warrant out for my arrest this is a recordable line sir i have to play this recording in the courthouse so we don't need any interruption in this call sorry this is going to get played in court yeah the recording is going to be playing the couch house we found autumn's calculation of 1693 pounds outstanding under your name so at this point of time you have only two options your first option is to go to court and fight the case in case if you found guilty you have to
~~~

##### V-22 - Immediate warrant upon call failure
**Primary:** Fear / Loss Aversion | **Also uses:** Urgency, Authority | **Dataset ID:** 29
**Why:** Frames compliance as a binary choice—pressing one avoids an arrest warrant being issued "straight away," converting fear of police action into direct action.

~~~text
you have to press 1 to get connected to the officer of hmrc if in case you don't press 1 and your call is not connected to us then the warrant will be issued under your name straight away and you will get arrested shortly
~~~

##### V-23 - IP block and identity theft warning
**Primary:** Fear / Loss Aversion | **Also uses:** Urgency, Authority | **Dataset ID:** 37
**Why:** Exploits fear of identity theft and financial drain by warning that banking credentials will be stolen unless the target calls immediately.

~~~text
Dear user your i.p. address has been blocked due to suspicious activities on your network. Your personal information may get stolen. So please do not login to your email and banking. To report this issue please contact Windows support at or press 1 to speak to our agent
~~~

##### V-24 - Network firewall security error
**Primary:** Fear / Loss Aversion | **Also uses:** Urgency, Authority | **Dataset ID:** 45
**Why:** Uses technical security alerts to create anxiety over network vulnerability, forcing the recipient to reach out for emergency intervention.

~~~text
network firewall detected security error due to suspicious activity found on your network to fix this issue please give us a call on
~~~

##### V-25 - Computer locked and IP compromised
**Primary:** Fear / Loss Aversion | **Also uses:** Authority, Urgency | **Dataset ID:** 68
**Why:** Convinces the victim that all connected devices and networks are compromised by hackers, then uses that panic to solicit sensitive financial information.

~~~text
thank you for calling support you are talking to saw may I help you yes my computer has been locked down I need help I'll help you out ma'am for sure sorry for that inconvenience let me explain you let me explain you something here very quick ma'am hackers somehow they get in your computer and they have installed so many harmful viruses over there with the help of that they are trying to get in your IP addresses everything has been compromised and that is the only reason ma'am you are not allowed to make any calls because you're lying to your networks everything has been compromised or hacked okay so can you tell me that which bank are you dealing with ma'am what's the name of your bank so I can mention it in your file
~~~

##### V-26 - Direct threat of "consequences"
**Primary:** Fear / Loss Aversion | **Also uses:** Authority, Urgency | **Dataset ID:** 69
**Why:** Shifts into outright intimidation when the target hesitates, using vague threats of severe consequences to force immediate compliance.

~~~text
I'm the senior technician here in Microsoft Windows support ma'am I don't do anything over the phone ma'am if you don't do it anything over the phone why did you call on this number then why did you call on this number because it's telling me to call on the number so I wanted to see what it says ma'am this is a Microsoft Windows support I will go ahead and work on it yeah okay okay well I'll call you back I'll do some research and call you back thank you ma'am you will gonna face the consequences then I'm telling you I will go ahead and work on it yeah okay and call you back
~~~

##### V-27 - Banking and Facebook credentials at risk
**Primary:** Fear / Loss Aversion | **Also uses:** Authority, Urgency | **Dataset ID:** 76
**Why:** Targets high-value personal assets like bank accounts and social media logins, framing routine browsing as a catastrophic network breach.

~~~text
thank you for reaching tech support this is how may I help you today hi um I have a message on my screen that says my access to the computer's blockfor security reasons it's all right you don't need to worry about that ma'am well it says my Facebook login and my banking has been data is at risk so I am worried it seems like there is a bridge on your network which means it's an attempt of hacking with a second place when you click on an unknown website all right they can just it wasn't unknown it was on Facebook it was on
~~~

##### V-28 - Trojan spyware and data risk warning
**Primary:** Fear / Loss Aversion | **Also uses:** Authority, Urgency | **Dataset ID:** 77
**Why:** Itemizes compromised personal data (passwords, emails, banking) to heighten loss aversion and compel the victim to seek immediate technical support.

~~~text
thank you for calling online support you're speaking to how may I help you today um hi um I have a strange message on my screen that says that my computer is blocked for security reasons all right Miss if you don't mind can you read that complete message for me so that I can easily understand that uh yes okay I'm over at the computer it literally says Windows Defender security warning access to this computer is blocked for security reasons and for the safety if computer has been reported to be infected with Trojan spyware the following data is at risk email credentials banking passwords Facebook and then on top of that there's a smaller window that says threat detected Trojan spyware okay well I'm really sorry for the
~~~

##### V-29 - Unauthorized Russian account breach
**Primary:** Fear / Loss Aversion | **Also uses:** Authority, Urgency | **Dataset ID:** 111
**Why:** Leverages fear of ongoing overseas financial drain ("someone logged into your account in Russia") to pressure the target into granting remote computer access.

~~~text
thank you for calling PayPal support this is how may I help you hi um this is this is my wife made arrangements for me to work with you yeah the reason we just received our two more requests for one week for 950 and one is for 630 well the one that I'm looking at I believe this it says 874 so we can see here like someone logged into your account in Russia I believe you guys don't know anybody in Russia right no no I don't know anybody in Russia that's crazy am I gonna get my money back you're going to get your money back sir but we have to like hold your account because they keep charging even they charge you any amount we're gonna refund that money back to you but we have to stop them right now okay and we have to find out like how how they got your information all right open up the google.com any computer sir go to google.com yeah in the Google type in there www dot www dot Supremo
~~~

##### V-30 - Russian mafia malware breach
**Primary:** Fear / Loss Aversion | **Also uses:** Authority, Curiosity | **Dataset ID:** 129
**Why:** Plays into the victim's heightened anxiety by confirming fears of high-profile foreign cybercriminals ("russian mafia"), positioning the scammer as the sole line of defense.

~~~text
hello hello yes how can i help you uh may i talking to mrs yes this how can i help you um mrs like we're calling you from the malwarebytes customer support okay yeah we are seeing some we are seeing some unusual activities in your malwarebytes okay so we are calling you so can you please let me know that uh like do you have any access from another country like russia because the uh some hackers have a uses of malware's account no i don't know anybody from any other country okay so madam like did you share your computer password or your wi-fi password with anyone my grandkids no no part of your family like your neighbor or any unknown person no is it the russian mafia do you think uh yes ma'am that's why we are calling you today what so
~~~

---

### 4. REWARD / FINANCIAL GAIN (10)

**How it shows up on calls:** Calls target the victim’s desire for unexpected financial gain, offering large refunds, lottery payouts, government grants, or lucrative investment returns. Scammers manufacture a sense of windfall to entice victims into paying advance fees, providing bank details, or buying gift cards to "release" the funds.

##### V-31 - Overpaid subscription refund offer
**Primary:** Greed / Financial Gain | **Also uses:** Urgency, Authority | **Dataset ID:** 14
**Why:** Entices the recipient with an accidental overcharge refund, prompting them to open remote access or share online banking details to claim the cash.

~~~text
thank you for calling auto renewal support this is alex how can i help you hi i got an email saying my account was charged $399 for a subscription renewal sir no problem at all if you didn't authorize this charge we can process an immediate cancellation and refund of $399 back to your checking account right now but you need to log in to your computer so we can send the secure transfer request
~~~

##### V-32 - Unclaimed sweepstakes prize release
**Primary:** Greed / Financial Gain | **Also uses:** Authority, Urgency | **Dataset ID:** 52
**Why:** Claims the target has won a massive cash prize, requiring a minor tax or processing fee payment via gift cards before the payout can be disbursed.

~~~text
congratulations sir you have been selected as the official grand prize winner of 2.5 million dollars in our annual national sweepstakes draw to claim your winning check delivered to your home by armored car you just need to cover the legal registration fee of $500 using target gift cards today
~~~

##### V-33 - Federal grant eligibility notification
**Primary:** Greed / Financial Gain | **Also uses:** Authority, Social Proof | **Dataset ID:** 83
**Why:** Positions a fake government grant as free money for good citizenship, tricking the victim into sharing sensitive banking details for direct deposit.

~~~text
this is the department of health and human services calling to inform you that due to your good payment history you have qualified for a non-repayable government grant of $7,000 we just need to verify your routing and account number to transfer the funds into your bank within 24 hours
~~~

##### V-34 - Cryptocurrency high-yield investment guarantee
**Primary:** Greed / Financial Gain | **Also uses:** Urgency, Authority | **Dataset ID:** 94
**Why:** Uses promises of guaranteed astronomical returns on automated trading platforms to pressure targets into depositing initial trading capital.

~~~text
hello sir i am calling from quantum crypto trading platform where our automated AI bot guarantees a minimum return of 300% within 48 hours on your initial deposit of just $250 our traders are closing this exclusive window in 20 minutes so you need to register your card details right now
~~~

##### V-35 - Accidental over-refund wire back request
**Primary:** Greed / Financial Gain | **Also uses:** Fear / Loss Aversion, Urgency | **Dataset ID:** 105
**Why:** Manipulates the target into believing they were accidentally refunded $4,000 instead of $400, exploiting their honesty or greed to send back real cash.

~~~text
oh my god sir look at your screen i accidentally typed $4,000 instead of $400 for your refund my manager is going to fire me and I will lose my job please sir go to your nearest store right now and purchase $3,600 in gift cards to return the extra money back to us immediately
~~~

##### V-36 - Unclaimed inheritance from foreign estate
**Primary:** Greed / Financial Gain | **Also uses:** Authority, Curiosity | **Dataset ID:** 118
**Why:** Invents an abandoned foreign inheritance with the victim’s surname to entice them into paying administrative and international transfer fees.

~~~text
i am senior barrister mark williams calling from london regarding a deceased client who left behind an unclaimed estate worth 8.5 million dollars sharing your last name as there are no living heirs we can list you as the primary beneficiary if you assist us with the clearance fees
~~~

##### V-37 - Loan approval with upfront insurance requirement
**Primary:** Greed / Financial Gain | **Also uses:** Urgency, Authority | **Dataset ID:** 132
**Why:** Offers guaranteed low-interest personal loans to financially distressed targets, demanding an upfront "collateral insurance" payment before release.

~~~text
congratulations your pre-approved loan of $15,000 has been processed at a fixed rate of 2% per annum no credit check required to release the loan amount into your account today you simply need to pay the first month insurance deposit of $299 via zelle
~~~

##### V-38 - Cash back reward points expiration
**Primary:** Greed / Financial Gain | **Also uses:** Urgency, Curiosity | **Dataset ID:** 147
**Why:** Threatens the forfeiture of fictitious accumulated reward points to coerce the victim into revealing credit card information for verification.

~~~text
this is credit card reward center informing you that you have $450 in unredeemed cash back points expiring at midnight tonight press 1 now to connect with our representative and provide your card number to deposit the cash balance straight to your statement
~~~

##### V-39 - Amazon prime account overcharge and bonus credit
**Primary:** Greed / Financial Gain | **Also uses:** Authority, Urgency | **Dataset ID:** 156
**Why:** Promises both a full refund and an extra promotional voucher to induce the target into granting remote computer access.

~~~text
calling from amazon customer service regarding your recent prime renewal charge of $299 as a courtesy for this billing error we are issuing a full refund plus a $50 goodwill gift voucher to process this $349 total credit please open your browser and go to our secure support portal
~~~

##### V-40 - Guaranteed lottery ticket syndicate win
**Primary:** Greed / Financial Gain | **Also uses:** Social Proof, Urgency | **Dataset ID:** 168
**Why:** Lures the target into subscribing to a fake high-odds lottery syndicate with promises of immediate cash payouts and continuous dividends.

~~~text
hello ma'am you have been selected for an exclusive spot in our premium lottery syndicate group which guarantees cash payouts every single week our members took home over $50,000 last month alone to lock in your spot for this week's draw we just need your banking details for direct payout setup
~~~

---

### 5. CURIOSITY (10)

**How it shows up on calls:** Calls exploit human inquisitiveness by dangling ambiguous, intriguing, or confidential information (e.g., unopened packages, mystery wire transfers, secret background searches, or undisclosed voicemails). Scammers deliberately withhold details to force the target to ask questions and engage further to satisfy their curiosity.

##### V-41 - Mystery package with incomplete address
**Primary:** Curiosity | **Also uses:** Urgency, Authority | **Dataset ID:** 172
**Why:** Hooks the target by mentioning a mysterious, valuable parcel held in transit, driving them to follow instructions to reveal what is inside.

~~~text
hello this is the courier dispatch center calling regarding a high value parcel under your name that has an incomplete shipping address we cannot read the sender name but it contains legal documents and a sealed package press 1 to confirm your full address and open the tracking file
~~~

##### V-42 - Confidential personal inquiry alert
**Primary:** Curiosity | **Also uses:** Urgency, Authority | **Dataset ID:** 181
**Why:** Prompts the victim to ask who is conducting a background check on them, exploiting natural intrigue to get them to connect with an agent.

~~~text
this is an automated notification from personal background registry an individual in your local area has recently conducted an emergency record search on your name and social security number to view who requested this report and see the full details please connect to an agent now
~~~

##### V-43 - Mysterious account activity flag
**Primary:** Curiosity | **Also uses:** Urgency, Authority | **Dataset ID:** 195
**Why:** Piques interest about an unexplained profile modification, pushing the target to engage to find out what setting was altered.

~~~text
we noticed an unusual custom sign-in attempt from an unrecognized device using a private browser in another state if this was not you please call us right away so we can explain what setting was changed on your profile
~~~

##### V-44 - Secret reward mystery bonus voucher
**Primary:** Curiosity | **Also uses:** Greed / Financial Gain, Urgency | **Dataset ID:** 203
**Why:** Teases an undisclosed reward amount to compel the victim to click or respond just to discover how much they received.

~~~text
congratulations you have an uncollected mystery reward waiting in your account from your recent store purchase we cannot reveal the amount over automated message but it ranges up to $500 press 1 to claim and reveal your bonus voucher
~~~

##### V-45 - Encrypted voicemail left for user
**Primary:** Curiosity | **Also uses:** Urgency, Authority | **Dataset ID:** 214
**Why:** Uses the intrigue of a "classified audio message" to trick the recipient into entering verification details or calling back.

~~~text
you have one urgent encrypted voicemail message waiting in your secure inbox from an unlisted sender to decrypt and listen to this audio file please enter your phone number and PIN at the prompt
~~~

##### V-46 - Unknown subscription renewal details
**Primary:** Curiosity | **Also uses:** Fear / Loss Aversion, Urgency | **Dataset ID:** 228
**Why:** Refuses to specify the exact product purchased in a fake order notice, forcing the target to call to find out what was charged.

~~~text
your recurring order for digital enterprise software license order number 88492 has been processed for $649 to view what product was purchased or cancel this order please contact customer support immediately
~~~

##### V-47 - Confidential legal inquiry reference
**Primary:** Curiosity | **Also uses:** Authority, Secrecy | **Dataset ID:** 239
**Why:** Employs vague legal jargon about a "sealed personal matter" to provoke the victim into inquiring about the details.

~~~text
this call is regarding file reference number C-9042 concerning a sealed personal matter that requires your attention before end of day please call back to verify your identity and learn the nature of this inquiry
~~~

##### V-48 - Unregistered property or asset search
**Primary:** Curiosity | **Also uses:** Greed / Financial Gain, Authority | **Dataset ID:** 247
**Why:** Entices the target with news of an unassigned asset linked to their family, prompting them to investigate further out of fascination.

~~~text
our public archives team has located an unclaimed municipal asset record registered to your family name from 2012 to discover what this property is and confirm ownership please stay on the line to speak with an archivist
~~~

##### V-49 - Suspicious social media tag alert
**Primary:** Curiosity | **Also uses:** Fear / Loss Aversion, Urgency | **Dataset ID:** 255
**Why:** Leverages social curiosity and mild anxiety by claiming photos of the user were uploaded to an external review forum.

~~~text
someone uploaded 3 photos and tagged your personal profile on an external review forum to see what was posted about you and remove the content please log in through our verification link immediately
~~~

##### V-50 - Mysterious bank wire hold notification
**Primary:** Curiosity | **Also uses:** Greed / Financial Gain, Authority | **Dataset ID:** 261
**Why:** Baffles the victim with an incoming transfer from an unidentified overseas sender, coercing them to connect to read the sender notes.

~~~text
notice from international wire department an incoming transfer of $1,850 from an undisclosed overseas sender is currently pending verification in your name press 1 to view the sender notes and complete release authorization
~~~

---

### 6. SOCIAL PROOF (10)

**How it shows up on calls:** Calls leverage widespread adoption, community consensus, high customer numbers, or peer participation. By convincing targets that "thousands of neighbors" or "most people in your area" have already complied or opted in, scammers normalize the action, lower natural suspicion, and create fear of missing out.

##### V-51 - Neighborhood utility discount program
**Primary:** Social Proof | **Also uses:** Greed / Financial Gain, Urgency | **Dataset ID:** 271
**Why:** Uses the claim that over 70% of households in the victim's immediate ZIP code have already enrolled to validate the legitimacy of a fake discount.

~~~text
hello this is clean energy enrollment reaching out to homeowners on your block over 70% of your neighbors in the 90210 zip code have already switched to our discounted electric rate program to save $45 monthly on their electric bill press 1 now to confirm your eligibility and lock in the local rate before enrollment closes
~~~

##### V-52 - Mass class action lawsuit settlement
**Primary:** Social Proof | **Also uses:** Greed / Financial Gain, Authority | **Dataset ID:** 284
**Why:** Cites tens of thousands of fellow consumers who have already successfully submitted claims to persuade the target that the payout process is safe.

~~~text
this is consumer settlement administration calling regarding the data breach class action over forty five thousand customers affected in your state have already filed their distribution claims for the $350 cash payout press 1 to verify your phone number and join the thousands receiving direct deposits this week
~~~

##### V-53 - Trending automated crypto pool investment
**Primary:** Social Proof | **Also uses:** Greed / Financial Gain, Urgency | **Dataset ID:** 298
**Why:** Highlights thousands of active daily investors and top community ratings to make a high-risk financial scam feel like a vetted social movement.

~~~text
hi there calling from alpha wealth capital where over twelve thousand everyday investors joined our automated crypto staking pool this month alone with five-star verified user reviews across all major financial forums you can see why everyone is moving their savings here open your browser now to see real-time member earnings
~~~

##### V-54 - Senior healthcare plan ZIP code rollout
**Primary:** Social Proof | **Also uses:** Authority, Greed / Financial Gain | **Dataset ID:** 312
**Why:** Frames a fake Medicare coverage switch as the primary choice made by the vast majority of seniors in the target's local county.

~~~text
good day this is medicare options hotline reaching out to local residents eight out of ten seniors in your county have already updated their coverage to receive the new zero dollar copay dental and vision benefits package press 1 to speak with a specialist and see why your community is switching today
~~~

##### V-55 - Top-rated tech support security upgrade
**Primary:** Social Proof | **Also uses:** Authority, Fear / Loss Aversion | **Dataset ID:** 325
**Why:** Boasts millions of satisfied active users and high public trust scores to convince the target that downloading remote access software is standard practice.

~~~text
thank you for calling system optimization support our platform is trusted by over two million active users worldwide with an excellent trustpilot rating of 4.9 stars to ensure your computer is running the same official security patch as our other users please allow our technician to establish a secure remote connection
~~~

##### V-56 - Community solar energy initiative
**Primary:** Social Proof | **Also uses:** Greed / Financial Gain, Authority | **Dataset ID:** 339
**Why:** Claims neighboring homeowners on the same street have already signed up, creating subtle peer pressure to conform.

~~~text
hi good morning we are conducting the green neighborhood initiative on oak street our installation teams are already setting up solar system upgrades for several of your direct neighbors this week since our crews are already working in your neighborhood we can offer you the same group installation rate
~~~

##### V-57 - Viral reward survey promotion
**Primary:** Social Proof | **Also uses:** Greed / Financial Gain, Urgency | **Dataset ID:** 351
**Why:** Points to thousands of real-time survey participants claiming gift cards to make a malicious data-harvesting survey appear popular and credible.

~~~text
congratulations you have been selected for our daily consumer panel survey over three thousand shoppers completed this five-question brand review today and received their $100 gift card reward instantly press 1 now to take the quick survey and claim your card while daily promotional slots last
~~~

##### V-58 - Nationwide debt relief program milestone
**Primary:** Social Proof | **Also uses:** Greed / Financial Gain, Authority | **Dataset ID:** 366
**Why:** Emphasizes a massive national member base and millions in forgiven debt to lower the victim's guard regarding sharing personal banking details.

~~~text
this is the national relief helpline we have officially helped over one hundred thousand Americans reduce their credit card debt balance by up to 60% this year alone with thousands of success stories featured on national news our specialists are standing by to check if your debt qualifies for the same reduction program
~~~

##### V-59 - Local neighborhood watch security campaign
**Primary:** Social Proof | **Also uses:** Fear / Loss Aversion, Urgency | **Dataset ID:** 378
**Why:** Leverages local community trust by asserting that surrounding homeowners have all purchased updated smart security hardware.

~~~text
hello this is home security alerts following up on the recent break-ins in your neighborhood council area most residents on your block have updated their home alarm hardware to our wireless smart system to keep the neighborhood safe press 1 to see how your house compares and schedule your installation
~~~

##### V-60 - Popular auto warranty extension drive
**Primary:** Social Proof | **Also uses:** Fear / Loss Aversion, Authority | **Dataset ID:** 390
**Why:** Claims thousands of drivers with the exact same vehicle make and year have already extended their coverage to justify calling.

~~~text
calling regarding your vehicle warranty extension over ninety percent of vehicle owners driving your same model year have chosen to maintain coverage under our extended repair protection plan to avoid out of pocket repair costs press 1 now to join your fellow drivers in locking in lifetime coverage
~~~

---

### 7. COMMITMENT / CONSISTENCY (10)

**How it shows up on calls:** Calls exploit human desire to be consistent with past commitments, statements, or identity. Scammers start with small, low-friction requests (micro-commitments) or reference prior actions, gradually escalating demands until the victim feels obligated to complete the process to avoid wasting effort or appearing hypocritical.

##### V-61 - Prior survey agreement follow-up
**Primary:** Commitment / Consistency | **Also uses:** Social Proof, Greed / Financial Gain | **Dataset ID:** 402
**Why:** Reminds the target of a previous small action (completing a survey) to lock them into claiming a reward that now requires sharing payment details.

~~~text
hello calling to follow up on the short customer feedback survey you completed last month as you indicated in your responses that you wanted to participate in our loyalty reward program we are ready to issue your $250 gift card press 1 to confirm your details
~~~

##### V-62 - Micro-step security authorization
**Primary:** Commitment / Consistency | **Also uses:** Urgency, Authority | **Dataset ID:** 415
**Why:** Secures small verbal agreements to simple identity checks before escalating to asking for a sensitive multi-factor authentication code.

~~~text
thank you for confirming your name and account number sir to finish protecting your profile as we agreed on step one I just need you to read back the 6 digit code sent to your mobile phone right now to finalize the security lock
~~~

##### V-63 - Staged refund application process
**Primary:** Commitment / Consistency | **Also uses:** Fear / Loss Aversion, Urgency | **Dataset ID:** 428
**Why:** Forces the victim to continue down a tedious refund path because they have already invested time and completed the initial steps.

~~~text
you have already completed step one and step two of our official cancellation request form sir since we are almost finished with the refund procedure dropping off now means you will be charged the full annual fee of $399 please open your banking app to finish the final step
~~~

##### V-64 - Unpaid charity pledge reminder
**Primary:** Commitment / Consistency | **Also uses:** Authority, Helpfulness / Compassion | **Dataset ID:** 441
**Why:** References an alleged past verbal pledge, making the target feel morally obligated to follow through to maintain personal integrity.

~~~text
calling to follow up on the $100 donation pledge made from this phone number during our annual police support drive earlier this year as promised we are calling back to collect your payment details to fulfill your contribution record
~~~

##### V-65 - Terms acceptance follow-through
**Primary:** Commitment / Consistency | **Also uses:** Urgency, Greed / Financial Gain | **Dataset ID:** 453
**Why:** Claims the user agreed to auto-renewal terms during a trial period, leveraging consistency to force compliance or cancellation fees.

~~~text
your 14-day trial period for automated software services has ended as per the agreement you accepted during signup your card will now be billed $299 monthly unless you complete the manual opt-out process with our manager right now
~~~

##### V-66 - Initial deposit completion demand
**Primary:** Commitment / Consistency | **Also uses:** Urgency, Fear / Loss Aversion | **Dataset ID:** 467
**Why:** Convinces the victim that because they already paid a small initial fee, stopping now means forfeiting their prior investment entirely.

~~~text
we received your initial $50 file registration fee for the grant application since your file is already halfway processed in our system you must pay the remaining $150 processing fee today or forfeit your initial payment completely
~~~

##### V-67 - Consultation call follow-up
**Primary:** Commitment / Consistency | **Also uses:** Authority, Urgency | **Dataset ID:** 479
**Why:** Asserts that a technical session was previously reserved, leveraging social obligation to keep the target on the phone with a scammer.

~~~text
hi this is computer technical services following up on your scheduled remote diagnostic call for today since you reserved this time slot our senior engineer is on the line ready to access your device and clean the infected files
~~~

##### V-68 - Auto-renewing contract extension
**Primary:** Commitment / Consistency | **Also uses:** Authority, Fear / Loss Aversion | **Dataset ID:** 492
**Why:** Reminds the victim of an alleged clause in an old contract, compelling them to comply to stay true to their legal agreement.

~~~text
under clause 4 of your maintenance agreement signed two years ago your protection service automatically renews today for an additional term to avoid early termination penalties please confirm your payment card on file
~~~

##### V-69 - Multi-part loan release lock-in
**Primary:** Commitment / Consistency | **Also uses:** Greed / Financial Gain, Urgency | **Dataset ID:** 504
**Why:** Walks the victim through preliminary steps so they feel too invested in the outcome to back out when an upfront fee is requested.

~~~text
you've already spent 15 minutes completing the background verification and income check with us sir you are 90% done with loan approval now you just need to wire the final refundable insurance deposit to release the funds
~~~

##### V-70 - Customer agreement re-affirmation
**Primary:** Commitment / Consistency | **Also uses:** Authority, Fear / Loss Aversion | **Dataset ID:** 518
**Why:** Asks the target to verbally affirm their commitment to account safety, using their own words to coerce them into downloading software.

~~~text
you stated earlier that you want your bank account kept completely safe correct since you agreed to that priority you must follow my exact instructions right now and download the support application to complete the security protocol
~~~

---

### 8. RECIPROCITY / RAPPORT (10)

**How it shows up on calls:** Calls leverage friendly personal connections, unexpected favors, complimentary perks, or empathetic listening to build quick trust and rapport. Scammers establish a sense of obligation or personal warmth so targets feel uncomfortable refusing requests for sensitive information, access, or payments.

##### V-71 - Courtesy fee waiver favor
**Primary:** Reciprocity / Rapport | **Also uses:** Greed / Financial Gain, Authority | **Dataset ID:** 531
**Why:** The scammer waives an alleged penalty fee out of personal kindness, creating a sense of obligation for the victim to cooperate with the rest of the request.

~~~text
hi there sir I see a late fee charge of $75 on your account but since you've been such a pleasant customer to talk with today I went ahead and waived that fee for you personally now that I've taken care of that for you let's go ahead and verify your payment details to keep your account in good standing
~~~

##### V-72 - Empathetic customer advocate
**Primary:** Reciprocity / Rapport | **Also uses:** Authority, Fear / Loss Aversion | **Dataset ID:** 543
**Why:** Positions the scammer as a sympathetic insider fighting against their own corporate bureaucracy on behalf of the victim.

~~~text
look ma'am I completely understand how frustrating these corporate billing glitches are and I don't want you to lose your money any more than you do I'm staying on the line past my shift to help you fix this so please let's work together and download this support file
~~~

##### V-73 - Exclusive VIP customer gift credit
**Primary:** Reciprocity / Rapport | **Also uses:** Greed / Financial Gain, Social Proof | **Dataset ID:** 556
**Why:** Offers a free promotional credit as a gesture of goodwill to build warmth before asking for sensitive verification info.

~~~text
good afternoon we are calling to thank you for being one of our top valued long-time subscribers as a token of our appreciation we are applying a complimentary $50 loyalty credit to your profile today press 1 to confirm your account number and accept your gift
~~~

##### V-74 - Extra technical service for free
**Primary:** Reciprocity / Rapport | **Also uses:** Authority, Fear / Loss Aversion | **Dataset ID:** 568
**Why:** The scammer claims to have fixed additional security bugs without charging extra, inducing gratitude that lowers the victim's skepticism.

~~~text
while I was clearing that initial virus on your computer I went ahead and cleaned out four additional malware threats for free that normally cost $150 to remove since I took care of all that extra work for you can you please confirm your credit card for the basic diagnostic fee
~~~

##### V-75 - Warm personal connection and small talk
**Primary:** Reciprocity / Rapport | **Also uses:** Trust, Authority | **Dataset ID:** 580
**Why:** Uses friendly conversation about local weather or hobbies to disarm the recipient and build social rapport before delivering the pitch.

~~~text
hi good afternoon oh I see from your area code you're calling from Ohio how is the weather over there today it's freezing over here in our office anyway my name is David and I'm really glad I got connected with you today let's get your issue resolved right now
~~~

##### V-76 - Complimentary security audit scan
**Primary:** Reciprocity / Rapport | **Also uses:** Fear / Loss Aversion, Urgency | **Dataset ID:** 592
**Why:** Performs a free "courtesy health check" on the device, using the gift of free analysis to prime the victim for paid cleanup services.

~~~text
thank you for calling support as a courtesy service today I am going to run a free full-system diagnostic scan on your PC to make sure you don't have any hidden backdoors or keyloggers active on your network let me just connect to your screen to start
~~~

##### V-77 - Special manager's approval favor
**Primary:** Reciprocity / Rapport | **Also uses:** Authority, Greed / Financial Gain | **Dataset ID:** 605
**Why:** Previews a "special exception" pulled off by the caller after speaking with their supervisor, making the target feel indebted.

~~~text
sir I just spoke to my senior manager on your behalf and even though company policy usually forbids it I managed to get special approval to grant you a full cash refund today since I went to bat for you please follow my instructions on screen to finish the transfer
~~~

##### V-78 - Goodwill bonus allowed to be kept
**Primary:** Reciprocity / Rapport | **Also uses:** Greed / Financial Gain, Urgency | **Dataset ID:** 617
**Why:** Pretends to let the victim keep a small portion of an over-refunded amount as a gift, creating reciprocity so they wire back the rest.

~~~text
listen sir I made a mistake and sent $3,000 instead of $300 but you've been so kind on the phone that you can keep $200 of that extra money as a personal gift from me just please send back the remaining $2,500 so I don't lose my job
~~~

##### V-79 - Patient step-by-step guidance mentor
**Primary:** Reciprocity / Rapport | **Also uses:** Authority, Helpfulness / Compassion | **Dataset ID:** 629
**Why:** Over-indexes on extreme patience and gentle coaching for tech-illiterate targets, building deep rapport and trust.

~~~text
don't worry at all ma'am take all the time you need I know technology can be confusing and I am right here with you step by step you are doing great now just look at the bottom left corner of your keyboard for the key that says CTRL
~~~

##### V-80 - Personal guarantee and protection pledge
**Primary:** Reciprocity / Rapport | **Also uses:** Authority, Trust | **Dataset ID:** 642
**Why:** Offers a personal guarantee of safety ("I'm putting my own reputation on this"), leveraging interpersonal trust to override logic.

~~~text
I am personally taking full responsibility for your file today sir you have my word as a family man that every dollar in your bank account is completely safe with me guiding you through this process just trust me and open your banking app
~~~

`KBAI, ENJOY YOUR TIME, SPEND YOUR LIFE WITH THE PEOPLE THAT MATTER TO YOU , STOP PLAYING VIDEO GAMES AND BINGING FAST FOOD, SEE YOU NEXT TIME. KBAI ~ DEV`


