# Quantum Computing — Comprehensive Technical Knowledge Database

A structured, deeply technical knowledge base covering quantum computing — physics fundamentals, all major qubit modalities, error correction, algorithms, software stacks, hardware engineering (control electronics, cryogenics, fabrication), networking, sensing, and the competitive/business/geopolitical landscape. **Audience:** hardware and algorithm engineers, and technical business analysts, who need working-level understanding, not conceptual overviews.

The database comprises **25 technical files plus this index**, totaling **~185,000 words**. Technical depth is the highest priority; every advantage claim is paired with its caveats, every roadmap with its track record, every headline number with its context. The throughline is **honest, evidence-based assessment** — the disciplined middle between hype and dismissal.

> **Last updated:** 2026-07-01. **Verification note:** hardware specifications, qubit counts, and fidelity records change on a months-long cadence via press release. Any specific figure claimed for the current or a future year should be **re-verified via web search** before quoting, given the field's fast-moving press-release cadence. Figures here reflect the documented trajectory through ~2024–2025.

---

## Reading Paths

Different readers should traverse the database differently:

1. **Hardware engineer path:** Files **02 → 03 → 04 → 05 → 11 → 23** (formalism → superconducting, trapped-ion, neutral-atom modalities → cryogenics/control → materials/fabrication).
2. **Algorithms / software engineer path:** Files **02 → 08 → 12 → 13 → 14** (formalism → compilation → software stack → algorithms → classical simulation).
3. **Error-correction specialist path:** Files **09 → 10 → 18** (error correction → mitigation → resource estimation).
4. **Business / strategy path:** Files **01 → 19 → 20 → 21 → 24** (overview → roadmaps → vendors → geopolitics → market/investment).
5. **Skeptical due-diligence path:** Files **10 → 14 → 17 → 19 → 22** (the cross-cutting honesty/caveat-heavy files: mitigation limits → classical simulation → NISQ applications → roadmap credibility → benchmarking).

⭐ marks **PRIMARY files** (≥12,000 words each, with full derivations, named papers/arXiv IDs, device parameters, and named systems).

---

## Table of Contents

**01. [Overview and Landscape](01_overview_and_landscape.md)** — Why quantum computing (proven/conjectured advantages and what quantum is *not* good for); complexity theory (BQP, P, NP, BPP; supremacy vs. advantage vs. utility); the timeline from Feynman (1982) to the below-threshold era (2024); the DiVincenzo criteria; industry structure and national initiatives.

**02. ⭐ [Quantum Mechanics Fundamentals for Computing](02_quantum_mechanics_fundamentals_for_computing.md)** — Qubit formalism (state vectors, Bloch sphere, density matrices, entanglement, measurement); gates and universality (Pauli/Clifford+T, Solovay–Kitaev, Gottesman–Knill, no-cloning); decoherence and noise (Lindblad, T₁/T₂, Kraus channels, RB/XEB, crosstalk/leakage); key theorems (QFT, QPE, Holevo); worked examples and conventions.

**03. ⭐ [Superconducting Qubits](03_superconducting_qubits.md)** — Josephson-junction physics; the transmon and fluxonium; circuit QED and dispersive readout; coupling and two-qubit gates (cross-resonance, tunable-coupler CZ); fabrication and TLS loss; IBM (Eagle/Condor/Heron), Google (Sycamore/Willow); performance and the connectivity/wiring limits.

**04. ⭐ [Trapped-Ion Qubits](04_trapped_ion_qubits.md)** — Paul traps and motional modes; ion species and hyperfine/optical qubits; cooling, initialization, readout; the Mølmer–Sørensen gate; QCCD shuttling and photonic interconnects; IonQ and Quantinuum; record fidelities and the fewer-better-qubits economics.

**05. ⭐ [Neutral-Atom Qubits](05_neutral_atom_qubits.md)** — Optical tweezers and array rearrangement; Rydberg states and the blockade; Rydberg CZ gates and global-gate parallelism; reconfigurable and zoned architectures; QuEra's 48-logical-qubit demonstration; QuEra/Pasqal/Atom Computing; erasure conversion.

