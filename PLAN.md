# PLAN — lab-protocols-open

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) ·
> Lane: donated · Risk tier: **medium** project-wide, with **high** for all
> safety-bearing and human/animal-material content (see §8).

---

## Executive summary

Cancer wet-lab research runs on protocols — the step-by-step recipes for culturing cells,
extracting nucleic acids, running assays, building animal models, and preparing samples for
sequencing. In practice these protocols are scattered, inconsistently versioned, paywalled
(Nature Protocols, Cold Spring Harbor Protocols, Springer Protocols are copyrighted and **not**
open), or buried as unstructured PDFs and lab-bench tribal knowledge. Critically, **safety
information is the most uneven part**: hazard warnings, PPE, waste disposal, and emergency steps
are frequently absent, outdated, or wrong — and getting them wrong can injure a researcher.

**lab-protocols-open** produces a small, high-quality library of **versioned, structured,
openly-licensed wet-lab protocols for cancer research**, each with a machine-readable schema,
explicit provenance on every claim, a rigorous safety section, and **semantic versioning with a
human-readable changelog**. The deliverable is *documentation and tooling* (a protocol schema, a
validator, a renderer, and reviewed protocol records) — **never** the execution of experiments
and **never** patient data.

This project is deliberately conservative on two axes:

1. **Cancer data guardrails (binding).** It uses **only open-access / aggregate / de-identified**
   reference material. Controlled-access genomic data (dbGaP, EGA, individual-level biobanks) and
   any identifiable patient data are **out of scope**. Protocols that *touch* human-derived
   material (primary tumor culture, PDX establishment, biospecimen handling) are documented only
   as **method descriptions** that explicitly require local **IRB/ethics approval and informed
   consent**, and contain **no patient data**.
2. **Safety is treated as high-stakes content.** All safety notes and any protocol involving
   hazardous reagents, biohazards, animals, or human-derived material require **credentialed
   expert sign-off** (biosafety/EHS professional and, where relevant, the source domain expert)
   before a protocol is marked "delivered." Every protocol carries a prominent **"not a
   substitute for your institution's biosafety, EHS, IBC, IACUC, or IRB requirements"** notice.

No partner institution, lab, or protocol repository has yet agreed to accept or co-maintain this
work. Therefore `verifiedNeed = false` on every task and the partner/requestor is **TO BE
SECURED**. The general need is real and well-documented; per-deliverable demand is unproven until
a named lab, core facility, or repository commits to adopt the output.

---

## Problem & beneficiaries

**Who is helped (intended beneficiaries):**

- **Early-career and resource-limited cancer researchers** — graduate students, postdocs, and
  technicians in labs (especially in low- and middle-income countries) that cannot afford
  subscriptions to copyrighted protocol journals and lack a senior bench mentor for every method.
- **Reproducibility of cancer science** — the field-wide beneficiary. Unstructured, unversioned,
  under-cited protocols are a documented driver of the reproducibility crisis; structured,
  provenance-bearing, versioned protocols are a direct mitigation.
- **Core facilities and teaching labs** that need a citable, openly-licensed, safety-reviewed
  baseline protocol to adapt locally rather than re-deriving from scratch.
- **Patient-advocate and citizen-science groups** (secondary) who want to *understand* how
  research is done — though this project produces **researcher-facing** material, not
  patient-facing content.

**The verified need:** **TO BE SECURED.** The macro-need (open, reproducible, safe protocols) is
supported by the reproducibility literature and the existence of community efforts
(protocols.io, Bio-protocol). But Elyos's bar is a *named requestor who will accept and use the
specific deliverable*. Until a lab, cancer core facility, or repository (e.g., a protocols.io
collection owner, a teaching program, or an open-science org) commits in writing, every task
records `verifiedNeed: false`.

**Partner org:** **TO BE SECURED.** Outreach candidates (no commitment yet): protocols.io
collection maintainers, Bio-protocol editors, academic cancer-center core facilities, the Reproducibility
Project / open-science nonprofits, and biosafety/EHS professional networks for the safety reviewer
role. Securing at least one adopting partner **and** one credentialed safety reviewer is a hard
M0 exit gate.

---

## Goals and non-goals

**Goals**

