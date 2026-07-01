# Control Electronics, Cryogenic Systems, and Classical Infrastructure

> **⭐ PRIMARY FILE.** Behind every quantum processor is an enormous, often-underappreciated stack of *classical* infrastructure: dilution refrigerators reaching 10 mK, thousands of microwave/flux control lines, quantum-limited amplifiers, FPGA-based real-time controllers, laser systems, and the vacuum and shielding that make it all work. This "boring" infrastructure is, in fact, the dominant *scaling bottleneck* for several modalities — the **wiring bottleneck** for superconducting qubits and the **laser bottleneck** for atomic qubits. This file develops dilution refrigeration, the cryogenic wiring and heat-load problem, cryo-CMOS and multiplexing solutions, the room-temperature control stack, real-time feedback for error correction, and the laser systems for trapped-ion/neutral-atom platforms. It is the engineering foundation beneath Files 3 (superconducting), 4 (trapped ion), 5 (neutral atom), 7 (spin), and 9 (real-time decoding), and a key input to resource estimation and scaling (Files 18, 25).

---

## Part I — Dilution Refrigeration

### 1. Why ultra-low temperature is required

Superconducting qubits (File 3) operate at **~10–20 mK** — among the coldest environments in the universe, far colder than interstellar space (~2.7 K). Two physical requirements set this:

- **Suppressing thermal excitation of the qubit.** A superconducting qubit has a transition frequency ω_q/2π ≈ 5 GHz, corresponding to an energy ℏω_q ≈ k_B × 0.24 K. For the qubit to stay reliably in its ground state (not be thermally excited by ambient energy), the thermal energy k_B T must be *far below* ℏω_q: T ≪ 0.24 K. At T = 10 mK, the thermal excited-state population is e^{−ℏω_q/k_B T} ≈ e^{−24} ≈ 4×10⁻¹¹ — negligible. At 1 K it would be ~e^{−0.24} ≈ 0.79 (79% excited — completely unusable). This exponential sensitivity is why millikelvin temperatures are non-negotiable for superconducting qubits.
- **Suppressing thermal photon noise** in the control/readout lines, which would otherwise dephase and spuriously excite the qubit.

**Spin qubits** (File 7) also require sub-kelvin operation (typically <100 mK to ~1 K). **Trapped ions** (File 4) and **neutral atoms** (File 5) do *not* require cryogenic *qubits* — the atoms/ions are laser-cooled in room-temperature vacuum — a major architectural advantage, though ion trap *chips* are often cooled to ~4–10 K for better vacuum and lower motional heating, and photonic *detectors* (SNSPDs, File 6) need ~1–4 K. So the dilution-refrigerator burden is specifically a superconducting/spin-qubit challenge.

### 2. How a dilution refrigerator works

The **dilution refrigerator (DR)** is the workhorse achieving continuous cooling to ~10 mK, based on the physics of **³He/⁴He mixture phase separation**:

- Below ~0.87 K, a liquid mixture of ³He and ⁴He *phase-separates* into a ³He-rich "concentrated" phase (floating on top) and a ³He-dilute phase (~6.6% ³He in ⁴He, below). Crucially, the dilute phase retains ~6.6% ³He *even at absolute zero* — so ³He can always "evaporate" from the concentrated phase across the phase boundary into the dilute phase.
- This "evaporation" of ³He across the phase boundary (in the **mixing chamber**) is *endothermic* — it absorbs heat — providing **continuous cooling power** down to ~10 mK (unlike single-shot methods like adiabatic demagnetization). ³He is then extracted from the dilute phase (in the "still," at ~0.7 K), pumped, purified, recondensed, and returned — a closed cycle.

The DR achieves cooling through **staged temperatures**, each stage a physical plate in the fridge:

- **~50 K stage** and **~4 K stage:** cooled by a **pulse-tube cryocooler** (a closed-cycle mechanical cooler using compressed helium gas, replacing the liquid-helium baths of older "wet" fridges — modern "dry" DRs are cryocooler-based).
- **Still (~700 mK).**
- **Cold plate / intermediate (~100 mK).**
- **Mixing chamber (~10–20 mK):** where the qubit chip is mounted.

A modern dry dilution refrigerator is a room-sized-adjacent apparatus (the "chandelier" of gold-plated plates and wiring visible in quantum-computer photos is the DR's cold stages) costing hundreds of thousands to millions of dollars.

### 3. Cooling power and the fundamental constraint

The crucial constraint is **cooling power**, which *decreases dramatically at colder stages*:

- The 4 K stage (pulse-tube) offers ~1 W of cooling power.
- The mixing chamber offers only **~10–1000 μW** at ~10–100 mK — *microwatts*.

This tiny cooling budget at the coldest stage is the root of the **wiring bottleneck** (Part II): every control/readout wire running from room temperature down to the qubit carries **parasitic heat** (via thermal conduction along the metal and via microwave power dissipated in attenuators), and the total heat load must stay within the mixing chamber's μW-scale budget. Because cooling power at 10 mK is nearly fixed (set by the fridge's ³He circulation rate, itself limited), while heat load scales with the *number of wires*, there is a hard ceiling on how many qubits a single DR can support with conventional per-qubit wiring — the central scaling challenge for superconducting quantum computing.

---

## Part II — The Wiring Bottleneck and Cryogenic Signal Delivery

### 4. The heat-load problem, quantified

Each superconducting qubit needs, roughly, a microwave drive line (XY control), a flux-bias line (Z control, for tunable qubits/couplers), and a share of a readout line (File 3, Section 53). Each coaxial line:

- **Conducts heat** from warmer to colder stages (metal is a thermal conductor). This is mitigated by using **thermally-resistive but electrically-conductive** materials (e.g., stainless steel or superconducting NbTi coax, which conduct electricity but poorly conduct heat) and by **thermalizing** (heat-sinking) each line at every stage.
- **Dissipates power** in the **attenuators** placed at cold stages (Section 5), depositing heat directly at those stages.

A rough figure: each fully-instrumented qubit contributes on the order of ~microwatts of heat load to the coldest stages. With a mixing-chamber budget of ~hundreds of μW to ~1 mW, a single conventional DR can support on the order of **~100–1000 qubits** before the heat load exhausts the cooling power — precisely the regime current large processors (IBM Condor 1121, File 3) are pushing against. Scaling to the **millions of physical qubits** that fault tolerance requires (File 18) is *impossible* with one wire per qubit and room-temperature electronics — the wiring bottleneck is an existential scaling barrier, motivating the solutions of Part III.

### 5. Attenuation, thermalization, and filtering

The signal chain going *down* the fridge must deliver clean control signals while *not* carrying room-temperature thermal noise to the qubit:

- **Attenuation:** microwave drive lines are heavily attenuated (e.g., 20 dB at 4 K, 20 dB at the mixing chamber) to suppress the **Johnson–Nyquist thermal noise** from warmer stages. Attenuating the thermal noise (and the signal, which is then sent at higher power from room temperature) ensures the qubit sees a cold electromagnetic environment. But attenuators *dissipate* the absorbed power as heat at their stage — the trade-off between noise suppression and heat load (Section 4).
- **Thermalization:** every line and component is heat-sunk to its stage's plate so heat flows to the plate (where the fridge removes it) rather than continuing down to the qubit.
- **Filtering:** **low-pass and eccosorb (microwave-absorbing) filters** remove high-frequency noise and, critically, **infrared photons** that would otherwise reach the qubit and break Cooper pairs into **quasiparticles** (File 3, Section 52) — a known T₁-limiting mechanism. Light-tight, IR-absorbing shielding around the qubit is standard.

### 6. The output (readout) chain and quantum-limited amplification

The *upward* (readout) chain must amplify the microscopic dispersive readout signal (a few microwave photons, File 3) without adding noise:

- **Quantum-limited parametric amplifier** (JPA or TWPA, File 3, Sections 38–39) at the mixing chamber / cold stage — adds near the minimum quantum-limited noise (half a photon), the critical first amplification stage enabling fast single-shot readout.
- **Cryogenic HEMT amplifier** (~4 K) — a semiconductor low-noise amplifier providing the bulk of the gain.
- **Room-temperature amplification and demodulation** — final gain, IQ demodulation, digitization.
- **Isolators/circulators** protect the qubit from amplifier back-action noise (bulky microwave components at the cold stage, themselves a packaging/scaling challenge — replacing them with on-chip nonreciprocal elements is a research goal).

This amplifier chain is what makes fast, high-fidelity readout — and hence real-time error correction (File 9, Section 11) — possible, and its per-qubit components (especially bulky isolators) add to the packaging and scaling burden.

---

## Part III — Cryo-CMOS and Wiring-Scaling Solutions

### 7. The scaling problem, stated sharply

A fault-tolerant superconducting machine needs ~10⁶ physical qubits (File 18). At ~3 wires/qubit and one room-temperature control channel per wire, that is ~3 million cables threading the fridge and ~3 million channels of room-temperature electronics — physically, thermally, and economically impossible. The heat load alone (Section 4) would exceed any dilution refrigerator's capacity by orders of magnitude. **Solving the wiring/control-scaling problem is therefore as essential to fault tolerance as improving qubit fidelity or error-correction overhead** — it is a co-equal pillar of scaling, and one that is *not yet solved*. The approaches:

### 8. Cryo-CMOS control

**Cryo-CMOS** places classical control electronics (DACs, multiplexers, signal generation, even some digital logic) at a *cold stage* (typically ~4 K, where cooling power is ~1 W — far more than the mixing chamber) rather than at room temperature. This drastically reduces the number of room-temperature-to-4K cables: instead of one analog cable per qubit all the way from room temperature, a few digital control lines carry instructions to a cryogenic controller chip that *generates* the many analog signals locally at 4 K, with only short lines from 4 K to the qubits at 10 mK.

