# Quantum Networking, Communication, and the Quantum Internet

> This file covers quantum networking — distributing and transmitting quantum information (especially entanglement) between separated nodes — spanning quantum-secure communication (QKD), distributed/modular quantum computing, and networked quantum sensing. It develops QKD (BB84, E91), quantum repeaters and entanglement swapping, the quantum-memory challenge, entanglement-distribution experiments (including the Micius satellite), photon–matter interfaces, and distributed quantum computing. It builds on File 2 (no-cloning, Bell states, teleportation), File 6 (photonics), and Files 4 (ion–photon links) and connects to Files 16 (sensing) and 21 (QKD vs. post-quantum cryptography policy).

---

## Part I — Why Quantum Networking Is Distinct

Quantum networking is distinct from quantum *computing*: the goal is *distributing/transmitting quantum information* — often entanglement — between physically separated nodes, rather than performing a local computation. Its applications span three areas:

- **Quantum-secure communication** — quantum key distribution (QKD) providing information-theoretically-secure key exchange (Part II).
- **Distributed / modular quantum computing** — linking smaller quantum processors into a larger effective system (directly relevant to the modular-scaling approaches of trapped ions, File 4, and photonics, File 6, and to superconducting modular architectures, File 11).
- **Networked quantum sensing** — using entanglement to enhance sensing precision across distributed sensors (File 16).

The physical foundation is the DiVincenzo *networking* criteria (File 1): the ability to interconvert stationary (matter) and flying (photonic) qubits, and to faithfully transmit flying qubits. Because photons are the only practical carrier of quantum information over distance (File 6), quantum networking is fundamentally *photonic*, and it shares deep technology with photonic quantum computing (File 6) — sources, detectors, memories, and interfaces serve both.

---

## Part II — Quantum Key Distribution

### 1. BB84

**Quantum Key Distribution (QKD)** enables two parties to establish a shared secret key with **information-theoretic security** — security guaranteed by physics (the laws of quantum mechanics), not by computational assumptions. The foundational protocol is **BB84 (Bennett–Brassard, 1984)**:

- Alice encodes random key bits in **non-orthogonal quantum states** (e.g., photon polarizations in two randomly-chosen bases: rectilinear {H,V} or diagonal {+,−}).
- Bob measures in a randomly-chosen basis. After transmission, Alice and Bob publicly compare *bases* (not values) and keep only the bits where their bases matched (the "sifted key").
- **Security via no-cloning (File 2, Section 15) and measurement disturbance:** an eavesdropper (Eve) cannot copy the non-orthogonal states (no-cloning), and any attempt to measure them necessarily *disturbs* them (measuring in the wrong basis randomizes the result). By comparing a subset of their bits, Alice and Bob detect Eve's disturbance as an elevated error rate — if the error rate is below a threshold, they can distill a secure key (via error correction and privacy amplification); if above, they abort. Eavesdropping is *detectable*, which is the security guarantee.

BB84's security is a direct application of no-cloning and measurement disturbance (File 2) — the physical impossibility of undetectably copying quantum information.

### 2. E91 and entanglement-based QKD

The **E91 protocol (Ekert, 1991)** uses **entangled pairs** instead of prepared states: a source distributes entangled photon pairs to Alice and Bob, who each measure their photon in randomly-chosen bases. The security check is a **Bell-inequality (CHSH, File 2, Section 5) violation** — if Alice and Bob's measurements violate the Bell inequality, their correlations are genuinely quantum (not intercepted by Eve, whose interference would reduce the violation). E91 is elegant because the security follows from Bell-inequality violation itself (device-independent-ish security), and it connects QKD to the foundational Bell-test physics (File 2). Entanglement-based QKD is also the natural fit for repeater-based long-distance networks (Part III), which distribute entanglement anyway.

### 3. Practical QKD and its limits

Practical QKD systems exist and are commercially deployed (companies: ID Quantique, Toshiba, and others; national QKD networks in China and Europe):