1. Define an open, machine-readable **protocol schema** (JSON Schema) that captures steps,
   materials with hazard data, equipment, safety, provenance, ethics flags, QC, troubleshooting,
   and versioning — interoperable with existing standards (Bioschemas `LabProtocol`,
   schema.org `LabProtocol`, protocols.io export).
2. Produce a **small seed library** (~15–20) of **common, low-to-moderate-hazard** cancer-research
   protocols, each with full provenance, a rigorous safety section, and expert sign-off.
3. Enforce **provenance on every assertion** (each step, reagent, hazard claim, and safety
   instruction cites an authoritative source) and **per-source license verification**.
4. Ship **tooling**: a schema validator, a provenance/safety completeness linter, and a renderer
   (structured record → human-readable Markdown/HTML/PDF).
5. Establish **semantic versioning + changelog discipline** so adopters can pin, diff, and trust
   protocol revisions.

**Non-goals**

- **Not** a protocol *execution* service, lab automation system, or LIMS.
- **Not** a replacement for institutional biosafety, IBC, IACUC, IRB, or EHS authority — outputs
  are references to adapt locally, not approvals to proceed.
- **Not** medical, clinical, or treatment advice; **not** patient-facing care content.
- **Not** a host for patient data, controlled-access data, or any identifiable human data.
- **Not** an exhaustive catalog (protocols.io already aggregates at scale) — value is
  *curation + structure + safety + provenance + versioning*, not volume.
- **Not** a venue for novel, unvalidated, or dual-use-of-concern methods (see §7/§14).

---

## Success metrics (outcomes)

Outcome-based, beneficiary-centric. Vanity metrics (page views, stars) are explicitly **not**
success.

| # | Outcome metric | Baseline | Target (12 mo) | How measured |
|---|---|---|---|---|
| 1 | Protocols **adopted/forked/cited** by a real external lab or facility | 0 | ≥ 5 distinct external adopters | Citations, forks, written adoption confirmations |
| 2 | Protocols passing **full safety + expert sign-off** (Definition of Shipped) | 0 | ≥ 15 | Review ledger |
| 3 | **Provenance completeness** — % of assertions with a cited, license-verified source | n/a | 100% on shipped protocols (hard gate) | Linter report |
| 4 | **Safety completeness** — shipped protocols with full hazard/PPE/waste/emergency section | n/a | 100% (hard gate) | Linter + reviewer checklist |
| 5 | **Reuse value** — adopters reporting time saved or a safety improvement vs. prior protocol | 0 | ≥ 3 qualitative testimonials | Adopter survey |
| 6 | **Defect/correction rate** — safety errata raised post-publication | n/a | < 1 safety erratum per 10 shipped protocols; **0 unresolved** | Issue tracker, errata log |
| 7 | **Versioning integrity** — shipped protocols with valid semver + changelog | n/a | 100% | Validator |

A protocol that is structurally perfect but adopted by no one is **not** a success. Metric #1
(real adoption) is the headline outcome; everything else is a quality precondition for it.

---

## Scope

**In scope**

- A versioned **protocol schema** + JSON Schema validation.
- A **safety taxonomy** (hazard classes, PPE, waste, emergency) and a controlled vocabulary
  aligned to GHS, NIH/CDC biosafety levels, and standard waste categories.
- **Curating and re-structuring** existing **openly-licensed** protocols (verifying each source
  license) and authoring original adaptations under CC-BY-4.0.
- **Provenance + license metadata** on every protocol and assertion.
- **Tooling**: validator, provenance/safety linter, renderer, contribution workflow.
- A **seed library** of common, low-to-moderate-hazard cancer-research protocols (e.g., DNA/RNA
  extraction, qPCR setup, cell-line culture/passaging/cryopreservation, mycoplasma testing,
  Western blot, immunofluorescence, viability assays, gDNA library prep front-ends).
- **Method descriptions** for human-derived/animal protocols that *point to* required ethics
  approvals — without any patient data and without lowering the ethics bar.

**Out of scope (explicit)**

- Any **patient data, identifiable human data, or controlled-access data** (dbGaP/EGA/biobank
  individual-level data).
- **Re-publishing copyrighted protocols** (Nature Protocols, CSH Protocols, Springer Protocols,
  Methods in Molecular Biology) — these may be **cited** but not reproduced.
- **High-hazard / specialist methods** without an expert author *and* reviewer: select-agent
  work, BSL-3/4 procedures, high-activity radioisotope work, oncolytic-virus / gain-of-function /
  any dual-use-of-concern method.