- **Intel's Horse Ridge** (2019) and Horse Ridge II are leading cryo-CMOS controller chips, generating microwave control for multiple qubits at cryogenic temperature — Intel leveraging its CMOS-manufacturing expertise (and its spin-qubit program's CMOS synergy, File 7).
- **Google, Delft/QuTech, and others** pursue cryo-CMOS multiplexing and control.
- **The challenges:** CMOS circuits must function correctly at 4 K (transistor behavior changes at cryogenic temperature, requiring re-characterization and redesign), and — critically — must dissipate *little enough power* to stay within the 4 K stage's ~1 W budget when scaled to many channels (power dissipation per channel × channel count must fit the budget). Low-power cryo-CMOS design is the key enabling technology, and reaching the power efficiency needed for millions of qubits is an active, unsolved challenge.

### 9. Multiplexing

**Multiplexing** reduces wire count by sharing lines among qubits:

- **Frequency-domain readout multiplexing** (File 3, Section 11) is already standard: multiple readout resonators at different frequencies share one feedline and one amplifier chain, reading 6–10+ qubits per line.
- **Control multiplexing** (sharing drive/flux lines among qubits via frequency- or time-domain multiplexing) is more challenging (control signals must be individually addressable and high-fidelity) but is actively researched to reduce the per-qubit control wire count.
- The multiplexing factor directly reduces the wire count and heat load, complementing cryo-CMOS.

### 10. Packaging, interconnects, and 3D integration

Physical packaging innovations reduce wiring footprint:

- **High-density interconnects:** flexible printed-circuit cabling instead of discrete coax, high-density microwave connectors.
- **3D integration / flip-chip** (File 3, Section 19): separating the qubit chip from a control-wiring chip, bonded with indium bumps, with vertical interconnects (through-silicon vias) — enabling denser wiring without crowding the qubit plane.
- **Modular multi-fridge architectures:** rather than one giant fridge, link *multiple* dilution refrigerators (each holding a module of qubits) via quantum interconnects (microwave-to-optical transduction or direct cryogenic links, Section 11) — spreading the qubit count and heat load across many fridges. IBM's modular roadmap (System Two, File 3, 19) embodies this.

### 11. Quantum interconnects and transduction

Linking separate cryogenic modules (or a quantum computer to a network, File 15) requires moving quantum information between them. **Microwave-to-optical transduction** — converting a superconducting qubit's microwave photon into an optical photon (which can travel through room-temperature fiber) and back — is a key enabling (and unsolved-at-high-efficiency) technology, pursued via electro-optic, piezo-optomechanical, and atomic transducers. High-efficiency, low-noise transduction would enable modular superconducting machines and networked quantum computing (File 15), breaking the single-fridge scaling ceiling. It is a major research frontier (File 25).

---

## Part IV — The Room-Temperature Control Stack

### 12. Arbitrary waveform generators and the signal chain

At room temperature (or, increasingly, at cryogenic stages via cryo-CMOS), the control system generates the analog signals that manipulate qubits. For superconducting and other microwave-controlled qubits, the signal chain is:

**digital waveform memory → DAC (digital-to-analog converter) → IQ mixing (upconversion to the GHz qubit frequency) → amplification → attenuated cryogenic delivery.**

- **Arbitrary Waveform Generators (AWGs):** generate the baseband pulse envelopes (shaped Gaussians, DRAG pulses, File 3) from stored waveform memory via high-speed DACs.
- **IQ mixing:** the baseband pulse is upconverted to the qubit frequency (4–6 GHz) by mixing with a local oscillator, using in-phase (I) and quadrature (Q) components to control both amplitude and phase (realizing the arbitrary-axis Bloch rotations of File 2, Section 23). IQ imbalance and mixer nonlinearity are calibrated out.
- **Direct digital synthesis (DDS):** an increasingly favored alternative as DAC sample rates rise (into tens of GS/s), directly synthesizing the GHz signal without analog IQ mixing — reducing analog-component count and calibration burden. The trend is toward higher-speed DACs and more digital signal generation.

### 13. Control-hardware vendors

A distinct industry supplies quantum control hardware (File 20 has the business context):

- **Quantum Machines** (OPX systems) — a dedicated quantum-control-stack vendor with an integrated processor-based controller (the "Operator-X") supporting complex real-time feedback and classical processing alongside pulse generation.
- **Zurich Instruments** (part of Rohde & Schwarz) — quantum control and readout instruments.
- **Keysight** — AWGs, digitizers, and control systems.
- **Qblox** — modular control instruments.
- **Quantware, and FPGA-platform vendors** for custom control.

These vendors provide the "classical brains" that many quantum-hardware groups and companies use rather than building control from scratch — a maturing supply chain (File 24). The control system's specifications (channel count, sample rate, latency, real-time processing capability) directly determine what experiments (mid-circuit measurement, adaptive circuits, error correction) are possible.

### 14. FPGA-based real-time control and low-latency feedback

The most demanding control requirement is **low-latency real-time feedback**, needed for:

- **Mid-circuit measurement and adaptive circuits** (File 4, 8): measure some qubits, and *conditionally* apply subsequent gates based on the outcome — requiring the control system to read out, process, and act within the qubits' coherence budget.
- **Real-time error correction** (File 9, Section 11): the round trip of *measure syndrome → amplify/digitize readout → classically decode → apply conditional correction* must complete within roughly one code cycle (~1 μs for superconducting) to avoid a decoding backlog.

This is achieved with **FPGA (Field-Programmable Gate Array) based controllers** that perform readout demodulation, state discrimination, and simple conditional logic in hardware with sub-microsecond latency. For error correction, the **decoder** (MWPM/Union-Find, File 9) must run in real time — implemented on FPGAs or dedicated **ASICs** — a hardware/software co-design challenge that is an active frontier (File 25). The latency budget is severe: for superconducting qubits, the entire feedback loop must fit in ~1 μs; for slower modalities (ions, atoms, with ms-scale operations), the budget is looser (a benefit of slow gates for the *decoding-latency* problem, even as it hurts throughput). Real-time decoding at the *scale* of thousands-to-millions of logical qubits — processing the enormous aggregate syndrome stream within latency — is one of the field's major unsolved classical-systems problems (Files 9, 25).

### 15. Local oscillators, phase noise, and spectral purity

The GHz signals driving qubits must be spectrally pure: **phase noise** on the local oscillator or synthesized signal directly dephases the qubit (it is indistinguishable from a fluctuating qubit frequency) and degrades gate fidelity. High-fidelity gates thus require low-phase-noise microwave sources, and the phase-noise specification tightens as gate fidelity targets rise. This is analogous to the laser-linewidth requirement for atomic qubits (Part VI) — in both cases, the *coherence of the control field* must exceed the qubit coherence for the control not to be the fidelity bottleneck. As the field pushes toward 99.99% gates (File 3), control-field spectral purity becomes an increasingly important limiting factor, and it is a specification the control-hardware vendors (Section 13) compete on.

---

## Part V — Real-Time Feedback and the Classical Co-Processor

### 16. Why classical co-processing is inseparable from quantum computing

A recurring theme: a quantum computer is *inseparable from* a substantial classical computer that controls it. This classical co-processor:

- **Generates** the pulse waveforms (Sections 12–13).
- **Processes** readout signals (demodulation, state discrimination).
- **Decodes** error syndromes in real time (Section 14; File 9).
- **Runs** the classical optimizer in variational algorithms (File 13) and the classical parts of hybrid algorithms.
- **Compiles** and schedules circuits (File 8), sometimes adaptively.

The scale of this classical infrastructure grows with the quantum machine: a million-qubit fault-tolerant computer needs a classical control-and-decoding system of correspondingly enormous scale (millions of control channels, real-time decoding of a massive syndrome stream). The classical infrastructure is thus not a peripheral detail but a *co-equal component* of a quantum computer, and its scaling (control channels, decoding compute, power, and cost) is as much a determinant of feasibility as the qubits themselves (Files 18, 25). This is why "quantum computing" is really "quantum–classical hybrid computing" at the systems level, and why the control/decoding infrastructure of this file is a first-class scaling concern.

### 17. The decoding-latency budget in detail

For real-time surface-code error correction (File 9, Section 11):

- Syndrome extraction repeats every **~1 μs** (superconducting cycle time, File 3).
- The decoder must process each round's syndrome and keep up with this rate *on average*, and provide corrections with bounded latency to avoid a growing **backlog** (the "backlog problem": if decoding is slower than syndrome generation, the queue grows unboundedly and protection fails).
- For a *single* logical qubit, fast decoders (Union-Find on FPGA) can meet this. For a *machine* with thousands of logical qubits, the aggregate syndrome rate is enormous, requiring *parallel* decoding (decomposing the decoding problem across many decoder units) and dedicated **decoder ASICs** — an emerging chip-design niche analogous to AI accelerators (File 25).
- **Modality dependence:** trapped-ion/neutral-atom cycles are slower (ms-scale gate/measurement times), giving a *looser* latency budget — a decoding-latency advantage of slow modalities, partially offsetting their throughput disadvantage (Files 4, 5). Superconducting's fast cycle is a throughput advantage but a decoding-latency *challenge*.

Solving real-time decoding at scale — algorithms, FPGA/ASIC implementations, and parallelization — is a recognized major open problem whose difficulty is easy to underestimate (File 25), because it is a *classical* systems problem hidden behind the quantum one.

---

## Part VI — Laser Systems for Atomic Qubits

### 18. The laser bottleneck

For trapped-ion (File 4) and neutral-atom (File 5) qubits, the analogue of superconducting's wiring bottleneck is the **laser bottleneck**. These qubits need *no cryogenics* (a major advantage), but they require **multiple precisely-stabilized laser beams** per operation type:

