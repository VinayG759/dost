STATUS: waiting for the next Sara

# Handoff: Dost (written 21 Nov 2026)

## Read first, in this order
1. `decisions.md`: every decision so far. **Check all work against it, line by line, before calling anything done.**
2. `hackathon-details.md`: rounds, dates, scoring, our progress.
3. `dost-idea.md`: the full idea (core / next / later), the real user Gangamma, who pays.
4. `prompt-submission.txt`: the submitted build prompt. This is the prototype spec.
5. `research-latency-interruptions.md` and `research-voice-providers.md`: speed, interruptions, voice providers, the Plivo Hindi call as the naturalness bar.

## Where we are
- Team CodeXplorers (Vinay is owner, 3 members, locked).
- Round 1 all submitted: Idea (passed), Prompt (submitted 14 Nov), Impact (passed 21 Nov). Scores appear after 26 Nov.
- Round 2 (27–29 Nov) is a **new shared problem statement**, not necessarily Dost. Masterclass in the week of 23 Nov.

## Next: preparing to build the Dost prototype
Plan proposed to Vinay on 21 Nov (not yet approved):
1. **His setup:** accounts and API keys (Sarvam, Anthropic, Supabase; ElevenLabs he already has), a cheap Android phone like Gangamma's, Gangamma's consent to record her voice.
2. **First architecture decision (his):** where the live voice loop runs: on a small server the phone streams to (Pipecat), or phone to provider APIs directly. Explain the trade-off before building.
3. **Voice core first**, reusable for Round 2: speech in → Claude → speech out in Kannada, with turn detection, interruptions, noise removal, and a recorded test set that measures latency.
4. **Listening test with Gangamma** to pick the voice.
5. **The app**, one flow at a time, starting with medicine time + "Idu sarina?".

## Decisions that are his
- The pilot claim in the Impact submission (8 weeks, Gangamma + 20 families): he hasn't explicitly confirmed it.
- "Ravi" as the son's name in the prompt: real or kept on purpose? Unconfirmed.
- Spending on API accounts.
- Uncommitted docs since the last push: `decisions.md`, `research-voice-providers.md`, `impact-submission.txt`, updated prompt and handoff. Don't commit the WhatsApp recording (public repo).
