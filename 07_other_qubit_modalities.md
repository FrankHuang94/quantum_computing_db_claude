# Spin Qubits, Topological Qubits, and Bosonic Codes

> This file covers the modalities beyond the "big three" (superconducting, trapped ion, neutral atom) and photonics: **silicon spin qubits** (the strongest claim to semiconductor-foundry manufacturability), **topological qubits** (the highest-risk, potentially most-error-resilient approach), and **bosonic/cat qubits** (encoding in oscillator states for biased-noise error correction). It closes with a **consolidated cross-modality comparison table** — the strategic map of the entire hardware landscape. It complements Files 3–6 (the other modalities), 9 (error correction, where cat qubits and bosonic codes are central), and 23 (materials/fabrication).

---

## Part I — Silicon Spin Qubits

### 1. The concept and its commercial appeal

A **spin qubit** encodes information in the spin of a single electron (or hole, or nucleus) confined in a **gate-defined quantum dot** in a semiconductor. The qubit states are spin-up |↑⟩ and spin-down |↓⟩, split by a magnetic field (Zeeman splitting). The overwhelming commercial appeal is **manufacturability**: spin qubits are made in **silicon** (or Si/SiGe heterostructures) using processes closely related to standard **CMOS semiconductor fabrication** — the same industry that manufactures billions of transistors per chip. If spin qubits work at scale, the argument goes, the semiconductor industry's decades of fabrication, yield, and integration expertise could be brought to bear, potentially enabling qubit densities and manufacturing maturity no other modality can match. Intel (leveraging its chip-manufacturing base) and academic groups (Delft/QuTech, UNSW, RIKEN, Wisconsin) lead this modality; startups include Diraq (Australia), Quantum Motion (UK), and equal1.

### 2. Physics: quantum dots and confinement

A **quantum dot** is a nanoscale region (defined by voltages on lithographic gate electrodes) that confines a single electron in all three dimensions — an "artificial atom." The confining potential is created by depleting electrons from a 2D electron gas (in a Si/SiGe or Si-MOS heterostructure) using patterned gates, leaving a controllable number of electrons (ideally exactly one) in the dot. The electron's spin is the qubit; its energy splitting is set by an applied magnetic field (Zeeman effect) and, for some designs, by micromagnets or spin–orbit coupling that create local field gradients enabling electric control.

### 3. Isotopic purification — the coherence breakthrough

Natural silicon contains ~4.7% **²⁹Si**, which has a nuclear spin that couples to the electron spin (hyperfine interaction), causing decoherence via the fluctuating nuclear-spin bath — the dominant dephasing source in early spin qubits. The breakthrough was **isotopic purification**: growing silicon enriched in **²⁸Si** (nuclear-spin-*zero*), removing the ²⁹Si bath. Isotopically purified ²⁸Si improved spin coherence times by **orders of magnitude** (T₂ from μs to ms, and nuclear-spin T₂ to seconds or longer) — the enabling advance that made silicon spin qubits competitive. This is a direct materials-science lever (File 23) with outsized impact, analogous to the transmon's charge-noise suppression (File 3) or the ion's clock transition (File 4).

### 4. Single- and two-qubit gates

- **Single-qubit gates:** driven by **electron spin resonance (ESR)** — microwave magnetic fields (or, via micromagnets/spin–orbit coupling, microwave *electric* fields, "EDSR") that rotate the spin. Gate fidelities now exceed 99.9% in leading demonstrations.
- **Two-qubit gates via exchange coupling:** the dominant mechanism. Two electrons in adjacent quantum dots interact via the **Heisenberg exchange interaction** J, whose strength depends *exponentially* on the inter-dot barrier (controlled by a barrier gate voltage). Pulsing the exchange on and off implements a two-qubit gate (√SWAP, CZ, or CNOT depending on the scheme) in **nanoseconds** — fast gates. The exponential sensitivity of J to voltage means gates are fast but require *extremely precise* voltage control, and the short coherence (relative to ions/atoms) demands fast, well-calibrated operation.

### 5. The 2022 fault-tolerance-threshold milestone

