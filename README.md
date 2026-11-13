# QHTM — Quantum Hardware Threat Matrix

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24047636.svg)](https://doi.org/10.5281/zenodo.24047636)

**An open taxonomy of adversary techniques against quantum computing hardware and its classical control stack.**

**Version 1.3** · 2026 · Originated by Garrv Sipani · Open community resource
**License: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)** (free to use and adapt with attribution)

**Featured in:** [*Quantum Computing Report* — "Why Quantum Hardware Needs Its Own Threat Model"](https://quantumcomputingreport.com/why-quantum-hardware-needs-its-own-threat-model/)

*Competing interests: the maintainer also develops detection methods for the techniques cataloged here. QHTM is a defense-agnostic, vendor-neutral catalog maintained under open [governance](GOVERNANCE.md) (§8); it names no product and endorses none.*

---

## What this is

QHTM is a **defense-agnostic** catalog of how adversaries attack the quantum-computing stack — the quantum processor, its quantum-error-correction (QEC) syndrome/decoder pipeline, and the classical control and software stack around it. It is modeled on MITRE ATT&CK®: it names and organizes **attack techniques**, not products or defenses, so the field has one shared vocabulary to map attacks and defenses to.

QHTM is intended as a community resource and is offered for **neutral stewardship**. The vocabulary is a public good; it names no vendor and discloses no product's detection method.

## What this is *not*

- **Not** post-quantum cryptography (PQC) or quantum-safe cryptography. PQC protects *classical* systems against *future* quantum computers — the opposite problem, already served by other communities (NIST, CSA, ETSI).
- **Not** a defense, product, or detection specification. QHTM catalogs techniques; whether and how any tool detects a technique is tracked separately and is out of scope here.
- **Not** quantum key distribution or quantum networking.
- **Not** the quantum *communication / network* attack surface — complementary efforts (e.g., SQOUT) cover that. QHTM is scoped to the quantum computer and its classical control stack.

## Scope

Attacks on the quantum computer itself: the physical qubit layer, the QEC syndrome/decoder pipeline, the classical control and compilation stack, cross-tenant/co-tenancy interactions, covert channels, and the reconnaissance and discovery that precede them. Superconducting gate-based systems are the primary reference; techniques generalize to any modality that produces stabilizer syndromes and/or quantum measurement outputs under a classical control stack.

## How to cite

> Quantum Hardware Threat Matrix (QHTM) v1.3, G. Sipani, 2026. CC BY 4.0.

---

## Maturity legend

Each technique carries an honest maturity tag — **including whether current production hardware already suppresses it.** Over-claiming a demonstrated threat is a category error this matrix exists to prevent.

| Tag | Meaning |
|-----|---------|
| ◆ **HW-demonstrated** | Demonstrated or measured on real quantum hardware (by anyone in the field) |
| ▲ **Simulation** | Demonstrated in faithful simulation; on-device confirmation pending (often awaits logical-qubit or multi-tenant hardware) |
| ○ **Theoretical** | Described in the literature but not yet demonstrated |
| ✓ **Suppressed** | Largely engineered out on current production hardware (e.g., tunable-coupler isolation) — tracked for completeness and for future/other hardware |

---

## The matrix (6 tactics, 33 techniques)

| ID | Tactic (adversary goal / locus) | # |
|----|----------------------------------|---|
| T1 | Physical-Layer Interference | 5 |
| T2 | QEC & Decoder Compromise | 7 |
| T3 | Control-Stack & Compilation Tampering | 9 |
| T4 | Cross-Tenant / Co-Tenancy | 3 |
| T5 | Covert Channels & Information Leakage | 7 |
| T6 | Reconnaissance & Discovery | 2 |

### T1 — Physical-Layer Interference
*Adversary perturbs qubits or readout through device physics. On current tunable-coupler hardware most return a statistical null; retained for completeness and for other/future hardware. Recent 2026 reports (arXiv:2611.07983; arXiv:2504.07875) show large but strongly device-dependent induced-outcome shifts on some current devices — QHTM holds the conservative tags pending methodology that isolates a true state flip from readout crosstalk, leakage and heating.*

| ID | Technique | What it does | Maturity |
|----|-----------|--------------|----------|
| T1.1 | Idle crosstalk | Spectator-qubit disturbance from neighbouring idle qubits | ✓ |
| T1.2 | Active gate crosstalk (incl. double-sided) | Aggressor gates corrupt a victim qubit (e.g., double-sided crosstalk, 2025) | ◆ / ✓ |
| T1.3 | Induced-flip disturbance | Aggressor activity flips a victim state ("Quantum Rowhammer"-class, 2025) | ◆ / ✓ |
| T1.4 | Readout / measurement crosstalk | Measurement of one qubit biases a neighbour's readout | ✓ |
| T1.5 | Reset / state-residue leakage | Incomplete reset leaks prior-tenant/prior-job state on qubit reuse | ✓ |

### T2 — QEC & Decoder Compromise
*Adversary corrupts the QEC syndrome stream or the decoder, forcing mis-correction and inducing LOGICAL errors at unchanged physical noise. The surface the field is pivoting to with fault tolerance; largely absent from prior defensive literature.*

| ID | Technique | What it does | Maturity |
|----|-----------|--------------|----------|
| T2.1 | Targeted syndrome injection | Alter specific syndrome bits to steer a chosen logical outcome | ▲ |
| T2.2 | Random syndrome injection | Inject random syndrome errors to raise the logical error rate | ▲ |
| T2.3 | Syndrome replay | Feed stale/repeated syndromes to mask a live error | ▲ |
| T2.4 | Syndrome suppression | Drop/zero syndromes so real errors go uncorrected | ▲ |
| T2.5 | Decoder-path tampering | Compromise the classical decoder path feeding corrections | ▲ |
| T2.6 | Adversarial trajectory selection | Choose physically-valid but low-confidence error explanations to force decoder mis-correction | ▲ |
| T2.7 | Decoder backpressure / throughput exhaustion | Crafted high-syndrome-weight circuit drives the real-time decoder (FPGA/ASIC MWPM or Union-Find, on microsecond budgets) into backlog, pushing decode latency past the fault-tolerance window — resource exhaustion, not corruption | ○ |

### T3 — Control-Stack & Compilation Tampering
*The live, near-term surface: the classical compiler/transpiler, calibration pipeline, scheduler, result path, and any persistent implant in the control software. The classical control-plane itself — schedulers, calibration DBs, the pulse/AWG layer (T3.9) — is a primary soft target, not only the compiler. Vendor-agnostic, deployable-today risk. These techniques assume an adversary already holds a foothold in the classical pipeline; real initial-access vectors into it include vulnerabilities in the quantum SDK's circuit-serialization and control code (e.g., CVE-2026-116499 — remote code execution via a crafted Qiskit QPY file; and CVE-2026-124035 — code execution via a tampered Amazon Braket task-result document — two vendors within one week).*

| ID | Technique | What it does | Maturity |
|----|-----------|--------------|----------|
| T3.1 | Rotation-angle perturbation | Silently shift gate angles below notice to bias results | ◆ |
| T3.2 | Gate deletion | Silently drop gates from the compiled circuit | ◆ |
| T3.3 | Hidden-gate insertion | Insert gates not in the submitted circuit | ◆ |
| T3.4 | Qubit remapping | Reassign logical→physical qubits to a worse/attacker-chosen layout | ◆ |
| T3.5 | Gate substitution | Replace gates with near-equivalents that change the result | ◆ |
| T3.6 | Calibration-pipeline tampering | Corrupt calibration data so honest circuits misbehave | ○ |
| T3.7 | Persistent control-stack implant (APT) | Within-tolerance implant tampering just under per-circuit integrity checks | ▲ |
| T3.8 | Result / measurement-path tampering | Alter returned counts/measurements after execution (the result path is also a code-execution delivery vector — cf. CVE-2026-124035) | ○ |
| T3.9 | Classical control-plane / scheduler injection | Tampering at the orchestration layer — scheduler/controller plugins (Slurm/SPANK-style), calibration databases, pulse-synthesis/AWG memory — rather than the circuit representation | ○ |

### T4 — Cross-Tenant / Co-Tenancy
*Attacks on a shared (multi-tenant) QPU: one tenant corrupts or observes another. **Temporal (time-sliced) tenancy** — today's common commercial model — exposes state-remanence / calibration drift (T4.3, cf. T1.5); **spatial co-tenancy** on one die (T4.1/T4.2) is rarer and more forward-looking. On-hardware confirmation awaits broader multi-tenant access.*

| ID | Technique | What it does | Maturity |
|----|-----------|--------------|----------|
| T4.1 | Cross-tenant crosstalk corruption | A co-resident tenant corrupts a victim tenant's computation | ▲ |
| T4.2 | Tenant state-residue / isolation break | Victim state leaks to a co-resident or subsequent tenant | ▲ |
| T4.3 | Temporal-tenant state-remanence / calibration degradation | On time-sliced tenancy, an aggressor leaves residual excitations, cryo-stage heating, or detuning drift degrading the next tenant's gate fidelity — no simultaneous co-residency needed | ○ |

### T5 — Covert Channels & Information Leakage
*Techniques that move information where it should not flow, or leak it through side channels.*

| ID | Technique | What it does | Maturity |
|----|-----------|--------------|----------|
| T5.1 | Feed-forward covert channel (within-job) | Mid-circuit measurement plus classical feed-forward carries information from one register to another that shares no two-qubit gate with it, within one job. The primitive itself is standard and hardware-demonstrated by third parties (arXiv:2403.09514, not a security paper). Its capacity as a channel has one reported measurement (preprint, arXiv:2611.05722): 0.69–0.90 bit of mutual-information capacity on three production superconducting processors, with matched controls near zero. That measurement is single-source and is the maintainer's own (see the competing-interests note above); it has not been independently replicated | ◆ |
| T5.2 | Cross-tenant covert channel | The same primitive across tenants on shared hardware | ▲ |
| T5.3 | Control-flow / dynamic-circuit side channel | Conditional execution leaks data via observable control flow | ✓ |
| T5.4 | Decoder-timing / passive side channel | An observer who sees only how long the decoder takes on each shot can infer properties of the protected computation and its hardware: the code distance, an estimate of the logical error rate, and enough to distinguish among the three devices tested. Shown for repetition-code memory experiments by timing two standard decoders offline on syndrome data recorded from three production superconducting processors (preprint, arXiv:2609.12145). How an attacker would obtain the timing on a real system is not demonstrated, and no recovery of circuit contents or logical data is shown. See also T6.1 | ▲ |
| T5.5 | Power side channel | Device power traces classify or recover the running circuit | ◆ |
| T5.6 | Partial-code logical-state eavesdropping | An observer holding the data-qubit readout of only a region of an error-correcting code — one module's window in a partitioned decoder, or one shared readout line — infers the protected logical value when that region contains a representative of the logical operator being read out. Syndrome data alone does not carry this leakage. Leakage quantified on real-device readout with an emulated observer (arXiv:2611.13174) | ▲ |
| T5.7 | Decoder-side gate / circuit inference | An untrusted or passively compromised decoder infers which logical operations, and in aggregate which circuit, a victim is running, from gate-dependent patterns in the syndrome stream it is given to decode. T5.7 concerns the identity of the computation at the decoder; T5.6 concerns the logical value at partial readout. Shown in simulation only; the classifier's code distance is not stated (arXiv:2607.12174, preprint) | ▲ |

### T6 — Reconnaissance & Discovery
*Techniques that gather information about the platform, or about a target's presence on it, to enable a later technique. They are carried out from ordinary user access. Boundary with T5: T5 covers leakage of a tenant's workload or data; T6 covers information about the platform or a target's presence.*

| ID | Technique | What it does | Maturity |
|----|-----------|--------------|----------|
| T6.1 | Device fingerprinting / infrastructure mapping | Derive a stable identifier for a specific processor, or a region of it, from its error signature using ordinary user access, to map a provider's fleet and target later attacks (arXiv:2202.12731; the same signal is also used defensively for backend verification, cf. arXiv:2506.16614) | ◆ |
| T6.2 | Co-tenant activity sensing | A co-resident adversary detects that a neighbour is computing from disturbance on qubits it controls. The effect is measured on hardware; co-tenancy was emulated within one submission (arXiv:2605.14325) | ▲ |

---

## Governance & contributing

- **Open licence (CC BY 4.0).** Free to use, map to, and build on, with attribution. The vocabulary is the public good.
- **Stable IDs.** A technique ID (e.g., `T3.7`) never changes meaning. New techniques append; maturity tags update as the field demonstrates them.
- **Versioning.** Semantic versions (v1.0 → …).
- **Contributions welcome.** Propose new techniques or maturity evidence via pull request / issue. Submissions are reviewed by the maintainer per the [Governance](GOVERNANCE.md) process; participation follows the [Code of Conduct](CODE_OF_CONDUCT.md).
- **Governance.** QHTM is maintained under an open, documented [governance model](GOVERNANCE.md) — stable technique IDs, conservative maturity tags, a defined contribution and review process, and an explicit path to neutral institutional stewardship.
- **Neutral stewardship intended.** QHTM is offered for adoption and co-stewardship by a neutral institutional home; its technique classes are being contributed to MITRE CWE/CAPEC where applicable.
- **Changelog — v1.3.** Added a sixth tactic, T6 Reconnaissance & Discovery, with T6.1 (device fingerprinting / infrastructure mapping, hardware-demonstrated) and T6.2 (co-tenant activity sensing, simulation-tagged). Added T5.7 (decoder-side gate / circuit inference, simulation-tagged). Tactic count 5 → 6; technique count 30 → 33. No existing ID or maturity tag changed; the maturity legend is unchanged.
- **Changelog — v1.2.** Added T5.6 (partial-code logical-state eavesdropping, simulation-tagged), a confidentiality technique at logical readout on the partitioned QEC data path. Technique count 29 → 30; tactic count unchanged. Added the hardware-evidence citation for T5.1 (the maintainer's own measurement, disclosed as such). Corrected reference titles and authors across the reference lists.
- **Changelog — v1.1.** Added T2.7 (decoder backpressure / throughput exhaustion), T3.9 (classical control-plane / scheduler injection), T4.3 (temporal-tenant state-remanence / calibration degradation); sharpened the T3 and T4 framing (temporal vs spatial multi-tenancy; control-plane as a near-term target). Technique count 26 → 29. Added references for T1.3 (Row Hammer / QubitHammer, maturity held), a second-vendor initial-access CVE (Amazon Braket, CVE-2026-124035), and related-work positioning (QaaS STRIDE model). Incorporates community review from the r/QuantumComputing and Unitary Foundation communities.

## Acknowledgements & prior work

QHTM builds on a growing body of published research on attacks against quantum hardware and on an earlier preliminary mapping of quantum attacks to generic weakness categories (B. Blakely, J. Chung, A. Poczatek, R. Syed & R. Kettimuthu, *Toward a Quantum Information System Cybersecurity Taxonomy and Testbed: Exploiting a Unique Opportunity for Early Impact*, arXiv:2404.12465, 2024). QHTM extends that direction with a named, versioned, hardware-grounded technique namespace. Complementary pipeline-level models of the quantum-cloud attack surface — e.g. Rahman, Haghparast & Mikkonen, *An End-to-End Threat Model for the Quantum-as-a-Service Pipeline* (arXiv:2608.05836, 2026), a six-stage STRIDE analysis — map into this namespace; QHTM supplies the named-technique granularity such narrative models leave open. References for individual techniques are maintained in the per-technique pages.

*MITRE ATT&CK® is a registered trademark of The MITRE Corporation; QHTM is independent and not affiliated with or endorsed by MITRE.*
