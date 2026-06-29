# Competitive & Improvement Analysis — `lab-protocols-open`

> Scope: versioned, structured, openly-licensed wet-lab protocols for cancer research, with
> rigorous safety annotations, provenance-per-assertion, and credentialed expert review.
> Risk posture: medium project-wide, **high** for all safety/human/animal-material content.
> Date: 2026-06-29.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually rigorous and internally consistent. It correctly leads with cancer
guardrails, treats safety as high-stakes despite a nominal "medium" tier, and is honest about
`verifiedNeed: false` with both partner and safety reviewer "TO BE SECURED." Findings below are
ordered roughly by importance; most are refinements, not defects.

**Strong (verified present):**

- **Safety auto-escalation** (`riskTier: high` on any hazard/biohazard/animal/human-material flag,
  authors cannot downgrade) is the single best design decision — it removes human discretion from
  the most dangerous lever. Good.
- **Non-skippable credentialed biosafety/EHS sign-off** as a blocking ship gate, with a Sev-1
  safety-erratum lifecycle (pull/fix/re-review/re-version/notify adopters). Correct and rare.
- **Provenance-per-assertion** (each step and each safety claim cites an authoritative source) as a
  hard 100% linter gate. This is the project's signature and is correctly load-bearing.
- **Per-source license verification with default-EXCLUDE** (snapshot + SHA-256 + Wayback + cited
  clause), explicit cite-only list (Nature/CSH/Springer Protocols, Methods Mol Biol), CC-BY-NC
  contamination rule, and the protocols.io "licenses vary per protocol — never assume CC-BY" caveat.
  All correct and conservative.
- **Dual-use/DURC screen** with explicit exclusions (viral-vector, gain-of-function,
  select-agent-adjacent, gene-drive). Present.
- **Human-derived-material protocols documented as method-only** with mandatory IRB/consent
  pointers and zero donor data; animal protocols require IACUC + 3Rs. Correct.
- **"Not a substitute for institutional biosafety/EHS/IBC/IACUC/IRB"** disclaimer + RUO disclaimer
  standardized on every protocol. Correct and distinct from the "not medical advice" framing.

**Gaps / weaknesses to fix (most important first):**

1. **The dual-use/biosecurity screen is under-specified against the 2024 US DURC/PEPP policy.**
   The plan names DURC and exclusions but does not anchor the screen to the May 2024 USG Policy for
   Oversight of DURC and PEPP (Category 1 / Category 2 framework) or the April 2024 *Framework for
   Nucleic Acid Synthesis Screening* / Oct 2023 *Screening Framework Guidance for Providers and
   Users of Synthetic Nucleic Acids*. For a cancer-protocol corpus this mostly bites at the edges
   (oncolytic viruses, CRISPR/gene-editing reagents, oncogene/tumor-suppressor constructs,
   immortalization via viral oncogenes, lentiviral/retroviral transduction — all *common* in
   cancer labs and all dual-use-adjacent). The screen needs a concrete, citable checklist mapped to
   these documents and a named human owner, not a boolean `dualUseScreen.screened`. **This is the
   most consequential correctness gap.**

2. **"Safety completeness" is asserted as a linter gate but the linter can only check
   *structural* presence, not *correctness*.** A protocol can have a fully-populated `safety` block
   that is wrong. The plan's 100% safety-completeness metric (#4) risks being read as a safety
   *assurance*; it is only a *presence* check. The plan should explicitly state that the linter
   verifies presence + resolvable provenance, and that correctness is owned solely by the
   credentialed reviewer — otherwise a green linter could be mistaken for "safe."

3. **SDS provenance is version- and region-volatile.** Safety claims cite manufacturer SDS and
   GHS/ECHA classifications, but SDS documents are revised, vendor-specific, and jurisdiction-keyed
   (GHS adoption differs US/EU/UN rev). The snapshot-with-SHA-256 model handles drift detection but
   the plan should require recording SDS **revision date + jurisdiction + GHS revision** in
   `materials[].sdsRef`, and a re-review trigger when an SDS is superseded. Open question #5
   (regional regulation) surfaces this but it is not yet wired into the data model.