A landmark: in **2022, three groups simultaneously** (Delft/QuTech, RIKEN, and UNSW, published together in Nature) reported **two-qubit gate fidelities exceeding 99%** — above the commonly cited fault-tolerance threshold — in silicon spin qubits. This was the moment silicon spin qubits joined the ranks of modalities with above-threshold two-qubit gates, validating the platform's fidelity potential after years of trailing. Combined with isotopic purification and CMOS compatibility, it revitalized the modality's prospects.

### 6. Scaling architectures and cryo-CMOS integration

Spin qubits are tiny (~100 nm) and could in principle be packed at enormous densities — but the same wiring bottleneck as superconducting (File 11) looms: each qubit needs gate voltages and control. The proposed solutions leverage semiconductor integration:

- **Crossbar / shared-control architectures** (Intel, Diraq): using shared row/column control lines (like a memory array) to address many qubits with fewer wires — exploiting the qubits' uniformity and small size.
- **Cryo-CMOS co-integration:** placing classical control electronics (Intel's Horse Ridge chips, File 11) at the cryogenic stage, near the qubits — spin qubits' CMOS compatibility makes co-integrating qubits and control on the same chip a natural (if unproven-at-scale) goal, potentially solving the wiring problem more elegantly than for superconducting qubits.
- **Operating temperature:** spin qubits typically need <100 mK–~1 K, but some ("hot" spin qubits) operate at ~1 K, where cooling power is far greater than at 10 mK — easing the cryo-CMOS co-integration and wiring budget relative to superconducting's 10 mK.

### 7. Donor-based spin qubits

A distinct sub-approach encodes the qubit in the spin of a **donor atom** (typically **phosphorus**) implanted in silicon — the **Kane proposal (1998)**, pursued especially by **Michelle Simmons's group at UNSW** (and Silicon Quantum Computing). The phosphorus nuclear (or electron) spin offers *extremely* long coherence, and atomic-precision placement is achieved via **STM (scanning-tunneling-microscope) lithography** — positioning individual donor atoms with sub-nm precision. The trade-off is extraordinarily demanding fabrication (atom-by-atom placement) versus the exceptional coherence and the promise of atomically-identical qubits. Donor qubits are a longer-horizon, higher-precision bet within the silicon-spin family.

### 8. Assessment of spin qubits

Spin qubits' promise is **manufacturability** (CMOS compatibility, tiny size, potential co-integration with cryo-CMOS control) plus now-demonstrated **above-threshold fidelity** and **long coherence** (isotopic purification). Their challenges: **small qubit counts to date** (single digits to low tens, far behind superconducting/atoms — the platform is less mature at scale), **charge noise and voltage-control precision** (the exponential exchange-J sensitivity), **variability** (dot-to-dot uniformity, the semiconductor-fab yield problem applied to quantum dots), and **cryogenic operation** (milder than superconducting but still required). The strategic bet is that CMOS manufacturing leverage will eventually let spin qubits scale faster than modalities lacking that industrial base — a bet on the same manufacturing thesis as photonics (File 6), applied to matter qubits. Whether that leverage materializes at the required fidelity and uniformity is the open question (Files 19, 23, 25).

---

## Part II — Topological Qubits

### 9. The concept: information stored non-locally

**Topological quantum computing** aims for qubits that are *intrinsically* protected from local noise by encoding information **non-locally**, in a global topological property that no local perturbation can access. The theoretical basis is **Majorana zero modes (MZMs)** — exotic quasiparticle excitations predicted to appear at the ends of **topological superconducting nanowires** (semiconductor nanowires with strong spin–orbit coupling, proximity-coupled to a superconductor, in a magnetic field). A pair of MZMs encodes one qubit, but the information is stored *non-locally* across the two spatially separated Majoranas — so a local error (noise touching one end) cannot corrupt the logical information, which lives in the *joint* state. This non-local encoding would provide **hardware-level error protection**, potentially requiring far less error-correction overhead than other modalities.

### 10. Non-Abelian braiding

MZMs are predicted to be **non-Abelian anyons**: exchanging (braiding) two of them performs a quantum gate that depends only on the *topology* of the braid (which Majorana went around which), not on the details of the path. Because the gate depends only on topology, it is *intrinsically robust* against small errors in the braiding operation — a topologically protected gate. Braiding provides some (Clifford) gates natively and robustly; universality requires supplementing braiding with a non-topological "magic" operation (analogous to the T gate elsewhere). This combination of non-locally-protected qubits and topologically-protected gates is the dream: a qubit that is error-resilient *by construction*, reducing or eliminating the enormous error-correction overhead (File 9) that burdens other modalities.

### 11. Microsoft's program and the 2018 setback

**Microsoft** has pursued topological qubits for over a decade as its primary hardware bet, attracted by the potential overhead reduction. The path has been rocky:

- **The 2018 retraction:** a prominent 2018 Nature paper (Delft/Microsoft) claiming to observe a Majorana signature (a quantized zero-bias conductance peak) was **retracted in 2021** after the claimed signature was found to be non-reproducible / an artifact of data selection — a significant, cautionary episode that damaged the field's credibility and underscored how difficult it is to *unambiguously* confirm Majorana zero modes (many non-topological effects can mimic the signatures).
- **The 2023 "topological gap protocol":** Microsoft reported improved measurement protocols and claimed to have established the presence of a topological phase in its devices, continuing the pursuit. In 2024–2025 Microsoft announced further device milestones (a "topological qubit" chip, "Majorana 1"), though the underlying physical claims remained actively debated in parts of the community.

### 12. Honest assessment of topological qubits

The honest, non-dismissive assessment: **topological qubits remain the least experimentally mature mainstream approach.** The foundational physical signature (unambiguous, controllable Majorana zero modes with demonstrated non-Abelian braiding) is still actively debated — a very different maturity level from superconducting or trapped-ion qubits, which perform full algorithms and error correction routinely. Yet the *theoretical motivation* remains sound: if topological protection can be realized, it could yield a qualitatively more error-resilient qubit, dramatically reducing error-correction overhead. Topological qubits are thus a **high-risk, high-reward, long-horizon** bet: potentially transformative, but with the foundational physics not yet settled, and a track record (the 2018 retraction) that mandates skepticism toward claims. Microsoft's parallel strategy of *also* partnering across other modalities via Azure Quantum (Files 19, 20) is a rational hedge against its topological program's risk.

---

## Part III — Bosonic and Cat Qubits

### 13. The concept: encoding in oscillator states

**Bosonic qubits** encode information not in a two-level system but in the states of a **harmonic oscillator** (a microwave cavity mode, or a mechanical or optical mode) — a system with infinitely many levels. The key advantage: a single oscillator has a large Hilbert space in which to *redundantly* encode a qubit, enabling **hardware-efficient error correction** (correcting errors within one physical oscillator, rather than spreading a qubit across many two-level systems). Bosonic codes include **cat codes**, **binomial codes**, and **GKP codes** (the latter overlapping with CV photonics, File 6). Yale (Devoret, Schoelkopf, Girvin groups) pioneered bosonic quantum error correction, achieving the milestone of a bosonic logical qubit whose lifetime *exceeded* that of its best physical component (the "break-even" point for error correction).

### 14. Cat qubits and biased noise

A **cat qubit** encodes the logical states in **superpositions of coherent states** — |α⟩ and |−α⟩ (coherent states of opposite phase), the "Schrödinger cat" states. The defining feature is **biased noise**: by engineering the oscillator's dynamics (e.g., via two-photon driving and dissipation, which stabilizes the cat manifold), one can make the dominant error a **phase flip** while the **bit flip is exponentially suppressed** as the cat size |α|² grows (bit flips require the state to traverse the large phase-space distance between |α⟩ and |−α⟩, which becomes exponentially unlikely). This **noise bias** is the strategic advantage: if bit flips are exponentially rare at the hardware level, error correction only needs to handle *phase flips* — a *single* error type — allowing a much simpler, lower-overhead outer code (a 1D **repetition code** for phase flips) instead of a full 2D surface code. Concentrating error-correction resources on one error type, because the other is already suppressed by hardware, is the core efficiency argument.

### 15. Cat-qubit companies and architectures

- **AWS (Amazon):** an architecture combining **cat qubits** (for biased-noise bit-flip suppression) with **transmon ancillas** and an outer **repetition code** for the residual phase-flip errors — a "concatenated" approach unifying hardware-level bias engineering with code-level correction (File 9). AWS's cat-qubit research targets substantially reduced overhead versus the standard surface code.
- **Alice & Bob (France):** a company built *entirely* around the cat-qubit bet — dedicated cat-qubit ("Boson") hardware targeting very long bit-flip times (seconds to minutes) so that only phase-flip correction (a 1D repetition code) is needed, aiming for a dramatically lower physical-qubit overhead to fault tolerance than surface-code approaches (Files 9, 18, 20). It is a notable example of a company staking its identity on a single specific architectural bet on biased-noise error correction.
- **Yale** and other academic groups continue foundational bosonic-QEC research (GKP-in-cavity, binomial codes, error-transparent gates).

Bosonic/cat qubits illustrate a general theme (echoing neutral-atom erasure conversion, File 5, and photonic loss-as-erasure, File 6): **matching the error-correcting code to the hardware's specific, engineered error structure** can slash overhead — one of the most important levers for reducing the daunting resource requirements of fault tolerance (File 18).

---

## Part IV — Consolidated Cross-Modality Comparison

### 16. The master comparison table

This table is the strategic map of the entire hardware landscape (Files 3–7). "Best" values are leading demonstrations, not typical; re-verify for current claims (File 22). All numbers are order-of-magnitude/qualitative summaries.

| Property | Superconducting | Trapped ion | Neutral atom | Photonic | Silicon spin | Topological | Bosonic/cat |
|---|---|---|---|---|---|---|---|
| Qubit encoding | anharmonic LC (transmon) | ion internal states | atom internal states | photon DV/CV | electron/nuclear spin | Majorana zero modes | oscillator (cat) states |
| Energy scale / freq | 4–6 GHz | microwave/optical | microwave/optical | optical | GHz (Zeeman) | — | microwave (cavity) |
| T₁/T₂ | 50–300 μs | s–minutes | s (nuclear: 10s s) | loss-limited | ms (T₂, ²⁸Si) | (protected, in theory) | cavity-lifetime-limited |
| 1q gate fidelity (best) | >99.9% | >99.99% | >99.9% | deterministic | >99.9% | (topological) | high |
| 2q gate fidelity (best) | 99.5–99.9% | >99.9% | >99.5% | probabilistic | >99% | (braiding) | improving |
| Gate speed | ns–100s ns | μs–ms | 100 ns–1 μs | (measurement-based) | ns | (braiding) | μs |
| Native connectivity | fixed nearest-neighbor | all-to-all (intra-module) | reconfigurable | (MBQC cluster) | nearest-neighbor | (braiding topology) | (cavity-dependent) |
| Operating temp | ~10 mK | room-temp (chip ~4–10 K) | room-temp vacuum | room-temp (detectors ~1–4 K) | <100 mK–1 K | ~10 mK | ~10 mK |
| Primary error mechanism | TLS loss, leakage, crosstalk | spontaneous emission, heating | Rydberg decay, atom loss | photon loss | charge noise, ²⁹Si | (Majorana confirmation) | phase flips (bias) |
| Primary scaling challenge | wiring, cryogenics | lasers, shuttling, networking | lasers, atom loss, optics | loss, multiplexing, manufacturing | uniformity, wiring | confirming the physics | cavity/ancilla control |
| Fabrication leverage | lithographic (SC films) | atomic (no fab variation) | atomic (no fab variation) | silicon photonics (foundry) | CMOS (strongest) | semiconductor-SC hybrid | superconducting cavity |
| Maturity | very high (below-threshold QEC) | very high (record fidelity, QEC) | high (48 logical qubits) | medium (advantage demos) | medium (above-threshold 2q) | low (physics debated) | medium (break-even QEC) |
| Leading players | IBM, Google, Rigetti, IQM | IonQ, Quantinuum, AQT | QuEra, Pasqal, Atom Computing | PsiQuantum, Xanadu, Quandela | Intel, Diraq, UNSW, Delft | Microsoft | AWS, Alice&Bob, Yale |

### 17. Reading the table strategically

No modality dominates every axis. The strategic lessons:

- **Fidelity/connectivity leader:** trapped ions (and neutral atoms close behind) — favored for *quality-limited* early fault tolerance (fewer, better qubits; Files 4, 18).
- **Speed/fabrication leader:** superconducting — favored for *throughput* and leveraging the deepest engineering/investment base; first below-threshold QEC (File 3, 9).
- **Scale/reconfigurability leader:** neutral atoms — favored for *many-logical-qubit* demonstrations and flexible codes; largest arrays (File 5).
- **Manufacturing bets:** photonic (foundry photonics) and silicon spin (CMOS) — betting industrial manufacturing leverage beats per-qubit physics progress, at the cost of current immaturity (Files 6, 7).
- **Overhead-reduction bets:** bosonic/cat (biased noise) and topological (hardware protection) — betting that engineering the *error structure* (or eliminating errors at the hardware level) slashes the crushing overhead of fault tolerance (Files 9, 18), with cat qubits far more mature than topological.
- **Networking-native:** photonic — uniquely dual-use with quantum networking (File 15).

The field's health is that *multiple* modalities have reached the error-correction era from these different starting points (superconducting below-threshold, neutral-atom 48-logical-qubit, trapped-ion logical demonstrations), so the cross-modality comparison — not any single roadmap — is the essential lens. Which combination of properties proves decisive for *useful* fault tolerance is the central open question that Files 9 (error correction), 18 (resource estimation), and 19–20 (roadmaps/competition) address, and it will likely differ by application: quality-limited problems favor ions/atoms, throughput-limited problems favor superconducting, and overhead reduction (cat qubits, erasure conversion, qLDPC codes) could reshuffle the ranking entirely.

*Cross-references: the individual modalities in detail (Files 3, 4, 5, 6); the formalism these encodings realize (File 2); cat/GKP/biased-noise error correction and qLDPC codes (File 9); resource estimation quantifying the overhead each modality faces (File 18); cryogenics and cryo-CMOS relevant to spin/superconducting/bosonic (File 11); materials and fabrication, especially isotopic purification and semiconductor-SC hybrids (File 23); roadmaps and competitive positioning (Files 19, 20); networking synergy for photonic (File 15).*

---

## Part V — Additional Depth, NV Centers, and Worked Examples

### 18. Types of spin qubit

The "spin qubit" label covers several distinct encodings, worth distinguishing:

- **Loss–DiVincenzo (single-spin) qubit:** one electron spin per dot, the qubit being |↑⟩/|↓⟩ (the mainstream Intel/Delft approach, Sections 1–6).
- **Singlet–triplet qubit:** two electrons in a double dot, the qubit encoded in the singlet |S⟩ and triplet |T₀⟩ two-electron spin states — controlled entirely electrically (exchange + magnetic-gradient), avoiding microwave magnetic fields; a robust variant.
- **Exchange-only / triple-dot qubit:** three electrons across three dots, enabling all-electrical universal control via exchange alone (no ESR), at the cost of more dots per qubit.
- **Hole-spin qubits:** using holes (absent electrons) rather than electrons; holes have strong spin–orbit coupling enabling fast all-electrical control without micromagnets, and (in some materials) reduced hyperfine coupling — an increasingly popular variant (Delft, others).
- **Donor (Kane) qubits:** phosphorus nuclear/electron spins in silicon (Section 7).

These variants trade control complexity, coherence, and fabrication difficulty differently; the field has not fully converged, though single-spin and hole-spin qubits are currently most prominent for scaling.

### 19. NV centers as a modality (and a bridge to sensing)

The **nitrogen-vacancy (NV) center** in diamond — a nitrogen atom adjacent to a vacancy in the diamond lattice — hosts an electron spin that is optically addressable and coherent **at room temperature**, a rare property. NV centers have been explored as a qubit modality (with nearby nuclear spins as long-lived memory qubits, and optical interfaces for photonic networking), and Delft's NV-center work produced foundational quantum-network and loophole-free-Bell-test results (File 15). However, NV centers have proven hard to scale as a *computing* platform (placing and coupling many NV centers reproducibly is difficult), so they are pursued less for computing than for **quantum sensing** (room-temperature magnetometry, File 16) and as **quantum-repeater memory nodes** (File 15). NV centers thus sit at the intersection of computing, networking, and sensing — a reminder that the modality landscape blends into the broader quantum-technology ecosystem (Files 15, 16).

### 20. Worked example: cat-qubit overhead reduction

The cat-qubit value proposition (Section 14) is quantifiable. A standard **surface code** correcting both bit- and phase-flips at distance d uses ~2d²−1 physical qubits per logical qubit — e.g., ~1000 physical qubits at d≈23 for a low logical error rate. A **cat qubit** with hardware-suppressed bit-flips needs only a **1D repetition code** for the residual phase-flips, using ~d physical cat qubits (linear in d, not quadratic) — e.g., ~tens of cat qubits for comparable protection. Even accounting for each cat qubit requiring an oscillator plus ancilla/control overhead, the *scaling* improvement (linear vs. quadratic in distance) is dramatic. This is why AWS and Alice & Bob pursue cat qubits: if the bit-flip suppression holds (bit-flip times of seconds-to-minutes at usable cat sizes), the physical-qubit overhead to fault tolerance could drop by an order of magnitude or more versus the surface code (Files 9, 18). The risk is whether the biased-noise assumption survives realistic gate operations (gates can reintroduce bit flips), which is the central technical question for the approach — a caveat that resource estimates (File 18) must treat honestly.

### 21. Worked example: spin-qubit density argument

The spin-qubit manufacturability argument is also quantifiable. A silicon quantum dot is ~50–100 nm; at that pitch, a 1 cm² chip could in principle host ~10⁸–10⁹ dots — vastly more than any other modality's per-chip qubit count, and comparable to transistor densities in modern CMOS. Even accounting for control-gate overhead and the need for space between qubits, densities of 10⁶+ qubits per chip are conceivable *if* the wiring/control problem (Section 6) is solved via crossbar addressing and cryo-CMOS co-integration. This density argument — leveraging the semiconductor industry's demonstrated ability to fabricate and wire billions of nanoscale devices — is the core of the spin-qubit long-term thesis. The gap between this potential and current reality (single digits to low tens of qubits) is entirely about *uniformity, yield, and control integration* — the classic semiconductor scaling problems that the industry has repeatedly solved for transistors, but not yet for qubits (where the tolerance for variation is far tighter). Whether qubit fabrication follows a Moore's-law-like trajectory is the multi-decade question (Files 19, 23, 25).

### 22. Glossary and summary

**Glossary:**
- **Quantum dot:** gate-defined nanoscale electron trap (artificial atom) hosting a spin qubit.
- **Exchange interaction (J):** the tunable spin–spin coupling (exponentially voltage-sensitive) used for spin two-qubit gates.
- **Isotopic purification (²⁸Si):** removing nuclear-spin-carrying ²⁹Si to extend spin coherence — the key materials advance.
- **Majorana zero mode (MZM):** the exotic quasiparticle whose non-local, non-Abelian character underlies topological qubits.
- **Braiding:** exchanging non-Abelian anyons to perform topologically protected gates.
- **Cat qubit:** a bosonic qubit encoded in coherent-state superpositions |α⟩ ± |−α⟩, with hardware-suppressed (biased) bit-flip noise.
- **Biased noise:** an error structure where one error type (bit flip) is exponentially rarer than another (phase flip), enabling a simpler outer code.
- **Bosonic code (cat/binomial/GKP):** encoding a qubit in an oscillator's many-level Hilbert space for hardware-efficient error correction.
- **NV center:** a room-temperature-coherent diamond spin qubit, used more for sensing/networking than computing.

**Summary.** Beyond the big three and photonics, three further modalities each embody a distinct strategic bet: **silicon spin qubits** bet on CMOS manufacturability and density (now with above-threshold fidelity and long ²⁸Si coherence, but small current scale); **topological qubits** bet on hardware-level error protection via Majorana non-locality (potentially transformative, but the foundational physics remains debated after the 2018 retraction — the field's highest-risk approach); and **bosonic/cat qubits** bet on engineering biased noise to slash error-correction overhead (mature enough for break-even QEC, pursued by AWS and Alice & Bob). Together with Files 3–6, they complete the hardware landscape, whose consolidated comparison table (Section 16) is the strategic map the rest of the database — error correction (File 9), resource estimation (File 18), and roadmaps/competition (Files 19–20) — builds upon. The recurring meta-lesson: matching the error-correcting code to each modality's *specific engineered error structure* (biased noise, erasure conversion, loss heralding) is among the most powerful levers for reaching fault tolerance, and it is why the seemingly-narrow choice of qubit encoding cascades all the way up to the physical-qubit counts and timelines that define the path to utility.
