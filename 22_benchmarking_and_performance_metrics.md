# Benchmarking Quantum Computers — Metrics, Standards, and Their Limitations

> This file covers how quantum computers are benchmarked — and why benchmarking is unusually *contested* in this field. It surveys the metrics (raw qubit count, quantum volume, algorithmic qubits, CLOPS, randomized/cross-entropy benchmarking, application benchmarks), their strengths and blind spots, and the crucial meta-lesson: *no single number captures how good a quantum computer is*, and vendor-originated metrics should be read with awareness of their favorable framing. It is a core "honesty/due-diligence" throughline file, building on Files 2 (RB/XEB), 3–7 (modality specs), 9/18 (logical metrics), and 20 (vendor metric-shifting).

---

## Part I — Why Benchmarking Is Contested

Unlike classical computing — where clock speed, FLOPS, and standard benchmark suites (SPEC, MLPerf) are broadly accepted — quantum-computing benchmarking suffers from:

- **A proliferation of vendor-specific or vendor-favorable metrics** (Section 6, 7) — each often designed to flatter the originating company's architecture (File 20's metric-shifting).
- **Genuine difficulty constructing hardware-agnostic comparisons** across fundamentally different architectures (gate-model vs. annealing, different native connectivities, different modalities, Files 3–7).
- **A persistent gap between "headline" marketing metrics and metrics that actually predict useful-algorithm performance** — a device can score well on a benchmark yet perform poorly on a user's actual workload.

This makes benchmarking a domain requiring *skeptical scrutiny* — the same disciplined, evidence-based assessment the database applies to advantage claims (Files 14, 17) and roadmaps (File 19). The central lesson: treat *all* composite vendor metrics as *directional/marketing indicators*, not engineering-grade comparison tools, and benchmark the user's own target application directly (Section 9).

---

## Part II — The Metrics

### 1. Raw physical qubit count

The most commonly cited but *least informative* metric in isolation:

- It says *nothing* about gate fidelity, connectivity, or coherence (Files 1, 3) — yet remains the dominant headline number in press coverage.
- A 1000-qubit device with poor fidelity/connectivity may be *less useful* than a 50-qubit device with excellent fidelity/connectivity (File 3, Appendix B).
- Critically, physical qubit count ≠ logical qubit count (Files 1, 9) — a 1000-physical-qubit machine might host only a handful of logical qubits (File 9).

**Caution:** never use raw qubit count alone for cross-vendor or cross-modality comparison. It is the most misleading metric when cited in isolation, and its dominance in headlines is a persistent source of miscalibrated expectations (Files 1, 19).

### 2. Quantum Volume (IBM-originated)

**Quantum Volume (QV)** (Cross et al., 2019) is a single composite number: V_Q = 2^n, where n is the largest *square* circuit (n qubits × n layers of random two-qubit gates, compiled to the device) that the device runs with heavy-output-generation probability above a threshold (2/3).

- **Strength:** rewards qubit count *AND* connectivity *AND* gate fidelity *simultaneously* — a wide/poorly-connected/low-fidelity device scores poorly, addressing raw-count's blindness (Section 1).
- **Weaknesses:** exponentially expensive to measure at scale (limiting its use for large devices); uses *random* circuits unrepresentative of structured algorithms; and (like all composite metrics) does not directly predict any specific application's performance.
- **In practice:** Quantinuum's trapped-ion systems (File 4) post the highest QV (owing to all-to-all connectivity and high fidelity), while superconducting devices' QV is limited by connectivity — illustrating how QV captures more than raw count.

QV is a genuine improvement over raw count (it integrates count, connectivity, and fidelity), but it is still a random-circuit composite that doesn't predict application performance, and its exponential measurement cost limits it at scale.

### 3. Algorithmic Qubits (#AQ, IonQ-originated)