4. **No explicit handling of carcinogen/CMR-specific controls.** Cancer labs routinely use known
   human carcinogens and mutagens (ethidium bromide, formaldehyde, acrylamide, phenol/chloroform,
   DMBA/MNU and other chemical carcinogens in models, doxorubicin/cisplatin and other cytotoxic
   drugs). OSHA 29 CFR 1910.1450 Appendix A "particularly hazardous substances" (select carcinogens,
   reproductive toxins, high-acute-toxicity) demands designated areas, special handling, and
   decontamination — a richer requirement than generic GHS hazard classes. The `safety` sub-schema
   should add a `particularlyHazardous`/CMR flag with its own control vocabulary.

5. **Biosafety taxonomy should bind to BMBL 6th ed. risk-group vs. containment distinction.** The
   plan references BMBL and "biosafety levels" but conflates agent **risk group** (1–4) and
   **biosafety level** (BSL 1–4) loosely; they are related but not identical, and the assignment is
   institution/IBC-determined. The vocab should keep these as separate fields and flag that BSL is
   *recommended pending local IBC assignment*, never asserted as final.

6. **PDX / patient-derived organoid lineage provenance.** Even as "method only," PDX and
   patient-derived organoid/cell-line methods carry a known reproducibility hazard:
   misidentified/cross-contaminated cell lines and undisclosed donor-consent constraints on
   downstream sharing. The plan mandates IRB/consent pointers (good) but should also require an STR
   authentication / cell-line provenance step and a `mycoplasma`/identity QC callout in the schema's
   `qc[]` for any cell-line protocol (the plan already lists mycoplasma testing as a seed protocol —
   wire it in as a cross-cutting QC requirement).

7. **Versioning model is sound but lacks a deprecation/safety-supersession semantics.** Semver +
   changelog is correct. Add an explicit rule: a safety-affecting change is *always* at least a
   minor bump with a `safetyImpact` changelog tag, and superseded versions carry a machine-readable
   "do not use — safety erratum" status so pinned adopters are warned. Ties to Sev-1 lifecycle.

8. **"Not replacing institutional safety training" is stated but not enforced in the artifact.**
   The disclaimer exists; consider a required, non-removable header block in the *rendered* output
   (not just the record) so a downloaded PDF can never be stripped of the institutional-authority
   and training caveat.

9. **Minor:** Owner is "TBD"; outcome metric #1 (≥5 external adopters) is ambitious for a cold-start
   with no partner — reasonable as a 12-mo target but the M0/M1 gates correctly de-risk it. The
   schema's interop claims (Bioschemas `LabProtocol`, protocols.io round-trip) are aspirational —
   Bioschemas `LabProtocol` is still a DRAFT profile (0.8-DRAFT), so "round-trip" should be scoped
   as best-effort mapping, not a guarantee.

Overall: the plan meets and exceeds the Elyos quality bar; the principal real gap is operationalizing
the **biosecurity/dual-use screen** against current (2024) policy, and clearly separating
*structural* safety-completeness checks from *expert-owned* safety *correctness*.

---

## 2. Competitive landscape

| Resource | Open? | Structured/machine-readable | Versioned | Safety annotation | License clarity | Cancer coverage |
|---|---|---|---|---|---|---|
| **protocols.io** | Yes (public; CC-BY default) | Partially (steps/reagents; free API; CLOCKSS-archived) | Yes (per-version DOI) | Weak/inconsistent | Per-protocol (varies; NC/ARR exist) | High (volume) |
| **Bio-protocol** | Yes (curated, peer-reviewed, OA) | No (article prose) | Limited | Inconsistent | Per-article (often CC-BY) | High |
| **STAR Protocols** (Cell Press) | Yes (OA, peer-reviewed) | Semi (STAR = Structured/Transparent/Accessible/Reproducible template) | Journal versioning | Inconsistent | OA license per article | Moderate–High |
| **Nature Protocols** | **No** (subscription; Springer Nature Experiments) | No | No | Editorial | Copyright, ARR | High |
| **Cold Spring Harbor Protocols** | Mostly **No** (subscription; some free trials) | No | No | Editorial | Copyright, ARR | High |
| **Current Protocols** (Wiley) | **No** (subscription; 14,000+ procedures, 17 titles) | No | Periodic updates | Editorial | Copyright, ARR | High |
| **JoVE** | **No** (subscription; video + text) | No | No | Shown in video | Copyright, ARR | High |
| **OpenWetWare** | Yes (wiki, open) | No (free-text wiki templates) | Wiki history only | Ad hoc | CC (site-level) | Moderate |
| **Addgene protocols** | Yes (free) | No (curated prose + video) | No formal | Some (reagent-level) | Addgene terms | Moderate (plasmid/viral prep heavy) |

