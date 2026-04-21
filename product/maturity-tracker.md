# Maturity Tracker: CL-ARITY

**Product type:** Internal Developer Platform — CI/CD for Common Lisp

## Capability Maturity

| Capability | Current (0–3) | Target | Notes |
|---|:-:|:-:|---|
| CL Implementation Setup | 1 | 2 | Works for SBCL and ECL; no package manager integration |
| Package Manager Setup | 1 | 2 | Quicklisp works; OCICL not yet supported |
| ASDF Operations | 1 | 2 | asdf-operate exists; run and make are separate actions |
| Documentation Generation | 1 | 2 | make-lisp-system-documentation-texinfo works |
| Release Automation | 0 | 2 | No action exists |
| MacPorts Packaging | 0 | 1 | No Portfile generation or pkg building |
| OCICL Publication | 0 | 1 | No ORAS integration |
| QuickLisp Distribution Publishing | 0 | 1 | No action exists |
| Quality Pipeline (Workshop) | 0 | 2 | No centralised CI for actions |
| Marketplace Presence | 1 | 2 | Some actions listed; inconsistent metadata |
| Migration & Deprecation | 0 | 1 | No deprecation process exists |
| Community Engagement | 0 | 1 | No CL Foundation relationship |

**Scale:** 0 = Not present, 1 = Foundation (exists, ad-hoc), 2 = Acceleration (automated, consistent), 3 = Optimisation (measured, improved continuously)

## Quality Attributes (ISO 25010)

| Attribute | Current (0–3) | Notes |
|---|:-:|---|
| Reliability | 1 | Actions work but no pass@k guarantees |
| Maintainability | 0 | Seven repos with duplicated CI; consolidation not started |
| Portability | 1 | SBCL + ECL on Linux + macOS; inconsistent coverage |
| Testability | 1 | Some tests; no uniform framework |
| Usability | 1 | Works for experts; too many actions for newcomers |
| Operability | 0 | No observability into action health across the family |
| Security | 1 | MIT licensed; no supply chain hardening; no signing |

## Platform Coverage Matrix

| Implementation | Linux | macOS | Windows |
|---|:-:|:-:|:-:|
| SBCL | ✅ Tier 1 | ✅ Tier 1 | ⬜ Tier 3 |
| ECL | ⬜ Tier 2 | ⬜ Tier 2 | ⬜ Tier 3 |
| CLASP | ⬜ Tier 2 | ⬜ Tier 2 | ⬜ Tier 2 |
| ABCL | ⬜ Tier 3 | ⬜ Tier 3 | ⬜ Tier 3 |

**Tier 1:** Must work. Tested in workshop. Regressions block release.
**Tier 2:** Should work. Tested in workshop. Regressions flagged but do not block.
**Tier 3:** Nice to have. Tested if tractable.

✅ = currently passing, ⬜ = target, not yet validated

## Change History

| Date | Assessment | Changes |
|------|-----------|---------|
| 2026-04-09 | Initial baseline | All capabilities, quality attributes, and platform matrix scored from interview and research |