- **Fiber-based QKD:** limited to ~100s of km without repeaters, because photon loss in fiber grows exponentially with distance (~0.2 dB/km), and — crucially — **no-cloning forbids amplifying the signal** (you can't copy-and-boost quantum states as classical repeaters do). This distance limit is the central practical challenge, motivating quantum repeaters (Part III).
- **Free-space and satellite QKD:** for longer distances, free-space links (line-of-sight) and satellite links (China's **Micius satellite**, 2016, distributed entanglement and performed QKD over >1000 km, and intercontinental QKD) overcome fiber loss by transmitting through space (where loss is lower and doesn't accumulate as in fiber).

### 4. QKD vs. post-quantum cryptography — a crucial distinction

A frequently-confused distinction (File 21): **QKD** provides information-theoretically-secure *key distribution* using quantum physics (hardware-based, requiring special quantum-optical infrastructure). **Post-quantum cryptography (PQC)** uses *classical* algorithms believed hard even for quantum computers (lattice-based, hash-based; File 21) — a *software* solution deployable on existing classical infrastructure. These are *different, sometimes-conflated* approaches to the quantum threat:

- **QKD:** proven physics-based security for key exchange specifically, but requires quantum hardware (special fiber/satellite links, single-photon sources/detectors), is distance-limited, and does not address authentication or the broader cryptographic stack.
- **PQC:** classical, software-only, deployable everywhere, addresses the full cryptographic stack — the mainstream response to the quantum threat (NIST standardization, File 21).

The policy mainstream (File 21) favors **PQC** as the practical response (deployable now, everywhere, in software) while QKD remains a niche for specific high-security point-to-point links. Confusing the two — treating QKD as *the* answer to the quantum threat, or PQC as physics-based — is a common error the disciplined reader avoids (File 21).

---

## Part III — Quantum Repeaters and Entanglement Distribution

### 5. The quantum repeater challenge

Extending entanglement distribution beyond the fiber-loss limit (~100s of km) requires **quantum repeaters** — but *not* classical-style amplify-and-forward repeaters (forbidden by no-cloning, Section 3). Instead, quantum repeaters use **entanglement swapping**:

- Divide the long distance into segments. Generate entanglement across each *short* segment (feasible despite loss).
- At intermediate nodes, perform **entanglement swapping**: a **Bell-state measurement** (File 2) on the two photons from adjacent segments "swaps" the entanglement, extending it across both segments — entangling the two *distant* endpoints that were never directly linked (via the teleportation/swapping mechanism, File 2, Section 21).
- Repeat hierarchically to span the full distance.

Entanglement swapping is the core repeater operation — it extends entanglement without copying quantum states, circumventing the no-cloning barrier.

### 6. The quantum-memory bottleneck

The central *unsolved* challenge for repeaters is **quantum memory**. Because photonic entanglement generation is *probabilistic* (each attempt across a segment likely fails due to loss), the repeater must **store** one half of a successfully-generated entangled pair while *waiting* for the adjacent segment to also succeed — then perform the swap. This requires a **quantum memory** that holds a qubit coherently long enough for the neighboring links to succeed (potentially many attempt cycles). Building good quantum memories — high storage fidelity, long coherence, efficient photon interface, and fast retrieval — is the key enabling (and not-yet-mature) technology for repeaters. Candidate quantum-memory technologies:

- **NV centers in diamond** (File 7, 16) — room-temperature-coherent spin qubits with optical interfaces; used in Delft's quantum-network experiments.
- **Rare-earth-doped crystals** — ensemble memories with long coherence and multiplexing.
- **Atomic ensembles** (the DLCZ protocol) — collective atomic excitations as memories.
- **Trapped ions and neutral atoms** (Files 4, 5) — matter qubits with photon interfaces, overlapping with the computing modalities.

The quantum-memory challenge is why a full repeater-based quantum internet remains a multi-decade effort (Section 8).

### 7. Entanglement-distribution experiments and current state

Notable demonstrations mapping the field's progress:

- **Metropolitan-scale fiber networks:** academic testbeds distributing entanglement over city-scale distances — QuTech's Netherlands network (linking Delft, The Hague), the Boston-area and Chicago-area (US DOE) testbeds, and others. The **Quantum Internet Alliance** (Europe) pursues a continental testbed.
- **Satellite entanglement distribution:** China's **Micius satellite** (Pan Jianwei's group) distributed entanglement over >1200 km (2017) and enabled intercontinental QKD — a landmark demonstrating space-based quantum communication overcoming fiber loss.
- **Repeater-node demonstrations:** small-scale entanglement swapping and memory-based repeater primitives (Delft NV centers, and others) — proof-of-principle, not yet scalable repeater chains.