- **Cooling lasers** (Doppler and sub-Doppler/sideband cooling).
- **State-preparation (optical pumping) lasers.**
- **Repump lasers** (clearing dark states).
- **Qubit-manipulation lasers** (Raman beams for gates, or narrow-linewidth lasers for optical qubits; Rydberg-excitation lasers for neutral atoms).
- **Detection lasers** (fluorescence readout).
- **Trapping lasers** (optical tweezers for neutral atoms; the trap array).

Each must be **frequency-stabilized** (locked to atomic references or ultra-stable reference cavities, sometimes to sub-Hz linewidth for optical qubits and Rydberg lasers), **intensity-stabilized**, and **precisely delivered** to the atoms/ions with stable beam pointing. Managing this optical complexity — and scaling it to many qubits with individual addressing — is the central scaling challenge for atomic modalities, distinct from but analogous in spirit to the wiring bottleneck.

### 19. Laser stabilization and frequency referencing

- **Frequency stabilization:** lasers are locked (via Pound–Drever–Hall or similar feedback) to **atomic/molecular references** or to **ultra-stable optical reference cavities** (high-finesse cavities in vibration-isolated, temperature-stabilized enclosures) to achieve the narrow linewidth (kHz to sub-Hz) that high-fidelity gates demand. Laser **phase noise** directly dephases atomic qubits (File 4, 5), so laser linewidth is often the gate-fidelity-limiting factor — the optical analogue of microwave phase noise (Section 15).
- **Frequency combs** can provide multiple stabilized frequencies from one reference, easing multi-wavelength stabilization.
- The engineering of many simultaneous ultra-stable beams is a substantial, precision-optics undertaking — the reason atomic-qubit systems, while cryogenics-free, are optically complex laboratory-scale apparatus.

### 20. Beam delivery, modulation, and addressing

- **Acousto-optic modulators (AOMs)** control laser amplitude, frequency, and phase (diffracting light off an acoustic wave, driven by RF) — the optical analogue of the AWG/mixer generating microwave pulses (Section 12). The RF drive to the AOM *is* the gate pulse.
- **Acousto-optic deflectors (AODs)** and **spatial light modulators (SLMs)** steer and shape beams for **individual addressing** (hitting one ion/atom without disturbing neighbors, File 4) and for **optical-tweezer array generation and atom transport** (neutral atoms, File 5).
- **Free-space vs. fiber delivery:** beams are delivered either through free space (precise but alignment-sensitive) or via optical fiber (more stable, integration-friendly). **Integrated photonics** — routing laser light to ions/atoms via on-chip waveguides (File 4, Section 39) — is the emerging scalable solution, the optical analogue of cryo-CMOS: replace a brittle, non-scalable external optical system with an integrated, manufacturable one. Progress in integrated photonics for atomic qubits is a key roadmap dependency (Files 4, 5, 19, 23).

### 21. The parallel between wiring and laser bottlenecks

The deep structural parallel worth internalizing: **every scalable quantum modality faces a classical-control-scaling bottleneck, differing only in physical domain.**

- **Superconducting/spin:** the **wiring bottleneck** — control lines carrying heat into a fixed cryogenic budget; solved by cryo-CMOS, multiplexing, 3D integration, modular fridges.
- **Trapped ion/neutral atom:** the **laser bottleneck** — many stabilized beams and addressing optics; solved by integrated photonics, visible-wavelength species, microwave gates (ions), and shared/global optics (atoms).
- **Photonic:** the **component-count bottleneck** — millions of low-loss sources/switches/detectors; solved (bet) by silicon-photonics foundry manufacturing (File 6).

In every case, the *classical control infrastructure* — not the qubits themselves — is a, often *the*, dominant scaling challenge, and the solutions all take the form of *integration* (cryo-CMOS, integrated photonics, foundry photonics) — replacing brittle, per-qubit, laboratory-scale control with manufacturable, integrated, scalable control. This is arguably the single most important and most underappreciated theme in quantum-computer scaling: the qubits are hard, but the *control infrastructure* is at least as hard, and its integration is a co-equal prerequisite for fault tolerance (Files 18, 25).

---

## Part VII — Shielding, Vacuum, Worked Examples, and Infrastructure Vendors

### 22. Magnetic shielding and vibration isolation

Beyond temperature and wiring, qubits demand a quiet electromagnetic and mechanical environment:

- **Magnetic shielding:** flux-tunable superconducting qubits (File 3) and spin qubits (File 7) are dephased by stray magnetic fields — even Earth's field, nearby magnetized components, and 50/60 Hz line noise. **Mu-metal** (high-permeability alloy) shields and **superconducting shields** (which expel field via the Meissner effect) surround the qubit region. Magnetic-field noise is a leading dephasing source for field-sensitive qubits, so shielding directly affects coherence (Files 3, 4, 7).
- **Vibration isolation:** mechanical vibration (from the pulse-tube cryocooler itself, and from the building) modulates the qubit environment — microphonic noise shifts flux/charge and shakes optical alignments (for atomic qubits). Dilution refrigerators use vibration-isolating mounts and careful mechanical design; atomic systems use vibration-isolated optical tables and reference cavities. The pulse-tube's own vibration is an ironic challenge — the cooler that reaches millikelvin also shakes the sample, requiring careful decoupling.
- **Infrared and radiation shielding:** light-tight, IR-absorbing enclosures prevent stray photons (which generate quasiparticles, File 3) and, at the extreme, radiation shielding against cosmic rays and ambient radioactivity (which cause correlated error bursts, File 3, Section 52; File 9) — an emerging concern for large-scale error correction.

### 23. Vacuum systems for atomic qubits

Trapped-ion (File 4) and neutral-atom (File 5) qubits require **ultra-high vacuum (UHV)**, typically ≤10⁻¹¹ torr, to prevent background-gas collisions that eject or heat the qubits. This demands: careful chamber design (low-outgassing materials, File 23), bakeout (heating the chamber to drive off adsorbed gas), and getter/ion pumps. Many systems **cryopump** (cooling the chamber to ~4–10 K, which freezes out residual gas) to reach even lower pressures and longer qubit lifetimes (days to weeks for a trapped ion). The vacuum system, with its many precisely-aligned optical viewports (for the numerous laser beams and imaging, File 5, 23), is a substantial engineering component of atomic-qubit systems — the atomic-modality analogue of the dilution refrigerator's role for superconducting qubits: a large, precision enclosure providing the pristine environment the qubits need.

### 24. Worked example: the heat-load budget

Consider a dilution refrigerator with a mixing-chamber cooling power of 500 μW at 20 mK. Suppose each qubit's control lines deposit ~1 μW of heat at the mixing chamber (from attenuator dissipation and conduction, a representative figure). Then the fridge supports ~500 qubits before exhausting its cooling power — consistent with the ~100–1000-qubit ceiling of Section 4. Now consider scaling to 10⁶ qubits: at 1 μW/qubit, that is 1 W of heat load at 20 mK — **2000× the cooling budget**, utterly impossible. Even a 100× improvement in per-qubit heat load (via superconducting wiring, better thermalization, and multiplexing) leaves a 20× gap. This is why cryo-CMOS (moving control to the 4 K stage, where ~1 W is available), aggressive multiplexing (fewer wires), and *modular multi-fridge* architectures (spreading qubits across many fridges) are not optional optimizations but *existential requirements* for superconducting fault tolerance (Section 7). The arithmetic is unforgiving, and it is why the wiring/cryogenic problem ranks alongside qubit fidelity and error-correction overhead as a co-equal pillar of the scaling challenge (Files 18, 25).

### 25. Worked example: control-channel count

A 10⁶-physical-qubit superconducting machine at 3 control channels/qubit needs 3×10⁶ control channels. Room-temperature AWG channels cost and occupy substantial space (a rack of control electronics handles ~tens to low-hundreds of channels). 3 million channels of room-temperature electronics would fill a large building and cost billions — clearly untenable. Cryo-CMOS integration (generating many channels from a few digital inputs at 4 K) is the only plausible path, and it requires cryo-CMOS power dissipation per channel low enough that 3×10⁶ channels fit the 4 K stage's ~1 W budget — i.e., **sub-microwatt per channel** at 4 K, an extremely demanding low-power-design target that is not yet met at scale. This calculation quantifies why control-electronics *integration and power efficiency* is a hard, unsolved co-requisite for fault tolerance — the classical control system must itself become a massively-integrated, low-power, cryogenic chip, essentially a specialized supercomputer operating at 4 K (Files 18, 25).

### 26. Infrastructure vendors and the supply chain

A specialized supply chain underpins quantum computing (File 24):

- **Dilution refrigerators:** Bluefors (Finland, market leader), Oxford Instruments, and others — supplying the DRs nearly every superconducting/spin-qubit group uses.
- **Cryogenic components:** cryogenic amplifiers (Low Noise Factory for HEMTs; various for TWPAs/JPAs), attenuators, filters, cables, and connectors.
- **Control electronics:** Quantum Machines, Zurich Instruments, Keysight, Qblox (Section 13).
- **Lasers and optics:** Toptica, MSquared, and others for the stabilized lasers atomic qubits need.
- **Cryogenic CMOS:** Intel (Horse Ridge) and emerging startups.

This maturing supply chain is itself a sign of the field's industrialization (File 24) — quantum-hardware groups increasingly buy standardized infrastructure (fridges, control systems, lasers) rather than building everything bespoke, accelerating progress but also concentrating supply-chain dependencies (e.g., the near-monopoly on dilution refrigerators, or on He-3, a scarce resource). Supply-chain considerations — He-3 availability, cryogenic-component lead times, control-electronics scaling — are real factors in the field's scaling trajectory and a component of the national-program and market discussions (Files 21, 24).

