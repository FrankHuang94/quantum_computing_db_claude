# Near-Term (NISQ) Applications — Honest Assessment of Current and Plausible Near-Term Value

> This file is a deliberately **skeptical, evidence-based** survey of near-term (NISQ-era) quantum-computing applications, distinguishing (a) demonstrated results with honest classical benchmarking, (b) theoretically-motivated but not-yet-useful applications, and (c) speculative/overhyped claims common in marketing. It cross-references the error-mitigation caveats (File 10), classical-simulation caveats (File 14), and algorithm caveats (File 13) throughout. It covers quantum chemistry, optimization (including D-Wave annealing), quantum machine learning, finance, pharma, and — crucially — a checklist for evaluating any quantum-advantage claim. It is one of the database's "honesty/due-diligence" throughline files.

---

## Part I — Framing

The purpose of this file is to cut through the pervasive hype around near-term quantum computing with an evidence-based assessment. The essential distinction (Files 10, 14):

- **(a) Demonstrated results with honest classical benchmarking:** real hardware results where the *best current* classical method was actually run for comparison and the quantum approach genuinely competed or won. These are *rare* in the NISQ era.
- **(b) Theoretically-motivated but not-yet-demonstrated-at-useful-scale:** applications with sound theoretical basis (e.g., quantum simulation) but where current hardware is too small/noisy to exceed classical methods — the honest majority of "near-term applications."
- **(c) Speculative/overhyped claims:** marketing-driven claims of near-term advantage lacking rigorous classical comparison — unfortunately common.

The recurring reality (Files 10, 14): **the low-complexity circuits NISQ hardware and error mitigation can handle are often exactly the circuits advanced classical methods (tensor networks, File 14) can also handle** — so genuine, benchmarked NISQ advantage on a *useful* problem remains elusive. This is not pessimism but honest calibration: NISQ-era value is real but lies mostly in *capability-building and scientific exploration*, not in demonstrated practical advantage over the best classical methods.

---

## Part II — Application-by-Application Assessment

### 1. Quantum chemistry and materials science

- **The theoretical promise:** simulating molecular electronic structure is a natural quantum-computing application (exponential separation, File 13) — the most defensible application class.
- **Current NISQ reality:** demonstrations are limited to *small molecules* (H₂, LiH, BeH₂, small H-chains) or specific lattice models via VQE (File 13), generally *not exceeding* what classical methods (DFT, coupled-cluster, DMRG) already handle *well* for the same system sizes. Noise, barren plateaus (File 13), and measurement overhead (File 13) limit NISQ chemistry to sizes classical methods handle easily.
- **Honest value proposition:** the near-term value is in *developing and validating* algorithms, software, and techniques (fermion mappings, ansätze, error mitigation) *in preparation for* fault-tolerant-era scale-up — *not* in near-term chemistry results exceeding classical methods. The genuinely-useful quantum chemistry (FeMoCo, complex catalysts, File 13) requires fault tolerance (File 18) and is years away.

### 2. Optimization: QAOA and quantum annealing

- **D-Wave quantum annealing:** a fundamentally different, non-gate-model paradigm (adiabatic evolution toward an Ising ground state, File 2, Section 38), and the **longest-commercially-deployed "quantum computing"** — D-Wave's Advantage-series systems have thousands of qubits and paying customers using them for production-style optimization. This deserves explicit mention as the most commercially-mature quantum-computing hardware.
- **The contested track record:** despite D-Wave's commercial deployment, whether quantum annealing demonstrates genuine *advantage* over classical optimization heuristics (simulated annealing, specialized solvers, tensor-network methods) for practically-relevant problems is **contested** — many studies find classical methods match or beat annealing on the same problems. Gate-model **QAOA** (File 13) is similarly contested.
- **Honest summary:** **no broadly-accepted, reproducible, practically-significant optimization advantage has been demonstrated by any quantum platform** (annealing or gate-model) as of current literature, despite substantial investment and pilot programs (financial portfolio optimization, logistics/routing with various banks and logistics companies — frequently in press releases, rarely independently benchmarked against the best classical methods). Optimization is a heavily-marketed but *unproven* quantum application — a prime target for the skeptical assessment this file models.

