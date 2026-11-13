# Dost: idea notes

For the India Runs on Voice hackathon (see `hackathon-details.md`). Captured 10 Nov 2026 from our discussion. This is the full vision; the Idea submission (50–500 words) uses only the core.

## One line

Dost is a voice friend for older people and blind or low-vision people in India. It remembers what the doctor, the reports and the medicines say, reminds them in their own language, and does what a screen would otherwise stop them doing. Their families pay for it.

## Status

**Brainstorm, not final.** Everything below is a candidate. Nothing is committed to being built yet.

## Priority (core / next / later)

**Core:** what the Idea submission and a first version are about.
- A voice friend in the user's own language, talking naturally, eyes-free
- Health memory: doctor visit notes, reports read and compared, medicine reminders, diet nudges
- "Yaad rakhna" reminders
- Dost speaks first: the quiet-day check-in, and telling the family if there's no reply
- Knows its owner's voice
- Read anything through the camera, with "thoda left" guidance
- Call family by name or relation
- Daily family update (this is what the payer sees)

**Next:** strong, after the core works.
- SOS by voice, fall check, the bystander emergency flow
- Scam shield
- Medicine running out
- "Main kahan hoon", the no-reply decision tree, lost or stolen phone alerts
- Call Dost from any phone (a phone number)

**Later:** nice, but not where Dost wins.
- Opening apps and YouTube by voice
- Life stories
- Pension and Jeevan Pramaan reminders
- Colours and expiry dates
- Hand-offs to Be My Eyes and Lookout
- Students (lecture notes)

## First real user

- **Vinay's grandmother:** the first older user. She tests the app fully, and her reactions are recorded.
  - 70, lives in Bengaluru, has diabetes, check-up every 3 months.
  - Goes to the doctor alone and manages her medicines alone.
  - Her son lives with her but works long shifts as a security guard and doesn't get involved in her care. **He has diabetes too.** One Dost per household, or one per person, is a question for later.
  - **Her real problem:** her memory is hazy. She often forgets which tablet is for what, and when to take it. So the medicine mix-up check ("Ye kis liye hai?") is core for her, not a later feature.
  - **Speaks only Kannada.** So the hackathon's second language is Kannada (Hinglish + Kannada). This also matches the hackathon's Bengaluru brief, which is Kannada + English.
  - Still unknown: number of medicines.
- **Blind users:** none yet. Find one or two to talk to before building the blind-specific features.

## Mission

**Dost fills the gap between what a sighted, younger person does without thinking and what an older or blind person can't.** Every feature should be explained by that sentence.

## Users

- **Older people:** weak eyesight, little or no English, forget things, may not have a smartphone.
- **Blind and low-vision people:** cannot see the screen at all.
- Matches Track 5 (Everyday access), whose own example is "hands-free apps for older users and people with disabilities". The health features also cover Track 2 (Healthcare).
- Students (recording lectures into notes) are the "where Dost grows next" line, not part of this submission.

## Design rules