---

## Part VIII — Deeper Dives: Cryo-CMOS, Decoder Hardware, and the System View

### 27. Cryo-CMOS in depth

Cryo-CMOS (Section 8) is the leading hope for the superconducting wiring bottleneck, and its engineering is subtle:

- **Transistor physics at 4 K:** CMOS transistors behave differently at cryogenic temperature — threshold voltages shift, mobility changes, and new effects (e.g., freeze-out, kink effects) appear. Standard-cell libraries and models are room-temperature; cryo-CMOS requires *re-characterizing* transistor behavior at 4 K and often redesigning circuits. Foundries and research groups are building cryogenic device models.
- **Power dissipation is the binding constraint:** the 4 K stage offers ~1 W. A cryo-CMOS controller generating microwave signals for N qubits dissipates power per channel (DACs, mixers, logic); scaling to millions of channels demands *sub-μW per channel* (Section 25) — far below current cryo-CMOS power budgets. Ultra-low-power cryogenic circuit design is the make-or-break challenge.
- **Signal quality:** cryo-CMOS-generated control must match the fidelity of room-temperature electronics (low phase noise, precise amplitude/timing) — a demanding specification for cryogenic circuits.
- **Integration proximity:** the ideal is co-integrating control CMOS *with* the qubits (especially natural for spin qubits, which are themselves CMOS-compatible, File 7), eliminating most wiring. For superconducting qubits, the control CMOS sits at 4 K while qubits are at 10 mK, so short lines still cross that gap, but the room-temperature-to-4K cabling (the bulk of the problem) is drastically reduced.

Intel's Horse Ridge (Section 8) demonstrated the concept; scaling it to the power efficiency and channel count of a fault-tolerant machine is an unsolved, actively-pursued challenge — arguably as important as any qubit improvement for superconducting scaling (Files 18, 25). The convergence of cryo-CMOS for both superconducting *control* and spin-qubit *co-integration* is a reason Intel's dual bet (spin qubits + cryo-CMOS) is strategically coherent (Files 7, 19).

### 28. Decoder hardware for real-time error correction

The real-time decoding challenge (Sections 14, 17; File 9) is spawning a dedicated hardware sub-field:

- **FPGA decoders:** current demonstrations (Google, Riverlane, academic groups) implement Union-Find and streamlined MWPM decoders on FPGAs, achieving the ~1 μs latency for small codes. **Riverlane** (UK) is a company specifically building decoder hardware/IP (a "quantum error correction stack") as a picks-and-shovels play (File 20).
- **ASIC decoders:** for the scale of a full machine (thousands of logical qubits, aggregate syndrome rate overwhelming any single processor), dedicated **decoder ASICs** — custom chips optimized for the matching/BP+OSD decoding computation — will likely be needed, an emerging chip-design niche (File 25). The analogy to AI accelerators (custom silicon for a specific, massively-parallel workload) is apt: error-correction decoding is a specialized, latency-critical, high-throughput classical workload that general-purpose CPUs cannot meet at scale.
- **Decoder/hardware co-design:** the decoder, the control system, and the qubit hardware must be co-designed so the feedback loop (measure → decode → correct) closes within the coherence budget. This tight coupling of classical decoding hardware and quantum hardware is a defining feature of fault-tolerant system engineering, and its difficulty (a classical real-time-computing problem hidden behind the quantum one) is easy to underestimate.

### 29. The full system view

Assembling the picture, a quantum computer is a deeply layered *system*:

1. **Qubits** (Files 3–7) at the bottom — the quantum layer.
2. **Cryogenics / vacuum** (this file) providing the pristine environment (10 mK for superconducting; UHV for atomic).
3. **Analog control delivery** (wiring/attenuation/filtering, or laser beams/optics) carrying signals to the qubits.
4. **Signal generation** (AWGs/mixers, or AOMs/lasers) creating the control pulses — increasingly integrated (cryo-CMOS, integrated photonics).
5. **Real-time classical processing** (FPGA/ASIC readout demodulation, state discrimination, and — for error correction — decoding) with sub-μs latency.
6. **Higher-level classical control** (compilation, scheduling, variational optimization, File 8, 13).
7. **The user interface** (cloud access, File 12).

Each layer must scale together. A machine with excellent qubits but an inadequate control/cryogenic system cannot scale; the *system* scales at the pace of its *slowest-scaling layer*, which for superconducting is currently the wiring/cryogenic/control layer (this file), and for atomic is the laser/optical-control layer. This system view is why "how many qubits does it have?" is such an incomplete question (Files 1, 22): a quantum computer is a co-scaled quantum–classical system, and its capability is gated by the whole stack, not the qubit count alone.

### 30. Modality-specific infrastructure summary

| Infrastructure | Superconducting | Spin | Trapped ion | Neutral atom | Photonic |
|---|---|---|---|---|---|
| Qubit temperature | ~10 mK (DR) | <100 mK–1 K (DR) | room temp (chip ~4–10 K) | room temp, UHV | room temp |
| Aux cryogenics | — | — | vacuum/cryopump | vacuum | SNSPD ~1–4 K |
| Dominant bottleneck | wiring/heat load | wiring + uniformity | lasers/optics | lasers/optics | component count |
| Control signals | microwave + flux | voltages + microwave | lasers/microwave | lasers | optical switches |
| Integration solution | cryo-CMOS, multiplexing | cryo-CMOS co-integration | integrated photonics | shared/global optics | foundry photonics |
| Real-time feedback | FPGA/ASIC, ~1 μs budget | FPGA, tight budget | looser (ms gates) | looser (ms ops) | fast (measurement-based) |
| Key vendors | Bluefors, Quantum Machines | Intel | Toptica, integrated photonics | SLM/AOD optics | GlobalFoundries (PsiQ) |

The table makes concrete that *every* modality has a substantial classical-infrastructure challenge, differing in physical domain but universal in importance — and that the *integration* of that infrastructure (cryo-CMOS, integrated photonics, foundry photonics) is the common solution shape and a co-equal scaling prerequisite alongside qubit quality (Files 18, 25).

---

## Part IX — History, Supply Constraints, and Deeper Laser Engineering

### 31. A brief history of getting to millikelvin

