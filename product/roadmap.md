# Roadmap: CL-ARITY

**Last updated:** 2026-04-09

## Now

| Slice | Phase | Goal | Hypothesis status | Leading indicator target |
|-------|-------|------|:-:|---|
| 001: Workshop bootstrap + setup-macports pipeline | Planning | G5 | ⚠️ | Workshop repo exists; 1/7 actions with uniform CI |

## Next (1–2 cycles)

| Initiative | Goal | Hypothesis status | OKR contribution |
|-----------|------|:-:|---|
| 002: setup-common-lisp v2 | G1, G3, G5, G6 | ⚠️ | Absorbs setup-quicklisp; adds OCICL; Marketplace listing; migration guide |
| 003: asdf-operate v2 | G1, G5, G6 | ⚠️ | Absorbs run + make; entry-point/environment/arguments support; migration guide |

## Later (3+ cycles)

| Initiative | Goal | OKR contribution |
|-----------|------|---|
| 004: create-release | G2 | Tag-triggered release with optional docs/Portfile/MacPorts pkg/OCICL |
| 005: Platform expansion | G3 | ECL, CLASP support in matrix |
| 006: publish-quicklisp-distribution | G4 | Org-scoped Quicklisp dists |
| 007: CL Foundation engagement | G6 | Community recognition, CL Cookbook listing |

## Constraint work

*None identified.*

## Parked

| Initiative | Reason |
|-----------|---|
| MacPorts package signing | Secondary target; tractability on GH Actions runners unvalidated |

## Discovery needed

| Topic | Blocking |
|---|---|
| OCICL ORAS push from GitHub Actions | Slice 002 (OCICL feature of setup-common-lisp), Slice 004 (OCICL step of create-release) |
| quickdist (ultralisp fork) current state | Slice 006 |
| CLASP ASDF program-op compatibility | Slice 005 |
| CL Foundation process for listing community tools | Slice 007 |

## Revision History

| Date | Change |
|------|--------|
| 2026-04-09 | Initial roadmap from interactive goal interview |