### 3. Quantum machine learning

- Per File 13's caveats: current QML demonstrations (variational classifiers, quantum kernels, quantum neural networks) are generally *small-scale proof-of-concept* rather than benchmarked advantage, and face barren plateaus (File 13) and dequantization (Files 13, 25).
- **Honest framing:** QML is more credibly a *research direction* than a deployable near-term product. The credible near-term promise, where any exists, is in *learning about quantum data/systems* (a natural fit) rather than generic classical-data ML, and in narrow structured problems — not generic "quantum neural networks" replacing classical deep learning. QML's popularity (riding the AI boom) outruns its demonstrated results.

### 4. Financial services

- **Option pricing via quantum amplitude estimation (QAE):** one of the more *rigorously-grounded* near-term financial applications — QAE provides a theoretically-grounded *quadratic* speedup for Monte-Carlo-style estimation (derivatives pricing, risk analysis; File 13, Section 31). But it is *quadratic* (facing the overhead/crossover problem, File 13, Section 26) and requires more qubits/lower error than currently available for practical-scale problems, so it is a *future* (likely early-fault-tolerant) rather than near-term application.
- **Portfolio optimization:** largely QAOA/annealing-based (Section 2), subject to the same contested-advantage caveats. Frequent pilot programs with banks, rarely independently benchmarked.
- **Honest summary:** finance is an active area of quantum pilots, but the rigorously-grounded application (QAE) is quadratic and future, while the near-term pilots (optimization) have unproven advantage. Financial quantum-advantage claims warrant the same skepticism as optimization generally.

### 5. Pharma and drug discovery

- Similar caveats to quantum chemistry (Section 1): the promise (simulating drug-target interactions, molecular properties) is real but requires fault tolerance for useful sizes; current NISQ demonstrations are small and don't exceed classical methods.
- **Honest framing:** frequent pharma-quantum partnerships should be read as *long-term R&D bets and capability-building*, not near-term deployment of quantum-derived drug candidates. No quantum-derived drug has reached clinical relevance, and the useful chemistry requires fault tolerance (File 18). The partnerships build capability and hedge future risk, but near-term drug-discovery advantage is not demonstrated.

---

## Part III — The Evaluation Checklist and Meta-Assessment

### 6. Pattern recognition across hype cycles

The most valuable skill for evaluating *any* quantum-advantage claim (across chemistry, optimization, ML, finance) is a **checklist** of skeptical questions — the meta-level tool this file provides:

1. **What is the best known *classical* algorithm for this *exact* problem instance?** Not the naive baseline, but the state-of-the-art (tensor networks, specialized solvers, GPU methods, File 14).
2. **Has that classical algorithm actually been *run* for comparison, or just *cited* as an asymptotic-complexity argument?** Asymptotic arguments ("classically exponential") are frequently overturned by better classical methods on the *specific* instance (Files 10, 14).
3. **What is the actual problem *size*, and how does it compare to the classical-quantum *crossover* point?** A demonstration at a size classical methods handle easily is not evidence of advantage, however "quantum" it is.
4. **Is the result *reproduced/verified independently*?** Vendor demonstrations warrant independent replication (File 22).
5. **Is the task *useful* or *contrived*?** Random-circuit/boson sampling (Files 6, 14) is not a useful computation; "advantage" on a useless task is a physics demonstration.
6. **What error mitigation/correction was used, and what is its overhead/scaling?** (File 10 — mitigation has exponential sampling cost).
7. **Does the claim account for the full stack** (state preparation, readout, compilation overhead, File 12) — or just the "quantum" part?

Applying this checklist separates genuine, benchmarked advantage (rare in NISQ) from the theoretically-motivated (the honest majority) and the overhyped (unfortunately common). It is the practical embodiment of the classical-comparison discipline (File 14), the algorithm skepticism (File 13), and the benchmarking rigor (File 22).