### 8. The multi-decade quantum-internet outlook

The field consensus: a full-scale, repeater-based **"quantum internet"** — enabling entanglement distribution, distributed computing, and networked sensing at global scale — remains a **multi-decade engineering effort**, gated by the quantum-memory challenge (Section 6) and repeater maturity. Near-to-medium-term practical quantum networking focuses on **point-to-point QKD links** (fiber and satellite, deployable now) and **small-scale lab/metro testbeds**, not global infrastructure. The quantum internet is a long-horizon vision, and honest assessment (like the fault-tolerant-computing timeline, File 19) places broadly-useful quantum networking in the 2030s-and-beyond, with incremental milestones (better memories, longer repeater chains, metro testbeds) arriving steadily.

---

## Part IV — Interfaces, Distributed Computing, and Synthesis

### 9. Photon–matter qubit interfaces

A cross-cutting technical challenge (the DiVincenzo networking criteria, File 1): the **interface converting flying (photonic) qubits to stationary (matter) qubits and back**. This interface is needed for:

- **Quantum memories** in repeaters (storing a photonic qubit in matter, Section 6).
- **Modular quantum computing** — linking matter-qubit processors (trapped ions, File 4; superconducting, File 11) via photonic links, requiring matter-to-photon conversion.
- **Ion–photon entanglement** (File 4, Section 12) — a trapped ion emits a photon entangled with its state, the basis of modular ion-trap scaling.

For **superconducting** qubits (File 3, 11), the interface is especially hard: superconducting qubits operate with *microwave* photons, but long-distance transmission needs *optical* photons — requiring **microwave-to-optical transduction** (File 11, Section 11), a major unsolved efficiency challenge. For atomic qubits (ions, atoms, NV centers), the interface is more natural (they already interact with optical photons), which is why atomic modalities lead in networking demonstrations.

### 10. Distributed quantum computing

**Distributed quantum computing** uses quantum-network links to connect multiple smaller quantum processors into an effectively larger logical machine — a key scaling strategy (Files 4, 11) complementing monolithic scaling:

- **Modular scaling:** rather than building one enormous processor (limited by wiring/laser bottlenecks, File 11), link many smaller modules via photonic interconnects — the roadmap for trapped ions (File 4), photonics (File 6), and modular superconducting (File 11).
- **Entanglement-assisted gates:** using shared entanglement (distributed via the network) plus classical communication to perform logical two-qubit gates *between* separate processors (via gate teleportation, File 2, Section 21) — demonstrated at research scale.
- **Relevance to scaling:** distributed quantum computing is an alternative/complement to monolithic scaling, especially valuable given the per-module wiring/connectivity bottlenecks (File 11). Its feasibility depends on high-rate, high-fidelity photonic interconnects (the same challenge as repeaters, Sections 6, 9).

Distributed quantum computing blurs the line between "networking" and "computing" — a modular quantum computer *is* a small quantum network, and the same photonic-interconnect technology serves both. This synergy (File 6) is why photonic networking technology is strategically important regardless of whether photonic *computing* wins.

### 11. Networked quantum sensing

Entanglement can enhance *sensing* precision beyond the classical shot-noise limit (approaching the Heisenberg limit, File 16) across a *network* of distributed sensors — **networked quantum sensing**. By entangling sensors at different locations, one can measure spatially-distributed quantities (e.g., comparing distant clocks, or mapping fields) with enhanced precision. This is an active research connection between quantum networking (this file) and quantum sensing (File 16), and it is one of the near-term applications of entanglement distribution (alongside QKD), distinct from the longer-horizon distributed-computing vision.

### 12. Summary

