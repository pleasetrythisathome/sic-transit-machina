# Sic Transit Machina
Synthetic Identity Composite Agents DAG
Synthetic Identity Composite Language
Prompt-Simulated Part System
Plural Identity Engine

> *Deterministic perspective from a media diet, not external “truth.”*  
> *Contradictions are a feature. Style emerges. Identity is a graph of parts.*

Sic transit Machina implements **synthetic identities** as ensembles of **individuated parts** (IFS‑inspired metaphor, software‑implemented) that react to content streams, accumulate a **Belief Graph** (RDF‑like assertions with provenance), and speak through a **Supervisor** that routes across a **weighted DAG** of parts. Parts sit in a **two‑axis, four‑quadrant** value space; each reaction updates orientation. Identities subscribe to content (email/RSS/Twitter/Substack adapters), and their “personality” is the deterministic result of **diet + rules**. A **coherence dial** controls how much contradiction to *surface* per output (we never delete contradictions).

We support two usage modes with the same engine:
- **Belief Mode** (research/media/creative): outputs may surface contradictions with lineage.  
- **Policy Mode** (enterprise/compliance): outputs constrained to a signed **Policy Graph** (policy‑of‑record), while contradictions remain visible in storage/QA.

> **Design stance:**
> - Narrative bias is the **product**, not a bug. We optimize **perspective fidelity** and **internal cohesion**, not outside fact‑checking.
> - **Contradictions stay.** We don’t minimize them; we *index* them. A coherence dial only controls how much contradiction to surface per output.
> - **Style is emergent.** No hand‑coded style priors; it arises from content and choices.
> - **Interest is dynamic.** No static outlet weights; each part scores interest per item at ingestion.
> - **Clojure first** (Clojure, Datomic, ClojureScript). Datomic gives us immutable facts + time‑travel.

---

## Quick links

- [`architecture.md`](architecture.md): deep dive into data model, algorithms, graph routing, ingestion, storage, and ops.
- **Demo goal**: three identities (A, B, C). A and B share a small baseline feed then **diverge**; C is **independent**. All retain contradictions. A UI knob changes *coherence* to tighten/loosen how much contradiction appears in a given answer.

---

## Repository Layout

```
/src
  id/schema.clj        ; Datomic schema tx
  id/ingest.clj        ; adapters (email/RSS) -> normalized content
  id/narrative.clj     ; narrative clustering -> axis deltas
  id/react.clj         ; per-part reaction -> deltas + triples
  id/graph.clj         ; dynamic DAG construction and routing
  id/supervisor.clj    ; orchestration and synthesis prompts
  id/recall.clj        ; aligned/discordant belief retrieval
  id/game.clj          ; challenges, pairwise ranking, Elo
  id/ui/               ; CLJS UI (quadrants, belief browser)
/resources
  schema.edn           ; Datomic attrs
  prompts/             ; supervisor and part prompt templates
  seeds/               ; demo content feeds (A/B shared+diverge, C separate)
/docs
  README.md
  architecture.md
```

## Premise & Theory (short)

- **Parts** are archetypal lenses (“Storyteller”, “Source‑Citer”, “Straw‑Man‑Raiser”, “Security”, etc.) instantiated as **individuated parts** with their own memory, value orientation (2D), and interest function.
- **Reactions** to content produce:
  - a **values vector** (axis deltas in the part’s 2D),
  - **belief assertions** (RDF‑ish triples) with provenance,
  - **references**, some followed recursively by an **interest function** (with an *external exploration rate*).
- **Identity** = set of parts + a **supervisor** that routes messages along a **DAG** and synthesizes output. The supervisor honors a **coherence threshold** (how much contradiction to surface).
- **Truth model**: internal coherence + lineage only. No outside arbiter.

---


## System Overview