- **Eyes-free:** every feature must work with your eyes closed. Voice in, voice out, no step needs a tap the user can't see.
- **Talks like a real friend:** natural speech, warmth, filler words, Hinglish. This is the key part.
- **Multilingual, code-mixed:** understands people mixing languages mid-sentence (Hinglish, Kannada-English) and replies the same way. Hackathon scope: Hinglish plus one regional language done really well.
- **Wakes on a voice command** with a name the user chooses, while the app is open or switched on. Not always listening in the background (Android requires a permanent notification for that, iPhone doesn't allow it, and it drains the battery).
- **Never guesses.** If something is unclear (a faded receipt, handwriting), Dost says so and asks for a better look.
- **Never doses or diagnoses.** Dost repeats what the doctor said and gives general friend advice ("kam chawal, zyada chapati"). Nothing more.
- **Use specialists, don't rebuild them.** Where good tools exist, Dost hands off to them by voice.

## Features

### Core: Dost remembers, so you don't have to
1. **Goes to the doctor with you, and the doctor briefs Dost directly.** The doctor talks to Dost like a nurse or caretaker: "Metformin 500, raat ko khane ke baad, teen mahine. Har mahine sugar check." Dost **repeats it back** ("Raat ko ek goli, khane ke baad, teen mahine tak. Sahi?") so the doctor confirms Dost understood correctly before leaving. From then on Dost manages it: medicine reminders, answering "Doctor ne kya bola tha?", and the next check-up date. Dost still never changes anything on its own; the doctor is the only source of medical instructions. If the doctor isn't comfortable talking to Dost, Dost just listens to the visit (with permission) as a fallback.
   - **Fake doctor protection: the family confirms every visit** (Vinay's design; replaces a voice-locked "doctor mode", which was more complex):
     1. **Dost takes in whatever the doctor says.** No special mode to switch on.
     2. **Dost photographs the parchi** (the prescription slip) and the medicine strips, and checks they match what was said. A mismatch is flagged.
     3. **When Grandma gets home, Dost sends the son everything:** photos of the parchi and medicines, plus a short summary of what the doctor said. He confirms ("haan, sahi hai") or calls to check.
     4. **Until he confirms:** reminders for medicines that are on the parchi start anyway, because tonight's tablet can't wait for a busy son. They're marked "awaiting family check" and Dost keeps nudging him. **Risky changes wait for his yes:** stopping a medicine, doubling a dose, or anything not on the written parchi.
     5. **No son available:** any guardian set up at the start can confirm.
     6. **What the son can actually judge:** he isn't a doctor, so he can't tell if a medicine is medically right. He *can* tell whether the visit really happened, whether it's her usual doctor, and whether something looks off. And he can call the clinic. That's enough to catch a fake doctor.
     7. **Later:** doctors register with their medical registration number, and instructions from verified doctors are trusted more.
   - **Honest limit:** a determined person with a fake prescription could still get through. But without Dost, anyone can already tell Grandma anything and nobody would know. With Dost, every change is written down and the family sees it.
2. **Reads and remembers reports.** Camera on the report → Dost explains it in your language and compares it with past reports: "Sugar 180 hai, pichhli baar 150 tha."
3. **Medicine reminders** based on what the doctor said: "12 baj rahe hain, dawai le le."
   - **Medicines by how they look, not by name** (Vinay's insight from Gangamma). Older people don't remember "Metformin"; they remember "the small white round one", "the big red one", "the long one with a line in the middle". When a new medicine is given, Dost photographs the tablet and the strip once, and from then on talks about it the way she does: "Aa chikka bili gundu maatre tagolli."
   - For blind users, the same idea works by touch: size, shape, a line or groove across the middle, the feel of the strip.
   - Dost also learns the names she uses ("sugar maatre", "BP maatre") and uses those, not the chemical names.
   - **"Is this the right one?"** After a reminder, she can show the tablet or strip to the camera and ask "Idu sarina?". Dost confirms ("Haudu, adhe") or corrects her ("Illa, adu BP maatre. Bili gundu maatre tagolli"). This is the last check before she swallows it, and it directly fixes her real problem of mixing up tablets.
   - **Honest limit:** many tablets look alike (lots of small white round ones). Dost should check the strip's printed name too, not just the tablet's look, and say "Strip torisi" (show me the strip) when it can't tell from the tablet alone.
4. **Diet nudges** as a friend would, never as medical orders.
5. **"Yaad rakhna."** "Arey yaar, ye yaad rakhna, baad mein ye karna hai." Dost confirms a time ("kal subah 9 baje yaad dila doon?"), asks if none was given ("kab yaad dilaun?"), and is honest when it can't know ("jab beta aaye" → offer a time instead). Also: "chashma kahan rakha tha?", if they told Dost earlier.

### Read anything aloud (same capability as reports)
- Receipts: items, prices, total. Can add up the items and check the shop's total.
- Medicine strips, letters, bills ("15 tareekh tak bharna hai").
- Because Dost remembers: "Is mahine dawai pe kitna kharch hua?"
- **Camera guidance for blind users:** like a friend on a video call. Dost first finds the *shape* of the paper in the camera (the way document scanner apps find a page's edges and crop out the background), without reading it yet. Then it guides the hand: "thoda left… paas la… haath rok… haan, bas." When the whole page is in frame, still and sharp, Dost takes the picture **by itself** (a blind user can't find a shutter button), crops away the background, and only then reads it.

### The phone, by voice
- **Call anyone by name or relation:** "Bete ko phone laga", "Pappu ko lagao". Dost learns who "beta" is once. **Must have.**
- Open apps, including YouTube with a search ("bhajan dikhao").
- Open a WhatsApp chat or call.
- **"Main kahan hoon?"** → "Tu MG Road pe hai, Café Coffee Day ke saamne." "Bete ko bata de main kahan hoon" sends the location to family.
- **Call Dost from any phone:** a phone number, so someone who forgot their phone, or has no smartphone, can still reach Dost.

### Dost speaks first
Dost is not a tool that waits to be called. A friend notices when you've gone quiet.
- **Quiet-day check-in:** if the person hasn't talked to Dost all day, Dost starts the conversation in the evening: "Kya hua yaar, aaj baat hi nahi ki? Sab theek hai?"
- **If there's no reply:** Dost tries again a little later. If there's still nothing, it tells the family: "Papa ne aaj se baat nahi ki, ek baar phone kar lo." (The time limit before the family is told is set with the elder and the family.)
- **Medicine reminders** and "yaad rakhna" reminders also come from Dost first, in its friend's voice.
- Built with Android's exact alarms (the permission medicine-reminder apps use), so Android's battery saver doesn't delay them.

### Safety
- **SOS by voice:** "Dost, bachao" or "main gir gaya" → Dost calls family, sends the location, and keeps talking to the person until someone answers.
- **Fall check:** the phone notices a sudden jolt followed by stillness and asks "Tu theek hai?". No answer → SOS. Needs care to avoid false alarms.
- **Scam shield:** Dost cannot listen to phone calls (Android blocks it), but the person can tell Dost afterwards: "Ek aadmi bola bank se hai, OTP maang raha tha" → "Ruk! Bank kabhi OTP nahi maangta. Kisi ko mat batana. Bete ko bata doon?" Covers fake bank calls and "digital arrest" scams. Strong demo moment.
- **Medicine mix-up check:** "Ye kaunsi goli hai?" → "Ye BP wali hai. Sugar wali dusri hai." Uses the same camera reading.

### Family
- **Daily update for the family,** only with the elder's agreement: "Papa ne aaj dawai le li, doctor ke paas gaye the, mood achha tha." This is what the paying customer actually sees.
- **Medicine running out:** Dost counts doses taken: "Sugar ki goli 3 din mein khatam, bete ko bolun?"

### Company
- **Just talking:** Dost asks about their day and remembers what matters ("Pote ka exam Monday ko tha na, kaisa gaya?").
- **Life stories:** Dost asks about childhood, the village, the wedding, and saves the stories as recordings for the grandchildren.

### Everyday tasks
- **Pension and government reminders:** e.g. Jeevan Pramaan (the yearly proof-of-life certificate pensioners must submit, due in November). Covers the civic track.
- **Small things for blind users:** "Ye kurta kis rang ka hai?", "Doodh ki expiry kab hai?" Same camera reading.

### Knows its owner's voice
Like a real dost: anyone in the room can chat with him, but some things he only discusses with you.
- **Setup:** the owner says a few sentences once, and Dost learns their voice (called **speaker verification**: checking *who* is speaking, not *what* they say).
- **Open to anyone:** casual chat, "what time is it", opening YouTube. Other people are background characters, just like with a real friend.
- **Owner only:** health records and reports, the doctor's instructions, "yaad rakhna" notes, the family update, changing settings, and anything that counts as the owner being OK.
- **The quiet-day check counts only the owner's voice.** If someone else talks to Dost all day but the owner doesn't, Dost still checks in and tells the family if needed. A caretaker or stranger can't make everything "look fine".
- **When Dost isn't sure** whether it's the owner (a cold, a noisy room, an unfamiliar voice), it asks something only the owner would know, e.g. "Pote ka naam bata?". It doesn't just refuse.
- **Honest limit:** voice checks are not perfect. AI voice cloning can fool them, and voices change with illness and age. So voice is one check among several, never the only protection for anything serious; for those, add a second check (a question, or the phone's own screen lock).

### Emergency: a bystander can talk to Dost
If the owner collapses or has an accident, the people around them are the only ones who know what happened.
- **Anyone can start it:** a stranger picks up the phone and says "Emergency hai" or "Ye behosh ho gaye". No owner voice needed; this is the one private-ish thing open to anyone.
- **Dost acts immediately:** calls family, sends the location, and offers to call **112** (India's emergency number) or **108** (ambulance).
- **Dost asks the bystander what happened** ("Kya hua? Gir gaye? Saans le rahe hain?") and passes the answers to the family, so they hear it from the spot, not secondhand.
- **Dost tells the bystander what helps:** the emergency information the owner chose at setup, e.g. "Inhe sugar hai, BP ki dawai lete hain, blood group B+". Nothing else from the private records.
- **Misuse:** someone faking an emergency gets only that limited information, and the family is alerted anyway. A false alarm costs a phone call. A missed real one costs much more.
- **To verify:** whether Dost can be spoken to while the phone is locked. Android's own Emergency information (on the lock screen) and the emergency call button work while locked; a normal app needs special setup. Check before promising.
- Works together with the fall check: a jolt, then no answer from the owner → Dost asks out loud "Koi hai? Inhe madad chahiye" to anyone nearby.

### When the owner doesn't reply (Vinay's decision tree)

```
Dost asks the owner something
├── Owner replies → all good
└── No reply
    ├── Phone is at home → no alarm (he's around, or the phone is just lying there). Retry later.
    └── Phone is somewhere else
        ├── Phone is moving (in a pocket, walking) → he probably didn't hear. Retry louder, then later.
        └── Phone is still, away from home, no reply
            ├── He forgot the phone there, or
            └── He is unconscious next to it
            → Either way, the action is the same: Dost calls out "Koi hai?" for anyone nearby
              (→ emergency flow), and sends the live location to the guardians.
```

- **Theft and lost phones:** the same tree catches a phone left in an auto or a shop. Guardians get the live location before anyone has noticed it's missing. If the phone starts moving with a voice that isn't the owner's, Dost alerts the guardians at once.
- **Honest limit:** a thief who switches the phone off or removes the SIM stops Dost too. So Dost sends the location **early**, and once more when the battery is about to die. Google's Find My Device can still find some phones while they're offline; Dost can point the family to it.
- **What's new compared with Find My Device:** it only helps once someone notices the phone is gone and goes looking. Dost notices by itself and tells the family first.
- **Privacy:** location goes to guardians only when this tree triggers (or in an emergency), never all the time, and only with the owner's agreement at setup.

### Hand-offs to specialists
- Live volunteer help → open **Be My Eyes**.
- Currency notes, scene description → open **Google Lookout**, or use the same vision AI they use (Google and OpenAI models) directly.
- To verify: whether Be My Eyes, Lookout or Seeing AI offer a public API. Believed not; not yet checked.

## Feasibility on Android

| Easy (no special permission) | Hard (accessibility permission, strict Play Store review) |
|---|---|
| Calls by name or relation | Tapping buttons inside other apps |
| Opening apps, YouTube with a search | Typing into other apps |
| Alarms and reminders | Reading everything on screen |
| Opening a WhatsApp chat or call | Sending WhatsApp messages without a tap |
| Reading documents through the camera | |
| Location ("main kahan hoon") | |

### When the phone is locked (from memory, to verify with a small test app)

| Works while locked | Needs special setup | Not possible / unsure |
|---|---|---|
| Dost speaking out loud (reminders, quiet-day check-in) | Keep listening after the screen locks, *if Dost was switched on before* (a running service with a permanent notification) | A custom wake word as cheap as "Hey Google" (Google uses special low-power hardware other apps can't use) |
| Alarms firing on time | Showing Dost's screen on top of the lock screen (alarm and calling apps do this) | Camera reading while locked (unsure) |
| | Becoming the phone's **default assistant app**, so a long press of the power button opens Dost, even when locked | |

- **Privacy rule:** while the phone is locked, Dost gives private things only to the owner's verified voice. A locked phone in someone else's hands should only get casual chat and the emergency flow.
- Battery: listening all the time while locked drains it. Better: the owner switches Dost on, or opens it with the power-button long press.

**Camera guidance** needs fast, on-phone detection of the document's edges, not a round trip to a cloud AI, or the "thoda left" comes too late.

**Hard limits:** faded thermal receipts and handwritten prescriptions are hard to read even for good AI. Recording a doctor needs their consent.

## Speech technology (to compare before building)

- **Sarvam AI:** Indian company built for Indian languages
- **Bhashini:** the government's free language platform
- **ElevenLabs:** already used for demo voices

Understanding speech works reasonably well in major Indian languages; speaking naturally like a friend is the hard part outside Hindi and English. Check current coverage and quality, not memory.

## Competitors

- **Elderly companions:** Saathi AI (Hindi voice companion for 60+), Saathi (open source, calls elders on an ordinary phone), Near Ones (AI check-in calls with medicine reminders, ₹499/month), and hackathon builds like Helpy AI and DawAI. No confirmed Indian company doing this at scale. Most stop at reminders and chat.
- **General assistants:** Gemini Live (Hindi since 2024, camera), ChatGPT voice (Hindi voices since Oct 2026). Free, broad. We don't compete on breadth.
- **Blind users:** TalkBack, Google Lookout, Microsoft Seeing AI, Be My Eyes. We hand off to them rather than compete.
- **What's different about Dost:** it remembers across time (doctor, reports, medicines, tasks) and talks like a friend in the user's own mixed language.

## Who pays

Vinay's model: whoever cares for the user pays.

| User | Who pays |
|---|---|
| Older person at home | Their sons and daughters |
| Older person in an old-age home | The home (per resident), or the government for government-run homes |
| Blind adult who works | Themselves |
| Blind child or young person | Their parents |
| Blind older person | Their sons and daughters |

- **Note on government:** government buying is slow and needs tenders (formal bidding processes), so it's a later channel. Private old-age homes paying per resident are faster to reach.
- **Families:** children in cities or abroad paying for a parent. Benchmark: Near Ones charges ₹499/month for less.
- **What makes them pay:** the daily family update, SOS, the quiet-day alert and the scam shield. These are what the son sees and what lets him worry less.
- Market signal: India has roughly 100 million people with diabetes (to verify before the pitch).
- Still open: price, first paying customer, path to them.

## Hackathon reality

- Round 1 is written work: Idea (25 points, AI-checked), Prompt, Impact. 100 points total.
- The Idea check scores **voice being essential** first, then **how specific the problem is**. Lead with one person and one problem (health memory); other features follow in a line or two.
- Round 2 (27–29 Nov) has one shared problem statement for everyone, so we may not build Dost itself in this hackathon.
- Submit the Idea before 13 Nov so Activity 2 unlocks on time.
