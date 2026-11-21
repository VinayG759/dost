# Research: which voice providers for Dost (14 Nov 2026)

Question: is Sarvam good enough, or is there a more natural, colloquial voice for older users? **No decision yet.** The voice will be picked by ear, by Gangamma.

Each claim is marked: **[verified]** with a source, **[vendor claim]**, or **[our reasoning]**.

## Plivo
- Plivo is a phone-call and voice-agent *platform*, not a voice maker. Its voices come from **ElevenLabs or Cartesia**, and its speech-to-text from Deepgram or Sarvam, or you bring your own. **[verified: plivo.com/ai-agents]**
- The page text we fetched showed an English dental-clinic call, but **Vinay heard a Hindi call** on the landing page (it may differ by region). Vinay's ear wins; the Hindi sample is the **naturalness benchmark** Dost should reach.
- Plivo doesn't name which voice provider its default agent uses, so the Hindi sample's provider is unknown. **[verified: not on the page]**
- Where Plivo could matter later: the "call Dost from any phone" feature, which needs a real phone number. $0.03/min for agents plus telephony; $10 free credit. **[verified: plivo.com/pricing]**

## Benchmark: the Plivo Hindi call (analysed 14 Nov)

Vinay recorded the call from a laptop speaker (fan noise in the room). Claude can't hear audio, so it was **measured**: transcribed locally with faster-whisper (word timings), with rough pitch from the sound wave. The speakers were separated by context. Some words are garbled by the noise.

**The conversation (reconstructed):**
- AI: "Bright Smile Dental mein aapka swagat hai, main Priya bol rahi hoon, bataiye main aapki kaise help kar sakti hoon."
- Caller: "Hello, haan, mujhe apni Friday wali appointment reschedule karni hai."
- AI: "Ji, main abhi dekhti hoon. Kya aap apna booking ID ya naam bata sakte hain?"
- Caller: gives an ID, "Rahul Sharma ke naam se hoga."
- *(about 3.7 s lookup)*
- AI: "Aapki cleaning hai Doctor Patil ke saath Friday gyarah baje. Kab move karna chahenge?"
- Caller: "Agle hafte koi bhi time chalega mujhe. Tuesday possible hai kya?"
- AI: "Check karti hoon. Tuesday Doctor Patil ke saath teen PM ka slot, chalega?"
- AI: "Tuesday teen baje book kar diya, aur confirmation aapke phone pe aa jayega. Aur kuch help chahiye?"
- Caller: "Nahi, aur kuch nahi."
- AI: "Aapka… aapka din shubh ho."

**What makes it natural (measured or seen in the words):**
1. **Spoken Hinglish, not textbook Hindi.** English words sit inside Hindi grammar: help, appointment, reschedule, booking ID, cleaning, slot, check, confirmation. Never "sahayata" for help.
2. **It narrates the wait.** "Main abhi dekhti hoon" and "check karti hoon" come *before* each lookup, so the 3.7 s pause sounds like a person checking, not a machine stuck.
3. **Short turns that end with a question**, handing the turn back: "kab move karna chahenge?", "chalega?", "aur kuch help chahiye?"
4. **Reads back before acting:** it repeats the current booking before changing it.
5. **Politeness markers:** "ji", "bataiye", "aapka din shubh ho".
6. **Pitch moves a lot:** each turn ranges roughly 140–380 Hz, which means a lively, rising-and-falling voice, not a flat one. **[rough measurement: fan noise and a speaker-to-mic recording make exact pitch unreliable]**
7. **Pace about 2.6–3.4 words per second:** normal phone-agent speed, fine for a young caller. **For Gangamma, Dost should be slower.** [our reasoning]
8. **A possible human slip at the end:** "Aapka… aapka din shubh ho". Either a deliberate natural repetition or a transcription error; can't tell without hearing it.

**What Dost copies:** points 1–5 go into Dost's instructions, in Kannada: spoken Bengaluru Kannada with English words as people say them; narrate before every wait ("ondu nimisha, nodthini"); short turns ending in a question; read back before acting; respectful forms for an elder ("neevu", "-ri").

**Vinay's ear (14 Nov):** he vouches that the voice sounds like a real person. That is the bar for Dost's voice.

**Still unknown:** which voice provider Plivo used for this call. Worth asking Plivo support.

## Plivo's "why human-like" section, mapped to Dost (all [vendor claim])

