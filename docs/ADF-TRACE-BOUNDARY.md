# AFF, ADF, TRACE, and Proofpress

Status: **working compatibility note — November 4, 2026**. This note does not
revise the [AFF v0.1 specification](../spec/OAFF-v0.1.md), register an ADF
extension, implement a converter, or establish interoperable execution.

## Responsibilities

| Component | Primary object | Responsibility |
| --- | --- | --- |
| AFF v0.1 | One immutable Finding revision plus attributed receipts | Exchange a bounded conclusion, evidence descriptors, applicability, history references, and package integrity. |
| ADF draft | A contextual decision record | Explore explicit questions, choices, rationale, provenance, applicability, and links to evidence and outcomes. |
| TRACE | Attributed workflow events, including decisions and revisions | Record who proposed, accepted, rejected, or revised a decision and why. |
| Proofpress | Receiver-local governance and use | Evaluate evidence, decide authorized eligibility, maintain lifecycle, and record use. Broader decision runtime integration is a target design, not a consequence of importing a file. |

ADF is being drafted in the maintainer-only
[`continuara-lab/agent-decision-format`](https://github.com/continuara-lab/agent-decision-format)
repository. It is private and may not be accessible to other readers. AFF
does not depend on access to that draft. Do not treat the draft name or this
note as an established standard or stable API.

[TRACE](https://github.com/Thru-Echoes/TRACE) describes an explicitly logged
sidecar rather than a proxy for other tools. Its decision history can be
source material for a decision record, not receiver-local authorization.

## What stays compatible

- **Wire contract:** Keep `oaff_version: "0.1.0"`, `finding`, `receipts`, and
  `integrity` unchanged. New decision fields are not silently added to core.
- **Immutable identity:** Keep Finding IDs, revision URIs, exact bytes, links,
  and original package digests. Do not rename a Finding revision into an ADF
  decision or relabel an AFF digest as an ADF digest.
- **Receipt meaning:** Keep claimed issuer, method, subject revision, result,
  and authority basis. Imported admission remains attributed origin history.
- **Existing tools:** Keep `.aff`, legacy `.oaff.json`, the `aff`/`oaff`
  commands, and Python `oaff` imports compatible under the naming contract.

The [`finding.type` enum](../schema/oaff-0.1.schema.json) remains
`observation`, `procedure`, or `constraint`. A `procedure` label is not a
validated executable skill; a `constraint` label does not install a runtime
gate.

## Mapping contract for future adapters

This table describes safe source correspondences, not an implemented import
API. An adapter must follow the exact target schema version when one is
adopted and report unsupported fields rather than inventing them.

| AFF source | Possible decision-record role | Required limit |
| --- | --- | --- |
| Original package and `finding.revision` | Exact supporting-artifact reference | Preserve the original package digest and source bytes separately from any new record's digest. |
| `finding.statement` | Evidence-backed conclusion cited in a rationale | Do not manufacture the decision question, choice, alternatives, or reasons from a conclusion alone. |
| `finding.evidence[]` | Referenced source descriptors | Preserve availability, source-content digest, and locators. Do not copy restricted content or claim available bytes when none were checked. |
| `finding.applicability` | Conditions on this supporting Finding | Do not automatically broaden these into the whole decision's applicability. Keep plain-language limits visible. |
| `finding.producer`, `created_at` | Finding provenance | Do not relabel the producer as the decision-maker or the Finding time as the decision time. |
| `finding.links` | Existing explicit revision/dependency assertions | Preserve exact targets, type, and direction. Relevance does not establish a causal dependency. |
| `support_assessment` receipts | Attributed support evaluation for the Finding | Do not treat the evaluation as support for every decision that cites it. |
| `adoption_decision` receipts | Origin-local reuse history of that Finding | Do not map `admitted` to business-decision acceptance, receiver admission, or transaction permission. |
| `lifecycle` receipts | Attributed withdrawal history | Authenticate and reconcile locally before changing recipient state. A copied receipt does not automatically revoke or authorize another system's action. |
| `integrity` | Integrity check for the AFF snapshot | Do not upgrade a matching digest into source authentication, semantic truth, or authorization. |

Unknown extensions remain unknown; their contents cannot be promoted into
authority. The absence of a source field must remain explicit. If the target
requires a question and choice that the source lacks, import only the AFF
supporting artifact, or require an attributed new decision authoring step.

## Example: a conclusion supports a later decision

Consider a synthetic Finding: “Batch size 64 exhausted memory on accelerator
A with model M and configuration C.” Its exact revision, evidence, and scope
remain in its original AFF package.

A later decision can ask “Which batch size should this run use?” and select
32 based on that Finding **and** separate evidence about batch size 32. The
AFF record alone does not establish that 32 succeeds, explain all
alternatives, or grant permission to launch a job.

The new decision should reference the exact AFF artifact and identify the
person or agent who made the new choice. If an authorized process later
generalizes it into a reusable rule, that is a distinct reviewed derivation
with its own conditions. A subsequent model/configuration change may require
reassessment. Preserve both favorable and unfavorable run evidence.

## TRACE import boundary

A future TRACE adapter should preserve source event/session IDs, actor
attribution, recorded rationale, and explicit revision links. It should mark
retrospective extraction as inferred rather than rewriting it as an original
event. Preserve the schema version used for the mapping.

Three questions remain separate:

1. **Historical disposition:** Was this decision proposed, accepted, rejected,
   or revised in the original workflow?
2. **Current reuse eligibility:** Does this recipient authorize using its
   exact version under current conditions?
3. **Current action permission:** May this actor perform this specific
   operation with these arguments now?

No AFF receipt or TRACE status answers all three. Model advice, mechanical
verification, and Git review do not merge these authority boundaries.

## Compatibility report required before shipping an adapter

Record the source schema and immutable identities, target schema/profile,
mapped fields, omissions, inferred fields, unavailable evidence, receipt
authentication limits, and recipient authorization requirements. Preserve
the original artifact so a lossy projection can be inspected independently.

Conformance should include valid references and failure cases: conflicting
identity, digest changes, missing question/choice, unavailable source bytes,
ambiguous revisions, and attempted promotion of foreign admission. Test
runtime behavior separately: parseable records cannot prove that an agent
will retrieve a rule or that an operation cannot bypass a gate.

## Implementation status

This change adds documentation only. AFF v0.1 packages, verifier behavior,
candidate inbox, and collection operations are unchanged. There is no ADF
conversion command, business-decision engine, rule compiler, or new
execution-enforcement capability in this release.