**Strengths/weaknesses (cited):**

- **protocols.io** — the dominant open repository: CC-BY default, per-version DOIs, free public API,
  CLOCKSS archiving, interactive/forkable. Weaknesses: **no peer review/vetting by default**
  (anyone can publish and auto-mint a DOI), inconsistent safety sections, licenses **vary per
  protocol** (CC-BY-NC and all-rights-reserved exist), and structure is shallow vs. a strict schema.
  Sources: https://www.protocols.io/help/faq ; https://www.nature.com/nprot/protocolsio ;
  https://labcritics.com/blog/tools/protocols-io/ ;
  https://countway.harvard.edu/services/publishing-data-services/data-services/countway-protocolsio-service
- **Bio-protocol** — curated, peer-reviewed, open access, high quality; weakness: prose articles,
  not machine-readable, limited versioning. https://guides.library.illinois.edu/protocols
- **STAR Protocols** — Cell Press OA, peer-reviewed, the "STAR" template enforces some structure and
  a reproducibility ethos; weakness: still article-shaped, not a JSON schema; safety uneven.
  https://www.cell.com/star-protocols/home
- **Nature Protocols / CSH Protocols / Current Protocols / JoVE** — high editorial quality and
  breadth (Current Protocols: 14,000+ procedures across 17 titles), but **paywalled and
  copyrighted**, not machine-readable, not openly licensed; usable as **citations only**.
  https://cshprotocols.cshlp.org/ ; https://www.jove.com/ ;
  https://libraryhelp.ucsf.edu/hc/en-us/articles/360041497054-Methods-and-Protocols-Resources
- **OpenWetWare** — genuinely open wiki with protocol templates, but unstructured free text,
  variable quality, wiki-history "versioning," ad-hoc safety. https://openwetware.org/wiki/Protocols/Search ;
  https://openwetware.org/wiki/Protocols/Template
- **Addgene protocols** — free, reagent-grounded (plasmid/cloning/viral prep), some safety notes,
  but narrow and not a general structured corpus. https://www.addgene.org/protocols/
