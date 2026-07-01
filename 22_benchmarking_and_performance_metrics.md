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


**The benchmarking hierarchy — from components to applications:**

```text
   ┌─────────────────────────────────────────────┐  Most application-relevant
   │  APPLICATION benchmarks (QED-C, real tasks)  │        ▲
   ├─────────────────────────────────────────────┤        │
   │  SYSTEM benchmarks (Quantum Volume, CLOPS)   │        │ but harder to
   ├─────────────────────────────────────────────┤        │ interpret / game
   │  HOLISTIC (randomized benchmarking, XEB)     │        │
   ├─────────────────────────────────────────────┤        │
   │  COMPONENT (T1/T2, 1q/2q gate fidelity)      │  Most precise, least
   └─────────────────────────────────────────────┘  predictive of usefulness
```

*No single number captures a machine; a high qubit count with poor fidelity or
sparse connectivity can be worse than a smaller, cleaner device.*

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

---

## Part IV — Deeper Topics, Worked Examples, and Glossary

### 11. Calibration drift and reproducibility

A practical benchmarking complication (Files 3, 8): quantum-device performance *drifts over time* (calibration drift from TLS spectral diffusion, flux drift, temperature, File 3). Consequences for benchmarking:

- **Time-dependence:** a device's benchmark score (QV, fidelity) can vary day-to-day, so a single measurement is a *snapshot*, not a fixed property.
- **Reproducibility challenge:** reproducing a benchmark requires the *same calibration state*, which is hard — undermining cross-time and cross-lab comparison.
- **Cherry-picking risk:** a vendor could report a *best-case* benchmark from a favorable calibration, not the typical performance — a reason to prefer *distributions* (typical and worst-case) over single best-case numbers, and to weight *independent* measurement (Section 7; File 19).

Calibration drift makes quantum benchmarking a *moving target even for a single device*, and honest benchmarking reports the *distribution* of performance (not just the best), uses consistent methodology, and favors independent verification (File 19). This time-dependence is a distinctive challenge versus classical benchmarking (where hardware performance is stable).

### 12. Worked example: qubit count vs. usefulness

Concretely illustrate why raw count misleads (Section 1). Compare two hypothetical devices:

- **Device A:** 1000 physical qubits, 99.0% two-qubit fidelity, nearest-neighbor connectivity (degree 4).
- **Device B:** 50 physical qubits, 99.9% two-qubit fidelity, all-to-all connectivity.

For a structured algorithm requiring long-range interactions (e.g., a chemistry ansatz), Device B may *outperform* Device A: B's all-to-all connectivity avoids the SWAP overhead (File 8) that inflates A's effective gate count, and B's 10× better fidelity means far lower accumulated error. A's 20× larger qubit count is *irrelevant* if the algorithm fits in 50 qubits and A's connectivity/fidelity cripple it. This worked example (echoing File 3, Appendix B) demonstrates why raw count is the most misleading metric in isolation — usefulness depends on the *joint* fidelity-connectivity-count picture (which QV partly captures, Section 2), and on the *specific algorithm* (which only application benchmarks capture, Section 6). It is the quantitative case for the "no single number" meta-point (Section 8).

### 13. Worked example: reading a benchmark claim

Apply the benchmarking skepticism (Sections 7–9) to a hypothetical: *"Our device achieved Quantum Volume 2²⁰, the highest ever."* Assessment:

1. **What does QV measure?** Count × connectivity × fidelity on *random* circuits (Section 2) — a real composite, but random-circuit-based.
2. **Whose metric?** QV is IBM-originated but favors *connectivity* (where ions excel) — so a trapped-ion device (Quantinuum) posting high QV reflects its connectivity/fidelity advantage (Section 2).
3. **Does it predict my application?** Not directly — QV uses random circuits, not the user's structured algorithm (Section 8).
4. **Independently verified?** Was it independently measured, and is it typical or best-case (Section 11)?
5. **What matters for me?** For the user's specific application, run *that* application (Section 9), not trust QV.

