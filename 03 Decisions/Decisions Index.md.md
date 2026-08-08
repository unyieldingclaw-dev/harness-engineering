### HE-002 — Modular Capability Architecture

Status: PENDING HE-001

Evaluate whether Harness Engineering should expose AI engineering
capabilities as independently invokable, selectively composable units
rather than primarily as a prescribed end-to-end workflow.

Candidate model:

    context
    spec
    implement
    diagnose
    review
    architecture analysis

The Harness would determine when capabilities should be composed based on
task requirements rather than requiring every task to execute every stage.

This is not a decision to remove useful workflow sequencing.

Evidence required:

- reduced unnecessary workflow coupling;
- reduced unnecessary persistent context;
- simpler capability maintenance;
- improved handling of small or atypical tasks;
- preserved deterministic controls;
- preserved useful artifact dependencies;
- no measurable degradation in implementation or review quality;
- clear ownership between Harness, PMB, ACR, and capability-specific logic.

Risks to evaluate:

- capability proliferation;
- ambiguous routing;
- hidden dependencies;
- inconsistent artifacts;
- loss of useful sequencing;
- poor model-selected workflow decisions;
- duplicated responsibilities;
- governance complexity moving from workflow definition into routing.

No implementation should begin until HE-001 provides evidence supporting
the direction.