# Quantum Error Mitigation — NISQ-Era Techniques Without Full Error Correction

> This file covers **error mitigation** — the set of techniques that improve NISQ-era results *without* encoding qubits or actively correcting errors, by classically post-processing many noisy circuit executions to estimate noise-free expectation values. Mitigation is fundamentally distinct from error *correction* (File 9): it does not suppress logical error exponentially, but rather trades an *exponentially growing sampling cost* for reduced bias — making it a NISQ-only bridge, not a path to fault tolerance. This file develops zero-noise extrapolation, probabilistic error cancellation, measurement-error mitigation, dynamical decoupling, and the IBM utility-scale demonstration and its contested classical rebuttals. It complements Files 9 (correction), 14 (classical simulation), and 17 (NISQ applications).

---

## Part I — Mitigation vs. Correction

### 1. The fundamental distinction

**Error correction** (File 9) encodes each logical qubit redundantly across many physical qubits, measures error syndromes, and actively corrects *during* the computation — suppressing logical error *exponentially* in code distance, enabling arbitrarily long computation at the cost of large qubit overhead and below-threshold fidelity. **Error mitigation** does none of this. It runs *unencoded* noisy circuits (using all qubits as data qubits, no overhead) many times, and applies *classical post-processing* to the collection of noisy results to estimate what the noise-free **expectation value** would have been. The critical trade-off:

- Mitigation has **no qubit overhead** and works on *today's* hardware — its great advantage.
- But it does **not** exponentially suppress error. Instead, the classical post-processing amplifies statistical variance, so achieving a given accuracy requires a number of circuit repetitions (shots) that grows **exponentially with the circuit's total noise** (number of noisy gates × error rate). This **exponential sampling overhead** is the fundamental limit: as circuits get larger/deeper, the shot count needed becomes astronomically large, capping the useful circuit size.

So mitigation is a **NISQ-era-only strategy** — it makes noisy near-term devices more useful for *modest* circuits, but it cannot scale to the long computations (Shor, large chemistry) that only error correction enables. Confusing mitigation with a path to fault tolerance is a common and important error (Files 9, 17). Mitigation buys near-term utility; correction buys the fault-tolerant future; they are complementary, and the field uses mitigation to bridge the years until hardware reaches the fidelity and scale that correction requires.

### 2. What mitigation targets: expectation values

Mitigation techniques generally estimate **expectation values** ⟨O⟩ = ⟨ψ|O|ψ⟩ of observables (e.g., the energy ⟨H⟩ in VQE, File 13), not full output distributions or individual bitstrings. This is the right target because most near-term algorithms (VQE, QAOA, quantum simulation) need expectation values, and expectation values are amenable to the statistical extrapolation/cancellation that mitigation performs. Mitigation does not "fix" a single run; it corrects the *statistical estimate* built from many runs.

---

## Part II — The Core Techniques

### 3. Zero-noise extrapolation (ZNE)

**ZNE** is the most widely used mitigation technique. The idea: deliberately *increase* the noise to several controlled levels, measure the (biased) expectation value at each, and *extrapolate back* to the zero-noise limit.

- **Noise amplification:** scale the noise by a factor λ ≥ 1 via **gate folding** (replacing each gate G with G G† G, which equals G but with 3× the noise exposure) or **pulse stretching** (lengthening gate pulses to increase decoherence exposure). This gives biased expectation values at λ = 1, 1.5, 2, 3, etc.
- **Extrapolation:** fit the measured ⟨O⟩(λ) to a model (linear, polynomial, or **Richardson extrapolation**; exponential) and evaluate at λ = 0 — the estimated noise-free value.
- **Implementation:** built into Qiskit Runtime's Estimator primitive and the **Mitiq** open-source library.

