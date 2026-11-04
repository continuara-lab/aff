# AFF and Proofpress: open format and reference governance product

Both [AFF](https://github.com/continuara-lab/aff) and
[Proofpress](https://github.com/chenmingtang830/proofpress) are Apache-2.0
open-source projects. They have different responsibilities.

| Layer | Owner | Responsibility |
| --- | --- | --- |
| Portable Finding | AFF | v0.1 specification, schema, canonical package digest, evidence descriptors, identity and revision rules, attributed receipts, conformance corpus. |
| Simple distribution | AFF | `.aff` files, `aff/` Git collection, generated index, offline verifier, candidate capture, starter agent instructions. |
| Governed runtime | Proofpress | Authenticated workspace, separate agent and Owner credentials, evidence evaluation, Human Approval or Owner policy, current governed context, lifecycle handling, reliance and run records, self-hosted or hosted service. |
| Bridge | Proofpress adapter, tested against AFF | Project a bounded Proofpress claim into AFF; receive an external AFF package as an untrusted candidate. A foreign receipt does not grant receiver-local authority. |

## Decision-first direction — working design, November 4, 2026

Proofpress is exploring a product loop around capture, verify, govern, use,
and learn for production-agent decisions. ADF is a separate draft for the
decision record; AFF continues to describe portable Findings that can support
those decisions. This direction does not change AFF v0.1 or establish a
completed integration. The [compatibility note](ADF-TRACE-BOUNDARY.md) defines
the documentation-level boundary.

Runtime use has three distinct modes: **Advise** supplies applicable cases;
**Enforce** checks required conditions at a controlled operation; **Execute**
runs a validated procedure under explicit preconditions. AFF's verifier and
Git collection provide none of these execution guarantees by themselves.
Hooks, adapters, wrappers, and gateways must be verified in the actual runtime
before any claim of blocking or stopping old behavior.

ADF's exact draft schema and the commercial/open implementation split remain
under review. This document does not change the Apache-2.0 rights already
granted for AFF or Proofpress code. Basic AFF capture, verification, export,
and independent use do not require a Proofpress account.

## Extraction decision

The portable format and reference tooling already live in AFF. Do not copy
Proofpress's review or authorization kernel into AFF just to make the format
look complete: two implementations of local authority would diverge. Keep
Proofpress as an optional reference governance runtime, usable locally or
self-hosted, and keep AFF usable without it. When another independent runtime
needs a genuinely shared component, move only a small, versioned portable
contract or conformance utility into AFF after testing both implementations.

This is a product boundary, not an exclusivity rule. Another team can
implement its own receiver, policy, storage, and approval process against AFF.
The same `.aff` bytes can live in Git, SQLite, object storage, or a hosted
service. The consumer must still authenticate issuers and decide local use.

## Current interoperability limits

Proofpress exports a bounded subset and has authenticated hosted candidate
intake with a receiver-local proposal path. The complete Owner decision UI for
foreign Findings, authenticated cross-system revision and withdrawal
reconciliation, exact-revision use exchange, independent outside
implementations, and live deployment validation are not complete. The
[mapping](PROOFPRESS-MAPPING.md) and [lifecycle note](LIFECYCLE-RECONCILIATION.md)
record the current technical boundary. A green format verifier or a Git merge
cannot stand in for those gates.