Verdict: a high QV is a *genuine positive signal* (good count/connectivity/fidelity) but *not* a guarantee of application performance, and it should be read as a directional indicator, verified independently, and supplemented by application-direct benchmarking (Section 9). This worked assessment is the practical skill for reading any benchmark claim — understand what it measures, whose metric it is, whether it predicts the user's application, and whether it's independently verified.

### 14. The evolution of benchmarking

Quantum benchmarking has *evolved* alongside the hardware:

- **Early (pre-2019):** raw qubit count dominated (Section 1) — the "qubit race" era, misleading in isolation.
- **2019–2022:** composite metrics emerged (Quantum Volume 2019, #AQ, CLOPS) to address raw-count's blindness — but each vendor-favorable (Section 7).
- **2023–present:** the shift toward *logical* metrics (logical qubit count, logical error rate, Files 9, 18) as the field entered the error-correction era (File 9), and toward *application benchmarks* (Section 6) measuring real-workload performance. The "quality over quantity" reframing (File 19) is a benchmarking reframing as much as a roadmap one.

The evolution — from raw count to composite metrics to logical/application benchmarks — reflects the field's maturation (Files 9, 19) and its growing recognition that meaningful benchmarking must capture *usefulness* (logical qubits, application performance), not just raw scale. The current best practice (logical metrics + application benchmarks) is the most honest, and the trajectory is toward increasingly meaningful (if harder-to-measure) benchmarks.

### 15. Logical-qubit benchmarking (the fault-tolerant frontier)

As the field enters fault tolerance (File 9), the meaningful metrics shift to the *logical* level:

- **Logical qubit count:** how many error-corrected logical qubits (Files 9, 18) — far more meaningful than physical count (Section 1).
- **Logical error rate:** the error rate *per logical operation* (File 9) — the key fault-tolerant quality metric, and what below-threshold demonstrations (File 9) measure.
- **Below-threshold scaling (Λ):** whether increasing code distance *reduces* logical error (File 9, the Google Willow demonstration) — the crucial fault-tolerance-viability signal.

These logical metrics (File 9, 18) are the *right* metrics for the fault-tolerant era, and the field's 2023–2024 reframing (File 19) toward them is the correct maturation. An analyst assessing fault-tolerant progress should track *logical* qubit count, *logical* error rate, and *below-threshold scaling* (File 9) — not physical qubit count — as the meaningful benchmarks.

### 16. Glossary and summary

- **Raw qubit count:** physical qubits; least informative in isolation (ignores fidelity/connectivity, ≠ logical qubits).
- **Quantum Volume (QV):** IBM composite integrating count × connectivity × fidelity on random circuits; ions post highest.
- **#AQ (Algorithmic Qubits):** IonQ composite emphasizing useful circuits; favors ion strengths.
- **CLOPS:** IBM throughput metric; favors superconducting speed.
- **RB/XEB:** architecture-neutral gate/circuit fidelity protocols; most apples-to-apples but hide error structure.
- **Application benchmarks:** real-workload performance across vendors; most meaningful.
- **Logical qubit count / logical error rate / Λ:** the fault-tolerant-era metrics (File 9).
- **Calibration drift:** time-dependence of device performance, complicating reproducibility.
- **The meta-lesson:** no single number captures usefulness; vendor metrics favor their originators; benchmark your own application.

**Summary.** Quantum-computing benchmarking is contested because no accepted standards exist, cross-architecture comparison is genuinely hard, and headline metrics often don't predict useful performance. Raw qubit count is least informative (ignores fidelity/connectivity, ≠ logical qubits); Quantum Volume integrates count/connectivity/fidelity but is random-circuit-based; #AQ and CLOPS are vendor-originated (favoring ions and superconducting respectively); RB/XEB give architecture-neutral gate fidelities (most apples-to-apples, but hide error structure); and application benchmarks measure real-workload performance (most meaningful). No single number captures how good a quantum computer is for a specific purpose (usefulness is multidimensional), vendor composite metrics favor their originators, and calibration drift makes even single-device benchmarks time-dependent — so the only reliable approach is to benchmark the user's *own* application directly, grounded in resource estimation (File 18) and logical metrics (File 9), treating vendor composites as directional indicators verified independently. The field's evolution from raw count to composite metrics to logical/application benchmarks reflects its maturation, and the fault-tolerant era's logical metrics (logical qubit count, logical error rate, below-threshold scaling, File 9) are the right ones going forward. This disciplined, application-direct, vendor-metric-skeptical benchmarking is the throughline connecting this file to the classical-comparison discipline (File 14), NISQ-application skepticism (File 17), roadmap credibility (File 19), and competitive metric-shifting awareness (File 20).

---

## Part V — Cross-Paradigm Benchmarking and Reader's Guide

### 17. Benchmarking annealers and non-gate-model machines

Benchmarking gets *harder* across *paradigms* (Files 2, 17): comparing a gate-model quantum computer to a **quantum annealer** (D-Wave, File 17) is especially fraught, because:

- **Different operations:** annealers don't run gate circuits — they evolve toward an Ising ground state (File 2, Section 38) — so gate-model metrics (QV, #AQ) don't apply.
- **Contested advantage:** annealer benchmarking (File 17) is dominated by the *advantage* question — does the annealer beat classical simulated annealing / specialized solvers on the *same* optimization problem? — which is contested (File 17).
- **Problem-embedding overhead:** annealers require *embedding* the problem onto their fixed connectivity graph (Pegasus/Zephyr, File 17), which reduces the effective problem size and complicates comparison.

Cross-paradigm benchmarking (gate-model vs. annealing) requires *problem-specific* comparison (run the same optimization problem on both and on classical methods, File 14, 17) — there is no common composite metric. This reinforces the "no single number" meta-point (Section 8) at the paradigm level: even *within* quantum computing, different paradigms resist common benchmarks, and honest comparison is application-specific (Section 9).

### 18. Benchmarking quantum networks and sensors

Benchmarking extends to quantum *networking* (File 15) and *sensing* (File 16), with their own metrics:

- **Networking (File 15):** entanglement-distribution rate, fidelity, distance; QKD key rate and secure distance — metrics specific to communication tasks.
- **Sensing (File 16):** sensitivity (e.g., T/√Hz for magnetometers), fractional frequency uncertainty (clocks), spatial resolution — metrics specific to measurement tasks, where quantum sensing has *realized* advantage (File 16).

These adjacent-technology benchmarks (networking, sensing) are more *mature* and *task-specific* than computing benchmarks, reflecting those fields' greater maturity (Files 15, 16). They illustrate that benchmarking is *task-specific* across quantum technologies — computing, networking, and sensing each have their own meaningful metrics, and no universal "quantum benchmark" spans them.

### 19. Reader's guide to benchmarks

For an analyst or engineer encountering any quantum benchmark:

1. **Identify what the metric measures** (count, composite, gate fidelity, application) and its blind spots (Sections 1–6).
2. **Ask whose metric it is** and which architecture it favors (Section 7).
3. **Check if it predicts your application** — composite metrics rarely do; run your own workload (Section 9).
4. **Verify independently** and check typical vs. best-case (calibration drift, Section 11).
5. **For fault tolerance, use logical metrics** (logical qubit count, logical error rate, Λ, File 9) — not physical count.
6. **For cross-paradigm/cross-technology comparison, use task-specific benchmarks** (Sections 17–18).
7. **Treat composite vendor metrics as directional indicators**, not engineering-grade comparisons (Section 8).

Applying this guide — identify the metric, note its bias, check application-relevance, verify independently, use logical metrics for fault tolerance, task-specific for cross-paradigm, and treat composites as directional — is the practical benchmarking skill. It equips the reader to cut through the contested benchmarking landscape and assess quantum-hardware capability honestly, the disciplined approach the database maintains.

### 20. Final takeaway

Quantum-computing benchmarking is a domain where marketing and engineering reality frequently diverge, and the disciplined analyst's essential skill is *skepticism toward single numbers*. No metric — raw count, Quantum Volume, #AQ, CLOPS, or even RB/XEB fidelity — fully captures how good a quantum computer is for a specific purpose, because usefulness is multidimensional (count, fidelity, connectivity, coherence, speed, error structure, and logical qubits/error rate for fault tolerance) and application-dependent. Vendor-originated composite metrics favor their originators' architectures (#AQ favors ions, CLOPS favors superconducting, QV favors connectivity), calibration drift makes even single-device benchmarks time-dependent, and cross-paradigm comparison (gate-model vs. annealing) resists common metrics. The only fully reliable approach is to benchmark the user's *own* target application directly, grounded in resource estimation (File 18) and — for fault tolerance — logical metrics (File 9), treating vendor composites as directional indicators to be verified independently. The field's evolution toward logical and application benchmarks (Files 9, 19) is the correct maturation, and the fault-tolerant-era metrics (logical qubit count, logical error rate, below-threshold scaling, File 9) are the meaningful ones going forward. This benchmarking skepticism — no single number, vendor metrics favor originators, benchmark your own application — is a core "honesty/due-diligence" discipline (alongside Files 14, 17, 19) essential to assessing quantum computing's genuine capabilities amid its pervasive marketing, and it is the analytical tool that turns contested benchmarks into honest hardware assessment.

*Cross-references: RB/XEB and error structure (File 2); modality specs and fidelities (Files 3–7); physical vs. logical qubits and logical metrics (Files 1, 9); resource estimation for application-feasibility (File 18); vendor metric-shifting (File 20); roadmap credibility and independent benchmarking (File 19); classical-comparison discipline (File 14); annealing benchmarking (File 17); networking and sensing benchmarks (Files 15, 16); enterprise application-benchmarking (File 24).*

### One-line summary

No single number captures how good a quantum computer is — raw qubit count ignores fidelity/connectivity and ≠ logical qubits; Quantum Volume, #AQ, and CLOPS are vendor-favorable composites; RB/XEB give architecture-neutral gate fidelities but hide error structure; and application benchmarks are most meaningful — so treat vendor composites as directional indicators, benchmark your *own* application, and use logical metrics (logical qubit count, logical error rate, below-threshold scaling, File 9) for the fault-tolerant era.

### Extended note: the benchmarking discipline in one principle

All of this file's benchmarking guidance reduces to one principle: *benchmark what you actually care about, and distrust proxies you don't control*. Vendor composite metrics (Quantum Volume, #AQ, CLOPS) are proxies designed by vendors, favoring their architectures (Section 7) and using random circuits unrepresentative of real algorithms (Section 8); raw qubit count is a proxy that ignores fidelity, connectivity, and the physical-vs-logical distinction (Section 1); even RB/XEB fidelities characterize *gates*, not *applications*, and hide error structure (Section 5). The one reliable approach is to run *your* target application (or a close structured proxy) on the candidate hardware and measure the achieved accuracy/performance (Section 9) — and, for fault tolerance, to track logical-qubit count and logical error rate (File 9) grounded in resource estimation (File 18). This application-direct discipline, treating vendor composites as directional shortlisting indicators verified independently and preferring logical metrics for the fault-tolerant era, is the benchmarking counterpart to the classical-comparison discipline (File 14) and the roadmap framework (File 19) — the evidence-based, self-benchmarking rigor that cuts through the field's contested, marketing-laden benchmarking landscape.

### A closing worked reminder

When a vendor announces a headline metric, run the mental checklist: *What does it measure and what are its blind spots? Whose metric is it and which architecture does it favor? Does it predict my application? Was it independently verified, and is it typical or best-case (calibration drift)?* A high Quantum Volume signals good count-connectivity-fidelity on random circuits (a genuine positive) but not application performance; a high #AQ flatters trapped-ion strengths; a high CLOPS flatters superconducting throughput; a high raw qubit count means little without fidelity, connectivity, and the physical-vs-logical context (Files 1, 9). For fault-tolerance progress, ignore physical count and track logical-qubit count, logical error rate, and below-threshold scaling (File 9). This disciplined reading — treating every composite as a directional, architecture-biased, application-agnostic indicator to be verified independently and supplemented by your own workload benchmarking (File 18) — is the practical skill that turns the field's contested, marketing-laden benchmarking landscape into honest hardware assessment, and it is a core due-diligence discipline alongside the classical-comparison rigor (File 14), NISQ-application skepticism (File 17), and roadmap-credibility framework (File 19) the database maintains throughout.