The dilution refrigerator was proposed by Heinz London (1951) and first realized in the 1960s. For decades it was a specialized low-temperature-physics tool. The rise of superconducting qubits (File 3) transformed it into industrial equipment: as the field grew, "dry" (cryocooler-based, no liquid-helium bath) dilution refrigerators became commercially mature (Bluefors, Oxford Instruments), and the "chandelier" of gold-plated plates became the iconic image of quantum computing. The engineering has advanced from single-experiment lab fridges to systems designed for continuous operation, high wiring density, and (IBM's Goldeneye and System Two) very large volumes to accommodate more qubits and modular chips. The trajectory — from a physics curiosity to industrial infrastructure — mirrors the field's broader industrialization (File 24), and the dilution refrigerator's continued evolution (larger, higher-cooling-power, higher-wiring-density) is a quiet but essential enabler of scaling.

### 32. The helium-3 supply constraint

A specific supply-chain vulnerability: **helium-3 (³He)**, the working fluid of the dilution refrigerator, is *extremely scarce*. ³He is produced almost entirely as a byproduct of tritium decay (tritium being produced in nuclear reactors, historically for weapons), so its supply is limited, geopolitically concentrated, and not easily scaled. As quantum computing scales to thousands of dilution refrigerators (a modular fault-tolerant machine might need many fridges, Section 10; and every superconducting-hardware group needs fridges), ³He demand could strain supply and raise costs. This is a genuine, if under-discussed, scaling constraint and supply-chain risk (Files 21, 24) — a reminder that quantum-computer scaling depends not only on physics and engineering but on the availability of scarce physical resources. Mitigations include ³He recycling/reclamation (fridges are closed-cycle, but losses occur) and research into ³He-conserving or alternative cooling approaches.

### 33. Laser engineering in more depth

Expanding on Part VI, the laser systems for atomic qubits involve substantial precision engineering:

- **Diode lasers and amplification:** most atomic-qubit wavelengths are reached with semiconductor diode lasers (external-cavity diode lasers, ECDLs, for narrow linewidth), often amplified (tapered amplifiers) and frequency-doubled (for blue/UV wavelengths via nonlinear crystals). Reaching UV wavelengths (needed for some ions like Yb⁺ at 369 nm, File 4, and some Rydberg transitions, File 5) requires multiple frequency-doubling stages, with efficiency and stability challenges — a reason visible-wavelength species (Ba⁺, File 4) are attractive.
- **Reference cavities:** for the sub-Hz linewidths that optical qubits and Rydberg lasers need, lasers are locked to ultra-stable high-finesse optical cavities held in vibration-isolated, temperature-stabilized, sometimes cryogenic enclosures — precision instruments in their own right (shared with optical-clock technology, File 16).
- **Optical distribution:** delivering many stabilized beams to the qubits via free-space optics or fiber, with acousto-optic modulators (AOMs) for per-beam control and AOD/SLM systems for addressing and array control (Section 20). The optical table for a trapped-ion or neutral-atom system is a dense, precisely-aligned assembly — the atomic-modality counterpart to the superconducting fridge's wiring chandelier.
- **Integrated photonics** (Section 20; File 4, 23) is the scalable future — routing laser light on-chip — reducing the free-space optical complexity, analogous to cryo-CMOS reducing wiring.

### 34. Worked example: laser stability requirement

Consider a trapped-ion optical qubit (File 4) with a gate time of ~10 μs. For the laser phase not to be the fidelity bottleneck, the laser's coherence time (inverse linewidth) must far exceed the gate time and, ideally, the qubit coherence. A laser linewidth of 1 Hz gives a coherence time of ~1 s — comfortably exceeding the 10 μs gate and even long qubit coherence — but achieving sub-Hz linewidth requires locking to an ultra-stable reference cavity (as above). A laser linewidth of 1 kHz (coherence ~1 ms) would still exceed the gate time but might limit long-sequence coherence. This is why optical-qubit and Rydberg-laser systems invest heavily in laser stabilization: the *control-field coherence must exceed the qubit coherence*, exactly paralleling the microwave phase-noise requirement for superconducting qubits (Section 15). The lesson is universal across modalities: **the classical control field must be quieter than the qubit it controls**, and meeting this — whether via low-phase-noise microwave sources or sub-Hz-linewidth lasers — is a precision-engineering prerequisite for high fidelity.

### 35. Power, cost, and footprint

The classical infrastructure dominates a quantum computer's *physical* footprint, power, and cost:

- A dilution refrigerator plus its control electronics fills a room; a large atomic-qubit system fills an optical-lab-scale space.
- Power consumption is dominated by the *classical* systems — the cryocooler compressors (kilowatts), the control electronics, and the classical computing (decoding, optimization) — not the qubits (whose energy scale is minuscule). A quantum computer is, in power terms, mostly a classical machine.
- Cost is similarly dominated by infrastructure: dilution refrigerators (~$0.5–3M each), control electronics, lasers, and the facility.

This has a strategic implication (Files 18, 24): the *cost and power* of scaling a quantum computer are largely the cost and power of scaling its *classical infrastructure* — fridges, control channels, decoders, lasers — which is why infrastructure integration (cryo-CMOS, integrated photonics) is essential not just for physical feasibility but for economic viability. A million-qubit machine's cost and power will be dominated by its control-and-cryogenic system, making that system's efficiency a first-order determinant of whether fault-tolerant quantum computing is economically practical, not just physically possible.

---

## Part X — Frontier Problems, FAQ, and Extended Analysis

### 36. Frontier problems (infrastructure-specific)

Cross-referenced with File 25, the classical-infrastructure frontier problems are:

1. **Low-power cryo-CMOS at scale:** achieving sub-μW-per-channel control generation at 4 K to fit millions of channels within the cooling budget — the binding constraint for superconducting scaling (Sections 25, 27).
2. **Real-time decoding at scale:** decoder ASICs and parallel decoding architectures processing the aggregate syndrome stream of thousands of logical qubits within latency (Sections 14, 28; File 9).
3. **Microwave-to-optical transduction:** high-efficiency, low-noise conversion to enable modular multi-fridge and networked superconducting machines (Section 11; File 15) — breaking the single-fridge ceiling.
4. **Integrated photonics for atomic qubits:** on-chip laser delivery and control to tame the laser bottleneck (Sections 20, 33; File 4, 5).
5. **Higher-cooling-power, higher-density dilution refrigerators** and ³He-conserving cooling to support more qubits per fridge (Sections 31–32).
6. **Modular quantum interconnects:** high-fidelity chip-to-chip and fridge-to-fridge links to scale beyond a single module (Sections 10–11; File 3, 15).
7. **Wiring-and-packaging density:** 3D integration, high-density interconnects, and on-chip nonreciprocal elements (replacing bulky isolators) to fit more qubits' control into the fridge (Sections 6, 10).

These are *classical* engineering problems that gate quantum scaling, and progress on them is as essential as qubit-fidelity improvement — a point the qubit-count-focused public narrative (File 22) systematically underweights.

### 37. FAQ

**Q: Why is the fridge so big if the qubit chip is tiny?** Because the fridge is not cooling the tiny chip in isolation — it must provide staged cooling (multiple plates), house the thousands of thermalized/attenuated control lines, the amplifiers, filters, and shielding, and pump the ³He/⁴He circulation. The infrastructure to *create and maintain* the millikelvin, low-noise environment and to deliver clean control is what fills the room, not the qubits.

**Q: Do trapped ions and neutral atoms really avoid cryogenics?** The *qubits* do (laser-cooled in room-temperature vacuum), which is a genuine advantage. But many systems cool the ion-trap chip to ~4–10 K (for vacuum and lower heating), and the systems require large, precise laser/optical apparatus and UHV chambers — so they trade the fridge for a different, comparably substantial infrastructure (the laser bottleneck, Section 18). "No cryogenics" does not mean "no infrastructure."

**Q: Is the wiring bottleneck really as important as qubit fidelity?** Yes — it is a co-equal pillar. The heat-load and channel-count arithmetic (Sections 24–25) shows that conventional per-qubit wiring physically cannot reach millions of qubits; cryo-CMOS and multiplexing are *required*, not optional. A machine with perfect qubits but no wiring-scaling solution cannot be built at fault-tolerant scale.

**Q: What's the classical computer doing during a quantum computation?** A great deal (Section 16): generating pulses, demodulating and discriminating readout, decoding error syndromes in real time (File 9), running variational optimizers (File 13), and compiling/scheduling (File 8). For error correction, the classical decoding must keep pace with syndrome extraction in real time — a formidable classical-computing task that scales with the quantum machine.

**Q: Why can't we just use more fridges?** Modular multi-fridge architectures (Section 10) are indeed a key strategy, but they require high-fidelity *quantum interconnects* between fridges (Section 11) — microwave-to-optical transduction or direct cryogenic links — which are not yet mature. And each fridge still faces its own wiring bottleneck internally. Modularity spreads the problem but doesn't eliminate the per-fridge and inter-fridge challenges.

**Q: How does the infrastructure affect resource estimates (File 18)?** Directly: the cryogenic cooling power caps qubits-per-fridge; the control-channel count and power set the classical-electronics scale; the decoder latency sets the logical clock speed; and the modality's cycle time (fast superconducting vs. slow ion) trades throughput against decoding-latency budget. These infrastructure parameters are inputs to resource estimation alongside code overhead and gate fidelity.

### 38. Extended analysis: the co-scaling imperative

The deepest lesson of this file is the **co-scaling imperative**: a quantum computer scales only as fast as its slowest-scaling layer, and for the leading modalities that layer is currently the *classical control and cryogenic/optical infrastructure*, not the qubits. Historically, the field focused on qubit count and fidelity (the quantum layer), and the infrastructure was treated as a solvable engineering detail. But the arithmetic (Sections 24–25) reveals that conventional infrastructure *cannot* reach fault-tolerant scale, and that the required integration (cryo-CMOS, integrated photonics, decoder ASICs, transduction) is *itself* a set of hard, unsolved problems. The field's maturation (2020s) includes a growing recognition that **infrastructure integration is a co-equal grand challenge alongside qubit quality and error-correction overhead** — reflected in the emergence of infrastructure-focused companies (Riverlane for decoders, Quantum Machines for control, Bluefors for fridges, integrated-photonics efforts) and in roadmaps (File 19) that explicitly address control/cryogenic scaling. An analyst assessing a modality's or company's prospects must weigh not just its qubit metrics but its *infrastructure-scaling* plan: does it have a credible path to cryo-CMOS or integrated photonics, to real-time decoding at scale, to modular interconnects? These often-overlooked classical questions are as determinative of the path to utility as the qubit fidelities that dominate the headlines.

### 39. Worked example: modality clock speed and decoding

Combining infrastructure and error correction: a superconducting surface code runs at ~1 μs/cycle, so a distance-d logical operation (~d cycles, File 9) takes ~d μs. At d=25, that is ~25 μs/logical-gate, and the decoder must process syndromes at 1 μs/round × (number of logical qubits) — for 1000 logical qubits, ~10⁹ syndrome bits/second aggregate, demanding parallel ASIC decoding (Section 28). A trapped-ion machine, with ~1 ms operation times, runs the *same* logical operation ~1000× slower (~25 ms/logical-gate) but faces a ~1000× *looser* decoding-latency budget (ms instead of μs per round) — so its decoder can be simpler/slower. This trade — fast throughput but tight decoding latency (superconducting) vs. slow throughput but relaxed decoding (ions) — is a concrete infrastructure-level manifestation of the modality speed/quality trade-off (Files 3, 4), and it feeds resource estimation (File 18): superconducting fault-tolerant machines run algorithms faster but need harder decoding hardware; ion machines run slower but with simpler decoding. Neither is strictly better — it depends on whether the application is time-limited or the decoding hardware is the binding constraint.

---

## Part XI — Component Deep-Dives and Glossary

### 40. The cryogenic amplifier chain, component by component

The readout output chain (Section 6) deserves component-level detail, because it enables the fast single-shot readout on which error correction depends:

- **Parametric amplifier (JPA/TWPA):** at the mixing chamber (~10 mK) or a nearby cold stage. The JPA (Josephson Parametric Amplifier) is narrowband (~tens of MHz) but quantum-limited; the TWPA (Traveling-Wave Parametric Amplifier) is broadband (~GHz), enabling multiplexed readout of many qubits (File 3). This first stage sets the overall noise performance — near the quantum limit of half a photon added noise. Pumped by a strong microwave tone, it must be carefully biased and its pump routed and filtered.
- **Isolators/circulators:** ferrite-based non-reciprocal components protecting the qubit and parametric amplifier from noise reflected back down the line. They are *bulky* (cm-scale) and are a packaging bottleneck at scale — replacing them with compact on-chip nonreciprocal elements (Josephson-based circulators) is a research goal.
- **HEMT amplifier:** at ~4 K, a High-Electron-Mobility-Transistor semiconductor amplifier providing ~30–40 dB of gain with low (but not quantum-limited) noise — the workhorse second stage.
- **Room-temperature chain:** further amplification, IQ demodulation (down-converting the GHz readout signal to baseband), and digitization (ADC), followed by digital state discrimination (often a trained classifier separating |0⟩/|1⟩ readout clouds, File 3).

Every element adds latency and noise, and every qubit (or multiplexed group) needs a chain — so the amplifier chain is both a fidelity enabler and a scaling burden (bulky isolators, per-group amplifiers). Improving it (integrated nonreciprocal elements, higher-multiplexing TWPAs, faster digitizers) directly aids scaling.

### 41. Flux control and pre-distortion

For tunable superconducting qubits and couplers (File 3), the **flux-bias (Z control)** lines carry DC + fast baseband currents that set the qubit/coupler frequency. A subtlety: the flux line's frequency response *distorts* fast flux pulses (the line acts as a filter with its own transfer function), so the qubit sees a *distorted* version of the intended waveform — degrading gate fidelity. The fix is **pre-distortion**: measure the line's response (via a "cryoscope" — using the qubit itself as a sensor of the flux waveform it experiences) and pre-distort the applied waveform so the qubit sees the intended shape. Cryoscope calibration and flux-line pre-distortion are standard, essential techniques for high-fidelity tunable-coupler gates (File 3, Section 33). This is an example of the control system needing to compensate for its own imperfections — a recurring theme in precision quantum control.

### 42. Timing, synchronization, and phase coherence

A large quantum processor requires all control channels to be **precisely synchronized** and **phase-coherent**: the microwave sources driving different qubits must share a common phase reference (so that, e.g., a two-qubit gate's relative phase is well-defined), and the timing of pulses across channels must be aligned to sub-ns precision. This demands a common clock/local-oscillator distribution network and careful calibration of channel-to-channel delays. As channel counts grow (Section 25), maintaining synchronization and phase coherence across millions of channels is a nontrivial systems challenge, and it is part of why control-system *integration* (cryo-CMOS with shared on-chip clock distribution) is attractive — integration eases synchronization compared to many discrete room-temperature instruments.

### 43. Glossary

- **Dilution refrigerator (DR):** the closed-cycle ³He/⁴He fridge reaching ~10 mK continuously; the environment for superconducting/spin qubits.
- **Mixing chamber:** the coldest stage (~10–20 mK) where the qubit chip mounts; its μW-scale cooling power sets the wiring bottleneck.
- **Pulse-tube cryocooler:** the mechanical cooler providing the 4 K stage in "dry" DRs.
- **Cooling power:** heat the fridge can remove at a stage; ~1 W at 4 K, ~μW at 10 mK — the key constraint.
- **Wiring bottleneck:** the limit on qubits-per-fridge from control-wire heat load vs. fixed cooling power.
- **Cryo-CMOS:** classical control electronics operating at cryogenic temperature (~4 K) to reduce room-temp wiring (Intel Horse Ridge).
- **Multiplexing:** sharing control/readout lines among qubits (frequency-domain readout is standard).
- **Attenuation/thermalization/filtering:** suppressing thermal noise, heat-sinking lines, and blocking IR photons/quasiparticles.
- **TWPA/JPA/HEMT:** traveling-wave / Josephson parametric amplifiers (quantum-limited) and HEMT (4 K) — the readout amplifier chain.
- **AWG:** arbitrary waveform generator producing control pulse envelopes.
- **FPGA/ASIC decoder:** real-time hardware for error-correction decoding within the cycle-latency budget.
- **Microwave-to-optical transduction:** converting microwave qubit signals to optical photons for modular/networked links.
- **Laser bottleneck:** the atomic-modality analogue — many stabilized beams and addressing optics; solved by integrated photonics.
- **AOM/AOD/SLM:** acousto-optic modulators/deflectors and spatial light modulators for laser control and beam steering.
- **UHV:** ultra-high vacuum (≤10⁻¹¹ torr) for atomic qubits.
- **³He supply:** the scarce dilution-refrigerator working fluid — a supply-chain constraint.
- **Cryoscope / pre-distortion:** measuring and compensating flux-line distortion of fast pulses.

### 44. Summary

Behind every quantum processor stands an enormous classical infrastructure that is, for the leading modalities, the dominant scaling bottleneck. Superconducting and spin qubits require dilution refrigeration to ~10 mK, where the microwatt-scale cooling power caps qubits-per-fridge and creates the **wiring bottleneck** — solved (in progress) by cryo-CMOS, multiplexing, 3D integration, and modular multi-fridge architectures with quantum interconnects. Atomic qubits avoid cryogenics but face the analogous **laser bottleneck** of many stabilized beams and addressing optics, solved (in progress) by integrated photonics, visible-wavelength species, and shared/global optics. The room-temperature (and increasingly cryogenic) control stack — AWGs, mixers, quantum-limited amplifiers, and FPGA/ASIC real-time controllers — must generate clean pulses and close the measure-decode-correct feedback loop within microseconds for error correction, spawning a dedicated decoder-hardware sub-field. The classical infrastructure dominates a quantum computer's footprint, power, and cost, and it scales only as fast as its integration (cryo-CMOS, integrated photonics, decoder ASICs, transduction) — making infrastructure integration a **co-equal grand challenge alongside qubit quality and error-correction overhead**, and one the qubit-count-focused narrative systematically underweights. An analyst assessing a modality or company must weigh its infrastructure-scaling plan as heavily as its qubit metrics: the path to fault tolerance (File 18) runs as much through cryo-CMOS, real-time decoding, and integrated photonics as through better qubits. The reader should carry the co-scaling imperative — a quantum computer is a co-scaled quantum–classical system, gated by its slowest layer — into resource estimation (File 18), roadmaps (File 19), and the frontier (File 25), where the classical infrastructure's maturation is a central determinant of when, and whether, useful fault-tolerant quantum computing arrives.

*Cross-references: superconducting readout chain, quasiparticles, flux control (File 3); trapped-ion lasers and integrated photonics (File 4); neutral-atom optics, SLM/AOD (File 5); SNSPD detector cryogenics (File 6); spin-qubit cryo-CMOS co-integration (File 7); real-time decoding for error correction (File 9); compilation and adaptive circuits needing feedback (File 8); resource estimation with infrastructure parameters (File 18); roadmaps addressing control scaling (File 19); ³He and supply chain (Files 21, 24); materials for cryogenic packaging and vacuum (File 23); infrastructure integration as a frontier (File 25).*

---

## Part XII — Extended Engineering Analysis and Additional Worked Examples

### 45. The thermal budget across all fridge stages

To fully appreciate the wiring bottleneck, consider the heat budget at *every* stage, not just the mixing chamber:

- **50 K stage:** cooling power ~tens of watts; here the room-temperature-to-cold thermal conduction of the wires deposits the most heat, but the cooling power is large, so many wires are tolerable.
- **4 K stage:** cooling power ~1–2 W; cryo-CMOS control electronics (if used) dissipate here, and this stage's budget is what limits cryo-CMOS channel count (Section 27).
- **Still (~700 mK):** cooling power ~milliwatts.
- **Cold plate (~100 mK):** cooling power ~hundreds of μW.
- **Mixing chamber (~10 mK):** cooling power ~10s–100s of μW — the tightest budget.

Each wire must be thermalized at *every* stage (heat-sunk so heat flows to the plate), and attenuators are placed at the 4 K and mixing-chamber stages to suppress thermal noise (Section 5). The design optimizes the attenuation distribution across stages to balance noise suppression against heat load at each stage's budget — a multi-stage thermal-engineering optimization. The mixing-chamber budget is tightest, but the 4 K budget becomes binding for cryo-CMOS. Understanding the *staged* thermal budget clarifies why the solution is not just "one clever trick" but a coordinated redesign — moving control to 4 K (cryo-CMOS), multiplexing to reduce wire count, and superconducting wiring to reduce conduction — each addressing a different stage's constraint.

### 46. Worked example: superconducting vs. normal-metal wiring heat load

A wire conducts heat between stages at a rate set by its thermal conductivity and geometry (Fourier's law). **Normal-metal** coax (e.g., copper) conducts heat well (bad — high heat load) but also electricity well (good — low signal loss). **Superconducting** coax (NbTi) conducts electricity with zero resistance (good) but conducts *heat* poorly below its critical temperature (good — low heat load), because in the superconducting state the electronic contribution to thermal conductivity is suppressed (the Wiedemann–Franz law breaks down — Cooper pairs carry charge but not entropy/heat). So superconducting NbTi coax is ideal for the *cold* segments (below NbTi's ~9 K critical temperature): it delivers the signal with low loss while conducting little heat. This is why the coldest wiring segments use superconducting cables — a materials choice (File 23) directly addressing the heat-load budget. The trade-off: superconducting cables are used only where the temperature is below their critical temperature, so the warmer segments use resistive materials (stainless steel) chosen for *low thermal conductance* at the cost of some signal loss (compensated by driving higher power from room temperature). This segment-by-segment materials optimization is a core part of cryogenic wiring engineering.

### 47. Worked example: readout multiplexing scaling

Frequency-domain readout multiplexing (File 3, Section 11; Section 9) shares one feedline and one TWPA among many qubits. Suppose a TWPA has ~4 GHz of bandwidth and each qubit's readout resonator occupies ~50 MHz of spectrum (frequency spacing to avoid crosstalk). Then one feedline+TWPA can read out ~4000 MHz / 50 MHz ≈ **~80 qubits** in principle (in practice fewer, ~6–20, due to crosstalk, power, and calibration constraints, but the trend is toward higher multiplexing). This multiplexing factor directly reduces the readout wire count and amplifier count by ~10×, materially easing the wiring bottleneck for the *readout* portion. Extending similar multiplexing to *control* (drive/flux) lines — harder, because control must be individually addressable and high-fidelity — is a key research direction (Section 9). The multiplexing factor is a direct lever on qubits-per-fridge: higher multiplexing means more qubits per wire means more qubits per cooling budget.

### 48. Worked example: decoder throughput

Quantify the real-time decoding challenge (Sections 14, 28). A distance-d surface-code logical qubit generates ~d² stabilizer measurements per cycle, at ~1 μs/cycle. For d=25, that is ~625 syndrome bits/μs = 6.25×10⁸ bits/second *per logical qubit*. For a 1000-logical-qubit machine, the aggregate is ~6×10¹¹ syndrome bits/second — a torrent that must be decoded with bounded latency to avoid backlog (Section 17). No single CPU can process this; the solution is *spatial parallelism* (each logical qubit's syndrome decoded by its own decoder unit, with communication only at logical-gate boundaries) implemented in FPGAs/ASICs (Section 28). This throughput calculation quantifies why real-time decoding at scale is a genuine unsolved problem (File 9, 25) and why dedicated decoder silicon is emerging — the classical decoding load scales with the quantum machine and is easy to underestimate. It is a striking illustration of the co-scaling imperative (Section 38): a million-physical-qubit machine implies a correspondingly enormous *classical* real-time decoding system.

### 49. The economics of infrastructure at scale

Projecting to a fault-tolerant machine (File 18): if a machine needs, say, 20 fridges (modular, Section 10), each ~$2M, that is ~$40M in fridges alone; plus millions of control channels (cryo-CMOS chips, whose cost depends on yield and integration); plus a large decoder-ASIC system; plus lasers (if atomic) or the full microwave stack (if superconducting); plus the facility, power, and cooling. The *classical infrastructure* — not the quantum chips — dominates this cost, and its *scaling economics* (cost per qubit as the machine grows) depend entirely on *integration*: bespoke, discrete infrastructure has terrible scaling economics (linear or worse cost per qubit), while integrated infrastructure (cryo-CMOS chips manufactured at scale, integrated photonics on wafers) could achieve the favorable per-qubit cost scaling that makes large machines economically viable. This is why the *manufacturing* theses (photonics' foundry bet, File 6; spin qubits' CMOS bet, File 7; and cryo-CMOS/integrated-photonics control integration, this file) are so strategically central: they are bets that *integration* will deliver both the physical feasibility *and* the favorable cost scaling that fault-tolerant quantum computing requires. An analyst evaluating the path to utility (Files 18, 24) must therefore assess not just whether the infrastructure *can* be built, but whether it can be built *economically at scale* — a question of integration and manufacturing as much as of physics.

### 50. Closing analysis

The classical infrastructure of quantum computing is the field's great hidden challenge — less glamorous than qubit physics, but co-equally determinative of the path to utility. The dilution refrigerator's microwatt cooling budget and the atomic system's laser complexity impose hard scaling ceilings that conventional per-qubit control cannot surpass; the solutions (cryo-CMOS, integrated photonics, multiplexing, modular interconnects, decoder ASICs, transduction) are themselves hard, partly-unsolved integration problems; and the infrastructure dominates the machine's footprint, power, and cost, making its *economic* scaling as important as its physical feasibility. The co-scaling imperative — a quantum computer scales only as fast as its slowest layer, currently the classical infrastructure for the leading modalities — reframes the scaling challenge: it is not enough to build better qubits; one must simultaneously build the integrated, low-power, manufacturable classical control-and-cooling system that can deliver control to, and extract syndromes from, millions of qubits within the thermal, latency, and cost budgets. This file's lesson — that the "boring" classical infrastructure is in fact a central grand challenge — should recalibrate any assessment of quantum-computing progress: track the fridges, the control-channel integration, the decoder hardware, and the laser/photonic integration as closely as the qubit counts and fidelities, because the path to useful fault-tolerant quantum computing (Files 18, 19, 25) runs through all of them together.

---

## Part XIII — Additional Topics and Reader's Takeaway

### 51. Photonic-detector cryogenics

Photonic quantum computing (File 6) is often described as "room-temperature," but its **detectors** — superconducting nanowire single-photon detectors (SNSPDs) — require **~1–4 K** cryogenics. This is a milder requirement than superconducting qubits' 10 mK (a simpler cryostat, often a closed-cycle 4 K cryocooler without the full dilution stage), but it is not zero. For a large photonic machine with millions of detectors (File 6), integrating and cooling that many SNSPDs, and reading out their signals, is a substantial cryogenic-and-electronics challenge in its own right — the photonic analogue of the wiring bottleneck, applied to detectors. So photonics' cryogenic escape is *partial*: the qubits (photons) are room-temperature, but the detectors are cold, and detector integration/cooling at scale is a real infrastructure problem (Files 6, 23). This nuance is important for honest cross-modality comparison (File 7): no leading modality is entirely cryogenics-free at scale.

### 52. Isolator-free and on-chip nonreciprocal architectures

The bulky ferrite isolators/circulators (Section 40) protecting qubits from amplifier back-action are a packaging bottleneck — cm-scale components, one or more per readout chain, crowding the fridge. A research thrust builds **on-chip nonreciprocal elements** — Josephson-junction-based circulators and directional amplifiers integrated onto the qubit or readout chip — replacing the bulky ferrites with compact, integrated components. Success here would substantially ease the packaging density (more qubits' readout in the same fridge volume) and is part of the broader integration theme (cryo-CMOS, on-chip amplifiers, on-chip isolators) that shrinks the per-qubit infrastructure footprint. It is a good example of how *packaging and component integration* — not just qubit or code improvements — are essential to scaling, and of how the field is progressively integrating what were once bulky discrete lab components onto chips.

### 53. Comparison with classical supercomputer infrastructure

An illuminating comparison: a large classical supercomputer or data center is also dominated by *infrastructure* — power delivery (megawatts), cooling (chilled water/air), interconnect, and physical footprint — with the compute chips a small fraction of the total system cost/power/space. Quantum computers follow the same pattern, *more extremely*: the cooling (to millikelvin, not just below ambient), the control wiring, and the classical co-processing dominate, while the qubit chip is tiny. The analogy suggests that quantum-computer scaling, like classical-supercomputer scaling, will be an *infrastructure and systems-integration* discipline as much as a device-physics one — and that the companies and national programs that master the *systems integration* (not just the qubit physics) will be advantaged (Files 19, 20, 21). It also suggests that quantum-classical *hybrid* infrastructure (quantum co-processors tightly integrated with classical HPC, File 25) is the natural deployment model, sharing cooling, power, and interconnect infrastructure between the quantum and classical parts. This systems-level framing — quantum computing as a specialized, infrastructure-heavy addition to classical computing infrastructure, not a standalone replacement — is a grounding corrective to "quantum computer" imagery (File 25).

### 54. The control system as the true product interface

From a user's perspective (File 12), the control system *is* the quantum computer — it is what the cloud interface talks to. When a user submits a circuit (File 12), the control system compiles it to pulses, generates the analog waveforms, executes the circuit with any real-time feedback, reads out, and returns results. The control system's capabilities (channel count, real-time feedback, mid-circuit measurement, error correction support) determine what the user can do, and its calibration state (drifting, File 3, 8) determines the quality of results. So the control system, though "classical infrastructure," is also the *functional interface* between the abstract quantum computer and the user — a further reason it is central, not peripheral. The maturation of control systems (from bespoke lab setups to standardized, vendor-supplied, cloud-integrated platforms with primitive-based execution, File 12) is a key part of the field's productization (File 24).

### 55. Reader's takeaway for File 11

When you assess a quantum-computing modality, company, or roadmap, look past the qubit count and fidelity to the *infrastructure-scaling* story:

- **Cooling:** does the modality need dilution refrigeration (superconducting/spin) or "just" vacuum/lasers (atomic) or detector cryogenics (photonic)? What is the qubits-per-fridge (or per-optical-system) ceiling, and the modular-scaling plan?
- **Control integration:** is there a credible cryo-CMOS (superconducting/spin) or integrated-photonics (atomic) plan to escape the wiring/laser bottleneck at scale, with the required power efficiency?
- **Real-time decoding:** is there a plan for real-time error-correction decoding at scale (FPGA/ASIC decoders), within the modality's latency budget?
- **Interconnects:** for modular scaling, is there a quantum-interconnect (transduction, photonic link) plan?
- **Cost/power/supply:** what dominates the cost and power (it will be infrastructure), and are there supply-chain risks (³He, cryogenics, control electronics)?

These infrastructure questions — cooling ceiling, control integration, decoding hardware, interconnects, and cost/power/supply — are as determinative of the path to utility as the qubit metrics, and they are systematically underweighted in the qubit-count-focused narrative. An analyst who asks them (alongside the hardware-assessment discipline of Files 3–7, the QEC-assessment discipline of File 9, and the benchmarking skepticism of File 22) will form a far more accurate picture of a modality's or company's true prospects. The classical infrastructure is not the boring part of quantum computing — it is a co-equal grand challenge, and mastering it is a prerequisite for the fault-tolerant future the rest of this database describes.

---

## Part XIV — Further Worked Examples and Quantitative Scaling Models

### 56. A simple qubits-per-fridge model

Build a back-of-envelope model for superconducting qubits-per-fridge. Let the mixing-chamber cooling power be P_cool (say 500 μW), and let each qubit's cold-stage heat load be h (from attenuator dissipation and residual conduction after thermalization). With per-qubit heat load h ≈ 1 μW (a representative value with good thermalization and moderate readout power), the ceiling is N_max ≈ P_cool/h ≈ 500 qubits. Now apply the levers:

- **Multiplexing** readout by 10× reduces the readout contribution to h, perhaps halving h → N_max ≈ 1000.
- **Cryo-CMOS** moves signal generation to 4 K (where cooling power is ~2000× larger), so the mixing-chamber load per qubit drops to just the short 4K→10mK line's contribution — potentially h ≈ 0.1 μW → N_max ≈ 5000 per fridge.
- **Modular multi-fridge** with M fridges → M × N_max total qubits, limited by inter-fridge interconnect quality (Section 11).

To reach 10⁶ qubits: even at N_max = 5000/fridge (optimistic cryo-CMOS), one needs ~200 fridges linked by high-fidelity quantum interconnects — a massive but not inconceivable facility, *contingent* on cryo-CMOS reaching that per-qubit heat load *and* interconnects maturing. This model makes concrete that fault-tolerant superconducting scaling requires *all three* levers (multiplexing, cryo-CMOS, modular interconnects) working together — none alone suffices — and quantifies why each is a hard requirement rather than an optimization (Sections 7, 45).

### 57. The atomic-modality scaling model

For atomic qubits, the scaling model is different — set by *optics*, not heat:

- **Neutral atoms** (File 5): one global control laser drives all gates; the limit is trap-laser power (a few watts per ~1000 atoms) and optical field-of-view/aberrations. Scaling to 10⁴–10⁵ atoms needs ~10–100× more laser power and larger corrected optics — an optical-engineering problem, softer than the superconducting heat-load wall. Individual-addressing optics (AOD channels) scale with the number of *simultaneously addressed* atoms, not the total, and integrated photonics (Section 33) is the long-term scalable delivery.
- **Trapped ions** (File 4): the limit is the per-ion laser/addressing complexity and the shuttling/networking overhead (File 4); integrated photonics and microwave gates are the scaling paths. Modular photonic interconnects link ion modules.

The key structural contrast: superconducting scaling is bounded by a *fixed thermal budget* (a hard wall requiring integration to move control to warmer stages), while atomic scaling is bounded by *optical complexity* (a softer wall, since much control is global/shared, but still requiring integration for individual addressing). Both converge on *integration* as the solution, but the atomic modalities' larger *raw* scaling headroom (global control, no heat wall) is a reason neutral atoms hold the qubit-count records (File 5) — their infrastructure scales more gracefully in raw count, even as fidelity and other factors qualify that advantage.

### 58. Worked example: the cost of cooling a logical qubit

Combine infrastructure cost with QEC overhead (File 9). A distance-25 surface-code logical qubit needs ~1250 physical qubits (File 9). At a superconducting qubits-per-fridge of ~1000–5000 (Section 56), a single logical qubit occupies a meaningful fraction of a fridge, and a 1000-logical-qubit machine needs ~250,000–1,250,000 physical qubits → ~50–1250 fridges (depending on cryo-CMOS success), i.e., a large facility. With lower-overhead qLDPC codes (File 9, ~10× fewer physical qubits) or biased-noise cat qubits, the fridge count drops proportionally — another way error-correction overhead reduction (File 9) and infrastructure scaling (this file) *multiply* together to determine feasibility (File 18). This calculation ties the three co-equal pillars — qubit fidelity (setting d), code overhead (setting physical-per-logical), and infrastructure (setting qubits-per-fridge) — into a single feasibility estimate, and shows that improving *any* of them (higher fidelity → smaller d; better code → fewer physical qubits; better cryo-CMOS → more qubits-per-fridge) reduces the facility scale. This multiplicative interplay is the essence of resource estimation (File 18) and the reason no single metric (qubit count, fidelity, or fridge size) captures the path to utility.

### 59. Why "just add more fridges" isn't simple

The modular multi-fridge answer (Sections 10, 56) sounds simple but hides a hard problem: **inter-fridge quantum interconnects**. Moving a qubit's state from one fridge to another requires either a direct cryogenic microwave link (limited range, and both ends must be cold) or **microwave-to-optical transduction** (Section 11) — converting the microwave qubit signal to an optical photon that travels through room-temperature fiber to the other fridge, then back to microwave. High-efficiency, low-noise transduction is *not yet solved* — current transducers have low efficiency and add noise, insufficient for high-fidelity logical operations between fridges. Until transduction (or an alternative interconnect) matures, modular scaling is limited, and the single-fridge ceiling (Section 56) binds. This is why microwave-to-optical transduction is a critical, roadmap-gating research problem (Sections 11, 36; File 25): it is the enabler of both modular superconducting scaling *and* networked quantum computing (File 15), and its difficulty means "just add fridges" is a research program, not a turnkey solution.

### 60. Final synthesis and forward pointer

The classical infrastructure — dilution refrigeration, cryogenic wiring, amplifier chains, control electronics, real-time decoders, and laser/optical systems — is the substrate on which quantum computing runs and, for the leading modalities, its dominant scaling bottleneck. The microwatt cooling budget creates the wiring bottleneck; the laser complexity creates its atomic analogue; and the solutions — cryo-CMOS, integrated photonics, multiplexing, modular interconnects, decoder ASICs, and transduction — are hard, partly-unsolved integration problems that determine the physical feasibility, economic viability, and timeline of fault-tolerant quantum computing as much as qubit quality and error-correction overhead do. The co-scaling imperative and the three-pillar interplay (fidelity, code overhead, infrastructure) developed here feed directly into resource estimation (File 18), where these parameters compose into concrete machine specifications, and into roadmap assessment (File 19), where a company's or modality's infrastructure-scaling plan is a first-order determinant of credibility. The reader who carries forward this file's central lesson — that the "boring" classical infrastructure is a co-equal grand challenge, and that its *integration* is the common solution shape and a prerequisite for utility — will assess the field's progress and prospects far more accurately than one who tracks qubit counts alone. Proceed to File 12 for the software stack that sits atop this infrastructure, and to File 18 for the resource estimation that composes infrastructure, code, and fidelity into the physical-qubit and runtime numbers defining the path to useful quantum computing.

> **One-paragraph summary of File 11.** A quantum computer is a co-scaled quantum–classical system whose *classical* infrastructure — millikelvin dilution refrigeration (superconducting/spin) or stabilized-laser/UHV systems (atomic) or cryogenic detectors (photonic), plus the control electronics, quantum-limited amplifiers, and real-time decoders — is, for the leading modalities, the dominant scaling bottleneck. The dilution refrigerator's microwatt cooling budget caps qubits-per-fridge and creates the wiring bottleneck; atomic qubits face the analogous laser bottleneck; and both are addressed by *integration* (cryo-CMOS, integrated photonics, multiplexing, modular interconnects, decoder ASICs, microwave-to-optical transduction), which is itself a set of hard, partly-unsolved problems that gate the physical feasibility, cost, and timeline of fault tolerance as much as qubit quality and error-correction overhead do. Infrastructure integration is a co-equal grand challenge, systematically underweighted by the qubit-count narrative, and the single most important "hidden" determinant of the path to utility.

---

## Appendix — Representative Infrastructure Parameters and Design Rules

### Representative parameters (current era; re-verify)

| Parameter | Representative value |
|---|---|
| Superconducting qubit temperature | 10–20 mK |
| Spin qubit temperature | <100 mK – 1 K |
| Ion/atom qubit temperature | room temp (trap chip sometimes 4–10 K) |
| SNSPD detector temperature | 1–4 K |
| Mixing-chamber cooling power | ~10–1000 μW |
| 4 K stage cooling power | ~1–2 W |
| Attenuation (drive line) | ~40–60 dB distributed across stages |
| Parametric amp added noise | ~½ photon (quantum limit) |
| Control channels per qubit (SC) | ~2–3 (drive, flux, shared readout) |
| Readout multiplexing factor | ~6–20 qubits per feedline (growing) |
| Real-time decode latency budget (SC) | ~1 μs per cycle |
| Qubits per fridge (current) | ~100–1000 (conventional wiring) |
| Dilution refrigerator cost | ~$0.5–3M |
| Laser linewidth (optical qubit/Rydberg) | kHz to sub-Hz (cavity-stabilized) |
| UHV pressure (atomic) | ≤10⁻¹¹ torr |

### Design rules of thumb

1. **The control field must be quieter than the qubit** (microwave phase noise / laser linewidth below the qubit's coherence-limiting scale) — else control noise, not the qubit, limits fidelity (Sections 15, 34).
2. **Heat load scales with wire count; cooling power at 10 mK is nearly fixed** — so qubits-per-fridge is capped, requiring cryo-CMOS + multiplexing + modular fridges to scale (Sections 4, 24, 56).
3. **Cryo-CMOS is bounded by the 4 K power budget** — sub-μW/channel is the target for millions of channels (Sections 25, 27).
4. **Decoding must keep pace with syndrome extraction** — real-time FPGA/ASIC decoders within the cycle-latency budget, parallelized across logical qubits (Sections 14, 48).
5. **Superconducting wiring is used where T < the material's critical temperature** — low signal loss with low heat conduction (Section 46).
6. **Every downward line must be attenuated, thermalized, and filtered** at each stage to deliver clean signals without carrying thermal noise or IR photons to the qubit (Section 5).
7. **Modular scaling requires quantum interconnects** (transduction or photonic links), not yet mature — "just add fridges" is a research program (Section 59).
8. **Integration is the universal solution shape** — cryo-CMOS, integrated photonics, decoder ASICs, on-chip nonreciprocal elements — replacing bespoke discrete infrastructure with manufacturable integrated systems, for both physical feasibility and cost scaling (Sections 38, 49).

These parameters and rules are the practical distillation of the file: they are what a systems engineer uses to estimate a machine's qubits-per-fridge, control-channel count, decoding load, and scaling path, and they are inputs to the resource estimation of File 18. Together with the co-scaling imperative (a machine scales only as fast as its slowest layer, currently the classical infrastructure), they equip the reader to assess any modality's or company's true scaling prospects — looking past the qubit count to the fridges, the control integration, the decoders, and the interconnects that will actually determine whether, and when, useful fault-tolerant quantum computing arrives.
