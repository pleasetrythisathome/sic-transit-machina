# Pitch Narrative — Sic transit Machina

## 1. Hook
**Deterministic perspective, not external truth.**  
We build synthetic identities as composites of parts that ingest a media diet, accumulate belief graphs with provenance, and speak via a supervisor routing across a weighted DAG. Style emerges. Contradictions are preserved and adjustable at output time.

---

## 2. Problem
Today's agents collapse voice and erase contradiction:
1) **Flat, generic tone** — no enduring, legible identity.  
2) **Black-box “facts”** — no lineage; no way to time-travel.  
3) **Forced coherence** — contradictions are filtered out, not surfaced.

Researchers, media teams, and creators need **stable perspectives with provenance** that evolve with their inputs — not brittle “truthy” chatbots.

---

## 3. Why Now
- **Agentic content is exploding**; brand and authorship matter again.  
- **Provenance** is becoming mandatory for trust (boards, regulators, readers).  
- **Model volatility** (quality, cost, policy) demands vendor-agnostic design.  
- We can now **simulate supervised composites** in a single prompt (PSPS) and externalize them later to a robust DAG (SICA).

---

## 4. ICP & Initial Use Cases
**ICP v1 (land):** labs & research groups, media/newsletters, think tanks, long-form creators.  
**Use cases:**  
- **Perspective engines** for columnists/newsletters with time-travel and coherence control.  
- **Research identities** that ingest domain corpora and keep contradictory stances visible.  
- **Brand voices** trained on owned archives with lineage for every claim.

---

## 5. Wedge — Interactive Identity Builder (IIB)
**We don’t ship prompts; we ship the machine that manufactures them.**

The **Builder** constructs a **Composite Identity**:
- a **Supervisor**,
- a library of **Parts** (archetypal lenses),
- a **Weighted DAG** for routing,

…then exports a **PSPS** artifact: a single‑prompt composite (supervisor + parts + DAG + belief references) deployable in one model context.

**Workflow**
1) **Ingest → Belief Graph (RDF‑ish)** with provenance from your corpus/feeds.  
2) **Narrative Reaction**: parts “react” to real content to individuate tone & constraints.  
3) **DAG Composer**: propose/edit routing weights and guardrails.  
4) **Export PSPS**: paste-and-run composite; ≤14 days to first demo.

**Benefits**
- **Determinism & voice** via supervisor + parts.  
- **Provenance** for every claim.  
- **Coherence dial** to surface or tighten contradictions.  
- **No infra** to pilot: single‑model prompt artifact.

---

## 6. How It Works (PSPS internals)
- **Supervisor** co-simulates routing across parts in one prompt.  
- **Parts**: deterministic style, 2‑axis orientation, scoped interests, belief extraction.  
- **Weighted DAG**: enforces turn-taking and influence types (amplify/challenge/reframe/evidence).  
- **Belief Graph**: deduped claims; identity‑ and part‑specific assertions with weights & sources.  
- **Evidence strings**: each output cites lineage and records which parts spoke.  
- **Soft permissions**: in‑prompt constraints; runtime isolation comes later (SICA).

---

## 7. Security & Provenance
- **Immutable lineage** of beliefs and outputs.  
- **Time‑travel** (as‑of) to answer from a historical identity state.  
- **Safety/TOS gate** for publishing pipelines.  
- **Clojure + Datomic** for immutable facts and Datalog recursion.

*(Enterprise Mode adds RBAC, SSO/SCIM, and policy‑of‑record constraints—see addendum.)*

---

## 8. Platform Vision — SICA DAG
**Synthetic Identity Composite Agent DAG:** externalize PSPS to runtime.

- Each **Part** becomes an isolated node with **hard permissions**.  
- The **Supervisor** is a real orchestrator.  
- **Vendor/model agnostic**; nodes are hot‑swappable.  
- **Compliance isolation** by node/domain.  
- **Data moat**: telemetry (route stability, drift), curated belief/policy packs.

---

## 9. Go‑to‑Market
- **Land:** Builder pilots for one identity (newsletter, lab persona, research bot).  
- **Expand:** More identities; richer diets; multi‑persona publications.  
- **Scale:** Graduate to SICA for runtime isolation, parallelism, and team governance.

Channels: founder-led design partners; public demos of coherence dial & time‑travel; collaborations with labs/media.

---

## 10. Business Model
- **Builder license** (per environment): ingestion adapters, belief graph curation, reaction harness, DAG composer.  
- **Identities (PSPS exports)**: per-identity monthly; tiers by corpus size and usage.  
- **SICA platform**: platform fee + node packs + usage (syntheses with provenance).

---

## 11. Competition & Differentiation
**Copying a prompt ≠ copying an identity system.** Our defensibility accrues in:
1) **Belief Graphs** — curated, provenance‑rich, customer‑specific.  
2) **Narrative Individuation** — reaction harness & constraints tuned on real content.  
3) **Weighted DAGs** — workflow‑specific routing & influence.  
4) **Telemetry & Policy Packs** — stability metrics, exception taxonomies, coherence profiles.

Frameworks sell “agents.” Prompt libs sell templates.  
We sell the **Builder** and the **identity lifecycle** (diet → beliefs → routing → synthesis → evidence).

---

## 12. Proof & Roadmap
- **P0**: schema, ingestion, two parts, end‑to‑end reaction→synthesis.  
- **P1**: three‑identity demo (A/B share baseline then diverge; C independent); coherence dial; time‑travel UI.  
- **P2**: contextual DAG edges; 2 deliberation rounds; games & Elo.  
- **P3**: adapters (Twitter/Substack/email), TOS gate, caching, ops.

Pilot metrics: coherence control works; belief lineage completeness; route stability; style emergence over time.

---

## 13. Team / Why Me
Repeat founder (venture‑backed → later acquired by Noodle), C‑suite growth & narrative at Vouch (~50% YoY ARR), principal engineer & app‑team lead; synthetic identity thought leadership (Incoming Adjunct, NYU ITP). We’ve shipped narrative + systems, not just one or the other.

---

## 14. The Ask
Raise **$X** to: (1) GA Builder (belief graph, reaction, DAG composer), (2) adapters & TOS gate, (3) 5 design partners, (4) SICA DAG beta.

---

## 15. Key Risks & Mitigations
- **Prompt leakage:** Value lives in curated belief graphs, individuation, weights, and telemetry → requires Builder to replicate.  
- **Diet bias / epistemic closure:** ε‑explore & provenance visualization; owner controls diet boundaries.  
- **Scale limits (Datomic tx/indices):** batch per‑content transactions, noHistory on churn, raw storage out‑of‑db.  
- **Enterprise expectations:** offer **Policy Mode** (below) that constrains outputs to policy‑of‑record.

---

## Addendum — Enterprise Mode (Policy‑of‑Record)
Same engine, different output contract.

- **Policy Graph**: separate, signed corpus (policies, plans, product docs).  
- **Belief Graph** remains in storage; **outputs** draw only from Policy Graph.  
- **Contradictions** retained in storage; surfaced only in QA/analytics.  
- **Controls**: SSO/SCIM, RBAC, immutable logs, exportable evidence, data residency.

This supports **customer support, compliance, and internal assistants** without discarding the perspective engine that makes identities legible.
