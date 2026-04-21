# Slice 001: Workshop Bootstrap and setup-macports Pipeline

**Status:** Planned
**Type:** Bet
**Goal:** G5 (action quality)
**OKR contribution:** Action reliability — establish the quality pipeline pattern
**Hypothesis assumption:** A centralised dispatch workflow architecture in a dedicated repository (cl-arity-workshop) can enforce a uniform unit → QA → integration pipeline on each action, and the pattern can be proved on `setup-macports` as the simplest action (shell + YAML, no Lisp dependency).
**Hypothesis prediction:** After this slice, `setup-macports` will have a complete test pyramid callable from cl-arity-workshop on any gitref, and the pattern will be replicable to remaining actions with bounded effort (≤ 2 days per additional action).
**Hypothesis disposition:** ⚠️
**Leading indicator:** Actions with complete dispatch pipeline — baseline 0, target 1.
**Kill criterion:** If the dispatch workflow pattern requires more than 2 days to replicate per additional action, the architecture is too coupled and must be simplified.
**Planned start / end:** TBD
**Actual end:**
**Implementation phases:**
  - Phase 1: product/slice/001-workshop-bootstrap-and-setup-macports/implementation-1.md — Planned

---

## Customer Journey

After this slice, a `setup-macports` maintainer can:
1. Push a commit or tag on `setup-macports`.
2. Trigger the cl-arity-workshop dispatch workflow with that gitref.
3. The workshop runs unit tests → QA checks → integration tests in sequence.
4. If the gitref is a tag, a release is prepared with notes and artifacts.
5. Results are visible on the workshop's Actions tab.

The pattern established here will be replicated for every subsequent action.

## Personas served

- Action maintainer: validates any gitref through a uniform pipeline with a single dispatch
- CL library author (indirect): gains confidence that published action versions passed integration tests

## Stories

### S1: Create the cl-arity-workshop repository skeleton

**As a** maintainer, **I want to** have a dedicated repository with README, LICENSE, and .github/workflows/ directory, **so that** all dispatch workflows have a single home.

**Acceptance criteria:**
- Given the repository is created, when I visit it on GitHub, then I see a README explaining CL-ARITY, listing the supported actions, and linking to each action's repo
- Given the repository exists, when I look at .github/workflows/, then the directory is ready for workflow files

### S2: Dispatch workflow for setup-macports — unit tests

**As a** maintainer, **I want to** trigger unit tests for `setup-macports` on an arbitrary gitref from cl-arity-workshop, **so that** I can validate any branch or tag without modifying the action's own repo.

**Acceptance criteria:**
- Given the workflow is triggered with `ref: main`, when the unit test job runs, then it checks out that ref and runs the action's own test suite on macOS
- Given the workflow is triggered with a non-existent ref, when the checkout step fails, then the workflow reports a clear error
- Given the workflow is triggered with `ref: v1.1.6` (a tag), when the unit test job runs, then it tests that exact version

### S3: QA checks for setup-macports

**As a** maintainer, **I want to** run static analysis on setup-macports code (shellcheck on shell scripts, YAML lint on action.yaml, license header check), **so that** code quality is enforced before integration testing.

**Acceptance criteria:**
- Given the QA job runs after unit tests pass, when a shell script has shellcheck warnings, then the job fails
- Given the QA job runs, when action.yaml has a schema issue, then the job fails
- Given all checks pass, then the integration test stage is triggered

### S4: Integration test for setup-macports

**As a** maintainer, **I want to** exercise `setup-macports` on macOS runners by actually installing MacPorts and verifying the `port` command works, **so that** I know the action works end-to-end.

**Acceptance criteria:**
- Given the integration test runs on macOS (matrix: macos-13, macos-15), when the action completes, then `port version` returns a valid version string
- Given the integration test runs, when it installs a port (e.g. `cl-quicklisp`), then the port is available

### S5: Release preparation stage (tag-only)

**As a** maintainer, **I want to** have a release preparation job that runs only when the gitref is a tag, assembles release notes from commit history, and creates a GitHub Release on setup-macports with a source zip, **so that** tagged versions are published automatically.

**Acceptance criteria:**
- Given the gitref is `v1.2.0` (a tag), when the full pipeline passes, then a GitHub Release is created on setup-macports with auto-generated notes
- Given the gitref is `main` (a branch), when the pipeline passes, then no release is created

### S6: Document the workshop pattern for replication

**As a** maintainer, **I want to** have a PATTERNS.md in cl-arity-workshop explaining how to add a new action's workflow, **so that** extending to remaining actions is straightforward.

**Acceptance criteria:**
- Given I read the documentation, when I follow the steps, then I can create a new workflow for a different action within 30 minutes
- Given the documentation exists, when it references setup-macports, then all configurable extension points (repo name, test commands, QA tools, runner matrix) are clearly marked

## Quality Attribute Acceptance Criteria

- [ ] **Maintainability:** Adding a new action's workflow requires copying one template file and changing ≤ 10 lines of configuration — verified by documentation walkthrough in S6
- [ ] **Reliability:** The dispatch workflow completes successfully on 8/8 runs against `setup-macports@main` (pass@8)

## Capability Maturity Transitions

- Quality Pipeline (Workshop): 0 → 1 (Foundation — first centralised pipeline exists)

## Definition of Ready

- [x] Hypothesis disposition ⚠️
- [x] Stories sized ≤ 2 days each
- [x] Acceptance criteria written for all stories
- [x] QA acceptance criterion defined
- [x] Leading indicator baseline established: 0 actions with uniform CI on 2026-04-09
- [x] Dependencies clear

## Dependencies

- GitHub Actions `workflow_dispatch` event support (available)
- macOS runners on GitHub (available, potential queue delays)
- Cross-repo dispatch: cl-arity-workshop checks out setup-macports source directly at the specified gitref (no special permissions needed for public repos)