- **Clinical, diagnostic, or treatment** protocols; anything patient-facing.
- **Hosting, distributing, or shipping reagents, cells, or biological materials.**
- **Asserting fitness for any regulatory/clinical use** (RUO disclaimer only).

---

## Solution approach & architecture

This is a **content + tooling** project (a curation pipeline plus a small software toolkit),
not a service. Architecture follows Elyos rules: agent-neutral core, no vendor lock-in, MIT for
code / CC-BY-4.0 for content.

### Pipeline (source → shipped protocol)

```
 candidate source ──▶ [License + Ethics + Dual-use gate] ──▶ structure into schema
   (open protocol,         (blocking, non-skippable)              │
    standard, SDS)                                                ▼
                                              author/adapt protocol record (YAML/JSON)
                                                                  │
                                ┌─────────────────────────────────┤
                                ▼                                  ▼
                     [validator: schema valid]        [linter: provenance 100% +
                                                        safety section complete]
                                ▼                                  ▼
                                └────────────▶ [Expert review] ◀───┘
                                      (technical + biosafety/EHS, +domain SME)
                                                  │
                                                  ▼
                                      semver tag + changelog ──▶ render (MD/HTML/PDF)
                                                  │
                                                  ▼
                                      Definition of Shipped (adopter handoff)
```

### Components

- **`packages/schema`** (additions) — the **Protocol JSON Schema** + a `Safety` and `Provenance`
  sub-schema; validated with `ajv` (consistent with the existing Task/Registry schemas).
- **`packages/cli` adapter / `tools/`** — `protocol validate` (schema), `protocol lint`
  (provenance + safety completeness, dangling SDS/CAS refs, missing changelog), `protocol render`
  (record → Markdown/HTML, optional PDF). Agent-neutral; no agent invocation.
- **Content repo** — `protocols/<slug>/<version>/protocol.yaml` + rendered outputs +
  `CHANGELOG.md` per protocol, under git for version control and diffable history.
- **Controlled vocabularies** — `vocab/hazards.yaml`, `vocab/ppe.yaml`, `vocab/waste.yaml`,
  `vocab/biosafety-levels.yaml`, mapped to GHS / NIH-CDC BMBL / institutional waste categories.

### Data model (protocol record, abridged)

| Field | Notes |
|---|---|
| `id`, `slug`, `title` | Stable identifier |
| `version` (semver), `status`, `changelogRef` | Versioning + lifecycle (`draft`→`in-review`→`shipped`→`deprecated`) |
| `category` | e.g. cell-culture, nucleic-acid, protein, imaging, seq-prep, animal-model |
| `riskTier` | `low`\|`medium`\|`high`; auto-escalates to `high` if any hazard/biohazard/animal/human-material flag set |
| `materials[]` | `{name, casNumber?, vendorAgnostic, hazardClass[], sdsRef, ghsPictograms[]}` |
| `equipment[]` | with safety-relevant settings |
| `steps[]` | `{n, action, duration, temperature?, critical:boolean, safetyCallout?, provenance}` |
| `safety` | `{ppe[], hazards[], biosafetyLevel?, wasteDisposal[], spillResponse, emergency, exposureFirstAidPointer}` — **every item provenance-backed** |
| `ethics` | `{humanDerivedMaterial:boolean, animalUse:boolean, requiresIRB, requiresIACUC, requiresIBC, consentRequired, notes}` |
| `qc[]`, `troubleshooting[]`, `expectedResults` | reproducibility aids |
| `provenance` | `{adaptedFrom[], license{id,url,permitsDerivatives,snapshotRef}, retrievedAt, attribution}` |
| `dualUseScreen` | `{screened:boolean, concern:boolean, reviewer, notes}` |
| `review` | `{technical, safety, domain}` each `{reviewer, date, signedOff}` |
| `disclaimers` | RUO + "not a substitute for institutional biosafety/EHS/IBC/IACUC/IRB" |

### Key decisions

- **Schema-first.** Nothing is authored until the schema + safety taxonomy exist; the schema is
  the contract that makes validation, linting, rendering, and interop possible.
- **Provenance per assertion, not per document.** Each step and each safety claim carries its own
  source — this is the project's signature quality bar.
