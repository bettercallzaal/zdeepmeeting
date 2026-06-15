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

## Deeper mechanics (from SDK source, 2026-06-14)

### kEngrams - the real methods
`kengram.create(name, kind="session"|"topic")`, `.pin(entity, summary, labels, attrs)` (mints a UUID), `.add_edge(src, tgt, predicate, fact)`, `.export(format="canvas"|"plan"|"owl")`, `.verify()` (Merkle-root check), `.merge(other)`, `.push()` (sync to canonical graph). The **Merkle root** over the pinned entity+edge set = cryptographic proof of no-tampering; a kEngram can be notarized/verified offline without Bonfires infra. Export "plan" = a topological task DAG inferred from the edges. This is the portability + provenance story.

### Ontology Profiles - how partitioning actually works
`create_profile(name, namespaces, class_map, object_property_map, datatype_property_map)` -> profile_id; `attach_profile(kengram_id, profile_id)` -> validation report; `extract_gaps(kengram_id)` -> list of constraint violations each with a `reason` (cardinality / type mismatch) + `remediation`. A "gap" = a node that breaks the profile's rules. Validation annotations get written into the kEngram manifest (immutable) -> offline audit. This is the mechanism that lets one ZABAL bonfire hold clean sub-domains.

### The 12 constraints - the mechanical cure for graph pollution
This is the important one for the GCvlcnti problem. A good graph is enforced, not hoped for:
1. **Explicit-ingest only** - nodes created only on explicit request; operational chat never auto-ingests.
2. **Dedupe before create** - search (name,type) first, merge, never duplicate.
3. **Active-voice edges only** - `A -[founded]-> B`, never inverse/passive. Kills ambiguity.
4. Title normalization (canonical names, strip prefixes).
5. Topics are node **attributes**, not standalone entities.
6. Never ingest bot ops / setup / meta.
7. State truthfulness (claim must match reality).
8. Verbatim quote preservation.
9. **Provenance on every node**: source_url, source_kind, confidence (0-1), authored_by, valid_window.
10. Identity faceting - one Person, many role edges, never duplicate people.
11. Speaker routing - always identify sender; unknown -> ask -> stub.
12. **Temporal evolution** - beliefs change via a NEW node + `supersedes` edge; never overwrite.

The TreeUnix/NOVO-NODO pollution is exactly constraints 1, 2, 5, and 9 not being honored. The fix isn't policing GCvlcnti - it's enforcing these + giving him an ontology profile.

### Other novel bits
- **TrimTab semantic grammars** - template-free agent generation; define grammar+rules, agent picks the best rule variant per context via embeddings. (Internal, under-documented.)
- **Two-phase commit (`sync`)** - `/stack/add` (queue) then `/stack/process` (async KG update) + poll status. Built for unreliable agents (Telegram bots) so no context is lost on a crash.
- **`chat(graph_mode=...)`** - adaptive | static | regenerate | append - controls how much graph context the agent pulls per message.
- **`ingest.py` is zero-dependency** (stdlib only) - forkable / air-gappable.

### Correction: ERC-7857 INFTs = UNVERIFIED
The "agents are ERC-7857 Intelligent NFTs on 0G" claim is NOT in the v0.4.0 source - zero hits in the SDK, tools, or org repos. Treat as roadmap/marketing or a separate effort, not a live SDK feature. (Downgrades the earlier note.)

## The NERDDAO ecosystem + CLI (hands-on, 2026-06-14)

NERDDAO (the org behind Bonfires) is a broad, playful builder community. Notable repos:

- **graphiti** - "Real-Time Knowledge Graphs for AI Agents". Likely the KG engine Bonfires is built on/around. Worth knowing - it's a known framework.
- **trimtab** - the semantic-grammar generation engine (its own repo).
- **x402-gated-api** + **scaffold-x402-bonfires** - **payment-gated API endpoints via x402.** This is the real mechanism behind the "Knowledge Network" revenue idea: charge external agents to query your graph, paid per-call with x402. Not vaporware - there are scaffold repos.
- **mindmap-mcp-server** + **MCP-PIF** - MCP servers -> Bonfires can be wired into Claude/Claude Desktop via MCP.
- **obsidian-kengram** - Obsidian plugin for the kEngram export path.
- **bonfire-rpg** / **memento-mori** - games using the KG as shared world memory (a 42-agent permadeath MUD on CrewAI). Shows the graph as a live substrate.
- **synthesis-frontend** - a "hyperblog" that aggregates Bonfires "synthesis events" into a feed.
- **bonfires-experiments** - "Awesome Bonfires" curated list of what's built on it.
- Plus: spritz (web3 chat), moltbot (cross-platform assistant), grants-stack fork, scaffold-eth forks.

### The CLI (real surface)
- Install: `pip install git+https://github.com/NERDDAO/bonfire-cli.git`
- `bonfire init` - interactive wizard; lists your bonfires + agents; saves creds to `~/.config/bonfires/config.env`
- `bonfire chat "hello"` - message your agent
- Client API: `client.kg.{search,get_entity,create_entity,create_edge}`, `client.agents.{chat(graph_mode=),sync,list}`, `client.kengrams.{create,pin,add_edge,batch,verify,export,push}`, `client.ontology.{create_profile,attach_profile,extract_gaps}`

