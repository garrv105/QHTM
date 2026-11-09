# QHTM Governance

*Version 1.0 of this governance document · applies to the Quantum Hardware Threat Matrix (QHTM)*

This document defines how QHTM is maintained, how it evolves, and how it may transition to neutral institutional stewardship. It exists so that QHTM can be adopted, cited, and relied upon as shared infrastructure — not as any single party's asset.

---

## 1. Purpose and scope

QHTM is an **open, defense-agnostic taxonomy of adversary techniques against quantum computing hardware** and its classical control stack. It is a public good, released under **CC BY 4.0**.

**In scope:** attack *techniques* — what an adversary does — across the physical qubit layer, the QEC syndrome/decoder pipeline, the classical control and compilation stack, cross-tenant/co-tenancy interactions, and covert channels.

**Explicitly out of scope** (to preserve neutrality and prevent scope creep):
- **Detection, mitigation, or defensive methods.** QHTM names techniques; whether or how any tool detects a technique is tracked elsewhere and is never part of the matrix.
- **Product, vendor, or implementation specifications.** QHTM endorses and markets nothing.
- **Post-quantum cryptography (PQC).** A separate, well-served problem.

This scope boundary is a governance invariant: contributions that cross it are declined regardless of merit.

## 2. Guiding principles

1. **Defense-agnostic.** Each entry names an adversary behavior, independent of any detector or product.
2. **Honest maturity.** Every technique carries a maturity tag; over-claiming a demonstrated threat is the cardinal error this project exists to prevent.
3. **Stable, citable identifiers.** A technique ID (e.g., `T3.9`) never changes meaning.
4. **Neutrality.** QHTM names no vendor, ranks no product, and favors no architecture.
5. **Openness.** CC BY 4.0, public process, permanent attribution.

## 3. Roles

- **Founding Editor / Maintainer — Garrv Sipani.** Stewards the matrix, curates contributions, assigns technique IDs and maturity tags, manages releases and versioning, and holds final editorial decision until a formal steering body exists. Responsible for upholding the scope boundary and maturity integrity.
- **Contributors.** Anyone may propose techniques, evidence, corrections, or maturity updates via issues and pull requests.
- **Reviewers (as the project grows).** Recognized domain experts invited to review proposals for correctness, novelty, and tag accuracy.
- **Steering Committee (future).** Upon adoption by a neutral institutional home (see §10), an open multi-stakeholder committee assumes governance, with the Founding Editor retaining a founding role.

## 4. How techniques are proposed and accepted

**Proposal.** Open an issue or pull request describing:
- the **mechanism** (what the adversary does) and the **locus** (where in the stack);
- **evidence** — a described mechanism in the literature, or a demonstrated effect, with citations;
- a **proposed maturity tag** and **tactic placement**.

**Inclusion criteria.** A technique is included when it has (a) a described mechanism in the literature *or* a demonstrated effect, and (b) is distinguished from neighbouring techniques by its **locus or mechanism**, not by implementation detail. Variant inflation (many near-identical entries) is explicitly avoided.

**Review.** The Editor, with reviewers where available, assesses novelty, evidence quality, correct tactic placement, and the honesty of the proposed maturity tag.

**Decision.** Each proposal is **accepted, returned for revision, or declined with a stated rationale.** Decisions and their reasoning are recorded in the issue/PR thread for transparency.

## 5. Maturity-tag integrity

Tags are: **HW** (demonstrated/measured on real hardware), **Sim** (faithful simulation; on-device confirmation pending), **Theo** (described but not demonstrated), **Supp** (largely suppressed on current production hardware).

- Tags are **conservative by default.** When in doubt, a technique is tagged lower, not higher.
- A technique is promoted to **HW only with a citable hardware demonstration.**
- Maturity tags are **not** a severity or priority ranking; they record evidentiary status only.

## 6. Stable identifiers, versioning, and deprecation

- **Stable IDs.** A technique ID never changes meaning. New techniques **append** (e.g., `T2.7`).
- **Deprecation, not deletion.** A retired or merged technique is **marked deprecated**; its ID is retired and never reused for a different meaning.
- **Semantic versioning.** The matrix uses `MAJOR.MINOR.PATCH`. New techniques or re-framings are MINOR; corrections are PATCH. Each release is tagged and archived (Zenodo), minting a citable DOI.
- A **CHANGELOG** records every substantive change.

## 7. Attribution and credit

- Contributors are credited by the **name or handle they choose** — never without consent, never by de-anonymizing anyone.
- The **originator attribution** (Garrv Sipani) and the **CC BY 4.0 license** carry forward in every copy, fork, or adaptation, as the license requires.

## 8. Neutrality and conflict of interest

- QHTM describes techniques; it never ranks, benchmarks, or markets any tool or vendor.
- Contributors with a commercial interest in quantum security **disclose it** in their contribution. Disclosure does not disqualify a contribution; concealment does.
- No contributor — including the Founding Editor — may use the governance process to advantage a specific product. The matrix stays vendor-neutral by rule, not by goodwill.

## 9. Licensing and marks

- **Content:** CC BY 4.0 — free to use, adapt, and build on with attribution.
- **"QHTM" / "Quantum Hardware Threat Matrix":** used to identify the authentic project and its originator; adaptations must not imply endorsement by, or official status within, the project without agreement.
- *MITRE ATT&CK® is a registered trademark of The MITRE Corporation; QHTM is independent and not affiliated with or endorsed by MITRE.*

## 10. Neutral stewardship and evolution

QHTM is offered for adoption and co-stewardship by a **neutral institutional home** — a standards body, foundation, or consortium — so the field can converge on one trusted reference. Upon such adoption, these invariants carry forward **unchanged**:

1. The framework remains **open (CC BY 4.0)**; no adoption may make the vocabulary proprietary.
2. The **scope boundary** (§1), **maturity integrity** (§5), and **stable-ID policy** (§6) are preserved.
3. The **Founding Editor's authorship is permanent**, and a **founding role** in the new governance is retained.
4. Governance transitions to an **open, documented, multi-stakeholder process.**

Any prospective steward is evaluated against these invariants.

## 11. Code of conduct

All participation is governed by the project's [Code of Conduct](CODE_OF_CONDUCT.md), based on the Contributor Covenant. Collaboration is professional, evidence-based, and respectful.

## 12. Amending this document

Changes to governance are proposed via pull request and are approved by the Founding Editor until a Steering Committee exists, after which the committee's documented process applies. Amendments are versioned.

---

*Questions about governance or stewardship: open an issue, or contact the Founding Editor via the repository.*