- **Safety auto-escalation.** Any hazard/biohazard/animal/human-material flag forces
  `riskTier: high` → mandatory credentialed sign-off; authors cannot downgrade.
- **Interop over reinvention.** Map to Bioschemas `LabProtocol` / schema.org and support
  protocols.io import/export so the work plugs into existing ecosystems.
- **Curate, don't crawl.** Manual, license-checked curation of a *small* high-quality set, not
  bulk scraping.

---

## Data, licensing & compliance

**THIS SECTION LEADS THE PROJECT. Cancer guardrails are binding and non-negotiable.**

### Cancer-domain guardrails (binding)

- **Open-access / aggregate / de-identified only.** No controlled-access genomic data (dbGaP,
  EGA), no individual-level biobank data, no identifiable patient data of any kind enters this
  project — not in protocols, not in examples, not in test fixtures.
- **Human-derived-material protocols** (primary tumor culture, PDX, organoids from patient tissue,
  biospecimen prep) are documented as **methods only**, with mandatory `ethics` flags requiring
  **IRB/ethics approval + informed consent** at the executing institution. The protocol contains
  **no donor data** and asserts no exception to consent requirements.
- **Animal protocols** require explicit **IACUC** pointers and 3Rs (replacement/reduction/
  refinement) framing; this project documents method structure, not approval.
- **No medical/clinical advice.** Research-use-only. Every protocol carries the RUO and
  "not a substitute for institutional biosafety/EHS/IBC/IACUC/IRB" disclaimers.

### Source licensing — verify EVERY source (conservative)

A source may be **cited** freely; it may only be **reproduced/adapted** if its license permits
derivatives. The License+Ethics gate (blocking) records, per source: license id, URL, a
**text snapshot** (committed copy + SHA-256 + Wayback URL), and a `permitsDerivatives` boolean
derived from a **cited clause** — missing/unparseable evidence = **EXCLUDE** (no default-allow).

| Source type | Typical license | Reuse stance |
|---|---|---|
| protocols.io protocols | Often **CC-BY**, sometimes **CC-BY-NC** or all-rights-reserved — **varies per protocol** | Reproduce/adapt only if that specific protocol is CC-BY (or CC0); **NC excluded** from CC-BY-4.0 output (cite only) |
| Bio-protocol | Per-article license (often CC-BY) | Verify per article |
| Nature Protocols, CSH Protocols, Springer Protocols, Methods Mol Biol | **Copyrighted, all rights reserved** | **Cite only — never reproduce** |
| Manufacturer SDS / product inserts | Vendor copyright | **Cite/link** for hazard data; do not republish verbatim; keep vendor-agnostic |
| Government safety guidance (NIH BMBL, CDC, OSHA, EPA, ECHA/GHS) | Public domain (US gov) or open gov license | Safe to cite and quote with attribution; verify per source |
| COSMIC / OncoKB (if ever referenced) | **Non-commercial** | Cite only; NC content excluded from CC-BY output |

**License of our output:** **CC-BY-4.0** for all protocol content and documentation; **MIT** for
all tooling/schema. We do **not** produce CC-BY-NC output (it would contaminate the CC-BY-4.0
commons), so NC source material is **cite-only, never adapted**.

### Provenance model

- Every protocol record has a `provenance` block (adaptedFrom[], license, snapshotRef,
  retrievedAt, attribution) **and** per-step / per-safety-claim source references.
- Safety claims must cite **authoritative** sources (SDS for chemical hazards; NIH BMBL/CDC for
  biosafety; OSHA/EPA for waste; ECHA/GHS for classification) — never an uncited "lab wisdom" note.
- A protocol cannot pass the linter with any assertion lacking a resolvable source reference.

### Privacy / PII stance

- **Zero PII / PHI by design.** No patient, donor, or identifiable data. Contributor identities
  are handled per standard open-source contribution norms (DCO sign-off), not as research subjects.
- No secrets/tokens/keys in any record, fixture, log, or commit (per CLAUDE.md).

### Dual-use / refusal screen

Per CLAUDE.md refusal guardrails, every candidate protocol passes a **dual-use-of-concern**
screen. Reject (and flag) anything that enhances pathogen transmissibility/virulence, aids
weaponization, or otherwise meets DURC criteria. Standard cancer wet-lab methods are
out-of-concern, but viral-vector, gene-drive, gain-of-function, and select-agent-adjacent methods
are **excluded** unless explicitly cleared.

