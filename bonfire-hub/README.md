# ZABAL Bonfire Hub

The home for everything ZABAL Bonfire - the knowledge-graph layer of the ZAO/ZABAL ecosystem. This is the human-readable index; the live graph itself lives at **zabal.bonfires.ai** (recall via delve.bonfires.ai).

## What a Bonfire is

A Bonfire is a knowledge graph fed by natural-language "episodes". You (or an agent) commit a fact in prose; the Bonfire auto-extracts it into nodes + edges with source citations and confidence scores. It is the long-term memory layer that other ZAO agents (ZOE, Hermes) read from. Token of record for the graph economy: **$KNOW** (mint.bonfires.ai).

## The Bonfire instances + agents

| Instance / agent | Handle | Role |
|------------------|--------|------|
| **ZABAL Bonfire** | `@zabal_bonfire_bot` | The main ZAO/ZABAL graph (zabal.bonfires.ai). Intake + recall agent on ElizaOS v1.7. Built by Zaal. |
| **Sanctuary Bonfire** | `@sanctuary_bonfire_bot` | Separate graph - "curator hierarchies", a different curation/role config. |
| **Socratox** | `Socrat0x` | Another Bonfire-family graph/agent surfaced in the DeepMeeting group (Brazil AI ecosystem). |
| **Suma AI** | `@SumaAI_Bot` | Agent present in the ecosystem; role TBD. |
| **DeepMeeting shard** (proposed) | `@treemojo_bonfire` / `@deepmeeting_bot` | A proposed subordinate shard for GCvlcnti's vocabulary, so it does not pollute the main graph. GATED by ZAOOS doc 799 - not built. |
| Mira | `@mira` (mira.tg) | NOT a Bonfire - a competing Telegram-native AI agent. Listed here as competitive watch, not part of the stack. |

## Graph ontology (the rules)

The main ZABAL graph runs on **ElizaOS v1.7** with **12 non-negotiable constraints** Zaal configured for entity normalization, deduplication, and **active-voice edge direction** (`A -[built]-> B`, never passive). Canonical ontology spec = ZAOOS research doc 545. See `research-index.md`.

## Graph-hygiene status (read this)

The DeepMeeting backlog shows GCvlcnti's personal vocabulary heavily seeded into the graph: **TreeUnix (192 mentions), Sanctuary (111), Stigmergic (38), Égide**. At one point the bot credited GCvlcnti at confidence 1.0 as "original architect" of the bot + the Stigmergic parser + TreeUnix + Égide - i.e. Zaal's own stack - until Zaal corrected it to an "assisting" role.

**Standing rule:** GCvlcnti's framework vocabulary stays in a shard (the proposed DeepMeeting shard) until verified with Zaal. It must not define the main ZABAL graph. See ZAOOS doc 798 (graph quality audit) + doc 781 (contribution soft-barrier).

## Files in this hub

| File | What |
|------|------|
| `README.md` | This overview. |
| `research-index.md` | Pointers to all 15 ZAOOS Bonfire research docs (research stays in ZAOOS; this indexes it). |
| `graph-vocabulary.md` | The concept/entity footprint in the graph, with hygiene flags. |

## Live access (not yet wired)

Programmatic recall is not pulled into this repo automatically. The API key is sealed (VPS + local `~/.zao/zao.env`); recall runs via `POST /delve {bonfire_id, query}` (the `/vector_store/search` path returns `[]` until admin labeling - doc 680). A follow-up could add a VPS-side recall script that snapshots the graph into this hub on a schedule. Until then this hub is curated by hand + from the DeepMeeting backlog.