**06. [Photonic Quantum Computing](06_photonic_quantum_computing.md)** — Why photons (room-temp, flying qubits, no photon–photon interaction); discrete- and continuous-variable encodings; KLM, measurement-based and fusion-based computing; PsiQuantum and Xanadu; boson sampling; photon loss as the defining error.

**07. [Spin, Topological, and Bosonic Qubits](07_other_qubit_modalities.md)** — Silicon spin qubits and isotopic purification; topological (Majorana) qubits and the 2018 retraction; bosonic/cat qubits and biased noise; the **consolidated cross-modality comparison table**.

**08. [Quantum Gates, Circuits, and Compilation](08_quantum_gates_circuits_and_compilation.md)** — Gate decomposition (KAK, Clifford+T, virtual-Z); the NP-hard SWAP-routing problem (SABRE); scheduling, pulse-level control, calibration; optimization (ZX-calculus); noise-aware compilation; variational and cloud workflows.

**09. ⭐ [Quantum Error Correction](09_quantum_error_correction.md)** — Error digitization and the stabilizer formalism; the threshold theorem; the surface code, decoders (MWPM/Union-Find/ML), lattice surgery, magic-state distillation; qLDPC codes, color/Floquet/bosonic codes; Google's below-threshold and QuEra's 48-logical-qubit demonstrations.

**10. [Quantum Error Mitigation](10_quantum_error_mitigation.md)** — Mitigation vs. correction; zero-noise extrapolation, probabilistic error cancellation, measurement mitigation, dynamical decoupling, randomized compiling; the IBM utility demonstration and its classical rebuttals; the exponential-sampling wall.

**11. ⭐ [Control Electronics and Cryogenics](11_control_electronics_and_cryogenics.md)** — Dilution refrigeration; the wiring bottleneck and heat-load budget; cryo-CMOS and multiplexing; the room-temperature control stack; real-time feedback and decoding; laser systems and the laser bottleneck; the co-scaling imperative.

**12. ⭐ [The Quantum Software Stack](12_quantum_software_stack_and_compilers.md)** — The leaky-stack layering; Qiskit, Cirq, PennyLane, Q#, TKET, Braket; compiler internals; OpenQASM and QIR; variational/hybrid patterns; cloud execution and queueing; the software's role as the accessibility layer.

**13. ⭐ [Quantum Algorithms Deep Dive](13_quantum_algorithms_deep_dive.md)** — Shor's algorithm (factoring/discrete-log); Grover and amplitude amplification; quantum simulation (Trotterization, qubitization); VQE and QAOA (with barren plateaus); HHL and its caveats; quantum machine learning (honestly framed); the speedup landscape and dequantization.

**14. [Classical Simulation of Quantum Circuits](14_classical_simulation_of_quantum_circuits.md)** — State-vector, tensor-network, stabilizer, and low-T-count methods; entanglement/T-count as the hardness axes; the Google supremacy claim and its classical rebuttals; the moving-target principle.

**15. [Quantum Networking and Communication](15_quantum_networking_and_communication.md)** — QKD (BB84, E91, and variants); quantum repeaters and entanglement swapping; the quantum-memory bottleneck; entanglement distribution (Micius); the quantum-internet stack; distributed quantum computing; QKD vs. PQC.

**16. [Quantum Sensing and Metrology](16_quantum_sensing_and_metrology.md)** — Atomic clocks, NV-center magnetometry, atom-interferometry gravimetry, SQUIDs; networked sensing; the commercial landscape — the *already-realized* face of quantum advantage, and a calibration for computing.

**17. [NISQ Applications and Use Cases](17_nisq_applications_and_use_cases.md)** — A skeptical, evidence-based survey of chemistry, optimization (D-Wave annealing), QML, finance, and pharma; the evaluation checklist; the honest conclusion that near-term value is capability-building, not demonstrated advantage.

**18. ⭐ [Fault-Tolerant Resource Estimation](18_fault_tolerant_resource_estimation.md)** — The estimation pipeline; the Gidney–Ekerå RSA-2048 estimate (~20M qubits, ~8 hours) and quantum-chemistry estimates; the Azure Resource Estimator; sensitivity analysis and the compounding overhead-reduction levers; the honest timeline.