---

## Quality, review & risk gates

**Project risk tier: medium**, with **mandatory high-risk handling** for any protocol whose
content bears safety, biohazard, animal, or human-derived-material implications (which is most of
them). The safety dimension is treated as high-stakes per the Good Deed Definition.

**Required reviews before a protocol is "delivered":**

1. **Technical review** (method correctness, schema validity, reproducibility) — Technical reviewer.
2. **Safety / biosafety review** — **credentialed** biosafety/EHS professional (or qualified lab
   safety officer). **Non-skippable** for any hazard/biohazard/animal/human-material flag. This is
   the **high-risk expert sign-off** the Good Deed Definition requires.
3. **Domain SME review** — a researcher experienced with the specific method (e.g., the relevant
   assay), for protocols beyond routine.
4. **License + Ethics gate** — passed *before* authoring (blocking) and re-checked at ship.
5. **Dual-use screen** — recorded, with reviewer.

**Definition of Shipped (per protocol):**
- Schema-valid; linter green (provenance 100%, safety section complete, semver + changelog present).
- License + ethics gate PASS with recorded evidence; dual-use screen clear.
- Technical **and** safety sign-off recorded (plus domain SME where applicable); all disclaimers
  present.
- Rendered outputs generated; CHANGELOG updated; commit DCO signed-off; CI (build/test/lint)
  green.
- **Adopter handoff:** at least one named lab/facility/repository has the protocol available to
  use (the Elyos "delivered, not merged" bar). Without an adopter, status is at most `review`.

A safety erratum on a shipped protocol is a **Sev-1**: pull/flag the protocol, fix, re-review,
re-version, notify known adopters.

---

## Roadmap & milestones

Phased, with measurable exit criteria and realistic dependencies. M0 is a thin, cold-start
foundation; later milestones scale curation and adoption.

### M0 — Foundation & cold-start
**Goal:** the schema, safety taxonomy, gates, reviewer roles, and ONE fully-shipped pilot
protocol exist. Prove the whole pipeline end-to-end on one low-hazard protocol.
**Exit criteria:**
- Protocol JSON Schema + Safety/Provenance sub-schemas published and validating.
- Safety taxonomy + controlled vocabularies (hazards/PPE/waste/biosafety) published.
- License+Ethics gate checklist and dual-use screen published.
- **A credentialed safety reviewer and at least one candidate adopter identified** (commitment is
  the M1 gate; identification is M0).
- One low-hazard pilot protocol (e.g., genomic DNA extraction) **shipped** through full review.

### M1 — Authoring & validation pipeline
**Goal:** the toolchain that makes contribution safe, fast, and enforceable.
**Exit criteria:**
- `validate`, `lint` (provenance + safety completeness), and `render` tools pass CI.
- Contribution workflow + reviewer rotation documented; PR template enforces gates.
- **≥ 1 partner/adopter and ≥ 1 credentialed safety reviewer secured in writing** (`verifiedNeed`
  can begin flipping to true for adopted protocols).
- 3 protocols shipped through tooling-enforced pipeline.

### M2 — Seed library + review rotation
**Goal:** a credible starter corpus of common cancer-research protocols.
**Exit criteria:**
- ≥ 15 protocols shipped (all low-to-moderate hazard), each with full provenance + safety sign-off.
- Errata process exercised at least once; 0 unresolved safety errata.
- Versioning/changelog discipline verified across ≥ 2 protocol revisions (a v1→v2 diff exists).