Quantum networking distributes and transmits quantum information — especially entanglement — between separated nodes, enabling quantum-secure communication (QKD), distributed/modular quantum computing, and networked sensing. Its foundation is photonic (photons as flying qubits, File 6) and the photon–matter interface (the DiVincenzo networking criteria). **QKD** (BB84, E91) provides information-theoretically-secure key distribution via no-cloning and measurement disturbance, deployable now over fiber (~100s km) and satellite (Micius, >1000 km) — but it is a niche complement to, not a replacement for, the mainstream post-quantum-cryptography response to the quantum threat (File 21). **Quantum repeaters** extend entanglement via entanglement swapping (circumventing no-cloning), but are gated by the unsolved **quantum-memory challenge**, making a full quantum internet a multi-decade effort with near-term focus on point-to-point links and metro testbeds. **Distributed quantum computing** links smaller processors via photonic interconnects — a key modular-scaling strategy (Files 4, 6, 11) requiring high-rate, high-fidelity interconnects (the same challenge as repeaters), and for superconducting qubits requiring the hard microwave-to-optical transduction (File 11). The deep synergy between quantum networking and photonic computing (shared sources, detectors, memories, interfaces) ensures photonic-networking technology's strategic importance regardless of the computing outcome. Quantum networking is thus both a field in its own right (QKD, quantum internet) and an enabler of modular quantum computing — with the quantum-memory and interconnect challenges as its defining, and largely unsolved, engineering frontiers.

*Cross-references: no-cloning, Bell states, teleportation, entanglement swapping, CHSH (File 2); photonics as the flying-qubit platform (File 6); ion–photon links for modular trapped-ion scaling (File 4); microwave-to-optical transduction and modular superconducting scaling (File 11); NV centers as memory/network nodes (Files 7, 16); QKD vs. post-quantum cryptography, Micius, national QKD networks (File 21); networked quantum sensing and the Heisenberg limit (File 16); distributed computing as a scaling strategy (File 25).*

---

## Part V — Extended Topics: QKD Variants, Repeater Generations, and Worked Analysis

### 13. QKD variants and practical security

Real QKD systems face implementation imperfections that idealized BB84 (Section 1) ignores, and variants address them:

- **Decoy-state QKD:** practical single-photon sources are imperfect (they occasionally emit multiple photons, enabling a "photon-number-splitting" attack where Eve siphons the extra photons). Decoy-state protocols send pulses of varying intensity to detect such attacks, restoring security with realistic (weak coherent) sources — now standard in deployed systems.
- **Measurement-Device-Independent QKD (MDI-QKD):** removes vulnerabilities from imperfect *detectors* (a major attack surface) by having both parties send photons to an untrusted central node that performs a Bell measurement — the security no longer depends on trusting the detectors. MDI-QKD is more robust but lower-rate.
- **Continuous-Variable QKD (CV-QKD):** encodes key information in the *quadratures* of light (File 6's CV framing) and uses homodyne detection (standard telecom components) rather than single-photon detectors — potentially cheaper and more telecom-compatible, at some range/rate cost.
- **Twin-field QKD:** a recent advance extending QKD range by using single-photon interference at a central node, improving the rate-distance scaling and extending secure distances toward ~500+ km over fiber.

These variants show QKD is a mature, actively-engineered technology addressing real-world security and practicality — but they also underscore that QKD's security, while physics-based *in principle*, depends on careful implementation (device imperfections are real attack surfaces), and that its deployment requires specialized hardware and has range/rate limits that PQC (File 21) avoids. The gap between idealized and implementation security is a recurring theme, and side-channel attacks on QKD hardware are an active concern.

### 14. Repeater generations

Quantum repeaters (Part III) are classified into "generations" by how they handle loss and operation errors:

- **First-generation repeaters:** use quantum memories and entanglement swapping with two-way classical communication (heralding) — the memory-based approach (Section 6), gated by memory quality. Slow (limited by the classical round-trip and memory coherence) but conceptually straightforward.
- **Second-generation:** add quantum error correction to handle *operation* errors (not just loss), reducing the reliance on two-way communication for error correction while still using memories for loss.
- **Third-generation:** use quantum error correction (loss-tolerant codes) to handle *both* loss and operation errors with *one-way* communication — no memories needed, potentially much faster, but requiring high-quality error correction at each node (approaching fault-tolerant requirements). Third-generation repeaters blur into fault-tolerant photonic computing (File 6).

The generation progression mirrors the computing NISQ-to-fault-tolerant transition: from memory-based, heralded, slow (first-gen) toward error-corrected, fast, memory-less (third-gen). Current demonstrations are at the first-generation, proof-of-principle stage, with the higher generations requiring the same error-correction maturity as fault-tolerant computing (File 9) — another reason the full quantum internet is a long-horizon effort (Section 8).

### 15. Worked example: fiber loss and the repeater necessity

Quantify why repeaters are needed. Fiber loss is ~0.2 dB/km at telecom wavelength (1550 nm). Over 100 km, that is 20 dB = a factor of 100 loss (1% transmission); over 500 km, 100 dB = 10⁻¹⁰ transmission. So a single photon sent over 500 km of fiber has a ~10⁻¹⁰ chance of arriving — at a generation rate of, say, 10⁹ photons/second, that is ~0.1 successful photons/second, and over 1000 km it is ~10⁻²⁰ transmission, utterly hopeless (one photon per ~300,000 years). Because no-cloning forbids amplification (Section 3), this exponential loss cannot be overcome by boosting the signal — hence the *necessity* of repeaters (dividing the distance into short, low-loss segments with entanglement swapping, Section 5). This worked example makes concrete why long-distance quantum communication is fundamentally hard and why the quantum-memory-based repeater (or satellite links, which avoid fiber loss) is the *only* path to continental/global quantum networking — a stark contrast with classical communication, where amplifiers trivially extend range.

### 16. Worked example: satellite vs. fiber

Contrast the Micius satellite (Section 7) with fiber. A satellite at ~500 km altitude transmits entangled photons through the atmosphere (only the lower ~10 km is lossy; space is near-lossless) and free space to ground stations potentially >1000 km apart. The total loss (atmospheric + beam divergence) is far *less* than 1000 km of fiber's 200 dB — enabling the >1200 km entanglement distribution Micius achieved. This is why satellites are the near-term path to *long-distance* quantum communication (bypassing fiber's exponential loss), while fiber serves metropolitan (~100 km) links, and repeaters (when mature) will eventually connect them. The satellite approach has its own limits (line-of-sight, weather, limited pass times, ground-station infrastructure), but it demonstrated intercontinental QKD and remains a leading approach for global-scale quantum communication pending mature repeaters. China's leadership here (Micius, and planned constellations) is a notable feature of the geopolitical quantum landscape (File 21).

