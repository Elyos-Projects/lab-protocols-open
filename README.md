# lab-protocols-open

> Cancer wet-lab research runs on protocols — the step-by-step recipes for culturing cells, extracting nucleic acids, running assays, building animal models, and preparing samples for sequencing. In pra  ·  **Risk tier:** med  ·  **Status:** planning

Cancer wet-lab research runs on protocols — the step-by-step recipes for culturing cells, extracting nucleic acids, running assays, building animal models, and preparing samples for sequencing. In practice these protocols are scattered, inconsistently versioned, paywalled (Nature Protocols, Cold Spring Harbor Protocols, Springer Protocols are copyrighted and **not** open), or buried as unstructured PDFs and lab-bench tribal knowledge. Critically, **safety information is the most uneven part**: hazard warnings, PPE, waste disposal, and emergency steps are frequently absent, outdated, or wrong — and getting them wrong can injure a researcher.

**Definition of shipped:** | 3 | **Provenance completeness** — % of assertions with a cited, license-verified source | n/a | 100% on shipped protocols (hard gate) | Linter report | | 4 | **Safety completeness** — shipped protocols with full hazard/PPE/waste/emergency section | n/a | 100% (hard gate) | Lint

This is an **Elyos** good-deed project. Contributors pull a task, do it with their own coding agent, and open a PR. Platform: https://github.com/jdev1977/elyos

## Plan
- [PLAN.md](./PLAN.md) — robust enterprise plan (vision, architecture, roadmap, risks; includes an applied-improvements appendix + review sign-off)
- [TASKS.md](./TASKS.md) — schema-mapped task backlog
- [tasks/](./tasks/) — ready-to-pull task JSON(s)

## Contribute
```bash
elyos browse
elyos next --repo Elyos-Projects/lab-protocols-open --no-fork
```

## Licensing & review
- Open license (see PLAN.md).
- Risk tier **med** — deeds are *delivered, not merged*; a domain reviewer (and expert sign-off for any high-stakes content) must approve before merge.

> Planning stage; no adopting partner secured yet (`verifiedNeed: false` on delivery-dependent tasks).