**19. [Company Roadmaps and Milestones](19_company_roadmaps_and_milestones.md)** — A *methodology for assessing roadmap credibility* (track record, peer-review vs. press-release, independent benchmarking, metric-shifting); IBM, Google, Quantinuum, IonQ, PsiQuantum, Microsoft, D-Wave, and the Chinese programs.

**20. ⭐ [Vendor and Competitive Landscape](20_vendor_and_competitive_landscape.md)** — Full-stack giants, pure-play public companies, specialized/emerging-modality companies, and software firms; business models, funding, traction, moats; market structure (revenue reality, SPAC volatility, talent/IP concentration); the "who wins?" question.

**21. [National Programs and Geopolitics](21_national_programs_and_geopolitics.md)** — US NQI, China, EU Flagship, UK, and others; export controls and dual-use; the **post-quantum cryptography transition** and "harvest now, decrypt later" — the dominant near-term policy issue.

**22. [Benchmarking and Performance Metrics](22_benchmarking_and_performance_metrics.md)** — Why benchmarking is contested; raw qubit count, Quantum Volume, #AQ, CLOPS, RB/XEB, application benchmarks; the meta-lesson that no single number suffices and vendor metrics favor their originators.

**23. [Materials and Fabrication](23_materials_and_fabrication.md)** — Substrates and superconducting films (the tantalum advance); Josephson-junction fabrication (Dolan bridge) and yield; vacuum, photonic, and cryogenic-packaging fabrication; isotopic purification; the manufacturability comparison.

**24. [Business, Market, and Investment](24_business_market_and_investment.md)** — Market-size variance; the funding landscape and the SPAC era; the revenue reality; enterprise-customer patterns; the talent market; the PQC-migration economy as the largest quantum-adjacent activity.

**25. ⭐ [Future Research Frontiers](25_future_research_frontiers.md)** — Open problems in error correction (real-time decoding, qLDPC, magic states), hardware (hybrids, materials, control scaling), and algorithms (new speedups, dequantization, QML); speculative directions (beyond-stabilizer codes, topological qubits, quantum-classical co-processing); the honest timeline calibration.

---

## Key Concepts Index

- **DiVincenzo criteria** → File 01 (§4)
- **BQP and complexity theory** → Files 01 (§2), 13 (§23, §41)
- **Bloch sphere, density matrices, entanglement** → File 02 (Parts I)
- **No-cloning theorem** → File 02 (§15); its consequence for QEC → File 09
- **T₁/T₂ and decoherence; Lindblad; Kraus channels** → File 02 (Part III)
- **Quantum Fourier Transform / Phase Estimation** → File 02 (§16–17); in Shor → File 13
- **Clifford+T / Gottesman–Knill / magic** → Files 02 (§8), 09, 14
- **Threshold theorem** → File 09 (§6)
- **Surface code** → File 09 (Part III)
- **qLDPC codes** → File 09 (§15); resource impact → File 18
- **Magic-state distillation** → File 09 (§13, §35); T-count cost → File 18
- **Shor's algorithm** → File 13 (Part I); resource estimate → File 18; PQC response → File 21
- **Grover's algorithm / amplitude amplification** → File 13 (Part II)
- **Barren plateaus** → File 13 (§16)
- **Resource estimation** → File 18 (the quantitative capstone)
- **The wiring/laser bottleneck; cryo-CMOS** → File 11
- **Cross-modality comparison table** → File 07 (§16)
- **The classical-comparison / moving-target discipline** → Files 14, 17, 22
- **Harvest now, decrypt later; PQC (Kyber/Dilithium)** → File 21

---

## The Database's Throughline

Across all 25 files, the commitment is to **technical depth** (working-level understanding, not overviews), **honesty** (every claim paired with its caveats), and **evidence-based assessment** (grounded in demonstrated results, resource estimates, and classical comparison — not marketing). The synthesized view: quantum computing is a **real, transformative technology** with **proven but specific** exponential speedups (factoring, simulation) and **validated below-threshold error correction**, whose **broadly-useful realization is plausibly a 2030s-and-beyond development** gated by interconnected research frontiers — the **disciplined middle** between hype and dismissal. Read the files for the full technical development; return to this index to navigate, and re-verify fast-moving specifics via web search.
