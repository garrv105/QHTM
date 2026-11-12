# Contributing to QHTM

QHTM is an open, community resource. Contributions of new techniques, maturity
evidence, references, and corrections are welcome.

## What belongs in QHTM

QHTM is a **defense-agnostic** catalog of **attack techniques** against quantum
computing hardware and its classical control stack. It describes *what adversaries
do*, not how any particular product detects or prevents it.

**In scope:** attack techniques against the physical qubit layer, the QEC
syndrome/decoder pipeline, the classical control/compilation stack, cross-tenant
interactions, and covert channels.

**Out of scope:** post-quantum cryptography (PQC), quantum key distribution,
algorithmic attacks, and any product-specific detection or defense method.

## How to contribute

1. Open an issue proposing the change (new technique, maturity update, reference,
   or correction), with supporting references.
2. For a new technique, propose: a tactic (T1–T6), a short name, a one-line
   description, a maturity tag, and at least one public reference.
3. Submit a pull request. The maintainer reviews for scope, accuracy, and
   evidence.

## Maturity tags

- `◆ HW-demonstrated` — demonstrated/measured on real quantum hardware
- `▲ Simulation` — demonstrated in faithful simulation; on-device pending
- `○ Theoretical` — described in the literature, not yet demonstrated
- `✓ Suppressed` — largely engineered out on current production hardware

Honest maturity is a core principle: do not tag a technique higher than the
public evidence supports.

## Stable identifiers

Technique IDs (e.g., `T3.7`) are stable and never change meaning. New techniques
append; deprecated ones are marked, not removed.

## License

By contributing, you agree your contribution is licensed under CC BY 4.0, the
same license as the project.
