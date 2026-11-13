# Research: latency and interruptions for Dost

Written 10 Nov 2026. Research only, nothing built yet. Numbers come mostly from vendor blogs and docs, so treat them as estimates until we measure them ourselves, in Kannada, on a real phone.

---

## Part 1: Latency (the pause before Dost answers)

### How long is too long?
- In human conversation, the gap between one person stopping and the other starting is about **200 ms** (a fifth of a second).
- Good voice agents in 2026 aim for **under 800 ms**. The best claim under 500 ms.
- Real production systems average **1.4 to 1.7 seconds** (Hamming AI's data across 4M+ calls). That's the "long pause" that makes people lose interest.
- **Our target:** under 800 ms for normal chat, measured from the moment Gangamma stops talking to the moment she hears Dost. Track the **slowest 5%** (called P95), not just the average, because one long pause is what people remember.

### Where the time goes: a relay race
Every reply is a relay race with five runners:

```
She stops talking
  → 1. Is she done? (turn detection)          ← often the biggest hidden wait
  → 2. What did she say? (speech-to-text, STT)
  → 3. What should Dost say? (the AI model, LLM)
  → 4. Turn the words into voice (text-to-speech, TTS)
  → 5. Send the audio to her phone (network)
She hears Dost
```

A typical budget (about 800 ms): turn detection and audio 50 ms, STT 150 ms, LLM first words 400 ms, TTS first audio 150 ms, network 50 ms.

**The key trick is streaming:** each runner starts before the previous one has finished. STT sends words while she's still speaking, the LLM starts as soon as it has the text, and TTS starts speaking the first sentence while the LLM is still writing the second. Waiting for each step to fully finish wastes 400 to 800 ms.

### The ways to cut latency, most important first

**1. Stream every stage and overlap them.** This is the foundation; nothing else matters without it.

**2. Fix turn detection: knowing when she's done.** The simple method waits for silence (usually 500 to 800 ms of it) before deciding she's finished. That wait is pure delay. Better: **semantic turn detection**, a small AI model that judges from her words or her tone whether the sentence is complete.
- **Pipecat Smart Turn v3:** open source, listens to the *audio* (tone, pitch), not the words, runs on a normal CPU in about 12 ms, supports 23 languages. **To check:** whether Kannada is one of them.
- **LiveKit turn detector:** reads the *words*, supports Hindi (high accuracy), but a LiveKit forum thread says it **doesn't support Kannada**.
- **The big catch for Dost:** older people speak slowly and pause mid-sentence to think. Cutting Gangamma off is worse than a slightly longer pause. So Dost needs **more patience for older users**: a longer silence threshold that adapts per person, and a learned turn detector so it doesn't interrupt *her*.

**3. Make the AI model answer faster.**
- Use a **fast, small model** for everyday chat; save the big model for things like reading reports.
- **Keep replies short**, and make the first sentence especially short, because the first sentence is what she waits for.
- **Prompt caching:** the instructions that are the same every time (Dost's personality, her medicine list) are processed once and reused, which saves time on every reply.

**4. Stream the voice (TTS).** Start speaking as soon as the first sentence is ready, sentence by sentence.

**5. Servers in India.** Put the servers in an Indian region (e.g. Mumbai), close to Bengaluru. A round trip to the US adds 200+ ms by itself.

**6. Fill the silence like a person does.** Humans say "hmm" or "let me see" while thinking. Dost can play a short, natural filler **instantly from the phone itself** (pre-recorded in Dost's voice) while the real answer is being prepared: "Hmm…", "Ondu nimisha…", "Nodthini…" (let me see) before reading a report. The pause stops feeling like a pause. Use them sparingly, or they sound robotic.

**7. Prepare proactive messages in advance.** When Dost speaks first (medicine reminders, the quiet-day check-in), it knows ahead of time what it will say, so the audio can be generated before the moment arrives. **Zero wait.**

**8. Do the small things on the phone.** Voice detection, the wake word and the camera page detection run on the phone itself, with no network trip.

### The big architecture choice (decide by measuring, not guessing)

| | **Option A: three separate parts** (STT → LLM → TTS) | **Option B: one speech-to-speech model** |
|---|---|---|
| Example | Sarvam Saaras (STT) + an LLM + Sarvam Bulbul (TTS) | Gemini Live API (native audio) |
| Kannada | Sarvam is built for Indian languages; Kannada is supported | Kannada is in the language list, but no quality or latency data found |
| Speed | Sarvam claims under 150 ms for STT's first words; the rest adds up | Potentially fastest (one hop), but developers report it gets slower in long sessions |
| Control | Full: we can check every step (important for medicine) | Less: harder to see and check what it understood |
| Interruptions | We build them (frameworks help) | Built in: generation stops when she speaks |

**Recommendation:** build a small test bench and measure both **in Kannada, with real recordings of how Gangamma talks**. Pick the winner on our numbers, not on vendors' claims. For a health product, Option A's control (seeing exactly what was understood before acting on it) is a strong reason to prefer it if the speed is close.

### How to measure
- Log a timestamp at each handover: she stopped → turn detected → text ready → first words from the LLM → first audio played.
- Report the **median (P50)** and the **slowest 5% (P95)** for each stage, so we know exactly which runner is slow.

---

## Part 2: Interruptions (when she talks while Dost is talking)

Called **barge-in** in the industry. Your worry, stuttering or hallucinating after an interruption, has a specific cause. Each problem below has a known fix.

### What must happen when she really interrupts (within about 200 ms)
1. **Stop the audio on the phone immediately.** Dost's voice is usually generated faster than it plays, so several seconds may be queued on the phone. That queue must be cleared on the phone at once, or Dost keeps talking over her. This is the cause of most "stuttering".
2. **Cancel the reply being generated** on the server.
3. **Remember only what she actually heard.** This is the cause of most "hallucinating". If Dost's memory says it said the full sentence but she only heard half, Dost carries on as if she knows the rest, and the next reply makes no sense. The fix: track exactly how much audio was played and cut Dost's memory of its own reply at that point. OpenAI's Realtime API has a specific command for this (truncate, using "how many milliseconds were played"). Pipecat does it by only saving the words that were actually spoken.
4. **Listen to what she said and respond to *that*,** naturally: "Sari, sari, heli…" (OK, OK, tell me).

### Problem 1: Dost hears itself (echo)
Older people turn the volume up and use the loudspeaker. The microphone picks up Dost's own voice, and Dost "interrupts itself".
- **Fix:** Android's echo cancellation. Recording in "voice communication" mode turns on the phone's built-in echo canceller (since Android 10).
- **Catch:** it varies a lot by phone. Developers report Dost-style apps "talking to themselves" on **some Samsung phones** even with it on. **We must test on the kind of cheap phone Gangamma actually uses.**
- **Backup:** while Dost is speaking, require stronger evidence before treating a sound as an interruption (see Problem 3).

### Problem 2: "Haa", "hmm", "houdu" are not interruptions
When people listen, they make small sounds to show they're following: "haa", "hmm", "houdu" (yes), "sari" (OK). These are called **backchannels**. If Dost stops every time she says "hmm", the conversation becomes painful.
- **Fix:** if what she said is short **and** only made of backchannel words, Dost keeps talking. We need a **Kannada backchannel list** (haa, hmm, houdu, sari, ok, achcha…), which you and Gangamma can write better than any model.
- LiveKit has an "adaptive interruption" model that learns this, but it needs their cloud service, and Kannada support is unknown.

### Problem 3: The TV, a neighbour, the pressure cooker
Sounds that aren't her shouldn't stop Dost.
- **Minimum duration:** treat it as an interruption only if speech lasts about 200–300 ms or more. One guide reports this cuts false interruptions by 60–80%, at the cost of about 200 ms slower reaction to real interruptions (figure unverified).
- **Owner's voice check:** we already planned for Dost to know Gangamma's voice. The same check can decide interruptions: **only her voice can interrupt Dost.** A TV voice can't.

### Problem 4: Resume after a false alarm
If Dost stopped for something that turned out not to be an interruption (a cough, a "hmm"), it should continue naturally: "Haa, naanu heltidde…" (yes, I was saying…), not start again from the beginning or go silent.

### Problem 5: Medicine instructions must never be left half-said
**A Dost safety rule:** if she interrupts in the middle of a medicine instruction ("raatri oota aada mele, ondu…"), Dost answers her, then **repeats the full instruction** before moving on. Half an instruction is dangerous.

---

## Tools that already solve much of this
- **Pipecat** (open source, Python): streaming pipeline, interruption handling, only saves words that were actually spoken, **has a Sarvam integration**, and runs Smart Turn locally.
- **LiveKit Agents** (open source, Python/TypeScript): streaming pipeline, adaptive interruption (cloud), turn detector (no Kannada).

**Leaning:** Pipecat, because of Sarvam and Kannada. Tentative until tested.

---

## Proposed next step (not started, needs Vinay's go)
A **small test bench**, a few hours of work, separate from any app:
1. Record 20–30 short Kannada clips of how Gangamma talks (with her permission), including pauses, "haa"/"hmm", and interruptions.
2. Build two minimal pipelines in Pipecat: Option A (Sarvam + LLM + Sarvam) and Option B (Gemini Live).
3. Measure P50/P95 latency per stage, and count false and missed interruptions.
4. Test on a cheap Android phone on loudspeaker for echo.

Result: a table of real numbers that picks the architecture for us.

---

## Open questions to verify
- Is Kannada one of Smart Turn v3's 23 languages?
- Sarvam Bulbul (TTS) latency for Kannada: no numbers found.
- Gemini Live native-audio quality in Kannada.
- Can Android's echo cancellation be relied on for the phones our users have?

## Sources
- Latency budgets: [smallest.ai](https://smallest.ai/blog/designing-voice-assistants-stt-llm-tts-tools-and-latency-budget), [Future AGI](https://futureagi.com/blog/sub-500ms-voice-ai-guide-2026/), [Deepgram](https://deepgram.com/learn/real-time-voice-ai-stack-agents-architecture-guide), [Soniox](https://soniox.com/wiki/voice-agent-latency-budget), [tianpan.co](https://tianpan.co/blog/2026/04/09/voice-ai-production-300ms-latency-budget)
- Interruptions: [Hamming AI runbook](https://hamming.ai/blog/voice-agent-interruption-handling-runbook), [LiveKit adaptive interruption](https://docs.livekit.io/agents/logic/turns/adaptive-interruption-handling/), [Deepgram on backchannels](https://deepgram.com/blog/backchannels-vs-interruptions-voice-agents.md), [AssemblyAI](https://www.assemblyai.com/docs/voice-agents/universal-3-5-pro-streaming-api), [Future AGI barge-in](https://futureagi.com/blog/voice-ai-barge-in-turn-taking-2026/)
- Turn detection: [LiveKit turn detector](https://pypi.org/project/livekit-plugins-turn-detector), [Smart Turn GitHub](https://github.com/pipecat-ai/smart-turn), [Smart Turn v3 announcement](https://www.daily.co/blog/announcing-smart-turn-v3-with-cpu-inference-in-just-12ms/), [LiveKit Kannada thread](https://community.livekit.io/t/livekit-interrupt-logic/1653)
- Sarvam: [STT](https://www.sarvam.ai/speech-to-text), [streaming docs](https://docs.sarvam.ai/api-reference-docs/api-guides-tutorials/speech-to-text/streaming-api), [Pipecat Sarvam service](https://reference-server.pipecat.ai/en/latest/_modules/pipecat/services/sarvam/stt.html)
- Gemini Live: [Google docs](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/live-api), [language list](https://firebase.google.com/docs/ai-logic/live-api/limits-and-specs), [latency in long sessions](https://discuss.ai.google.dev/t/gemini-live-api-native-audio-response-latency-gradually-increases-during-long-sessions/115508)
- Truncating after interruption: [OpenAI forum](https://community.openai.com/t/realtime-api-interruptions-dont-properly-trim-the-transcript/1000703), [Agents SDK PR](https://github.com/openai/openai-agents-python/pull/4122)
- Echo on Android: [Android pre-processing docs](https://source.android.com/devices/audio/implement-pre-processing), [Samsung self-talking issue](https://community.openai.com/t/samsung-device-echo-self-talking-issue-with-websockets-and-voice-communication/1374490), [Coval on echo](https://www.coval.dev/blog/voice-ai-echo-cancellation)