ZNE is popular because it is simple, requires no detailed noise characterization, and often gives substantial improvement. Its limitations: the extrapolation is a *model assumption* (if the true ⟨O⟩(λ) doesn't match the fit model, the extrapolation is biased), and amplifying noise increases variance, requiring more shots. ZNE was central to IBM's 2023 utility demonstration (Section 8).

### 4. Probabilistic error cancellation (PEC)

**PEC** is more rigorous and more expensive. It **characterizes the noise channel** (via gate-set tomography or cycle benchmarking, File 2), then represents the *inverse* of the noise map as a **quasi-probability distribution** over *implementable* operations. Because the inverse of a physical (CPTP) noise channel is generally *not* itself a physical channel, it is decomposed as a signed combination Σᵢ cᵢ Oᵢ of physical operations with real coefficients cᵢ (some negative). One then **samples** operations according to |cᵢ|, applies them, and weights the results by sign(cᵢ), so that the *average* statistically cancels the noise — yielding an *unbiased* estimate of the noise-free expectation value.

- **Advantage:** unbiased (unlike ZNE's model-dependent extrapolation), given an accurate noise model.
- **Cost:** the sampling overhead grows **exponentially** with the total noise (the "cost" of the quasi-probability decomposition, γ, raised to the number of noisy gates: γ^(number of gates)), and PEC requires accurate noise characterization. This exponential cost is the concrete manifestation of mitigation's fundamental limit (Section 1).

PEC and ZNE are often combined (PEC's rigor where affordable, ZNE's cheapness elsewhere), and IBM's Qiskit Runtime offers both.

### 5. Learning-based mitigation (Clifford data regression)

**Clifford Data Regression (CDR)** and related methods exploit that **Clifford circuits are classically simulable** (Gottesman–Knill, File 2, 14): construct circuits *near* the target circuit but made entirely of Clifford gates (whose exact noise-free values are classically computable), run them on the noisy hardware, and *learn a correction function* (a regression) mapping noisy → true values from these Clifford training points. Apply the learned correction to the noisy result of the *actual* (non-Clifford) target circuit. This uses the classical simulability of Clifford circuits as a "training set" for noise correction — an elegant bridge between the classical-simulation boundary (File 14) and mitigation.

### 6. Measurement-error mitigation

Readout is a major, and separately-correctable, error source. **Measurement-error mitigation** characterizes the **readout confusion matrix** — the probabilities of misreading |0⟩ as |1⟩ and vice versa for each qubit (measured by preparing known basis states and observing the readout statistics) — and **inverts** it classically on the measured bitstring distribution to recover the true distribution. For n qubits, the full confusion matrix is 2ⁿ×2ⁿ (intractable for large n), so scalable variants assume the readout errors are *tensored* (independent per qubit) or *locally correlated* (correcting crosstalk between neighboring qubits' readout). Measurement-error mitigation is cheap, effective, and routinely applied (built into Qiskit and other stacks); it addresses readout errors specifically, complementing the gate-error mitigation of ZNE/PEC.

### 7. Dynamical decoupling

**Dynamical decoupling (DD)** (File 2, Section 26; File 8) is a *hardware-level* mitigation applied *during* the circuit: inserting sequences of pulses (XY4, CPMG, XY8, robust variants like KDD) on *idle* qubits to refocus and average out slow (low-frequency) dephasing noise, extending effective idle coherence. Unlike the post-processing techniques above, DD acts in real time during execution and costs no extra shots — it simply protects idle qubits. Compilers insert DD automatically during idle windows (File 8), often improving deep-circuit fidelity by tens of percent at no sampling cost. DD is also integral *within* error-correction cycles (File 9), protecting data qubits while ancillas are measured. It is the cheapest, most broadly applicable mitigation and is nearly always worth applying.

### 8. Symmetry verification and randomized compiling

Two further techniques: **Symmetry verification** (File 2, Section 40) uses a conserved quantity of the simulated system (particle number, parity) as a free error check — discarding (post-selecting away) any run whose measured state violates the symmetry, since such runs must contain errors. **Randomized compiling / Pauli twirling** (File 2, Section 34; File 8) converts *coherent* errors into *stochastic* Pauli errors by randomizing the Pauli frame around each gate — not reducing the error rate but making the error *benign and well-characterized* (stochastic errors accumulate linearly and are easier for ZNE/PEC to model than coherent errors). Twirling is often a *prerequisite* that makes the other mitigation techniques work well, because ZNE and PEC assume a noise structure that twirling enforces.

---

## Part III — The Utility Debate and Assessment

### 9. IBM's utility-scale demonstration

The flagship mitigation result is **IBM's 2023 "utility before fault tolerance" paper (Kim et al., Nature 618, 500 (2023); arXiv:2306.14887)**: using a **127-qubit Eagle** processor (File 3) with **ZNE-style error mitigation** (specifically a form of PEC/ZNE with learned noise models), IBM simulated the dynamics of a **kicked transverse-field Ising model** on all 127 qubits at circuit depths claimed to be **beyond the reach of brute-force classical simulation**, obtaining expectation values that agreed with exact results in classically-verifiable regimes and extended into classically-hard regimes. IBM framed this as demonstrating *quantum utility* — reliable results from a NISQ device at a scale where naive classical methods fail — even before fault tolerance.

### 10. The classical rebuttals

Within *weeks to months*, multiple groups produced **classical simulations matching or exceeding** IBM's results using improved methods — most notably **tensor-network** approaches (matrix product states / belief-propagation tensor networks) and sparse Pauli-dynamics methods (Tindall et al., and others) that exploited the specific circuit's limited entanglement growth (File 14). These classical methods reproduced the quantum results on a laptop or modest cluster, showing that the specific kicked-Ising circuit was *not* actually beyond classical reach. This exemplifies the **"moving target"** dynamic (Files 1, 14): a quantum advantage/utility claim is only as strong as the best classical method *at the time of scrutiny*, and classical methods improved rapidly in response.

### 11. What the debate teaches

The IBM utility episode is an instructive case study, not a failure — and the honest lessons are:

- **Mitigation genuinely works** for extracting reliable expectation values from NISQ hardware at nontrivial scale — IBM's results were *correct* (they matched exact and classical results), a real demonstration of mitigation's near-term value.
- **But "beyond classical" claims for mitigated NISQ circuits are contestable and often overturned**, because the circuits that mitigation can handle (limited depth, limited entanglement) are frequently exactly the circuits that advanced classical methods (tensor networks, File 14) can also handle. There is a deep tension: mitigation works best on *low-complexity* circuits, which are also the circuits most amenable to classical simulation.
- **The right posture** (Files 14, 17, 22): treat any "quantum utility/advantage" claim for a mitigated NISQ circuit as *provisional*, demanding a *specific, run* classical comparison against the *best* available method (not an asymptotic argument), and expect classical methods to improve in response. This disciplined skepticism is the throughline of Files 14, 17, and 22.

### 12. Assessment: mitigation's role and limits

Error mitigation is a valuable, actively-used NISQ-era tool: it improves real results today with no qubit overhead, is essential for current VQE/QAOA/simulation experiments (File 13, 17), and is deeply integrated into cloud stacks (Qiskit Runtime primitives, Mitiq). Its fundamental limit — **exponential sampling overhead in circuit noise** — means it cannot bridge to the long, complex computations that only fault-tolerant error correction (File 9) enables. Mitigation and correction are therefore complementary phases: mitigation extracts what value it can from noisy near-term hardware while the field works toward the below-threshold fidelity and qubit counts that make correction viable. The strategic honest framing: **mitigation is how you get modest near-term value; correction is how you get the transformative long-term value; and no amount of mitigation substitutes for correction on hard problems.** Reading mitigated-NISQ advantage claims with the classical-comparison discipline of Section 11 is essential to separating genuine near-term utility from over-claimed "quantum advantage."

*Cross-references: error correction as the scalable alternative (File 9); Clifford simulability underlying CDR and the classical-comparison discipline (File 14); dynamical decoupling and randomized compiling in compilation (File 8); coherent/stochastic error distinction and symmetry verification (File 2); NISQ applications relying on mitigation (File 17); benchmarking and the moving-target dynamic (File 22); the utility-vs-supremacy definitions (File 1).*

---

## Part IV — Worked Examples, Depth, and Glossary

### 13. Worked example: the exponential sampling wall

To make mitigation's fundamental limit concrete: suppose a circuit has G noisy gates each with error rate ε, so the total expected number of errors is μ = Gε. Probabilistic error cancellation's sampling overhead scales as γ² where γ ≈ e^{2μ} (the quasi-probability "cost" compounds per gate). For a circuit with G = 1000 gates at ε = 10⁻² (μ = 10), the overhead factor is roughly e^{2×10} ≈ e²⁰ ≈ 5×10⁸ — meaning ~10⁸–10⁹× more shots are needed than for a noiseless estimate of the same accuracy. Double the circuit to G = 2000 (μ = 20) and the overhead becomes e⁴⁰ ≈ 10¹⁷ — utterly intractable. This is the **exponential sampling wall**: mitigation's cost grows exponentially with the *total noise* (gates × error rate), so it works only while μ stays modest (small circuits, low error rates). Improving hardware fidelity (lower ε) directly extends mitigation's reach (smaller μ for the same circuit), which is why mitigation and fidelity improvement go hand in hand in the NISQ era — but no fidelity short of fault-tolerant encoding lets mitigation handle the deep circuits (millions of gates) that useful algorithms require. This calculation is the quantitative heart of why mitigation is a NISQ-only bridge (Section 1).

### 14. Worked example: measurement-error mitigation

Consider 2 qubits each with readout error: P(read 1 | prepared 0) = 3%, P(read 0 | prepared 1) = 5%. The single-qubit confusion matrix is M = [[0.97, 0.05],[0.03, 0.95]]. For 2 qubits (assuming independence), the 4×4 confusion matrix is M⊗M. Given a measured distribution p_measured over {00,01,10,11}, the corrected estimate is p_true ≈ (M⊗M)⁻¹ p_measured. The inversion redistributes the measured counts to undo the readout mixing — e.g., some counts measured as "01" are attributed back to "00" or "11" based on the error rates. The catch: the inverse matrix can produce *negative* (unphysical) probabilities, handled by constrained least-squares or maximum-likelihood reconstruction. For large n, the full 2ⁿ matrix is intractable, so one assumes tensored (independent) or locally-correlated readout, correcting each qubit (or small qubit group) separately. Measurement-error mitigation is cheap and effective and is almost always applied — a low-hanging-fruit improvement that addresses readout, one of the largest NISQ error sources (Files 3, 22).

### 15. Worked example: ZNE extrapolation

Suppose measuring an observable at noise scale factors λ = 1, 2, 3 gives ⟨O⟩ = 0.62, 0.48, 0.37. A **linear** extrapolation fits ⟨O⟩(λ) ≈ a + bλ; least-squares gives b ≈ −0.125, a ≈ 0.74, so the zero-noise estimate is ⟨O⟩(0) ≈ 0.74 — a substantial correction from the λ=1 value of 0.62 toward the (hypothetical) true value. A **Richardson** (higher-order polynomial) extrapolation through all three points would give a different (possibly better or worse) estimate depending on whether the true noise dependence is linear. The *risk*: if the true ⟨O⟩(λ) is, say, exponential rather than linear, the linear fit is biased — ZNE's accuracy depends on the extrapolation model matching reality, which is why ZNE is a *heuristic* (often good, not guaranteed) whereas PEC is *unbiased* (given an accurate noise model) but more expensive. Practitioners often try multiple extrapolation models and compare, and use ZNE where its assumptions are reasonable and PEC where rigor is needed. This model-dependence is ZNE's key caveat.

### 16. How the techniques stack

In practice, mitigation techniques are *layered*, each addressing a different error source:

1. **Dynamical decoupling** (Section 7) protects idle qubits during the circuit — hardware-level, no shot cost.
2. **Randomized compiling / Pauli twirling** (Section 8) converts coherent errors to stochastic Pauli errors — making the noise well-behaved for the post-processing steps.
3. **Measurement-error mitigation** (Section 6) corrects readout errors classically after measurement.
4. **ZNE or PEC** (Sections 3–4) corrects the residual gate errors via extrapolation or quasi-probability cancellation.
5. **Symmetry verification / post-selection** (Section 8) discards runs violating known conservation laws.

This layered "mitigation stack" is what modern NISQ workflows (Qiskit Runtime primitives, Mitiq) apply, and IBM's utility demonstration (Section 9) used several layers together. Each layer costs something (shots, characterization, or discarded runs), and the art is applying enough to extract reliable results without the combined sampling overhead becoming prohibitive (Section 13).

### 17. Mitigation, correction, and the transition

A forward-looking point: mitigation and correction are not entirely separate — the transition between them is a continuum. **Early fault-tolerant** devices (a handful of logical qubits, File 9) will still have residual logical errors, and mitigation techniques (ZNE, PEC) can be applied at the *logical* level to squeeze further accuracy from imperfect logical qubits — "mitigation on top of correction." Conversely, some mitigation ideas (symmetry verification, erasure detection) blur into lightweight error *detection*. So the NISQ-to-fault-tolerant transition is not a sharp switch from mitigation to correction, but a gradual shift in the *balance*: as hardware improves and logical qubits arrive, correction does more of the work and mitigation less, but mitigation remains a useful complement even in the early fault-tolerant regime. This nuanced view (mitigation as a persistent complement, not merely a NISQ placeholder) is the sophisticated framing that Files 9, 17, and 18 build on.

### 18. Glossary

- **Error mitigation:** reducing bias in NISQ expectation-value estimates via classical post-processing of many noisy runs — no encoding, no exponential error suppression, exponential *sampling* overhead.
- **Zero-noise extrapolation (ZNE):** amplify noise to several levels, extrapolate to zero noise; heuristic, model-dependent.
- **Gate folding / pulse stretching:** methods to amplify noise (G→GG†G, or lengthen pulses) for ZNE.
- **Probabilistic error cancellation (PEC):** sample from a quasi-probability inverse-noise decomposition to unbiasedly cancel noise; rigorous but exponentially expensive.
- **Quasi-probability:** a signed (some-negative) representation of the inverse noise map, sampled with sign-weighting.
- **Clifford data regression (CDR):** learn a noise-correction function from classically-simulable Clifford training circuits.
- **Measurement-error mitigation:** invert the readout confusion matrix to correct readout errors.
- **Dynamical decoupling (DD):** pulse sequences on idle qubits to refocus slow dephasing — hardware-level, no shot cost.
- **Randomized compiling / Pauli twirling:** randomize the Pauli frame to convert coherent errors into benign stochastic Pauli errors.
- **Symmetry verification:** post-select out runs violating a conserved quantity (particle number, parity).
- **Sampling overhead:** the (exponentially growing) number of extra shots mitigation requires — its fundamental scaling limit.
- **Mitiq / Qiskit Runtime:** software implementing mitigation (open-source library / IBM's primitive-based execution).

### 19. Summary

Error mitigation is the NISQ era's tool for extracting reliable expectation values from noisy, unencoded quantum hardware — via zero-noise extrapolation, probabilistic error cancellation, measurement-error mitigation, dynamical decoupling, randomized compiling, and symmetry verification, typically layered together and integrated into cloud stacks. Its defining virtue is requiring *no qubit overhead* and working on *today's* devices; its defining limit is an *exponential sampling overhead* in the circuit's total noise, confining it to modest-complexity circuits and making it fundamentally a NISQ-only bridge rather than a path to fault tolerance (that is error correction's role, File 9). IBM's 2023 utility demonstration showed mitigation's genuine near-term power (correct results at 127-qubit scale) *and* — through the rapid classical rebuttals — the enduring lesson that "beyond classical" claims for mitigated NISQ circuits are provisional and demand specific, run classical comparisons (Files 14, 17, 22), because the low-complexity circuits mitigation handles are often exactly those advanced classical methods handle too. Mitigation is thus best understood as a valuable, honestly-limited near-term complement to error correction: it buys modest present-day utility while the hardware climbs toward the fidelity and scale that fault-tolerant correction — and genuinely transformative computation — require.
