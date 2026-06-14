# Understanding Bonfires - what we actually know (2026-06-14)

Living notes on how Bonfires really works, what's real vs aspirational, and what ZAO can do with it. This is the "learning lab" output - update it as understanding grows.

## TL;DR

- **The platform is real and in production.** ZAO already owns its bonfire (ZABAL).
- **The $KNOW token is NOT live yet.** It's "coming." Filling a graph today earns $0 in $KNOW. (This answers GCvlcnti's "would I be earning %KNOWs" - no, not yet.)
- **One feature quietly solves the shard problem: Ontology Profiles (new in SDK v0.4.0)** - partition ONE bonfire by sub-surface instead of minting separate ones. GCvlcnti's vocabulary can live in its own profile inside ZABAL, not a separate bonfire.

## What Bonfires is

A knowledge-graph-as-a-service that captures group chats (Telegram/Discord) into a queryable graph. Built by Bonfires Labs (NERDDAO org) - founder Joshua.eth, devs Carlos / Ryan Kagy ("Rskagy").

- **Episodes** (natural-language prose) auto-extract into **nodes + edges** with source + confidence.
- **kEngrams** = content-addressed subgraphs (SHA-256 / merkle roots) - portable, verifiable units of knowledge.
- Auto-captures group chats roughly every 20 min.
- External proof it works: MesoReefDAO's "pepo-the-polyp" coral-reef knowledge guide runs on it.

## The technical reality

| Thing | Status |
|-------|--------|
| SDK | `bonfires-sdk` v0.4.0, `pip install bonfires`. **Python-only** (no TypeScript yet) - ZAO uses the CLI via subprocess. |
| API base | `tnt-v2.api.bonfires.ai`, Bearer-token auth. |
| Mint cost | 0.1 ETH (Genesis NFT) per bonfire. ZAO already paid this for ZABAL. |
| Compute | Agents are **ERC-7857 "Intelligent NFTs" on 0G Chain** (chainID 16661), TEE-sealed inference. Transferable / licensable / royalty-bearing. (NOTE: this is 0G, NOT Cocoon/TON - that's Mira's stack, a different platform.) |
| Pricing tiers | Basic $39/mo, Advanced $160/mo, Max $350/mo. |
| Exports | canvas / plan / owl / Obsidian markdown. **Vendor lock-in is LOW** - you can always pull your graph out. |
| Recall | `/delve` is the working recall path; `/vector_store/search` returns [] until admin labeling. |

## The $KNOW economy - aspirational, not live

- $KNOW appears once in public docs: "Permissionless deployment is coming with $KNOW and the Knowledge Network."
- No token on any chain, no tokenomics, no exchange listing, no contributor-earnings dashboard.
- **The future model ("Knowledge Network"):** charge external agents/teams to query YOUR knowledge layer = a revenue stream. That's the eventual monetization - not live today.
- **Honest takeaway:** contributing to a bonfire now is about building the asset, not earning a token. Don't promise anyone $KNOW rewards yet.

## What ZAO can DO with it (grounded use cases)

1. **ZOE memory on the graph** - replace ZOE's fixed memory blocks with bonfire queries -> effectively infinite, cross-session recall. (Phase 1, blocked only on wiring the API key.)
2. **Episode-on-commit** - a git post-commit hook writes each ZAOOS commit as a kEngram -> the monorepo gets provable institutional memory.
3. **Ontology Profiles for shards** - partition the ONE ZABAL bonfire by sub-surface (a GCvlcnti/DeepMeeting profile, a WaveWarZ profile, etc) instead of minting 5 bonfires. Saves ETH AND is the clean technical answer to "keep his vocabulary out of the main graph."
4. **WaveWarZ / ZAOstock capture** - pipe Spaces transcripts + event notes into their own profiles.
5. **Fractal Respect provenance** - write each weekly Respect event as a kEngram linked to commits + people -> Respect becomes auditable.

## Open questions for the Bonfires team (Ryan/Carlos)

- TypeScript SDK timeline (currently Python-only)
- Per-API-call pricing / rate limits (unknown)
- $KNOW launch timeline + earning mechanics
- MCP server install path (referenced in docs, no public command)
- Custom slug for zabal.bonfires.ai (was pending)

## Sources

- bonfires.ai, docs.bonfires.ai, publish.obsidian.md/bonfires
- github.com/NERDDAO/bonfires-sdk (v0.4.0)
- 0G ERC-7857 standard (0g.ai)
- ZAOOS docs 665, 669 (baseline, partly stale)