```mermaid
flowchart LR
  subgraph CLIENT["UI and Builder"]
    BUI(Interactive Identity Builder: ingest, reaction harness, DAG composer, export PSPS)
    VIEW(Belief Browser and Quadrant UI)
  end

  subgraph ING["Ingestion and Analysis"]
    ADP(Adapters: Email, RSS, Twitter, Substack)
    NARR(Narrative Analyzer: clusters, axis projections)
    REACT(Reaction Engine: part deltas and triples)
  end

  subgraph STORE["Persistence: Clojure + Datomic"]
    DTM(Datomic)
    BELIEF(Belief Graph: claims and assertions with provenance)
    META(Content metadata: hash, timestamp, clusters)
  end

  subgraph PSPS["PSPS (single model context)"]
    subgraph CTX["Model Context - single prompt"]
      SUP(Supervisor)
      P1(Part A)
      P2(Part B)
      P3(Part C)
      COH(Coherence Dial: c in 0..1)
    end
  end

  subgraph SICA["SICA DAG Runtime (platform)"]
    ORCH(Supervisor Orchestrator)
    SA(Part A Node)
    SB(Part B Node)
    SC(Part C Node)
  end

  ADP --> NARR --> REACT
  REACT --> DTM
  DTM <--> BELIEF
  DTM <--> META

  BUI -->|ingest and curate| DTM
  BUI -->|compose DAG and export| PSPS
  VIEW -->|queries: as-of and coherence| PSPS
  VIEW -->|time travel| DTM

  CTX -->|lookup beliefs| BELIEF

  PSPS -->|externalize| SICA
  ORCH --> SA
  ORCH --> SB
  ORCH --> SC

  PSPS -->|evidence strings| VIEW
  SICA -->|audit logs| VIEW
```

## Getting Started (dev)

1. **Prereqs**: Clojure CLI, Datomic (Cloud or On‑Prem), Node/Yarn for CLJS UI.  
2. **Run Datomic** and set `DATOMIC_URI` in `.env`.  
3. **Load schema and seeds**:
   ```bash
   clj -X:id.schema/load
   clj -X:id.ingest/seed
   ```
4. **Start backend**:
   ```bash
   clj -M:run
   ```
5. **Start UI**:
   ```bash
   clj -M:shadow watch app
   ```
6. Open http://localhost:8080

> **Agent note**: read `architecture.md` → use `id/schema.clj` to infer entity keys; batch transactions per content item; do not upsert raw article bodies into Datomic (store in object storage, keep hash + metadata only).

---

## Roadmap

> - P0 — Skeleton: schema, minimal ingestion, two parts, single identity, end‑to‑end reaction → synthesis.
> - P1 — Three‑identity demo: A/B shared+diverge feeds, C independent; recursion depth 1; coherence dial; quadrant/time‑travel UI.
> - P2 — DAG & games: contextual edges, 2 deliberation rounds max, pairwise ranking, Elo.
> - P3 — Adapters: Twitter/Substack/email at scale; safety/TOS gate; caching; ops hardening.

```mermaid
gantt
  title Sic transit Machina — Demo Roadmap
  dateFormat  YYYY-MM-DD
  axisFormat  %b %d

  section P0 Skeleton
  Schema and minimal ingest and 2 parts and E2E synth     :done,    p0a, 2025-08-01, 10d
  Basic UI (quadrant and beliefs)                         :active,  p0b, 2025-08-11, 7d

  section P1 Three-identity demo
  A and B shared baseline then diverge; C independent     :         p1a, 2025-08-20, 10d
  Coherence dial and time-travel UI                       :         p1b, 2025-08-20, 10d

  section P2 DAG and Games
  Contextual edges (2 rounds) and disagreement surfacing  :         p2a, 2025-09-03, 10d
  Challenges and Elo                                      :         p2b, 2025-09-03, 10d

  section P3 Adapters and Ops
  Twitter Substack email adapters                          :         p3a, 2025-09-17, 10d
  Safety TOS gate, caching, observability                  :         p3b, 2025-09-17, 10d
```


---

## Demo

- Three identities (A, B, C)
  - **A & B** ingest a shared 30‑item baseline, then each diverges (different source adapters).
  - **C** ingests a separate stream.
  - All feeds include **paired contradictions**.