### 17. Glossary and summary

- **QKD:** quantum key distribution — physics-based secure key exchange (BB84, E91).
- **BB84 / E91:** prepare-and-measure / entanglement-based QKD protocols.
- **Decoy-state / MDI / CV / twin-field QKD:** variants addressing source, detector, telecom-compatibility, and range challenges.
- **Entanglement swapping:** extending entanglement via a Bell measurement at an intermediate node (the repeater core operation).
- **Quantum repeater:** device extending entanglement distribution beyond fiber loss via swapping + memory (generations 1–3).
- **Quantum memory:** coherent qubit storage (NV centers, rare-earth crystals, atomic ensembles) — the repeater bottleneck.
- **Photon–matter interface:** converting flying (photonic) and stationary (matter) qubits — the DiVincenzo networking criterion.
- **Microwave-to-optical transduction:** the (hard) interface for superconducting-qubit networking (File 11).
- **Micius:** China's quantum-communication satellite (>1200 km entanglement distribution).
- **Distributed quantum computing:** linking processors via photonic interconnects (modular scaling).
- **Networked quantum sensing:** entanglement-enhanced distributed sensing (File 16).

**Summary.** Quantum networking distributes quantum information (entanglement) between separated nodes for secure communication, distributed computing, and networked sensing, founded on photonic transmission and the photon–matter interface. QKD (BB84/E91 and variants) delivers physics-based secure key distribution — deployable now over fiber (~100s km) and satellite (Micius, >1000 km) but a niche complement to mainstream post-quantum cryptography (File 21). Quantum repeaters extend entanglement via swapping (circumventing no-cloning's ban on amplification), gated by the unsolved quantum-memory challenge and progressing through generations toward error-corrected, memory-less designs that blur into fault-tolerant photonics — making a full quantum internet a multi-decade effort. Distributed quantum computing links processors via photonic interconnects (a key modular-scaling strategy, Files 4, 6, 11), sharing technology and challenges with repeaters and, for superconducting qubits, requiring hard microwave-to-optical transduction. The exponential fiber loss (worked example, Section 15) makes long-distance quantum communication fundamentally hard, driving the repeater and satellite approaches. Quantum networking is both a field in its own right and an enabler of modular quantum computing, with quantum memory and high-rate interconnects as its defining, largely-unsolved frontiers — and with deep, strategically-important synergy with photonic computing (File 6) and quantum sensing (File 16).

---

## Part VI — The Quantum Internet Stack, Applications, and Reader's Takeaway

### 18. A layered quantum-internet stack

Analogous to the classical internet's layered architecture, researchers (notably the QuTech group) have proposed a **quantum-network stack** with layers of increasing capability (a roadmap for the quantum internet's development):