### 7. Where near-term value genuinely lies

Despite the skepticism, NISQ-era quantum computing has *genuine* near-term value — just not where marketing claims:

- **Capability-building:** developing algorithms, software, error mitigation, and expertise *in preparation for* fault tolerance — real value even without near-term advantage.
- **Scientific exploration:** studying quantum many-body physics via analog simulation (neutral atoms, ions, Files 4, 5), where devices explore regimes of scientific interest (quantum phase transitions, scrambling) — genuine science, with the honest caveat that classical methods are a moving target (File 14).
- **Benchmarking and hardware development:** NISQ demonstrations drive hardware and classical-algorithm progress.
- **Specific structured problems:** narrow cases where a quantum approach genuinely fits, evaluated case-by-case with the checklist (Section 6).
- **Quantum-data tasks:** learning about quantum states/processes (a natural quantum fit).

The honest value proposition: NISQ is a *bridge and a laboratory*, building capability and doing exploratory science, *not* a source of demonstrated practical advantage over the best classical methods. This framing — real value in capability-building and exploration, not in demonstrated advantage — is the honest calibration that separates informed assessment from both hype and dismissal.

### 8. The analog-simulation exception

The one near-term application with the strongest claim to genuine scientific value is **analog quantum simulation** (Files 4, 5): programmable quantum systems (neutral-atom Rydberg arrays, trapped-ion spin chains) directly instantiating many-body Hamiltonians and observing their dynamics — used in *published scientific studies* of quantum phase transitions, many-body scars, and lattice models. This is the clearest case of quantum devices being used *today* for computations that are at least hard classically (large 2D quantum dynamics, File 14) — with the honest caveat (File 14) that "hard classically" is a moving target requiring case-by-case benchmarking. Analog simulation is genuine science with genuine (if contested) quantum-hardness, and it is the near-term application least vulnerable to the checklist's skepticism — though even here, the disciplined question (has the best classical tensor-network method been run for this specific instance?) applies.

### 9. Summary

