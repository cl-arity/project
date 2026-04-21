# Product Definition: CL-ARITY

**Last updated:** 2026-04-09
**Product type:** Internal Developer Platform — CI/CD for Common Lisp

## Vision

CL-ARITY helps Common Lisp developers working with GitHub achieve continuous delivery of their projects — from development through testing, release, packaging, and distribution — by providing a composable family of GitHub Actions, enabling any CL project to ship reliably across implementations and platforms with minimal workflow ceremony.

**Vision Research Summary:** Common Lisp has no integrated CI/CD story on GitHub. Developers cobble together shell scripts, Roswell invocations, or ad-hoc workflows. The melusina-org action family is the only structured attempt at a composable GitHub Actions stack for CL, covering setup (MacPorts, CL implementations, Quicklisp), ASDF operations, executable builds, and documentation generation. However, the current family has too many fine-grained actions (seven, soon to be consolidated), no release automation, no packaging or distribution actions, and no centralised quality pipeline. The CL community is small but dedicated; the CL Foundation exists as a potential partner for community infrastructure. OCICL is emerging as a modern alternative to Quicklisp for package distribution, using OCI registries (ghcr.io). No competitor offers an equivalent integrated stack on GitHub Actions.

**Vision status:** ✅ Validated

## Goals

| # | Goal | OKR link | Revision trigger |
|---|------|----------|-----------------|
| G1 | **Ease of use** — Every CL developer on GitHub can set up CI for their project with minimal ceremony: few actions, clear inputs, cross-platform matrix | Developer adoption: new-user onboarding time < 15 min | Two consecutive ❌ verdicts on onboarding friction |
| G2 | **Full release lifecycle** — When a developer pushes a tag, a complete release is created automatically: GitHub Release, documentation, MacPorts Portfile, MacPorts package, OCICL publication — each optional | Release efficiency: time-to-release < 5 min, zero manual steps for standard releases | Two consecutive ❌ verdicts on release automation |
| G3 | **Platform coverage** — CL-ARITY supports the major implementations on the major platforms. Bare minimum (short-term): SBCL on Linux and macOS. Full matrix not required — partial per-implementation coverage is acceptable (e.g. CLASP on Windows only is fine) | Platform reach: number of supported implementation×OS pairs | Two consecutive ❌ verdicts on the same implementation or OS |
| G4 | **Distribution management** — Organizations can publish QuickLisp distributions (public or private) from GitHub Actions, on a lifecycle independent from individual releases | Distribution reach: number of orgs publishing dists via CL-ARITY | Two consecutive ❌ verdicts on distribution adoption |
| G5 | **Action quality** — The actions themselves are developed and released through a uniform quality pipeline (cl-arity-workshop), with unit tests, QA, integration tests, and automated release | Action reliability: pass@8 on all critical workflows; zero regressions reaching tagged releases | Two consecutive ❌ verdicts on action reliability |
| G6 | **Adoption and community** — CL-ARITY is discoverable on the GitHub Marketplace, documented for self-service onboarding, and recognized by the CL community including the CL Foundation | Community adoption: Marketplace installs, repos using CL-ARITY, CL Foundation listing | Two consecutive ❌ verdicts on adoption metrics |

## Why-How Laddering

| Goal | Why | How |
|------|-----|-----|
| G1 | CL developers waste time wiring together three setup actions and writing boilerplate shell. The current action family is too granular. | Consolidate: `setup-common-lisp` v2 absorbs Quicklisp and adds OCICL; `asdf-operate` v2 absorbs run and make. Fewer actions, richer inputs. |
| G2 | Releases are manual, error-prone, and omit packaging. No CL project on GitHub has automated MacPorts or OCICL publication. | New `create-release` action: tag-triggered, with feature-flagged optional steps for docs, Portfile, MacPorts pkg, OCICL push via ORAS. |
| G3 | CL has multiple viable implementations (SBCL, ECL, CLASP) and the community uses macOS and Linux heavily. Windows is a secondary but real target. CI must reflect this. | Platform matrix in every action; tiered support commitments; workshop integration tests covering the declared matrix. |
| G4 | Organizations need private package distribution beyond what Quicklisp.org provides. OCICL covers per-system distribution; Quicklisp dists cover curated collections. | New `publish-quicklisp-distribution` action wrapping existing quickdist tooling. |
| G5 | The previous attempt at shared workflows (melusina-org/reusable) failed because GitHub reusable workflows are too rigid. Each action has ad-hoc CI with no consistency. | cl-arity-workshop: dispatch workflows per action with uniform unit → QA → integration → release pipeline. |
| G6 | A CI/CD platform nobody finds is a CI/CD platform nobody uses. The CL community is small enough that word-of-mouth and foundation endorsement matter disproportionately. | Marketplace listings with proper metadata, example workflows repo, migration guides for deprecated actions, CL Foundation engagement, CL Cookbook contribution. |

