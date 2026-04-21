# Prioritization: CL-ARITY

**Last updated:** 2026-04-09

## VEC Matrix

| Initiative | Value (1–5) | Effort (1–5) | Confidence | Adjusted Value | Score (AV/E) | Quadrant | Notes |
|-----------|:-----------:|:------------:|:----------:|:--------------:|:------------:|:--------:|-------|
| cl-arity-workshop bootstrap | 5 | 3 | ⚠️ 0.6 | 3.0 | 1.00 | Do now | Enables all other initiatives; must exist first |
| setup-common-lisp v2 consolidation | 5 | 3 | ⚠️ 0.6 | 3.0 | 1.00 | Do now | Core G1 deliverable; absorbs setup-quicklisp, adds OCICL |
| asdf-operate v2 consolidation | 4 | 3 | ⚠️ 0.6 | 2.4 | 0.80 | Do now | Core G1 deliverable; absorbs run and make |
| Platform matrix: SBCL×{Linux, macOS} | 4 | 2 | ⚠️ 0.6 | 2.4 | 1.20 | Do now | G3 minimum commitment; low effort — formalise what mostly works |
| create-release action | 5 | 4 | ⚠️ 0.6 | 3.0 | 0.75 | Do now | G2 core; high value but highest effort of new actions |
| Marketplace listings + docs + examples | 4 | 2 | ⚠️ 0.6 | 2.4 | 1.20 | Do now | G6; high adoption impact for low effort |
| Migration guides for deprecated actions | 3 | 1 | ✅ 1.0 | 3.0 | 3.00 | Do now | Constraint-adjacent: must accompany v2 releases |
| publish-quicklisp-distribution | 3 | 3 | ⚠️ 0.6 | 1.8 | 0.60 | Do later | G4; depends on quickdist spike |
| Platform expansion: ECL, CLASP | 3 | 3 | ⚠️ 0.6 | 1.8 | 0.60 | Do later | G3 expansion; after minimum commitment is solid |
| CL Foundation engagement | 3 | 2 | ⚠️ 0.6 | 1.8 | 0.90 | Do later | G6; requires existing material to present |
| MacPorts package signing | 2 | 3 | ⚠️ 0.6 | 1.2 | 0.40 | Do if time | Secondary target per interview |

## Ranked Initiative List

1. **Migration guides for deprecated actions** (score 3.00) — *near-zero effort, must ship with v2*
2. **Platform matrix: SBCL×{Linux, macOS}** (score 1.20) — *formalise existing coverage*
3. **Marketplace listings + docs + examples** (score 1.20) — *high adoption bang for buck*
4. **cl-arity-workshop bootstrap** (score 1.00) — *prerequisite for reliable delivery*
5. **setup-common-lisp v2 consolidation** (score 1.00) — *core ease-of-use*
6. **asdf-operate v2 consolidation** (score 0.80) — *core ease-of-use*
7. **CL Foundation engagement** (score 0.90) — *requires material from 3, 5, 6*
8. **create-release action** (score 0.75) — *high value, high effort*
9. **publish-quicklisp-distribution** (score 0.60) — *needs quickdist spike first*
10. **Platform expansion: ECL, CLASP** (score 0.60) — *after Tier 1 is solid*
11. **MacPorts package signing** (score 0.40) — *secondary*

## Slice Sequencing Rationale

The workshop (4) must exist before it can validate v2 actions. But migration guides (1) and marketplace/docs (3) are cheap wins that should be woven into the first slices. The natural grouping for slices:

- **Slice 001:** Workshop bootstrap + setup-macports pipeline (proves the workshop pattern on the simplest action)
- **Slice 002:** setup-common-lisp v2 (consolidation + workshop pipeline + migration guide + marketplace listing)
- **Slice 003:** asdf-operate v2 (consolidation + workshop pipeline + migration guide + marketplace listing)
- **Slice 004:** create-release action (the big new capability)
- **Slice 005:** Platform expansion (ECL, CLASP testing)
- **Slice 006:** publish-quicklisp-distribution
- **Slice 007:** CL Foundation engagement (after material exists)

## Revision History

| Date | Change |
|------|--------|
| 2026-04-09 | Initial scoring — from interactive goal interview |