### M3 — Interop, adoption & sustainability
**Goal:** plug into the ecosystem and hand off maintenance.
**Exit criteria:**
- Bioschemas `LabProtocol` / schema.org export + protocols.io import/export round-trip working.
- ≥ 5 distinct external adopters confirmed (headline outcome metric #1).
- Maintenance owner + reviewer rotation established for post-delivery sustainability.

---

## Work breakdown

The itemized, schema-mapped backlog lives in **`TASKS.md`**, organized by the M0–M3 milestones
above. Each task maps to an Elyos Task JSON (validated against
`packages/schema/src/schemas.ts`), with per-milestone task tables, acceptance criteria for the
most important tasks, a Definition of Done per milestone, a sized backlog, and a complete
schema-valid example Task JSON. The backlog targets ~15–18 well-formed tasks for the first robust
cut. Note the **two blocking role/gate tasks** (safety reviewer, License+Ethics gate) that must
land before authoring tasks begin.

---

## Governance, roles & stakeholders

- **Maintainer (Owner):** TBD — owns the schema, repo, releases, and the backlog.
- **Technical reviewer(s):** method correctness, schema, tooling, reproducibility.
- **Safety / biosafety reviewer (credentialed):** **TO BE SECURED** — biosafety/EHS professional or
  certified lab safety officer; holds the **non-skippable high-risk sign-off**. The project does
  not ship safety-bearing protocols without this role filled.
- **Domain SMEs (rotation):** method-specific cancer-research scientists.
- **License + Ethics gate owner:** runs the blocking license/ethics/dual-use checklist.
- **Steward (last-mile owner):** ensures each shipped protocol is actually handed to / adopted by a
  named lab or facility (owns outcome metric #1).
- **Partner / requestor:** **TO BE SECURED** — adopting lab, core facility, or repository.
- **Stakeholders:** the open-science / reproducibility community, resource-limited labs,
  protocols.io / Bio-protocol ecosystems.

Edge cases and any guardrail exceptions go to the Elyos board + community per the published
conflict-of-interest / veto checklist.

---

## Dependencies & integrations

- **Elyos pieces:** `packages/schema` (Task + new Protocol schema), `packages/cli`/`tools`
  (validator/linter/renderer as agent-neutral tooling), Elyos governance + registry, the
  good-deed gating workflow.
- **External standards:** GHS (hazard classification), NIH/CDC **BMBL** (biosafety levels),
  OSHA/EPA (waste/PPE), ECHA (chemical data), Bioschemas `LabProtocol`, schema.org `LabProtocol`.
- **External data/sources (open only):** protocols.io (per-protocol CC-BY items), Bio-protocol
  (per-article), government safety guidance (public domain / open gov), manufacturer SDS (cite).
- **Tooling deps:** TypeScript/ESM, pnpm, `ajv` (validation), a Markdown/HTML renderer, optional
  PDF generation.
- **Human dependencies (critical path):** a credentialed safety reviewer and an adopting partner —
  both **TO BE SECURED**; the project's value is gated on them.

---

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| **Incorrect/incomplete safety info injures a researcher** | Medium | **Critical** | Non-skippable credentialed biosafety sign-off; provenance on every safety claim; RUO + "not a substitute for institutional biosafety" disclaimers; Sev-1 errata process | Safety reviewer |
| Reproducing a copyrighted protocol (license violation) | Medium | High | Blocking License+Ethics gate; cite-only for copyrighted sources; snapshot + clause evidence; default-EXCLUDE | License+Ethics owner |
| Patient/identifiable/controlled-access data slips in | Low | **Critical** | Hard out-of-scope rule; no PII by design; ethics flags + IRB pointers; linter forbids data fixtures; review checklist | Maintainer |
| CC-BY-NC source contaminates CC-BY output | Medium | High | NC = cite-only, never adapted; license id recorded; linter flags NC adaptation | License+Ethics owner |
| Dual-use / DURC method curated | Low | High | Mandatory dual-use screen; exclude viral-vector/gain-of-function/select-agent-adjacent; reviewer recorded | License+Ethics owner |
| No partner/adopter secured (no real outcome) | **High** | High | M0 outreach task; honest `verifiedNeed:false`; M1 gate requires written commitment; steward role | Maintainer / Steward |
| No credentialed safety reviewer secured | Medium | **Critical** | Blocking M0 role task; do not ship safety content without it; recruit via EHS networks | Maintainer |
| Protocol drift / stale safety guidance over time | Medium | Medium | Semver + changelog; periodic review cadence; deprecation status; provenance dates | Maintainer |
| Scope creep into high-hazard/specialist methods | Medium | High | Explicit out-of-scope list; safety auto-escalation; require expert author+reviewer | Maintainer |
| Over-reliance on protocols.io license assumptions | Medium | High | Per-protocol verification (licenses vary); never assume CC-BY | License+Ethics owner |

---

## Security & privacy

- **Threat surface is low** (static content + small CLI tooling, no service, no user data), but
  the **safety-content surface is the real risk** (see §11) and is treated as Sev-1.
- **No secrets/tokens/keys** in records, fixtures, logs, or commits (CLAUDE.md). CI secret-scan on PRs.
- **No PII/PHI** anywhere — by design. No patient or donor data; no identifiable test fixtures.
- **Abuse/misuse prevention:** dual-use screen; refusal guardrails enforced; high-hazard methods
  out of scope; no instructions that primarily enable harm; clear "consult your IBC/IRB/IACUC/EHS"
  framing so the project is not mistaken for an authority to proceed.
- **Supply-chain:** pin tooling deps; `pnpm` lockfile; minimal dependency surface for the renderer/PDF path.
- **Integrity:** committed source snapshots with SHA-256 so provenance can't silently drift.

---

## Sustainability & maintenance

- **Maintainer + reviewer rotation** (technical, safety, domain) sustain the library after initial
  delivery; the safety-reviewer role is a standing, non-optional commitment.
- **Outcome tracking:** the steward tracks adoption (metric #1), errata, and adopter testimonials;
  a public ledger records shipped protocols, versions, reviewers, and adoption status.
- **Versioning discipline** (semver + changelog + deprecation) keeps protocols trustworthy as
  methods and safety guidance evolve; a periodic re-review cadence catches stale safety data.
- **Handoff:** if Elyos does not maintain long-term, the CC-BY-4.0 corpus + MIT tooling can be
  donated to an adopting repository (e.g., a protocols.io collection) — interop (M3) makes this clean.

---

## Open questions

1. **Who is the first adopting partner**, and will they co-maintain or just consume? (Blocks
   `verifiedNeed`.)
2. **Who is the credentialed safety reviewer**, and what's their availability/SLA for the
   non-skippable sign-off? (Blocks all safety-bearing protocols.)
3. Do we build **PDF rendering** in-house or rely on an external converter (dependency/security
   tradeoff)?
4. Should we **adopt protocols.io's data model directly** as our base (faster interop) or keep our
   own schema and map to it (more control)?
5. How do we handle **regional differences** in waste/PPE/biosafety regulation (US OSHA/EPA vs.
   EU vs. LMIC) without implying one jurisdiction's rules are universal?
6. What is the **minimum credential** we accept for "safety reviewer," and how is it verified?
7. Do we accept **community-submitted** protocols in M2+, and if so what is the contributor vetting?

---

## References

- Elyos `CLAUDE.md` (work rules, lanes, refusal guardrails, quality bar).
- Elyos `docs/good-deed-definition.md` (5 criteria + risk tiers).
- Elyos `packages/schema/src/schemas.ts` (Task JSON schema).
- Elyos `planning/ROADMAP.md` (portfolio; Track 8 cancer guardrails; `lab-protocols-open` entry).
- NIH/CDC **Biosafety in Microbiological and Biomedical Laboratories (BMBL)**, 6th ed.
- **GHS** (Globally Harmonized System of Classification and Labelling of Chemicals).
- OSHA Laboratory Standard (29 CFR 1910.1450); EPA hazardous-waste guidance.
- Bioschemas **LabProtocol** profile; schema.org **LabProtocol**.
- protocols.io and Bio-protocol (per-item license verification required).
- Datasheets for Datasets (Gebru et al.) — provenance discipline analog.

---

## Appendix A — Improvements applied

The following 25 specific improvements were identified during drafting and have been **applied**
to the plan above (and to `TASKS.md`).

1. **Safety content elevated to high-risk handling** despite the project being nominally "medium" —
   non-skippable credentialed biosafety sign-off added to §8/§11 and to every safety-bearing task.
2. **Safety auto-escalation rule** added to the data model: any hazard/biohazard/animal/
   human-material flag forces `riskTier: high`; authors cannot downgrade.
3. **Provenance-per-assertion** (not per-document) made the signature quality bar (§6/§7) and a
   hard linter gate (metric #3 = 100%).
4. **Per-source license verification with default-EXCLUDE** (snapshot + SHA-256 + Wayback + cited
   clause), mirroring the proven open-data-datasheets gate.
5. **Explicit copyrighted-source list** (Nature/CSH/Springer Protocols, Methods Mol Biol) marked
   **cite-only, never reproduce** — closes the biggest license trap.
6. **CC-BY-NC contamination rule** — NC sources are cite-only, never adapted, to keep output a
   clean CC-BY-4.0 commons.
7. **protocols.io license caveat** — never assume CC-BY; licenses vary per protocol (risk row +
   §8 added).
8. **Dual-use / DURC screen** added per CLAUDE.md refusal guardrails, with explicit exclusions
   (viral-vector/gain-of-function/select-agent-adjacent).
9. **Human-derived-material protocols documented as methods only** with mandatory IRB/consent
   pointers and zero donor data — keeps cancer guardrails intact while allowing useful content.
10. **Animal protocols require IACUC + 3Rs framing** rather than being silently excluded.
11. **"Not a substitute for institutional biosafety/EHS/IBC/IACUC/IRB"** disclaimer standardized
    on every protocol (distinct from the medical "not advice" framing).
12. **RUO (research-use-only) disclaimer** added — protocols assert no clinical/diagnostic fitness.
13. **Outcome-first metrics** — real external adoption (metric #1) is the headline; structural
    quality is reframed as a precondition, not success.
14. **Safety erratum = Sev-1** lifecycle (pull/fix/re-review/re-version/notify) added to §8.
15. **Two blocking role/gate tasks** (secure safety reviewer; License+Ethics gate) sequenced before
    authoring tasks in TASKS.md.
16. **Steward role** added to own last-mile adoption (metric #1) — the Elyos "delivered, not merged"
    bar.
17. **Interop-over-reinvention** decision (Bioschemas/schema.org/protocols.io) added so the corpus
    plugs into existing ecosystems and is donatable on handoff.
18. **Controlled vocabularies** (hazards/PPE/waste/biosafety) specified and mapped to GHS/BMBL/OSHA
    so safety data is machine-readable and consistent, not free text.
19. **Semantic versioning + per-protocol changelog** made a hard validator gate (metric #7) with a
    v1→v2 diff required at M2 exit.
20. **Curate-don't-crawl** decision — small, high-quality, license-checked set over bulk scraping.
21. **Regional-regulation open question** surfaced (US vs EU vs LMIC waste/PPE/biosafety) so the
    project doesn't imply one jurisdiction is universal.
22. **No-PII-by-design** extended to test fixtures and examples (linter forbids data fixtures).
23. **Source snapshot integrity via SHA-256** so provenance can't silently drift over time.
24. **Honest `verifiedNeed:false` + TO BE SECURED** for partner and safety reviewer throughout,
    with the M1 gate to flip it — no invented partner.
25. **Pilot-first M0** (one low-hazard protocol shipped end-to-end) to validate the entire
    pipeline before scaling, de-risking the seed-library investment.

---

## Review sign-off

**Reviewer pass (completeness + correctness):** Performed against PLAN_SPEC (all 17 H2 sections
present and in order), CLAUDE.md (lanes, refusal guardrails, quality bar), the Good Deed Definition
(5 criteria + risk tiers), and the cancer guardrails in ROADMAP Track 8.

Findings and fixes applied during review:
- **Confirmed** all 17 required sections are present, correctly ordered, and that
  **Data, licensing & compliance leads with the cancer guardrails** as required.
- **Confirmed** risk handling: nominally medium, but safety/human/animal content is treated as
  **high** with credentialed sign-off — consistent with the Good Deed Definition's high tier.
- **Confirmed** no patient/controlled-access/identifiable data anywhere; human-material protocols
  are method-only with IRB/consent pointers.
- **Fixed** a latent inconsistency: clarified that `verifiedNeed` may begin flipping to true only at
  M1 once a partner commits in writing (M0 only *identifies* candidates) — aligned §2, §9, M0/M1
  exit criteria, and TASKS.
- **Confirmed** license stance is conservative and self-consistent: CC-BY-4.0 output, NC and
  copyrighted sources are cite-only, default-EXCLUDE on unverifiable licenses.
- **Confirmed** TASKS.md is schema-mapped, the example Task JSON validates against
  `packages/schema/src/schemas.ts` (all required fields present, enums valid, `verifiedNeed:false`),
  and every milestone has acceptance criteria + a Definition of Done.
- **Confirmed** outcome metrics are beneficiary-centric with baselines + targets, and the headline
  gate (real adoption + credentialed safety sign-off) is explicit.

**Status:** Plan is internally consistent and compliant with Elyos guardrails. **Two human
decisions block real delivery:** (1) secure a credentialed safety reviewer; (2) secure an adopting
partner. Both are tracked as blocking tasks. Approved as a Draft v0.1.0 baseline.