## Personas

### All goals

| Persona type | Define |
|---|---|
| **CL library author** | Maintains one or more ASDF systems on GitHub. Pain: writing CI workflows from scratch, no standard way to publish releases with packaging. Workaround: copy-paste shell scripts between repos. Success: `uses: melusina-org/setup-common-lisp@v2` + `uses: melusina-org/asdf-operate@v2` and everything works. |
| **CL application developer** | Builds executables from CL systems, needs cross-platform binaries. Pain: no standard build-and-release pipeline. Success: tag push produces tested executables with MacPorts packaging. |
| **CL org maintainer** | Manages a GitHub org with multiple CL projects. Pain: no private distribution mechanism; each project has inconsistent CI. Success: org-wide QuickLisp dist, uniform CI via CL-ARITY, OCICL publication for all projects. |
| **Action maintainer** (internal) | Develops and releases the CL-ARITY actions themselves. Pain: six+ repos with duplicated CI, manual releases. Success: workshop validates everything, tag-and-ship. |

## Product Hypothesis Canvas

### H1 — Consolidation reduces onboarding friction (G1)

**Assumption:** The primary barrier to CL CI adoption on GitHub is the number of actions required and the ceremony of wiring them together (three setup steps, separate run/make actions).

**Prediction:** Consolidating to `setup-common-lisp` v2 (with package manager choice) and `asdf-operate` v2 (with operation modes) will reduce a standard CI workflow from 5+ action invocations to 3, and reduce onboarding time.

**Measure:** Number of `uses:` lines in a standard CL CI workflow (baseline: 5–6, target: 3). User-reported setup time.

**Kill criterion:** If after consolidation, users still report the setup as confusing or if no new repos adopt the v2 actions within 3 months, reformulate.

### H2 — Automated release with packaging drives adoption (G2)

**Assumption:** CL developers want automated releases but the effort of scripting GitHub Release creation, Portfile generation, SHA computation, ORAS push, and MacPorts packaging is prohibitive for individual projects.

**Prediction:** A single `create-release` action with optional feature flags will be adopted by CL projects that currently release manually.

**Measure:** Number of repos using `create-release`. Baseline: 0. Target: melusina-org projects + 3 external projects within 6 months.

**Kill criterion:** If after shipping `create-release`, zero external projects adopt it within 6 months, the value proposition is wrong.

### H3 — Platform coverage as stated commitment (G3)

**Assumption:** CL developers need confidence that actions work on their implementation×OS pair before adopting them.

**Prediction:** Declaring and testing a platform matrix (starting with SBCL on Linux and macOS) and publishing the results will increase adoption confidence.

**Measure:** Number of implementation×OS pairs passing integration tests. Baseline: inconsistent. Target: SBCL×{Linux, macOS} at 100% pass@8.

**Kill criterion:** If expanding beyond SBCL×{Linux, macOS} costs more than 1 week per additional pair without external demand, stop expanding.

### H4 — QuickLisp distribution publishing fills a gap (G4)

**Assumption:** Organizations with multiple CL projects need a private or curated distribution mechanism that Quicklisp.org does not provide.

**Prediction:** A `publish-quicklisp-distribution` action wrapping quickdist will be adopted by at least 2 organizations.

**Measure:** Number of orgs publishing dists. Baseline: 0.

**Kill criterion:** If after 6 months no org besides melusina-org uses it, the need is not real or the approach is wrong.

### H5 — Workshop enables reliable action delivery (G5)

**Assumption:** The primary cause of action defects reaching consumers is the absence of a uniform, mandatory test pipeline.

**Prediction:** The cl-arity-workshop dispatch pipeline will catch regressions before they reach tagged releases.

**Measure:** Post-release regressions attributable to issues the pipeline should have caught. Target: zero.

**Kill criterion:** If the workshop costs more than 2 days per action to maintain per quarter, the overhead exceeds the benefit.

### H6 — Marketplace presence and CL Foundation engagement drive adoption (G6)

**Assumption:** CL developers discover tools through the GitHub Marketplace, the CL Cookbook, and CL Foundation channels.

**Prediction:** Proper Marketplace listings, example workflows, and CL Foundation engagement will drive adoption beyond the melusina-org user base.

