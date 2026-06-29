# TASKS — lab-protocols-open

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) ·
> Lane: donated · Risk tier: medium (safety/human/animal content escalates to **high**).

See `PLAN.md` for full context. This file is the itemized, schema-mapped backlog.

---

## How these tasks map to Elyos

Each task below becomes an Elyos **Task JSON** validated against
`packages/schema/src/schemas.ts`. Field mapping:

- `id` — stable slug from the tables (e.g. `lab-protocols-open-schema-001`).
- `title` — the table's Title.
- `project` — `lab-protocols-open`.
- `type` — `code | research | writing | data | design-spec | maintenance` (per table).
- `lane` — `donated` for all current tasks. (No funded escrow; a funded task would add
  `fundedBudgetUsd`.)
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["cancer-research","open-science","lab-safety","reproducibility"]`.
- `riskTier` — `low | medium | high`. **Any task touching safety content, hazardous reagents,
  biohazards, animals, or human-derived material is `high`** and needs credentialed safety
  sign-off (PLAN §8/§11). Schema/tooling tasks are `low`; license/ethics/policy tasks are `medium`.
- `urgent` — boolean; `false` for all current tasks.
- `deliverable` — `pr | dataset | document | translation`. We **never** deliver `dataset` (no data
  in scope); code/tooling → `pr`; schema/protocols/docs → `document`.
- `tokenEstimate` — `small | medium | large` (Size column).
- `status` — `open | in-progress | review | delivered | done`; all start `open`.
- `context`, `objective`, `acceptanceCriteria[]`, `resources[]`, `output` — per task.
- `requestor` — **TO BE SECURED** until a partner/adopter is confirmed.
- `verifiedNeed` — **`false`** for all tasks now; flips to `true` per protocol only once a named
  lab/facility/repository commits to adopt it (M1 gate). General need is real; per-deliverable
  demand is unproven.
- `outputLicense` — **`CC-BY-4.0`** for protocol content/docs/schema docs; **`MIT`** for tooling code.

**Reviewer key:** *Maintainer* · *Technical* · *Safety* (credentialed biosafety/EHS — non-skippable
for high-risk) · *Domain SME* · *License+Ethics* (blocking gate owner).

---

## Milestone M0 — Foundation & cold-start

Prove the whole pipeline end-to-end on ONE low-hazard protocol; stand up schema, taxonomy, gates,
and reviewer roles.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| lab-protocols-open-reviewer-002 | Secure/name a credentialed safety (biosafety/EHS) reviewer — blocking, non-skippable gate role | research | small | medium | document | — | Maintainer |
| lab-protocols-open-gate-003 | License + Ethics + Dual-use triage checklist (blocking gate) | design-spec | small | medium | document | — | License+Ethics |
| lab-protocols-open-schema-001 | Protocol JSON Schema + Safety/Provenance sub-schemas + canonical data model | design-spec | medium | low | document | — | Technical |
| lab-protocols-open-vocab-004 | Safety taxonomy + controlled vocabularies (hazards/PPE/waste/biosafety) | writing | small | medium | document | schema-001 | Safety, Technical |
| lab-protocols-open-template-005 | Authoring template + disclaimer set (RUO + "not a substitute for institutional biosafety/IRB/IACUC") | writing | small | medium | document | schema-001, vocab-004 | Safety, Maintainer |
| lab-protocols-open-outreach-006 | Partner/adopter + safety-reviewer outreach; candidate protocol shortlist | research | small | low | document | — | Maintainer |
| lab-protocols-open-pilot-007 | Pilot protocol end-to-end: genomic DNA extraction (low hazard), fully reviewed | data | medium | high | document | schema-001, gate-003, vocab-004, template-005, reviewer-002 | Safety, Technical, Domain SME |

**Acceptance criteria — key tasks**

- **reviewer-002 (secure credentialed safety reviewer)**
  - [ ] A named individual (or role-holder) with a verifiable biosafety/EHS credential agrees to the
        non-skippable safety sign-off role; minimum credential + verification recorded.
  - [ ] Review SLA/availability and conflict-of-interest disclosure recorded.
  - [ ] Documented decision: **no safety-bearing protocol ships until this role is filled.**

- **gate-003 (license + ethics + dual-use gate)**
  - [ ] Enumerates accepted licenses (CC0, CC-BY) vs. cite-only (CC-BY-NC, all copyrighted:
        Nature/CSH/Springer Protocols, Methods Mol Biol); default = **EXCLUDE** on unverifiable license.
  - [ ] License PASS only with recorded license id + URL + text snapshot (committed copy + SHA-256 +
        Wayback) + `permitsDerivatives` from a cited clause.
  - [ ] Ethics checklist flags human-derived material (IRB/consent) and animal use (IACUC/3Rs);
        forbids patient/identifiable/controlled-access data outright.
  - [ ] Dual-use screen rejects DURC/weaponizable methods (viral-vector/gain-of-function/select-agent).

- **schema-001 (protocol schema + sub-schemas)**
  - [ ] JSON Schema validates with `ajv`; covers steps, materials (CAS/hazardClass/SDS ref),
        equipment, `safety`, `ethics` flags, `qc`, `troubleshooting`, `provenance`, `dualUseScreen`,
        `review`, semver `version` + `changelogRef`.
  - [ ] **Safety auto-escalation** encoded/documented: any hazard/biohazard/animal/human-material
        flag implies `riskTier:high`.
  - [ ] Every step and every safety item has a required `provenance` reference field.
  - [ ] Maps to Bioschemas `LabProtocol` / schema.org fields (interop crosswalk documented).
  - [ ] `pnpm build && pnpm test && pnpm lint` pass for committed schema/tooling; DCO signed-off.

- **pilot-007 (end-to-end pilot protocol)**
  - [ ] One low-hazard protocol authored to the schema; schema-valid and linter-green.
  - [ ] 100% of steps + safety claims carry resolvable, license-verified provenance.
  - [ ] Full safety section (PPE, hazards, waste, spill/emergency) with credentialed safety sign-off.
  - [ ] License+Ethics gate PASS recorded; dual-use screen clear; all disclaimers present.
  - [ ] semver v1.0.0 + CHANGELOG; rendered Markdown/HTML produced; commit DCO signed-off.

**Definition of Done (M0):** schema, vocab, template, and both gates published; credentialed safety
reviewer **and** ≥1 candidate adopter identified; one low-hazard pilot protocol **shipped** through
full review (technical + safety + domain), schema-valid and linter-green.

---

## Milestone M1 — Authoring & validation pipeline

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| lab-protocols-open-validate-008 | `protocol validate` CLI (schema + semver/changelog checks) | code | small | low | pr | schema-001 | Technical |
| lab-protocols-open-lint-009 | `protocol lint` — provenance 100% + safety-completeness + no-data-fixture linter | code | medium | low | pr | schema-001, vocab-004 | Technical, Safety |
| lab-protocols-open-render-010 | `protocol render` — record → Markdown/HTML (optional PDF) | code | medium | low | pr | schema-001, template-005 | Technical |
| lab-protocols-open-workflow-011 | Contribution workflow + reviewer rotation + PR template enforcing gates | design-spec | small | medium | document | gate-003, reviewer-002 | Maintainer, License+Ethics |
| lab-protocols-open-partner-012 | Secure ≥1 adopting partner in writing (flips verifiedNeed) | research | small | low | document | outreach-006 | Maintainer, Steward |

**Acceptance criteria — key tasks**

- **lint-009 (provenance + safety linter)**
  - [ ] FAILS any protocol with an assertion (step or safety claim) lacking a resolvable source ref.
  - [ ] FAILS incomplete safety sections (missing PPE/hazard/waste/emergency) and dangling SDS/CAS refs.
  - [ ] FAILS presence of any data fixture / patient-like data (no-PII-by-design enforcement).
  - [ ] FAILS missing semver/changelog or NC-source adaptation; emits a machine-readable report.

- **workflow-011 (contribution workflow + rotation)**
  - [ ] PR template requires: schema-valid, linter-green, license+ethics evidence, dual-use screen,
        and the required reviewer sign-offs (technical + safety, +domain where applicable).
  - [ ] Reviewer rotation + escalation documented; safety sign-off marked non-skippable.
  - [ ] Errata/Sev-1 process documented (pull/fix/re-review/re-version/notify adopters).

**Definition of Done (M1):** validate/lint/render pass CI; contribution workflow + rotation
published; **≥1 partner and ≥1 credentialed safety reviewer secured in writing**; 3 protocols
shipped through the tooling-enforced pipeline.

---

## Milestone M2 — Seed library + review rotation

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| lab-protocols-open-seed-nucleic-013 | Nucleic-acid protocol set (RNA extraction, qPCR setup) | data | medium | high | document | M1 pipeline | Safety, Technical, Domain SME |
| lab-protocols-open-seed-culture-014 | Cell-culture protocol set (passaging, cryopreservation, mycoplasma testing) | data | large | high | document | M1 pipeline | Safety, Technical, Domain SME |
| lab-protocols-open-seed-protein-015 | Protein/assay set (Western blot, immunofluorescence, viability assay) | data | large | high | document | M1 pipeline | Safety, Technical, Domain SME |
| lab-protocols-open-revision-016 | Exercise versioning: ship a v2 of one protocol with a v1→v2 diff/changelog | maintenance | small | high | document | seed-* | Safety, Technical |

**Acceptance criteria — key tasks**

- **seed-culture-014 (cell-culture set)**
  - [ ] Each protocol schema-valid, linter-green, 100% provenance, full safety section + sign-off.
  - [ ] Human-derived-material steps (if any) flagged with IRB/consent pointers and **no donor data**.
  - [ ] Biohazard handling, BSL level, and waste disposal cited to BMBL/OSHA; CC-BY-4.0 output.

- **revision-016 (versioning exercise)**
  - [ ] A protocol bumped to a new semver with a clear, reviewer-approved changelog and a v1→v2 diff.
  - [ ] Safety re-review performed on the revision; adopters notified per workflow.

**Definition of Done (M2):** ≥15 protocols shipped (all low-to-moderate hazard) with full
provenance + safety sign-off; errata process exercised once with 0 unresolved safety errata; a
v1→v2 revision demonstrates versioning discipline.

---

## Milestone M3 — Interop, adoption & sustainability

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| lab-protocols-open-interop-017 | Bioschemas/schema.org export + protocols.io import/export round-trip | code | medium | low | pr | schema-001, render-010 | Technical |
| lab-protocols-open-sustain-018 | Adoption tracking ledger + maintenance/rotation handoff plan | design-spec | small | low | document | partner-012 | Maintainer, Steward |

**Acceptance criteria — key tasks**

- **interop-017 (interop export/import)**
  - [ ] Protocol record exports to valid Bioschemas `LabProtocol` / schema.org JSON-LD.
  - [ ] protocols.io import → our schema → export round-trips without losing safety/provenance fields.

- **sustain-018 (sustainability + handoff)**
  - [ ] Public ledger records each shipped protocol: version, reviewers, license, adoption status.
  - [ ] Named maintenance owner + reviewer rotation; documented donation/handoff path for the corpus.

**Definition of Done (M3):** interop round-trip working; ≥5 distinct external adopters confirmed
(headline outcome metric); maintenance owner + rotation established.

---

## Backlog / future (sized, unscheduled)

| ID | Title | Type | Size | Risk | Notes |
| --- | --- | --- | --- | --- | --- |
| lab-protocols-open-seq-prep-019 | Sequencing library-prep front-ends (open methods only) | data | large | high | Verify per-method license; high hazard review |
| lab-protocols-open-imaging-020 | Microscopy/imaging sample-prep protocols | data | medium | high | Fixatives (formaldehyde) hazard handling |
| lab-protocols-open-regional-021 | Regional waste/PPE/biosafety variant notes (US/EU/LMIC) | writing | medium | high | Resolves Open Question on jurisdictions |
| lab-protocols-open-community-022 | Community-submission vetting policy + contributor guide | design-spec | small | medium | Gates external submissions |
| lab-protocols-open-i18n-023 | Translate shipped safety summaries (vetted) | writing | medium | high | High-risk; needs domain+safety review per language |

---

## Example task JSON

A complete, schema-valid Task JSON for the first M0 task (`reviewer-002`), the blocking
safety-reviewer role. Honest `verifiedNeed: false` (no partner/adopter secured).

```json
{
  "id": "lab-protocols-open-reviewer-002",
  "title": "Secure/name a credentialed safety (biosafety/EHS) reviewer — blocking, non-skippable gate role",
  "project": "lab-protocols-open",
  "type": "research",
  "lane": "donated",
  "priority": "high",
  "domain": ["cancer-research", "lab-safety", "biosafety", "open-science"],
  "riskTier": "medium",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "small",
  "status": "open",
  "context": "lab-protocols-open produces versioned, structured, openly-licensed wet-lab protocols for cancer research, each with a rigorous safety section. Per the Good Deed Definition, safety-bearing content is high-stakes and requires credentialed expert sign-off before a protocol is delivered. No such reviewer is yet secured. This blocking role task must be completed before any safety-bearing protocol (i.e., essentially all of them) can ship. No partner or adopter has committed, so verifiedNeed is false.",
  "objective": "Identify, vet, and secure a named credentialed biosafety/EHS reviewer (or qualified lab safety officer) to hold the non-skippable safety sign-off, and document the minimum credential, verification method, availability/SLA, and conflict-of-interest disclosure.",
  "acceptanceCriteria": [
    "A named individual or role-holder with a verifiable biosafety/EHS credential (e.g., RBP/CBSP or institutional biosafety officer) has agreed to the safety sign-off role.",
    "Minimum acceptable credential and the verification method are documented.",
    "Reviewer availability/SLA for sign-off and a conflict-of-interest disclosure are recorded.",
    "A written, ratified decision states no safety-bearing protocol ships until this role is filled.",
    "Document committed to the project repo; commit is DCO signed-off."
  ],
  "resources": [
    "C:\\Users\\jason\\AppData\\Local\\Temp\\claude\\C--code-elyos\\5eca0d44-6b8b-4c30-9696-37a524cb249a\\scratchpad\\plans\\lab-protocols-open\\PLAN.md",
    "C:\\code\\elyos\\docs\\good-deed-definition.md",
    "NIH/CDC Biosafety in Microbiological and Biomedical Laboratories (BMBL), 6th ed.",
    "American Biological Safety Association (ABSA) credential registry"
  ],
  "output": "A committed governance document naming/securing the credentialed safety reviewer, with credential verification, SLA, conflict-of-interest disclosure, and the blocking-gate decision.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```