Near-term (NISQ) quantum computing is, honestly assessed, a domain of *capability-building and scientific exploration* rather than demonstrated practical advantage. Quantum chemistry, optimization (including D-Wave's commercially-deployed but advantage-contested annealing), quantum machine learning, finance, and pharma all show the same pattern: sound theoretical promise (for chemistry/simulation) or heavy marketing (for optimization/ML/finance), but *no broadly-accepted, reproducible, practically-significant advantage* over the best classical methods, because the low-complexity circuits NISQ handles are often classically tractable too (Files 10, 14). The rigorously-grounded applications (quantum chemistry, amplitude-estimation finance) require *fault tolerance* for useful sizes (File 18); the near-term pilots (optimization, ML) have unproven advantage. The genuine near-term value lies in capability-building (algorithms, software, expertise for the fault-tolerant future), scientific exploration (analog simulation of many-body physics — the strongest case), and hardware/algorithm development — not in practical advantage. The essential skill is the evaluation checklist (Section 6): demand the best-run classical baseline, assess problem size vs. crossover, distinguish useful from contrived tasks, require independent reproduction, and account for the full stack and error-mitigation overhead. This disciplined, evidence-based, classically-benchmarked assessment — separating demonstrated from theoretical from overhyped — is the honest calibration this file contributes, and it is the throughline connecting the algorithm caveats (File 13), classical-simulation discipline (File 14), mitigation limits (File 10), and benchmarking rigor (File 22) into a realistic picture of what near-term quantum computing can and cannot do.

*Cross-references: algorithm speedup assessment and contested QAOA/QML advantage (File 13); classical-simulation moving-target and the low-complexity vise (File 14); error-mitigation limits and the IBM utility debate (File 10); D-Wave annealing roadmap (File 19); the resource estimation showing useful chemistry needs fault tolerance (File 18); analog simulation on neutral atoms and ions (Files 4, 5); benchmarking rigor and the checklist (File 22); the business/market hype dynamics (File 24); quantum sensing as realized (contrasting) advantage (File 16).*

---

## Part IV — Case Studies, D-Wave in Depth, and Worked Checklist Applications

### 10. The IBM utility case study

The **IBM 2023 "utility" demonstration** (File 10) is the definitive NISQ case study, illustrating every theme of this file. IBM ran a kicked-Ising simulation on 127 qubits with error mitigation, claiming results beyond brute-force classical simulation. Applying the checklist (Section 6):

- *Best classical algorithm?* Not brute-force state-vector (the naive baseline IBM implicitly compared against) but tensor networks and sparse Pauli dynamics (File 14).
- *Actually run?* Within weeks, multiple groups *ran* these classical methods and matched IBM's results (File 10).
- *Problem size vs. crossover?* The circuit's limited entanglement kept it in the classically-tractable regime (File 14).
- *Useful task?* The kicked-Ising model is physics-relevant but the specific demonstration was more a benchmark than a useful computation.
- *Reproduced?* The classical rebuttals were independent.

Verdict: IBM's results were *correct* (a genuine demonstration of error mitigation at scale), but the "beyond classical" framing did not survive scrutiny — the classic pattern of a NISQ advantage claim overturned by better-run classical methods. This case study is the template for evaluating *any* NISQ advantage claim, and it embodies the low-complexity vise (Files 10, 14): the circuits mitigation handles are those classical methods handle too.

### 11. D-Wave and quantum annealing in depth

D-Wave deserves detailed treatment as the most commercially-mature and most-debated quantum-computing platform:

- **Technology:** quantum annealing (File 2, Section 38) — a non-gate-model paradigm evolving a system of thousands of superconducting flux qubits toward the ground state of a programmable Ising Hamiltonian encoding an optimization problem. Advantage-series systems have >5000 qubits (though with limited connectivity — the Pegasus/Zephyr graph — requiring minor-embedding that reduces effective problem size).
- **Commercial reality:** D-Wave has a multi-year track record of *paying commercial customers* (Volkswagen, Denso, financial firms, and others) using current hardware for production-style optimization workloads — genuinely the most commercially-deployed quantum computing (Files 19, 20).
- **The advantage debate:** whether annealing provides genuine *advantage* is contested. Early "quantum speedup" claims were repeatedly challenged: classical simulated annealing, specialized solvers, and even classical algorithms *inspired by* analyzing D-Wave's dynamics often match or beat it on the same problems. D-Wave has published claims of advantage on specific *quantum-simulation* tasks (using the annealer as a quantum simulator rather than optimizer, where the advantage claim is more defensible), but broad *optimization* advantage remains unproven.
- **Strategic pivot:** notably, D-Wave has recently begun *also* pursuing gate-model fault-tolerant research (File 19), reflecting industry consensus pressure toward gate-model universality — a significant move for a company historically defined by its annealing-only approach.

D-Wave exemplifies the tension between *commercial deployment* (real, with paying customers) and *demonstrated advantage* (contested) — a distinction crucial for honest assessment: commercial use does not equal proven advantage, and D-Wave's customers use it as a *heuristic tool* (sometimes helpful, sometimes not) rather than a proven-superior solver. This nuance — commercial traction alongside contested advantage — is central to interpreting D-Wave's role (Files 19, 20).

### 12. Worked checklist: a hypothetical logistics claim

Apply the checklist (Section 6) to a hypothetical: *"Company X used a quantum computer to optimize delivery routes, reducing costs 15%."*

1. *Best classical algorithm?* State-of-the-art vehicle-routing solvers (Concorde-style, OR-Tools, specialized heuristics) are extremely good — was the 15% reduction relative to *these*, or to the company's *old* method?
2. *Actually run?* Was the best classical solver run on the same instance, or is "quantum" credited for a reduction a good classical solver would also achieve?
3. *Problem size?* Small enough for classical exact solvers, or genuinely large?
4. *Useful/reproduced?* Independent verification?

Almost invariably, such claims reduce to: a quantum (or quantum-inspired) method beat the company's *previous, suboptimal* approach — not the best classical method. The "15% reduction" is real but the *quantum* attribution is unjustified (a good classical solver would do as well or better). This is the single most common pattern in optimization-advantage marketing (Section 2), and the checklist exposes it: the comparison is against a weak baseline, not the best classical method. Recognizing this pattern is essential for anyone evaluating enterprise quantum-optimization pilots (File 24).

### 13. The hype-cycle dynamics

NISQ applications exist within a broader **hype cycle** (File 24): media coverage, investor enthusiasm, and vendor marketing frequently outrun demonstrated results, followed by disillusionment when advantage fails to materialize on schedule. The healthy response is neither uncritical enthusiasm nor blanket dismissal, but *evidence-based, classically-benchmarked assessment* (Section 6) — crediting genuine achievements (analog simulation science, capability-building, the occasional benchmarked result) while skeptically scrutinizing advantage claims. The pattern recognition this file builds — identifying weak-baseline comparisons, asymptotic-only arguments, contrived tasks, and unreproduced claims — is the antidote to the hype cycle, and it is a skill that transfers across every application domain and every new claim an engineer or analyst encounters. The field's credibility (File 24) depends on the community holding claims to this standard, and the disillusionment phases of past hype cycles (in quantum and other emerging technologies) are largely the consequence of claims that would not have survived the checklist.

### 14. What would change the assessment

To be clear about what evidence *would* update this skeptical assessment toward demonstrated NISQ advantage:

- A *useful* computation (not contrived sampling) where the *best current* classical method was *run* and genuinely beaten, at a *practically-relevant* size, *independently reproduced*.
- This has *not* yet happened for a useful problem in the NISQ era (analog simulation comes closest, with the moving-target caveat, File 14).

Conversely, the assessment would tilt *more* skeptical if (as has repeatedly happened) more NISQ advantage claims are overturned by classical methods. The honest current state: near-term quantum computing is a bridge and laboratory (capability-building, exploration), not a source of demonstrated practical advantage — and this will remain so until either a benchmarked NISQ advantage on a useful problem materializes (not yet) or fault tolerance (File 18) unlocks the proven exponential advantages (Shor, useful simulation; File 13). Tracking which of these happens — and holding all claims to the checklist standard — is the disciplined posture this file models, and it is the honest foundation for the roadmap-credibility (File 19) and market (File 24) assessments that follow.

---

## Part V — Sector Summary Table, Additional Domains, and Reader's Guide

### 15. Honest sector-by-sector summary

| Sector | Theoretical basis | NISQ demonstrated advantage? | Honest near-term status |
|---|---|---|---|
| Quantum chemistry | Strong (exponential, File 13) | No — small molecules, classical handles them | Capability-building; useful needs fault tolerance |
| Materials science | Strong (simulation) | No | Same as chemistry; analog simulation closest |
| Optimization (QAOA) | Weak/contested (File 13) | No | Heavily marketed, unproven |
| Optimization (annealing/D-Wave) | Contested | No (commercially used, not proven-advantaged) | Real customers, contested advantage |
| Quantum ML | Weak/mostly none | No | Research direction, overhyped |
| Finance (amplitude estimation) | Moderate (quadratic) | No — needs more qubits/fidelity | Future (early-FT), not near-term |
| Finance (portfolio opt.) | Weak (QAOA-based) | No | Pilots, unproven |
| Pharma/drug discovery | Strong (chemistry) but far | No | Long-term R&D bets |
| Analog quantum simulation | Strong (native) | Closest to yes (contested) | Genuine science; strongest near-term case |
| Logistics/routing | Weak (optimization) | No | Weak-baseline comparisons common |

The table's overwhelming pattern: *no sector* shows broadly-accepted, reproducible, practically-significant NISQ advantage on a useful problem — the honest bottom line. Analog simulation is the closest (genuine science, contested hardness), and the chemistry/simulation applications are theoretically strongest but require fault tolerance for useful sizes. Optimization, ML, and finance pilots are heavily marketed but unproven.

### 16. Additional domains and their status

- **Energy/materials for batteries and catalysts:** subset of quantum chemistry (Section 1) — real promise, fault-tolerant-era.
- **Cryptography (breaking):** Shor requires fault tolerance (File 13) — *not* NISQ; the "harvest now, decrypt later" threat (File 21) drives PQC migration regardless of NISQ.
- **Weather/climate modeling, fluid dynamics:** proposed via quantum differential-equation solvers (File 13) — same I/O-model caveats as HHL, far from useful.
- **Traffic/supply-chain optimization:** optimization caveats (Section 2, 12) — weak-baseline comparisons common.
- **Quantum-enhanced sensing** (File 16): the one *adjacent* area with realized advantage — but that is *sensing*, not *computing* (a distinction worth preserving, File 16).

### 17. Reader's guide

For an analyst or engineer assessing a NISQ application claim:

1. **Apply the checklist** (Section 6) — best-run classical baseline, problem size vs. crossover, useful vs. contrived, reproduced, full-stack accounting.
2. **Identify the comparison baseline** — is "quantum advantage" measured against the *best* classical method or a *weak* one (the company's old approach, or a naive baseline)? Weak-baseline comparisons are the most common overclaim (Section 12).
3. **Check the maturity spectrum** — is this capability-building/exploration (real near-term value) or a claimed practical advantage (scrutinize hard)?
4. **Distinguish commercial use from proven advantage** — D-Wave has customers but contested advantage (Section 11); commercial traction ≠ demonstrated superiority.
5. **Note the fault-tolerance dependency** — the rigorously-grounded applications (chemistry, useful simulation, factoring) need fault tolerance (File 18); near-term versions have unproven advantage.
6. **Track the moving target** — classical methods improve (File 14); today's advantage may be tomorrow's classical result.

### 18. Final summary

Near-term (NISQ) quantum computing, honestly assessed, delivers *no broadly-accepted, reproducible, practically-significant advantage* over the best classical methods on a *useful* problem — across chemistry, optimization, ML, finance, and pharma — because the low-complexity circuits NISQ handles are often classically tractable (Files 10, 14), the theoretically-strong applications (chemistry, useful simulation) require fault tolerance (File 18), and the near-term pilots (optimization, ML) have unproven, often weak-baseline-inflated advantage. The genuine near-term value is real but lies in *capability-building* (algorithms, software, expertise for the fault-tolerant future), *scientific exploration* (analog simulation of many-body physics — the strongest case), and *hardware/algorithm development* — not in demonstrated practical advantage. The essential skill is the evaluation checklist: demand the best-run classical baseline, assess problem size versus crossover, distinguish useful from contrived tasks and commercial use from proven advantage, require independent reproduction, and track the moving classical target. This disciplined, evidence-based, classically-benchmarked assessment — the antidote to the hype cycle — is the honest calibration this file contributes, connecting the algorithm caveats (File 13), classical-simulation discipline (File 14), mitigation limits (File 10), roadmap-credibility methodology (File 19), and market-reality assessment (File 24) into a realistic picture: near-term quantum computing is a valuable bridge and laboratory, not yet a source of practical advantage, with the transformative proven advantages awaiting fault tolerance.

*Cross-references: contested QAOA/QML advantage and the algorithm speedup landscape (File 13); the classical-simulation moving target and low-complexity vise (File 14); error-mitigation limits and the IBM utility case (File 10); D-Wave and other roadmaps (File 19); resource estimation showing useful applications need fault tolerance (File 18); analog simulation on atoms/ions (Files 4, 5); benchmarking and the checklist (File 22); market hype dynamics and enterprise pilots (File 24); quantum sensing as realized adjacent advantage (File 16).*