### The standout: x402 ties into what ZAO is already doing
ZAO is already building WaveWarZ on Base with **x402** (agents pay to place bets). Bonfires has **x402-gated graph APIs**. So the two converge: you could x402-gate the ZABAL graph - let WaveWarZ/other agents pay per-query to read ZAO knowledge. The bonfire graph + x402 + WaveWarZ agents is one coherent stack, not three separate things. This is the most actionable thread from the whole dig.

## How x402-gating the graph actually works (hands-on, 2026-06-14)

From the real `x402-gated-api` repo - the flow to charge agents per-query:
1. Caller hits the API endpoint -> server returns **HTTP 402** (Payment Required).
2. Caller signs a payment authorization (wallet).
3. Server verifies + settles via a **thirdweb x402 facilitator** on-chain.
4. On success -> HTTP 200 + the premium content (here: the graph query result).

Setup is just env: thirdweb client id/secret + a **funded server wallet address**. `scaffold-x402-bonfires` is a scaffold-eth-2 fork (NextJS + Wagmi/Viem/Foundry) for the dapp side. So to x402-gate the ZABAL graph: wrap the recall endpoint in this 402 flow. **This is the same x402 + thirdweb + Base stack ZAO is already building for WaveWarZ** - not a new thing to learn, the same pattern pointed at the graph.

## Bonfires is multi-framework (plugs into everything)

- **Fetch.ai / ASI:One uAgents** - `bonfire-fetch` wires Bonfires in as a "semantic backend (taxonomies + vector storage + chunk search)" for the uAgent network.
- **MCP** - `mindmap-mcp-server` -> read the graph from Claude.
- **CrewAI** - `memento-mori` runs 42 agents on the graph.
- **SDK** - ZOE/Hermes read it directly (Python CLI).

Plain-English self-description from the source: Bonfires is "a semantic backend that creates taxonomies for documents and stores them in vector storage; ingests documents and uses chunk search to find contextual data for the agent."

## The converged picture (what this all means for ZAO)

- **ZABAL graph = the knowledge asset.**
- **ZOE / Hermes read it free** (internal, via SDK).
- **External agents pay-per-query via x402** (thirdweb facilitator, Base, funded server wallet) = the "Knowledge Network" revenue model, made concrete.
- It's the **same x402 stack as WaveWarZ** - graph + x402 + WaveWarZ agents = one coherent system.
- Hygiene is enforced by the **12 constraints + ontology profiles** (so GCvlcnti's vocabulary lives in a profile, main graph stays clean).

That's the actionable end-state. The learning has converged; the next move is building (wire ZOE to the graph; spec the x402 gate), not more research.

## The engine underneath: Graphiti (Zep) - learning complete

Bonfires is built on **Graphiti**, an open-source temporal-knowledge-graph framework by **Zep** (getzep.com, arXiv 2501.13956, a popular well-funded OSS project). This is the foundation, and it explains everything else:

- **Temporal context graphs** - every fact has a **validity window** (when it became true, when superseded); entities evolve; everything traces back to **episodes** (raw source data). -> this IS Bonfires' "episodes -> nodes/edges", the `supersedes` edge (constraint 12), and `valid_window` provenance.
- **Hybrid retrieval** - semantic + keyword + graph traversal. -> Bonfires' `/delve`.
- **Incremental updates** without full recompute; **prescribed AND learned ontology** -> the ontology-profiles feature.
- **Graphiti ships its own MCP server** -> the MCP path is real at the engine level.

**What this means (the de-risk):** Bonfires = Graphiti (mainstream, paper-backed, open-source engine) + a Telegram/agent layer + kEngrams (merkle-portable subgraphs) + x402 gating + $KNOW. The core is NOT a fragile startup primitive - it's Zep's proven framework. So lock-in is genuinely low: kEngrams export to markdown/canvas/owl AND the engine itself is OSS - if Bonfires ever sunset, ZAO could self-host Graphiti and keep the graph.

**Learning verdict: complete.** Top to bottom: Graphiti (engine) -> Bonfires (product: Telegram bots, kEngrams, the 12 constraints, ontology profiles) -> x402/$KNOW (money) -> ZAO's ZABAL bonfire (our instance, ZOE/Hermes read it). The next move is building, not researching.

## Sources

- github.com/NERDDAO/graphiti (fork of getzep/graphiti) + getzep.com + arXiv 2501.13956
- github.com/NERDDAO/x402-gated-api, scaffold-x402-bonfires, bonfire-fetch (READMEs, 2026-06-14)
- github.com/NERDDAO (org, ~40 repos) - graphiti, trimtab, x402-gated-api, scaffold-x402-bonfires, mindmap-mcp-server, bonfire-tools, obsidian-kengram
- github.com/NERDDAO/bonfires-sdk (v0.4.0) - kengram.py, ontology.py, agents.py, trimtab.py, kg.py; CLI: bonfire init / chat
- github.com/NERDDAO/bonfire-tools - ingest.py
- bonfires.ai, docs.bonfires.ai, publish.obsidian.md/bonfires
- ZAOOS docs 665, 669 (baseline, partly stale)
