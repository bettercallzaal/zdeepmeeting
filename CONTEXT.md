# Context: the relationship, governance, and the ZAO DeepMeeting bot plan

## The relationship

GCvlcnti (alias DvlsMojo) is a 42yo Brazil-based networker, Business Administration background, and Zaal's designated **ZABAL Bonfire graph admin** (scoped to a shard, not the main graph). He connects people and projects; his strength is macro/industry reads (he correctly called Mira and the Manus->Meta/Telegram shift before most), his weakness is sprawl and unverified specifics that have polluted the main graph before.

Operating rule: **his macro reads = signal, his project-specific specifics = verify before they enter the shared graph.** Full relationship log = ZAOOS research doc 855.

## His concepts (from the backlog)

- **deepmeeting** - a naming/protocol pattern: a chat used as a memory database, with group resources + tools attached. The group itself is the canonical instance.
- **TheBlockMeeting** - a meeting protocol, "Circles Block inside Circles", fractal-style networking for long-term people + project connections.
- **Referral-everywhere** - his core ask: every project/app should carry an attribution/referral layer so sharing is counted, evaluated, rewarded (not only in money).
- **Telegram-as-web** - he treats Telegram folders/chats as public addressable web pages (`t.me/x`), and chat exports as html+css+js+json sites. Useful framing for graph ingest.

## Governance (why there isn't a bot yet)

ZAOOS CLAUDE.md: **no new Telegram bot / agent without a numbered research doc + explicit Zaal approval.** The gate already exists - ZAOOS doc 799 (DeepMeeting agent / shard architecture). Any bot here must be greenlit there first. The fleet was deliberately collapsed to 5 surfaces (doc 601); a DeepMeeting bot has to justify itself as a shard of the ZABAL Bonfire graph, not a standalone new brain.

## The ZAO DeepMeeting bot - plan

Two functions, two different mechanisms:

1. **Backlog extraction (DONE for this snapshot).** A BotFather bot CANNOT read past group history (Telegram Bot API only sees messages after it joins). Backlog was pulled via **Telegram Desktop -> Export chat history -> JSON/HTML**, redacted, and committed to `transcript/`. For a repeatable automated pull, the future option is an **MTProto userbot (Telethon/Pyrogram)** logged in as a user account.

2. **Live presence + Q&A proxy (TO BUILD, gated by doc 799).** A BotFather bot that:
   - lives in the group going forward,
   - reads/writes the ZABAL Bonfire graph (the source of truth),
   - answers questions on Zaal's behalf when he's unavailable.

### Q&A proxy guardrails - DEFERRED

Guardrail level (graph-only vs light-inference, labeling, what it will/won't commit Zaal to) is explicitly **deferred** - to be locked when the bot spec is written in doc 799. Until then: no proxy answering in production. Default intent: answers from the graph only, always labeled as Zaal's assistant (not Zaal), read-only, never commits Zaal or shares private info.

## Cross-links

- ZAOOS doc 855 - GCvlcnti relationship log + comms reset
- ZAOOS doc 799 - DeepMeeting agent / shard architecture (the bot gate)
- ZAOOS doc 798 - Bonfire graph quality audit (where his vocabulary polluted the main graph)
- ZAOOS doc 781 - ZABAL Bonfire contribution architecture (shared graph + soft barrier)
