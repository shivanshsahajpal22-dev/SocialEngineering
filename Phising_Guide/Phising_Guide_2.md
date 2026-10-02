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