**Measure:** GitHub Marketplace installs, unique repos using CL-ARITY actions. Baseline: current usage. Target: 2× within 12 months.

**Kill criterion:** If after Marketplace optimization and Foundation outreach, usage does not increase within 6 months, the discovery channels are wrong.

## Product Blueprint

| Goal | Initiative | OKR link | Leading indicator | Capabilities touched | Maturity transition | Hypothesis status |
|------|-----------|----------|------------------|---------------------|---------------------|:-:|
| G1 | Consolidate setup-common-lisp v2 | Developer adoption | Workflow line count | Action Consolidation, Setup | Foundation → Acceleration | ⚠️ |
| G1 | Consolidate asdf-operate v2 | Developer adoption | Workflow line count | Action Consolidation, ASDF | Foundation → Acceleration | ⚠️ |
| G2 | create-release action | Release efficiency | Time-to-release | Release Automation, Packaging | Foundation → Acceleration | ⚠️ |
| G3 | Platform matrix: SBCL×{Linux, macOS} | Platform reach | Passing impl×OS pairs | Platform Testing | Foundation → Acceleration | ⚠️ |
| G3 | Platform expansion: ECL, CLASP, Windows | Platform reach | Passing impl×OS pairs | Platform Testing | Acceleration → Optimisation | ⚠️ |
| G4 | publish-quicklisp-distribution action | Distribution reach | Orgs publishing dists | Distribution Management | Foundation → Acceleration | ⚠️ |
| G5 | cl-arity-workshop bootstrap | Action reliability | Actions with uniform CI | Quality Pipeline | Foundation → Acceleration | ⚠️ |
| G6 | Marketplace listings + documentation | Community adoption | Marketplace installs | Developer Experience | Foundation → Acceleration | ⚠️ |
| G6 | CL Foundation engagement | Community adoption | Foundation listing | Community Relations | Foundation → Acceleration | ⚠️ |
| G6 | Migration guides for deprecated actions | Community adoption | Migration completion rate | Developer Experience | Foundation → Acceleration | ⚠️ |

## Consolidated Action Inventory (Target State)

| Action | Status | Description |
|---|---|---|
| `setup-macports` | Exists, unchanged | Install MacPorts on macOS runners |
| `setup-common-lisp` v2 | Evolve | Install CL implementation + package manager (quicklisp, ocicl, none). Absorbs `setup-quicklisp`. |
| `asdf-operate` v2 | Evolve | Universal ASDF operation: load-op, test-op, program, compile-op. Absorbs `run-common-lisp-program` and `make-common-lisp-program`. Inputs: implementation, operation, system, entry-point, environment, arguments. |
| `make-lisp-system-documentation-texinfo` | Exists, unchanged | Generate TeXinfo documentation from a CL system |
| `create-release` | New | Tag-triggered. Optional steps: GitHub Release, attach docs, generate Portfile, build MacPorts pkg (optionally signed), push to OCICL via ORAS. |
| `publish-quicklisp-distribution` | New | Wrap quickdist. Org-scoped, public or private. Separate lifecycle from releases. |
| `cl-arity-workshop` | New | Quality gate: dispatch workflows for unit test, QA, integration test, release of every action above. |

**Archived after v2 migration:**

| Action | Replacement |
|---|---|
| `setup-quicklisp` | `setup-common-lisp` v2 with `package-manager: quicklisp` |
| `run-common-lisp-program` | `asdf-operate` v2 with `operation: load-op` + `entry-point` |
| `make-common-lisp-program` | `asdf-operate` v2 with `operation: program` |

## Assumptions Made

1. **The CL Foundation is open to listing community CI/CD infrastructure.** Not validated — requires outreach.
2. **OCICL's ORAS-based push mechanism works from GitHub Actions without special infrastructure.** Not validated — requires spike.
3. **quickdist (ultralisp fork) is maintained and works on current SBCL.** Not validated — requires spike.
4. **`setup-common-lisp` v2 can support OCICL setup without SBCL being required** (OCICL currently requires SBCL to build). If OCICL only works with SBCL, the `package-manager: ocicl` feature is SBCL-only. Needs investigation.
5. **Existing users of `setup-quicklisp@v1` and `run-common-lisp-program@v1` can be migrated** with deprecation notices and a transition period.
6. **MacPorts package signing is tractable on GitHub Actions runners.** Secondary target — not a hard requirement.
7. **The v1 branch convention continues for stable releases.** v2 actions get a `v2` branch.

## Revision History

| Date | Author | Change |
|------|--------|--------|
| 2026-04-09 | Claude + Michaël (interactive) | Initial draft — all sections from goal interview |