**Algorithmic Qubits (#AQ)** (IonQ) is a composite metric based on the largest *useful algorithmic* circuit the device runs successfully — framed around application-relevant circuits rather than random ones.

- **Framing:** emphasizes *useful* capability (closer to real algorithms than QV's random circuits).
- **Caveat:** it is *company-originated* (IonQ) and — not coincidentally — emphasizes trapped-ion strengths (high fidelity, connectivity, File 4). Like all vendor-originated metrics, it should be read with awareness of this favorable framing (File 20). IonQ's #AQ claims have drawn some scrutiny (File 20).

### 4. CLOPS (IBM-originated)

**CLOPS (Circuit Layer Operations Per Second)** (IBM) captures *throughput* — how fast the full stack (including classical control, compilation, data movement) executes circuit layers.

- **Framing:** emphasizes *speed/throughput* — where superconducting's fast gates (File 3) are advantaged over slow-gate ion traps (File 4).
- **Caveat:** like #AQ (which favors ions), CLOPS is company-originated (IBM) and emphasizes a dimension (throughput) where superconducting excels — the mirror image of #AQ's ion-favorable framing. This explicit point — that vendor metrics favor the originating architecture — is essential (File 20).

### 5. Randomized and cross-entropy benchmarking

**Randomized benchmarking (RB)** and **cross-entropy benchmarking (XEB)** (File 2, Section 14) are *lower-level, architecture-neutral* fidelity-characterization protocols:

- **RB:** measures average gate fidelity via random Clifford sequences, SPAM-robust and scalable (File 2). Reports a *directly comparable* per-gate fidelity across hardware when methodology is consistent.
- **XEB:** measures whole-circuit fidelity via cross-entropy with the ideal distribution (used in supremacy claims, File 14).
- **Strength:** these are *more architecture-neutral* than the composite metrics — an RB-measured two-qubit fidelity of 99.9% means roughly the same thing across modalities (Files 3–7), making them arguably the *most useful for genuine engineering comparison*.
- **Caveat:** they report *average* fidelity, hiding the error's *structure* (coherent vs. stochastic, correlated vs. independent, File 2, Section 34) — so a device's RB fidelity doesn't fully predict its algorithmic performance (coherent errors can accumulate worse than RB suggests, File 2).

RB/XEB fidelities (Files 3–7) are the most apples-to-apples cross-vendor numbers, more useful for engineering comparison than composite marketing metrics — though they characterize *gates*, not full applications, and hide error structure.

### 6. Application-specific and proxy benchmarks

An emerging, arguably *more meaningful* benchmarking direction: running *specific, fixed algorithmic workloads* across multiple vendors' hardware and comparing achieved performance/accuracy:

- **Examples:** specific VQE chemistry problems, specific QAOA MaxCut instances, quantum-volume-style structured circuits closer to real algorithms than random ones.
- **Efforts:** the QED-C application benchmarks (File 21), Supermarq, and academic/enterprise benchmarking suites running standardized workloads across vendors.
- **Strength:** *directly measures* what matters — performance on application-relevant circuits — rather than a composite proxy.
- **Growing adoption:** increasingly, *enterprise end-users* run their *own* pilot-workload comparisons across vendors (File 20, 24) rather than relying on any vendor's composite metric — the most honest benchmarking (measuring the user's actual workload).

Application-specific benchmarks are the most meaningful direction (measuring real-workload performance), championed by independent/academic efforts and enterprise users, and they are increasingly the standard for genuine cross-vendor comparison (File 24).

---

## Part III — The Honest Meta-Point and Assessment

### 7. Vendor metrics favor their originators

A crucial, explicit point (File 20): **vendor-originated composite metrics should always be read with awareness that they tend to favor the originating company's architecture:**

- **#AQ (IonQ)** emphasizes trapped-ion fidelity/connectivity strengths.
- **CLOPS (IBM)** emphasizes superconducting throughput/speed strengths.
- **Quantum Volume (IBM)** rewards connectivity (where ions excel — Quantinuum posts the highest QV).

This is not (necessarily) dishonesty but a natural consequence of each company designing a metric that captures *its* strengths. The lesson: no vendor metric is a *neutral standard*; each is a directional/marketing indicator reflecting the originator's architecture (File 20). Reading vendor metrics with this awareness — asking "which architecture does this metric favor?" — is essential to fair comparison.

### 8. No single number suffices

The honest meta-point: **no single number adequately captures "how good" a quantum computer is for a prospective user's specific purpose.**

- Raw qubit count (Section 1) ignores fidelity/connectivity.
- Composite metrics (QV, #AQ, CLOPS; Sections 2–4) are random-circuit-based and vendor-favorable.
- RB/XEB (Section 5) characterize gates, not applications, and hide error structure.
- Even application benchmarks (Section 6) depend on the specific application chosen.

A quantum computer's usefulness is *multidimensional* (qubit count, fidelity, connectivity, coherence, speed, error structure, and — for fault tolerance — logical qubits and logical error rate, Files 9, 18), and no scalar captures it. This multidimensionality is *why* benchmarking is contested (Section 1) and why single-number comparisons mislead.

### 9. The reliable approach

The *only fully reliable* approach: **benchmark the user's own target application/circuit structure directly** against the error-correction/mitigation and resource-estimation frameworks (Files 9, 10, 18), treating all composite vendor metrics as *directional/marketing indicators* rather than engineering-grade comparison tools.

- For a *specific* use case, run that use case (or a close proxy) on the candidate hardware and measure the achieved accuracy/performance.
- For *fault-tolerant* assessment, use resource estimation (File 18) with the hardware's actual parameters (fidelity, connectivity, speed) to estimate the application's feasibility.
- Treat vendor headline metrics as *starting points* for shortlisting, not final comparisons.

This application-direct, resource-estimation-grounded approach is the honest benchmarking methodology — measuring what matters (the user's workload) rather than trusting composite proxies. It is the benchmarking counterpart to the algorithm checklist (File 13), the NISQ-application checklist (File 17), and the roadmap framework (File 19).

### 10. Summary

Quantum-computing benchmarking is unusually contested because, unlike classical computing's accepted standards (FLOPS, SPEC), it suffers from a proliferation of vendor-favorable metrics, genuine cross-architecture-comparison difficulty, and a gap between headline metrics and useful-algorithm performance. The metrics — raw qubit count (least informative, ignores fidelity/connectivity, ≠ logical qubits), Quantum Volume (integrates count/connectivity/fidelity but random-circuit-based and exponentially costly), Algorithmic Qubits and CLOPS (vendor-originated, favoring ions' and superconducting's respective strengths), randomized/cross-entropy benchmarking (architecture-neutral gate fidelities, the most apples-to-apples, but hiding error structure), and application-specific benchmarks (the most meaningful, measuring real-workload performance) — each capture *part* of the picture, but *no single number* adequately captures how good a quantum computer is for a specific purpose. Vendor-originated composite metrics should always be read with awareness that they favor the originating architecture (File 20), and the only fully reliable approach is to benchmark the user's *own* target application directly, grounded in the error-correction, mitigation, and resource-estimation frameworks (Files 9, 10, 18), treating composite vendor metrics as directional/marketing indicators rather than engineering-grade comparisons. This disciplined, application-direct, vendor-metric-skeptical benchmarking is essential to honest hardware assessment, and it is the throughline connecting this file to the classical-comparison discipline (File 14), the NISQ-application skepticism (File 17), the roadmap-credibility framework (File 19), and the competitive-landscape metric-shifting awareness (File 20) — together forming the database's coherent, evidence-based approach to assessing quantum-computing claims.

*Cross-references: RB/XEB and the coherent/stochastic error distinction (File 2); modality gate fidelities and specs (Files 3–7); physical vs. logical qubits (Files 1, 9); logical error rate as the fault-tolerant metric (Files 9, 18); resource estimation for application-feasibility assessment (File 18); vendor metric-shifting and competitive positioning (File 20); roadmap credibility and independent benchmarking (File 19); the classical-comparison discipline and advantage claims (Files 14, 17); enterprise application-benchmarking (File 24).*
