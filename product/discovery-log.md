# Discovery Log: CL-ARITY

## Research — 2026-04-09 (pre-product)

**Segment:** Action maintainer
**Key finding:** The `melusina-org/reusable` repository attempted GitHub reusable workflows for shared CI but was abandoned — 18 open issues, README says they were "not flexible enough to be reused or extended." Reusable workflows are inlined back into each action repo.
**Assumption confirmed or invalidated:** Confirms that dispatch workflows (not reusable workflows) are the right architecture for the workshop.
**Confidence impact:** H5 (workshop) disposition remains ⚠️.

## Research — 2026-04-09 (OCICL)

**Segment:** CL library author
**Key finding:** OCICL packages ASDF systems as OCI artifacts on ghcr.io. Uses ORAS for push/pull. Supports sigstore signing. OCICL itself requires SBCL to build. The push mechanism uses the `oras` CLI bundled in the OCICL repo.
**Assumption confirmed or invalidated:** Partial confirmation that ORAS-based push should work from GH Actions. Uncertainty: whether `oras push` alone is sufficient or whether OCICL-specific metadata is needed. Spike required.
**Confidence impact:** H2 OCICL step disposition: 🔬 spike needed.

## Research — 2026-04-09 (quickdist)

**Segment:** CL org maintainer
**Key finding:** Two quickdist implementations exist: orivej/quickdist (original, 2012) and ultralisp/quickdist (fork with CLI, maintained more recently). Both generate static dist files servable from any HTTP endpoint. GitHub Pages would work as a hosting target.
**Assumption confirmed or invalidated:** Tooling exists. Freshness of ultralisp/quickdist needs validation.
**Confidence impact:** H4 disposition remains ⚠️. Spike needed to confirm ultralisp/quickdist works on current SBCL.