- **Standards/interop context** — Bioschemas `LabProtocol` (Thing > CreativeWork > HowTo >
  LabProtocol; ISA-model-inspired; still **0.8-DRAFT**) and schema.org `LabProtocol` are the
  emerging machine-readable vocabularies; academic efforts ("Building an Open Representation for
  Biological Protocols," wet-lab protocol corpora) show the field still lacks one unambiguous,
  safety-bearing, machine-actionable representation. https://bioschemas.org/profiles/LabProtocol/0.8-DRAFT ;
  https://www.biorxiv.org/content/10.1101/2022.07.05.498808.full.pdf

**Key competitive takeaway:** No incumbent combines *all five* of (open license) + (strict
machine-readable schema) + (rigorous, provenance-backed, expert-reviewed safety) + (semver +
diffable changelog) + (cancer focus). protocols.io has open+DOI+API but not vetting/safety/strict
schema; the paywalled journals have quality+editorial but not open/structured; OpenWetWare is
open but unstructured. The white space is real.

---

## 3. Gaps we can fill

1. **Safety as a first-class, provenance-backed, expert-signed field** — the single most uneven
   thing across all incumbents (PPE, GHS hazard class, carcinogen/CMR controls, biosafety
   level/risk group, waste category, spill/exposure response), each item citing SDS/BMBL/OSHA/EPA.
2. **A strict, validated machine-readable schema** (JSON Schema + ajv) where protocols.io/OWW are
   loose and journals are prose — enabling lint/validate/diff/automation.
3. **License-clarity by construction** — every protocol born CC-BY-4.0 (content) / MIT (tooling)
   with per-source license evidence, vs. protocols.io's per-protocol license lottery.
4. **Provenance-per-assertion** — neither incumbent cites a source on *every* step and *every*
   safety claim; this is a reproducibility-crisis mitigation no one ships.
5. **Real semver + human-readable changelog + safety-supersession** — beyond protocols.io's
   per-version DOI; pinnable, diffable, with "do-not-use" safety errata semantics.
6. **Cancer curation + ethics scaffolding** — IRB/IACUC/IBC pointers, 3Rs framing, RUO disclaimers,
   cell-line authentication QC — packaged as method-only with zero patient data.
7. **An operationalized dual-use/biosecurity screen** mapped to 2024 USG DURC/PEPP — essentially
   absent from open protocol repositories.

---

## 4. Differentiators to win

- **"Safety you can audit."** Every hazard/PPE/waste/exposure line is provenance-linked to an
  authoritative source (SDS rev-dated, BMBL, OSHA, GHS/ECHA) and signed off by a credentialed
  biosafety/EHS reviewer — a guarantee no incumbent makes.
- **Schema-first + lint-enforced** quality gates (provenance 100%, safety-section completeness,
  semver+changelog present) that *block* shipping — quality by construction, not editorial luck.
- **Born-open, license-clean CC-BY-4.0 commons** with default-EXCLUDE on any unverifiable source —
  cleanly donatable/forkable, no NC contamination.
- **Diffable, pinnable versions with safety-erratum supersession** — adopters can trust and track
  revisions the way they track software dependencies.
- **Interoperable, not siloed** — maps to Bioschemas/schema.org and protocols.io import/export, so
  it *augments* the ecosystem rather than competing for volume.
- **Cancer-specific ethics + biosecurity rigor** — IRB/IACUC/IBC + 3Rs + RUO + DURC screen baked in.

The winning wedge is **narrow + deep + safe + open**: a small set of common cancer protocols that
are demonstrably safer, better-cited, and machine-trustworthy than anything free that exists.

---

## 5. Claude API leverage (and hard limits)

**Where Claude adds leverage (human-in-the-loop, draft-only):**

1. **Structuring/standardizing** — convert an openly-licensed source protocol (or a lab's messy
   PDF/Markdown) into the strict JSON Schema (steps, materials, equipment, QC, troubleshooting),
   normalizing units, temperatures, durations, and reagent naming to vendor-agnostic terms. This is
   high-value, repetitive, and verifiable against the schema. Use tool-use/structured outputs with
   the JSON Schema as the contract; validate every output with ajv (not the model) before a human reviews.
2. **Drafting safety annotations from authoritative sources** — given a *human-supplied* SDS / BMBL
   / OSHA / GHS citation, draft the PPE/hazard/waste/spill/first-aid fields *with inline source
   pointers*, flagging any field it could not ground. Claude proposes; the credentialed reviewer
   disposes. Never let Claude invent a hazard value.
3. **Versioning/diffing & changelog drafting** — semantic diff of v1→v2 protocol records, classify
   change as patch/minor/major, auto-flag any change touching a `safety`/`ethics`/`dualUse` field as
   `safetyImpact`, and draft a human-readable changelog entry for reviewer approval.
4. **Documentation & rendering polish** — generate the human-readable Markdown/HTML from the record,
   draft contributor docs, reviewer checklists, and the controlled-vocabulary tables.
5. **Provenance/completeness triage** — pre-screen candidate protocols for missing citations,
   dangling SDS/CAS refs, or absent safety sections *before* human review, to make reviewers faster.
   (The *binding* linter remains deterministic code, not the model.)

**Where Claude must NOT decide (hard guardrails, per CLAUDE.md):**

- **No LLM safety/biosafety assurance.** Safety and biosafety *correctness* is owned by a
  credentialed human reviewer + institutional authority. A model-drafted safety block is a *draft*;
  it is never "verified" by the model, and a green automated lint is presence-only, not assurance.
- **Dual-use/DURC screening is human-owned.** Claude may surface candidate concerns (flag, never
  clear); the accept/exclude decision is a named human against the 2024 USG policy. The model must
  refuse to *generate* enhanced-hazard / weaponization / gain-of-function / select-agent methods.
- **License & attribution are human-verified.** Claude may extract a license clause and propose
  `permitsDerivatives`, but a human confirms against the snapshot; unverifiable = EXCLUDE.
- **No fabricated steps, reagents, quantities, or hazard data.** Every assertion must trace to a
  cited source; Claude must mark, not invent, anything ungrounded. No hallucinated CAS numbers,
  concentrations, temperatures, or SDS values.
- **Never lower the ethics bar** — no removing IRB/IACUC/IBC/consent pointers, no asserting
  clinical/diagnostic fitness (RUO only).

---

## 6. Ten concrete optimizations

1. **Operationalize the dual-use screen** as a versioned checklist mapped to the May-2024 USG
   DURC/PEPP policy (Category 1/2) and the 2023/2024 nucleic-acid synthesis screening frameworks;
   add cancer-specific trigger list (oncolytic virus, lenti/retroviral transduction, viral
   immortalization, gene-editing of oncogenes/tumor suppressors). Named human owner, recorded reviewer.
2. **Add a `particularlyHazardous`/CMR sub-field** to `safety` aligned to OSHA 1910.1450 App. A
   (select carcinogens, reproductive toxins, acute toxins) with designated-area/decon controls — not
   just generic GHS classes.
3. **Split `riskGroup` (1–4) from `biosafetyLevel` (1–4)** in the vocab; mark BSL as
   "recommended, pending local IBC assignment," never final.
4. **Version- and jurisdiction-stamp SDS provenance** (`sdsRef` = vendor + revision date +
   GHS revision + jurisdiction) with an auto re-review trigger on supersession.
5. **Make the safety/structural-completeness vs. correctness distinction explicit** in the linter
   output and docs: linter = presence + resolvable provenance; correctness = credentialed reviewer.
6. **Add cross-cutting cell-line QC requirements** (STR authentication + mycoplasma) to `qc[]` for
   any cell-line/PDX/organoid protocol; lint for their presence.
7. **Add `safetyImpact` changelog tag + machine-readable "do-not-use" supersession status** so
   pinned adopters are warned on a Sev-1 erratum; surface via the public ledger and (if used) DOI/version note.
8. **Non-removable rendered-output header** carrying the RUO + institutional-authority +
   "not a substitute for safety training" notice, so a downloaded PDF can't be stripped.
9. **Ship a deterministic CI gate bundle** (ajv schema validate + provenance linter + license-clause
   presence + dual-use-screen-recorded + DCO sign-off) on every PR; the LLM never gates.
10. **Bootstrap interop early** — publish a Bioschemas `LabProtocol` (0.8-DRAFT) and protocols.io
    export mapping with a round-trip *fixture test*, scoped as best-effort (the profile is draft), so
    the corpus is donatable and discoverable from day one.

---

## 7. Parallel & perpendicular spin-offs

- **`ewing-reproducibility` (parallel):** Ewing-sarcoma protocols are a natural seed subset —
  cell-line culture, EWS-FLI1 knockdown, organoid/PDX methods — feeding the same schema and
  provenance discipline; reproducibility findings there directly drive which protocols to harden.
- **`reproducibility-curriculum` (parallel):** Each shipped protocol + its provenance/safety
  apparatus is teachable material; the curriculum can use the schema, the "provenance-per-assertion"
  discipline, and the safety taxonomy as worked examples. Bidirectional reuse.
- **`onco-research-software-docs` (perpendicular):** Shared documentation tooling — the renderer,
  the controlled-vocabulary approach, and the changelog/semver discipline generalize from wet-lab
  protocols to research-software docs; the validator/linter pattern is common infrastructure.
- **Reusable protocol-standardization toolkit (spin-off product):** Extract `packages/schema`
  (Protocol JSON Schema + Safety/Provenance sub-schemas) + the validator/linter/renderer as a
  standalone MIT toolkit any lab or repository can adopt independent of the cancer corpus — the most
  broadly reusable artifact, and the natural handoff/donation vehicle (e.g., to a protocols.io
  collection).
- **MCP server (spin-off):** A "protocol-standardizer" MCP server exposing tools like
  `validate_protocol`, `lint_provenance`, `draft_safety_annotation(source)`, `diff_versions`, and
  `render_protocol` — letting any agent (including the donated-lane human's agent) structure and
  check protocols against the schema, with all binding checks running as deterministic code behind
  the tool boundary and dual-use/safety decisions explicitly returned as "needs human reviewer."

---

## 8. Open questions

1. **Biosecurity owner & policy mapping:** Who owns the dual-use screen, and against which exact
   policy version (the 2024 USG DURC/PEPP and synthesis-screening frameworks evolve)? How are
   cancer-specific edge cases (viral vectors, immortalization, gene editing) adjudicated?
2. **Credentialed safety reviewer:** minimum credential, verification method, availability/SLA for
   the non-skippable sign-off (PLAN open Q #2/#6) — still the hardest blocker.
3. **Regional regulation:** How to encode US (OSHA/EPA/BMBL) vs EU (ECHA/REACH) vs LMIC differences
   in waste/PPE/biosafety without implying one jurisdiction is universal (PLAN open Q #5)? Per-region
   safety overlays vs a single canonical record?
4. **Schema base:** Adopt protocols.io's data model / Bioschemas `LabProtocol` directly (faster
   interop, but it's a DRAFT) or keep our own and map (more control)? (PLAN open Q #4)
5. **First adopter:** Who, and will they co-maintain or only consume? Blocks `verifiedNeed`.
6. **Community submissions (M2+):** Contributor vetting, and how to keep the safety/provenance bar
   when scaling beyond a core team (PLAN open Q #7).
7. **LLM-draft disclosure:** Should records record that a field was Claude-drafted (then
   human-reviewed) for transparency/auditability of the pipeline?
8. **SDS redistribution:** Confirm the cite/link-only stance for vendor SDS is sufficient, and that
   no SDS text is reproduced verbatim in records or fixtures.

---

## Summary (for the requester)

**Top 3 competitors.** (1) **protocols.io** — the dominant open repository: CC-BY default,
per-version DOIs, free API, CLOCKSS archiving, but **no default vetting**, inconsistent safety, and
licenses that **vary per protocol**. (2) **Bio-protocol** — curated, peer-reviewed, open access, but
prose (not machine-readable) and lightly versioned. (3) **STAR Protocols** (Cell Press) — open,
peer-reviewed, "structured/transparent/reproducible" template, but still article-shaped with uneven
safety. Paywalled incumbents (Nature Protocols, CSH Protocols, Current Protocols, JoVE) are
high-quality but copyrighted, unstructured, and cite-only for us.

**Strongest single differentiator.** *Auditable safety:* every hazard/PPE/waste/exposure line is
provenance-linked to an authoritative source (rev-dated SDS, BMBL, OSHA, GHS/ECHA) **and** signed
off by a credentialed biosafety/EHS reviewer, inside a strict, lint-enforced, openly-licensed
(CC-BY-4.0) machine-readable schema with real semver + diffable changelogs. No incumbent combines
open + strict-schema + expert-reviewed-safety + versioning + cancer focus.

**Top 3 Claude-API ideas.** (1) Structure messy source protocols into the JSON Schema (units,
reagents, steps normalized), validated by ajv, human-reviewed. (2) Draft safety annotations *from
human-supplied authoritative sources* with inline provenance, flagging anything it cannot ground —
reviewer disposes. (3) Semantic version diffing + changelog drafting that auto-tags any
safety/ethics/dual-use change as `safetyImpact`. In all cases Claude drafts; deterministic code and
credentialed humans gate.

**Two most important plan-correctness findings.** (1) **Biosecurity/dual-use is the real gap:** the
screen is named but not operationalized against the **May-2024 USG DURC/PEPP policy** and the
2023/2024 nucleic-acid-synthesis screening frameworks — and cancer labs routinely touch dual-use-
adjacent methods (oncolytic/viral vectors, viral immortalization, gene editing). It needs a concrete
citable checklist with a named human owner, not a boolean. (2) **Safety "completeness" is presence,
not correctness:** the 100% safety-completeness linter gate can only verify a field is present and
sourced; it cannot verify the value is *safe*. The plan must state explicitly that correctness is
owned solely by the credentialed reviewer + institutional authority, so a green linter is never
mistaken for a safety assurance. Both reinforce the plan's own (correct) instinct to treat safety as
high-stakes despite the "medium" label.