| Plivo pillar | What it says | Dost status |
|---|---|---|
| 1. Real-time | Under 500 ms latency; **700–900 ms is the "optimal" response time**; one network hop | Matches our target (under 0.8 s) in `research-latency-interruptions.md`. Note: Plivo treats ~0.8 s as *optimal*, so an instant reply isn't the goal; natural timing is |
| 2. Audio | **AI noise cancellation**; their Hindi demo call took 42 s without it and 30 s with it; natural prosody; tone and hallucination control | **New for us.** Gangamma's home has a fan, a TV, a pressure cooker. Clean the audio *before* speech-to-text. Options to compare: on-phone noise removal (open-source models such as DeepFilterNet or RNNoise) or a service like Krisp |
| 3. Conversation | Smart turn detection; their Hindi demo took 38 s without it and 24 s with it; interruption detection, barge-in, silence detection | Already researched (Smart Turn, backchannels, barge-in). Their demo shows the same call **~37% shorter** with turn detection on |
| 4. Feedback loop | Automatic simulated calls and scoring, live monitoring, continuous improvement | **New for us, and important:** test Dost automatically with recorded and simulated Kannada conversations (with fan and TV noise) on every change, and score them, instead of only manual testing. This is our planned test bench, made permanent |

**Using Plivo itself?** Plivo is built for **phone calls**. It fits the later "call Dost from any phone" feature (a real number, Sarvam for understanding, ElevenLabs or Cartesia for the voice). Dost's main experience is inside the Android app, where an open-source pipeline (Pipecat or LiveKit) gives the same four pillars without per-minute phone charges. [our reasoning]

## The real cause of "standard language" [our reasoning]
Bookish-sounding speech has two separate causes:
1. **The words.** The AI writes the sentence before the voice speaks it. If it writes formal, textbook Kannada, even the best voice sounds like a news reader. Fix: instruct the AI to write **spoken Bengaluru Kannada with English words the way people say them** ("sugar maatre", "BP", "doctor"), short and casual. This costs nothing and matters as much as the voice.
2. **The voice.** Rhythm, warmth and pace. This is the provider choice.
3. **Pace for older users:** slower speech helps. Sarvam lets you set the pace. **[vendor claim]**

## Voice (text-to-speech) candidates for Kannada
| Provider | Kannada | Notes |
|---|---|---|
| **Sarvam Bulbul v4 Flash / v3** | Yes, Kannada voices tagged "conversation" (chaitra, chetan, suchitra) | Claims Kanglish code-switching and <250 ms streaming **[vendor claim]**. [voices](https://docs.sarvam.ai/api/api-guides-tutorials/text-to-speech/voices) |
| **ElevenLabs** | **Unclear**: one guide says Kannada is among its 74 languages, another says it has no Kannada model | Known for very natural voices in English and Hindi; must test Kannada directly. [Bolna guide](https://www.bolna.ai/docs/customizations/languages/kannada), [comparison](https://heyakashmaurya.com/blog/sarvam-ai-vs-elevenlabs) |
| **Gnani.ai Vachana TTS** | 12 Indic languages (Kannada not confirmed by name) | Indian, built for real-time conversation, zero-shot voice cloning **[vendor claim]**. [Business Today](https://www.businesstoday.in/technology/story/gnaniai-launches-zero-shot-voice-cloning-tts-model-for-12-indic-languages-516896-2026-02-19) |
| **Navana.ai Bodhi TTS** | 10 Indian languages (Kannada not confirmed) | 50+ voices, sub-100 ms, cloning, cheaper than ElevenLabs and Sarvam **[vendor claim]**. [Outlook Business](https://www.outlookbusiness.com/spotlight/news-wire/navana-ai-launches-bodhi-tts-indian-text-to-speech-voice-ai-model-in-10-indian-languages-priced-at-one-eighth-of-elevenlabs-60-below-sarvam) |
| **Cartesia** | Nothing found for Kannada | Fast and natural in English; check before shortlisting |

No independent listening test of colloquial Kannada across providers was found. **[verified: searched, none found]**

## Speech-to-text (understanding her)
- Sarvam leads most Indian-language comparisons found, but they are mostly Hindi and vendor-adjacent. **[verified that the sources say so; not independent]**
- The only Kannada numbers found (older models, blog): Sarvam Saaras v2 9.5% errors, Google 14.2%, Deepgram 28.1%. **[unverified blog]** [agent-rush.com](https://agent-rush.com/blog/speech-to-text-indian-languages-comparison-2026)
- **Older voices are harder for every system** (slower, softer, less clear). Only a test with Gangamma's own voice settles it. **[our reasoning]**
- Leaning: keep Sarvam for understanding, but test it.

## How to decide: a blind listening test with Gangamma
1. Write 5 Dost lines in spoken Kannada (a medicine reminder, "Idu sarina?" answers, the quiet-day check-in).
2. Generate each line with 3–4 shortlisted voices (Sarvam, ElevenLabs if Kannada works, Gnani, Navana if Kannada is confirmed), at normal and slower pace.
3. Play them to Gangamma **without saying which is which**. Ask: which one did you understand best? Which one sounds like someone you'd talk to?
4. For understanding her: record 20 of her real sentences and run them through Sarvam (and one alternative), counting mistakes.

Needs: free-tier accounts or API keys for each provider (Vinay's call).