1. **Trusted-repeater networks:** the simplest — intermediate nodes are *trusted* (they decrypt and re-encrypt), providing QKD over long distances *without* genuine end-to-end quantum security (China's backbone QKD network operates this way). Deployable now, but the trust assumption is a weakness.
2. **Prepare-and-measure networks:** end-to-end QKD without trusted intermediate nodes — requiring point-to-point quantum links.
3. **Entanglement-distribution networks:** distributing raw entanglement end-to-end (via repeaters) — enabling device-independent QKD and more.
4. **Quantum-memory networks:** with quantum memories at nodes, enabling more complex protocols (some distributed computing).
5. **Fault-tolerant few-qubit networks and beyond:** full distributed quantum computing and networked applications.

The current state is stages 1–2 (deployed QKD, some entanglement distribution); the higher stages await mature repeaters and memories (Sections 6, 8). This staged roadmap, like the fault-tolerant-computing roadmap (File 19), provides a realistic framing: incremental capability growth over decades, not a sudden "quantum internet" arrival. It also clarifies that today's deployed "quantum networks" (trusted-repeater QKD backbones) are far from the full end-to-end-entanglement vision — an important honest calibration against overstated "quantum internet exists now" claims.

### 19. Applications of a mature quantum network

Beyond QKD, a mature quantum internet would enable:

- **Distributed quantum computing** (Section 10) — modular scaling and cloud quantum computing with genuine end-to-end quantum security.
- **Networked quantum sensing** (Section 11; File 16) — entanglement-enhanced distributed metrology (comparing distant clocks for geodesy/fundamental physics, distributed telescopes, field mapping).
- **Blind quantum computing** — a client with limited quantum capability delegating a computation to a quantum server *without revealing* the computation (privacy-preserving quantum cloud computing), enabled by quantum-network protocols.
- **Distributed consensus and other quantum-network protocols** — Byzantine agreement, secret sharing, and other multi-party protocols with quantum advantages.
- **Fundamental science** — long-baseline entanglement for tests of quantum mechanics, relativity, and gravity.

These applications, mostly long-horizon (awaiting mature repeaters/memories), define the quantum internet's eventual value proposition — but honest assessment (Section 8) places broad deployment in the 2030s-and-beyond, with QKD and small testbeds the near-term reality. Blind quantum computing and networked sensing are among the more concretely-motivated near-to-medium-term applications.

### 20. Reader's takeaway for File 15

When assessing quantum-networking claims, distinguish carefully:

- **QKD vs. PQC** (Section 4): QKD is physics-based key distribution (niche, hardware-heavy, distance-limited); PQC is the mainstream software response to the quantum threat (File 21). Don't conflate them.
- **Trusted-repeater vs. end-to-end-quantum networks** (Section 18): deployed "quantum networks" (China's backbone) are often trusted-repeater QKD, *not* the full end-to-end-entanglement quantum internet — a significant distinction obscured in headlines.
- **Demonstrations vs. scalable infrastructure** (Sections 7–8): entanglement-distribution demonstrations (Micius, metro testbeds) are proof-of-principle; scalable repeater chains await the quantum-memory breakthrough.
- **The quantum-memory bottleneck** (Section 6): the make-or-break challenge for repeaters and hence for the quantum internet — track memory progress (coherence, fidelity, efficiency) as the key indicator.
- **Networking ↔ computing synergy** (Sections 9–10): quantum networking and modular quantum computing share technology (photonic interconnects, transduction) and challenges; progress in one aids the other.

This disciplined framing — distinguishing QKD from PQC, trusted from end-to-end networks, demonstrations from infrastructure, and tracking the quantum-memory bottleneck — separates genuine quantum-networking progress from overstated "quantum internet is here" claims, mirroring the honest-assessment discipline the database applies to computing (Files 1, 17, 22). Quantum networking is a real, progressing field with deployed QKD and impressive entanglement-distribution demonstrations, but the full quantum internet — like fault-tolerant computing — is a multi-decade effort gated by specific unsolved challenges (quantum memory, high-rate interconnects, transduction), and assessing it honestly requires holding both the genuine progress and the long road ahead in view.