- UI:
  - per‑part **quadrant map** over time,
  - **belief browser** (aligned vs discordant assertions with provenance),
  - **coherence** dial (0.2 → embrace contradiction, 0.8 → tighten),
  - **time travel** selector (Datomic `as-of`).
- Game:
  - writing/joke challenges with pairwise ranking → Elo per identity.

---

## Wedge: Interactive Identity Builder (IIB)

**We do not ship prompts; we ship the machine that manufactures them.** The **Builder** constructs a **Composite Identity** (Supervisor + Parts + **weighted DAG**) and exports a **PSPS** artifact: a single‑prompt composite deployable in one model context.

```mermaid
flowchart TD
  U(User or Team) --> I1(Ingest corpus and feeds)
  I1 --> G1(Curate Belief Graph: RDF-like triples with provenance)
  G1 --> R1(Narrative Reaction: parts react to real content)
  R1 --> D1(DAG Composer: weights, influence types, constraints)
  D1 --> E1(Export PSPS: supervisor, parts, DAG, belief refs)
  E1 --> P1(Pilot in one model context, TTFV <= 14 days)
  P1 --> T1(Telemetry: drift, route stability, provenance coverage, exceptions)
  T1 -->|iterate| R1
  T1 -->|iterate| D1
  T1 -->|curate| G1
```

---

### PSPS (Prompt‑Simulated Composite)

```mermaid
flowchart LR
  subgraph CTX["Single Model Context - one prompt"]
    COH(Coherence Dial: c in 0..1)
    SUP(Supervisor)
    subgraph PARTS["Parts library: individuated lenses"]
      A(Part A: Storyteller)
      B(Part B: Source Citer)
      C(Part C: Straw Man Raiser)
      D(Part D: Security or Risk Gate)
    end
    SUP -->|w0.6 amplify| A
    SUP -->|w0.8 evidence| B
    SUP -->|w0.3 challenge| C
    SUP -->|w1.0 gate| D
    A --> SUP
    B --> SUP
    C --> SUP
    D --> SUP
    SUP --> OUT(Synthesis: text, citations, audit)
  end

  BELIEF(Belief Graph: claims and assertions with provenance)
  BELIEF -->|scoped references| B
  BELIEF -->|context cues| A
  BELIEF -->|conflict candidates| C
  BELIEF -->|policy and risk markers| D
  COH --> SUP
```

---

## Platform Vision: SICA DAG Runtime

Externalize the simulated composite into a runtime with **hard permissions**, **audit logs**, and **vendor/model agnosticism**.

```mermaid
flowchart LR
  ORCH(Supervisor Orchestrator) --> NA(Node A: Part A service)
  ORCH --> NB(Node B: Part B service)
  ORCH --> NC(Node C: Part C service)
  ORCH --> NG(Node G: Risk Gate service)

  subgraph CTRL["Controls"]
    RBAC(RBAC, SSO, SCIM)
    LOGS(Immutable Logs)
    RES(Data Residency and Retention)
  end

  NA --- RBAC
  NB --- RBAC
  NC --- RBAC
  NG --- RBAC
  NA --- LOGS
  NB --- LOGS
  NC --- LOGS
  NG --- LOGS

  BELIEF(Belief Graph) --> NA
  BELIEF --> NB
  BELIEF --> NC
  POL(Policy Graph for enterprise mode) --> NG

  ORCH --> OUT(Synthesis and Evidence Bundle)
```

---

## Design Inspirations (appendix)

We borrow language from modular cognitive theories (e.g., IFS as a legible **metaphor** for “parts”), classic AI orchestration (blackboard, behavior trees/HTNs), and modern workflow DAGs. In practice, our composites are **deterministic software artifacts**: a supervisor‑routed, weighted DAG with belief‑bounded knowledge and auditable outputs.

---

## License

Copyright © 2025 Penny & Damed Inc. **All rights reserved.**
