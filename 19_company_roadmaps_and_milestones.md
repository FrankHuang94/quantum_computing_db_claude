# Hardware Roadmaps — Stated Milestones, Track Records, and Roadmap Credibility

> This file surveys the major quantum-hardware companies' stated roadmaps and — crucially — provides a *methodology for assessing roadmap credibility*: comparing past stated milestones against actual delivery, distinguishing peer-reviewed research milestones from press-release milestones, and weighting independent third-party benchmarking. It covers IBM, Google, Quantinuum, IonQ, PsiQuantum, Microsoft, D-Wave, and the Chinese programs. It builds on the hardware files (3–7), resource estimation (File 18), and connects to the competitive landscape (File 20) and geopolitics (File 21). Roadmaps are forward-looking claims; this file's central contribution is the *skeptical framework* for evaluating them.

---

## Part I — Methodology for Assessing Roadmap Credibility

Before surveying roadmaps, establish the *assessment framework* — the most valuable content of this file:

- **Track record on past stated milestones:** the single best credibility indicator is whether a company *hit* its previously-announced near-term targets, and by how much it slipped when it didn't. A company with a history of on-schedule delivery earns more credence for future claims than one that repeatedly slips or redefines metrics.
- **Research-paper vs. press-release milestones:** peer-reviewed results (Nature/Science papers with data) are generally credible; press-release milestones (especially those redefining metrics favorably) warrant skepticism. The gold standard is a *reproducible, peer-reviewed* demonstration.
- **Independent third-party benchmarking:** the strongest validation is *independent* replication of a claimed result (academic groups or benchmark consortia), not the vendor's own measurement.
- **Metric-shifting awareness:** companies sometimes shift to *favorable* metrics (File 22) — e.g., emphasizing "algorithmic qubits" or "quantum volume" when they favor the company's architecture. A roadmap's metrics should be read with awareness of this potential favorable framing.
- **Distinguishing demonstrated from projected:** a roadmap mixes *demonstrated* achievements (what's been shown) with *projected* future milestones (what's promised). Assessing credibility means separating these — a company's demonstrated results are facts; its projections are claims to be weighed against its track record.

Applying this framework — track record, peer-review vs. press-release, independent benchmarking, metric-shifting awareness, demonstrated vs. projected — is how to read *any* roadmap skeptically, and it is the throughline of this file. The roadmaps below are surveyed *with* this framework, not taken at face value.

---

## Part II — The Major Roadmaps

### 1. IBM

IBM has the most *public, granular, and regularly-updated* roadmap in the field:

- **Stated milestones:** IBM publishes a year-by-year roadmap of qubit-count and quality targets. Its trajectory: Eagle (127 qubits, 2021), Osprey (433, 2022), Condor (1121, 2023) — hitting the *count* targets roughly on schedule (a decent track record) — followed by a notable *pivot* (2023+) toward **quality over raw count** (Heron's tunable couplers, File 3) and **modular scaling** (System Two, File 3) with **qLDPC codes** (bivariate bicycle, File 9).
- **The qLDPC pivot:** IBM's roadmap now emphasizes low-overhead qLDPC codes (File 9) requiring long-range couplers — a research-backed (Nature 2024) but hardware-demanding direction (File 3, 11).
- **Credibility assessment:** IBM has a *reasonable track record* (hit its count milestones, though the pivot to quality implicitly acknowledged that count alone wasn't enough), publishes *peer-reviewed* results (the qLDPC and utility papers), and has the field's most transparent roadmap — earning it relatively high credibility, tempered by the fact that the *hardest* milestones (modular scaling, qLDPC in hardware, fault tolerance) are still ahead and unproven.

### 2. Google Quantum AI

Google's roadmap is *research-publication-driven* rather than product-roadmap-driven:

- **Stated path:** from NISQ demonstrations (Sycamore supremacy, 2019, File 3) through the **below-threshold milestone** (Willow, 2024, File 9 — the watershed demonstration that error correction scales) toward a stated long-term goal of a large-scale error-corrected quantum computer (Google has articulated a ~6-milestone path to a million-qubit fault-tolerant machine).
- **Communication style:** Google communicates through *peer-reviewed papers* (Nature) rather than granular product roadmaps, reflecting its research-lab (not commercial-cloud) posture (File 20).
- **Credibility assessment:** Google's *demonstrated* results (supremacy, below-threshold error correction) are peer-reviewed, high-impact, and independently significant — strong credibility for what it has *shown*. Its *projected* milestones (million-qubit machine) are long-horizon and unproven, but its demonstrated below-threshold result (File 9) is arguably the strongest single validation of the fault-tolerant path by any company. Google's credibility rests on delivered science more than on hitting product dates.

### 3. Quantinuum

Quantinuum's roadmap emphasizes *quality and logical qubits* (trapped ions, File 4):

- **Stated milestones:** record **quantum volume** progression (File 22, climbing past 2²⁰), the H-series generational cadence (H1, H2), and a path toward *logical* qubits leveraging trapped-ion high fidelity and mid-circuit measurement (File 4). Quantinuum has demonstrated real-time error correction and logical operations (File 9), and (with Microsoft) multiple logical qubits at low logical error.
- **Emphasis:** near-term high-fidelity NISQ value (chemistry, optimization) alongside longer-term fault tolerance, plus a substantial software business (TKET, File 12).
- **Credibility assessment:** Quantinuum has a *strong track record* on its stated quantum-volume and logical-qubit milestones (regularly hitting announced QV targets, peer-reviewed logical-qubit results), and its metrics (QV, logical error) are relatively rigorous (though QV is IBM-originated and favors ions' connectivity, File 22). High credibility for demonstrated results; the scaling path (beyond tens of qubits, via QCCD and photonic interconnects, File 4) is the open question.

### 4. IonQ

IonQ's roadmap centers on the *"Algorithmic Qubits" (#AQ)* metric (trapped ions, File 4):

- **Stated milestones:** #AQ progression targets (a metric IonQ originated, File 22, emphasizing useful-circuit capability), plus stated commercial-revenue/contract milestones (IonQ is publicly traded, File 20, and emphasizes revenue guidance). Barium-ion transition (File 4) for photonic-interconnect-compatible networking.
- **Credibility assessment:** IonQ's #AQ metric is *company-originated* (File 22) and should be read with awareness that it favors ions' strengths; its commercial-revenue milestones are trackable (as a public company). IonQ has faced some scrutiny over its metrics and claims (short-seller reports questioning its benchmarks) — a reminder to weight *independent* benchmarking (Part I). Moderate credibility, with the caveat that its headline metric is self-defined and its claims warrant independent verification.

### 5. PsiQuantum

PsiQuantum's roadmap is *distinctively different* — a single large discrete milestone (photonics, File 6):

- **Stated approach:** rather than incremental qubit-count milestones, PsiQuantum emphasizes a *single future goal* — a utility-scale, **million-qubit-class fault-tolerant machine** (fusion-based photonics, File 6) — funded by large private raises and government partnerships (e.g., Australian and US government co-investment, File 21).
- **Communication style:** comparatively *little intermediate public benchmarking* — PsiQuantum publishes less granular near-term progress than the gate-model incumbents, betting on the manufacturability thesis (silicon photonics, File 6) reaching the large machine.
- **Credibility assessment:** this roadmap style has a *higher execution risk/reward profile* — hard to assess incrementally (few intermediate milestones to check against), and dependent on the unproven manufacturability thesis (File 6) and photon-loss thresholds. PsiQuantum's credibility rests on the *plausibility of its thesis* (foundry manufacturability) rather than a track record of demonstrated milestones — making it a high-variance bet (File 6, 20) that an analyst can neither easily confirm nor refute from public information, warranting a "wait and see, high uncertainty" assessment.

### 6. Microsoft

Microsoft's roadmap is the field's most *cautionary case study* on roadmap risk (topological qubits, File 7):

- **History:** Microsoft's topological-qubit program has a *repeatedly-revised timeline*, including the **2018 retraction** (File 7) of a claimed Majorana signature — a significant credibility setback. Microsoft's 2023–2025 claims (topological gap protocol, "Majorana 1" chip) continued the pursuit amid ongoing scientific debate (File 7).
- **Hedged strategy:** notably, Microsoft *also* partners across multiple hardware modalities (Quantinuum, Atom Computing, File 20) via Azure Quantum — a rational hedge against its own topological program's risk.
- **Credibility assessment:** Microsoft's topological roadmap is the clearest example of *why track record matters* (Part I): the 2018 retraction and repeated timeline revisions mandate skepticism toward its topological claims, which remain scientifically contested (File 7). Its *resource-estimation and software* contributions (Azure estimator, Q#, File 12, 18) are credible and valuable; its *topological-hardware* claims warrant the highest skepticism of any major program, precisely because of the track record. The hedged multi-modality strategy is prudent given this risk.

### 7. D-Wave

D-Wave's roadmap reflects its *annealing-to-gate-model* evolution:

- **Stated path:** continued quantum-annealing scaling (Advantage2 system, thousands of qubits, File 17) plus a relatively-recent *pivot* toward *also* pursuing gate-model fault-tolerant research — a strategically-notable move reflecting industry consensus pressure toward gate-model universality, even from the company historically defined by annealing (File 17).
- **Credibility assessment:** D-Wave has a *real track record* of delivering annealing hardware with paying customers (File 17) — the most commercially-deployed quantum computing — but its *advantage* claims are contested (File 17). The gate-model pivot is unproven (D-Wave is a latecomer to gate-model). Moderate credibility for annealing hardware delivery; the advantage claims and the gate-model pivot warrant skepticism (Files 17, 20).

### 8. Chinese programs

China's roadmaps are *less transparent* than Western public companies (File 21):

- **USTC:** continued superconducting (Zuchongzhi) and photonic (Jiuzhang) supremacy-claim publication cadence (Files 3, 6, 14) — a research-publication-driven program producing high-profile supremacy claims (subject to the classical-rebuttal dynamics, File 14).
- **Commercial:** Baidu, Alibaba (historically, since scaled back), Origin Quantum — more commercially-framed but less financially transparent than Western public peers.
- **Credibility assessment:** the *relative opacity* of Chinese national-program-funded efforts (compared to investor-disclosure-driven Western public companies) makes independent assessment harder (File 21) — an important caveat for roadmap-credibility assessment in this region. USTC's peer-reviewed supremacy papers are credible science (though contested by classical methods, File 14); the commercial programs' claims are harder to verify. The opacity itself is a credibility caveat.

---

## Part III — Cross-Cutting Assessment and Summary

### 9. Roadmap-credibility methodology applied

Synthesizing the assessment framework (Part I) across the companies:

- **Strongest demonstrated results (peer-reviewed):** Google (below-threshold error correction), IBM (qLDPC codes, utility), Quantinuum (logical qubits, quantum volume), QuEra/Harvard (48 logical qubits, File 9). These are *facts*, independently significant.
- **Best track record on stated milestones:** IBM (hit count targets), Quantinuum (hit QV targets) — earning higher credence for projections.
- **Highest roadmap risk:** Microsoft topological (2018 retraction, contested physics), PsiQuantum (high-variance single-milestone bet, little intermediate benchmarking).
- **Metric-shifting to watch:** IonQ (#AQ, self-defined), and all companies' use of favorable composite metrics (File 22).
- **Opacity caveat:** Chinese programs (harder to independently verify, File 21).

The meta-lesson: **assess roadmaps by demonstrated (peer-reviewed, independently-benchmarked) results and track record, not by projected milestones or press releases.** A company's demonstrated below-threshold error correction (Google) or record quantum volume (Quantinuum) is far more credible than any company's projected "million-qubit machine by year X." The framework (Part I) — track record, peer-review vs. press-release, independent benchmarking, metric-shifting awareness, demonstrated vs. projected — is the durable tool for reading roadmaps, applicable to any current or future claim.

### 10. The field-wide roadmap reframing

A crucial 2023–2024 shift (File 9): the field-wide reframing from *"physical qubit count"* to *"logical qubit count and logical error rate"* as the meaningful progress metric. Roadmaps now target *logical* qubits and *application-feasibility crossovers* (File 18) rather than raw physical counts — a more honest framing (File 1, 18). IBM's quality pivot, Google's below-threshold milestone, and Quantinuum's logical-qubit emphasis all reflect this reframing. An analyst should now assess roadmaps by their *logical*-qubit targets and *resource-estimated application crossovers* (File 18), not raw physical qubit counts — the mature way to read the path to utility (File 18).

### 11. Summary

Quantum-hardware roadmaps are forward-looking claims that must be assessed with a *skeptical framework*: track record on past stated milestones, peer-reviewed research vs. press-release claims, independent third-party benchmarking, awareness of metric-shifting (File 22), and separation of demonstrated from projected. Applying this framework: IBM (transparent roadmap, decent count-milestone track record, qLDPC pivot) and Quantinuum (strong QV/logical-qubit track record) have relatively high credibility; Google's demonstrated below-threshold error correction (File 9) is the strongest single validation of the fault-tolerant path; IonQ's self-defined #AQ metric and Microsoft's retraction-marred topological program warrant skepticism; PsiQuantum's high-variance single-milestone photonic bet is hard to assess incrementally; D-Wave has real annealing deployment but contested advantage and an unproven gate-model pivot; and Chinese programs' opacity complicates verification. The field-wide reframing (2023–2024) from physical-qubit count to logical-qubit count and application-feasibility crossovers (Files 9, 18) is the mature framing, and roadmaps should be assessed by their *logical* targets and *resource-estimated* crossovers (File 18), not raw counts. The durable lesson: credit demonstrated (peer-reviewed, independently-benchmarked) results and track record; discount projected milestones and press releases; and read all metrics with awareness of favorable framing — the skeptical, evidence-based roadmap assessment this file models, and the throughline connecting it to the benchmarking rigor (File 22), the competitive landscape (File 20), and the honest path-to-utility assessment (File 18) the database maintains throughout.

*Cross-references: the hardware modalities behind each roadmap (Files 3–7); below-threshold error correction and qLDPC codes as roadmap milestones (File 9); resource estimation grounding application-feasibility crossovers (File 18); the competitive/business landscape (File 20); benchmarking metrics and metric-shifting (File 22); the classical-rebuttal dynamics affecting supremacy claims (File 14); Chinese program opacity and national context (File 21); the honest NISQ-application assessment tempering near-term roadmap claims (File 17).*

---

## Part IV — Additional Companies, Roadmap Table, and Worked Assessment

### 12. Neutral-atom and other companies' roadmaps

- **QuEra (neutral atoms, File 5):** roadmap from analog (Aquila, 256 qubits, on AWS) to digital gate-based systems with error correction, building on the 48-logical-qubit demonstration (File 9). Credibility: strong *peer-reviewed* results (the Nature logical-qubit paper) — academic-paper-driven credibility (File 20). Its stated logical-qubit scaling targets are among the more credible, backed by demonstrated results.
- **Pasqal (neutral atoms, File 5):** analog and digital roadmap, partnership/co-development commercialization (File 20). Credibility: real analog products and industrial pilots; the digital/fault-tolerant path is the open question.
- **Atom Computing (neutral atoms, File 5):** 1,225-atom array milestone (2023), Microsoft partnership for logical qubits. Credibility: demonstrated large arrays; logical-qubit results via the Microsoft partnership.
- **Rigetti (superconducting, File 3):** roadmap emphasizing fidelity improvement and multi-chip modular scaling; smaller counts/fidelities than IBM/Google in recent benchmarks. Credibility: a public company (File 20) with trackable milestones, but has faced execution challenges — moderate credibility.
- **Alice & Bob (cat qubits, File 7):** roadmap betting on biased-noise cat qubits reaching low-overhead fault tolerance with a 1D repetition code (File 9). Credibility: a focused, single-architecture bet; the key milestone is demonstrating high bit-flip suppression *during gates* (File 7) — an unproven but well-defined target.
- **IQM (superconducting, File 3):** European on-prem and HPC-integrated systems; tunable couplers. Credibility: real product delivery to European HPC centers.

### 13. Roadmap comparison table

| Company | Modality | Roadmap style | Strongest demonstrated result | Key open milestone | Credibility notes |
|---|---|---|---|---|---|
| IBM | Superconducting | Granular public roadmap | qLDPC codes; utility demo | Modular qLDPC in hardware | Transparent, decent track record |
| Google | Superconducting | Research-publication | Below-threshold error correction | Scaling to more logical qubits | Strong demonstrated science |
| Quantinuum | Trapped ion | Quality/logical-qubit | Record QV; logical qubits | Scaling beyond tens of qubits | Strong QV/logical track record |
| IonQ | Trapped ion | #AQ + revenue | #AQ progression | Networked/modular scaling | Self-defined metric; verify independently |
| PsiQuantum | Photonic | Single big milestone | (limited public benchmarking) | Million-qubit FT machine | High-variance; hard to assess |
| Microsoft | Topological | Repeatedly revised | (contested Majorana claims) | Confirming topological qubit | 2018 retraction; high skepticism |
| D-Wave | Annealing (+gate pivot) | Product + pivot | Deployed annealing customers | Gate-model FT; proven advantage | Real deployment, contested advantage |
| QuEra | Neutral atom | Analog→digital | 48 logical qubits | Digital FT scaling | Strong peer-reviewed results |
| USTC (China) | SC + photonic | Publication | Zuchongzhi/Jiuzhang supremacy | (opaque commercial path) | Credible science, opaque commercial |

The table applies the assessment framework (Part I) across the field: the companies with the strongest *demonstrated, peer-reviewed* results (Google, IBM, Quantinuum, QuEra) have the highest credibility for what they've shown; the companies with self-defined metrics (IonQ), retraction-marred track records (Microsoft), high-variance single-milestone bets (PsiQuantum), or opacity (Chinese programs) warrant more skepticism — regardless of the ambition of their projected milestones.

### 14. Worked assessment: reading a roadmap claim

Apply the framework (Part I) to a hypothetical: *"Company Z announces it will have 1000 logical qubits by 2028."* Assessment:

1. **Track record:** has Z hit its *past* announced milestones on schedule? If it has repeatedly slipped, discount the 2028 claim heavily.
2. **Demonstrated vs. projected:** how many logical qubits has Z *demonstrated* (peer-reviewed) *today*? If it's a handful (the current state, File 9), 1000 by 2028 is a ~100–200× scaling in ~3 years — aggressive; check against the resource-estimation-grounded feasibility (File 18) and the modality's scaling challenges (Files 3–7, 11).
3. **Metric:** is "logical qubit" defined rigorously (a specific code, distance, and logical error rate) or loosely? A "logical qubit" at trivial distance is not the same as a low-error-rate one.
4. **Independent benchmarking:** will the claim be independently verified (peer-reviewed, replicated)?
5. **Infrastructure:** does the roadmap account for the control/decoding/cryogenic scaling (File 11) that 1000 logical qubits (= ~100,000s+ physical qubits) requires?

Applying these, most aggressive projected-milestone claims should be *discounted* against the company's track record and the resource-estimation reality (File 18) — while its *demonstrated* results are credited as facts. This worked assessment is the practical skill this file builds: reading any roadmap claim through the framework, separating credible demonstrated results from discountable projections, and grounding the assessment in resource estimation (File 18) and the modality's real scaling challenges (Files 3–7, 11).

### 15. The pattern of roadmap slippage

A historical pattern worth noting: quantum-computing roadmaps have *frequently slipped*, and "useful quantum computing in N years" predictions have repeatedly been revised outward. This is not unique to quantum computing (emerging technologies commonly overpromise near-term timelines) but it is a strong prior: *default to skepticism* toward aggressive near-term roadmap claims, weight demonstrated results over projections, and expect slippage. The companies that have *avoided* the worst slippage (IBM's count milestones, Quantinuum's QV) earn credibility precisely by contrast. The pattern also cautions against the *reverse* error (dismissing the field because roadmaps slip) — the demonstrated results (below-threshold error correction, 48 logical qubits, File 9) are *real progress* even as the *timelines* slip. The disciplined view: credit real demonstrated progress, discount aggressive timelines, and track the resource-estimation-grounded feasibility (File 18) rather than the marketing timeline.

---

## Part V — Milestone Types, Historical Track Records, and Reader's Guide

### 16. Types of milestone and their credibility

Roadmap milestones come in types with different credibility (Part I):

- **Peer-reviewed scientific milestones** (below-threshold error correction, logical qubits, record fidelities): the *most credible* — reproducible, data-backed, independently significant. Weight these highly.
- **Qubit-count milestones** (Condor 1121 qubits): verifiable but *low-information* (count alone says little about quality, File 1, 22). Credible as facts but limited in significance.
- **Composite-metric milestones** (quantum volume, #AQ): informative but *potentially favorably-framed* (File 22) — read with awareness of which architecture the metric favors.
- **Press-release milestones** (announcements without peer review, or metric redefinitions): the *least credible* — warrant skepticism until independently verified.
- **Revenue/commercial milestones** (contracts, customers): trackable for public companies (File 20) but "revenue" is often R&D-services/pilot revenue, not production (File 24) — read with the market-reality caveats (File 24).

Classifying a milestone by type is the first step in assessing it: a peer-reviewed below-threshold result (Google) is far more credible than a press-release "breakthrough" or a self-defined-metric record. This taxonomy, applied to any milestone claim, sharpens the credibility assessment (Part I).

### 17. Historical track-record examples

Concrete track-record comparisons (Part I) build the assessment:

- **IBM:** announced and hit the Eagle (127, 2021), Osprey (433, 2022), Condor (1121, 2023) count milestones roughly on schedule — a *positive* track record on counts, though the subsequent quality pivot implicitly acknowledged counts weren't the right target.
- **Google:** delivered peer-reviewed supremacy (2019) and below-threshold error correction (2024) — a *positive* track record on high-impact science, on a research (not product-date) cadence.
- **Quantinuum:** hit successive quantum-volume records on its announced cadence — a *positive* track record on its stated metric.
- **Microsoft:** the 2018 Majorana retraction and repeated topological-timeline revisions — a *negative* track record on its flagship program, mandating skepticism.
- **General industry:** aggressive "useful quantum computing by year X" timelines have repeatedly slipped — a *negative* industry-wide prior on near-term timelines.

These track records are the *evidence base* for weighting future claims: companies with positive track records (IBM counts, Google science, Quantinuum QV) earn more credence; the negative examples (Microsoft topological, industry timeline slippage) mandate skepticism. Track record is the single most reliable credibility indicator (Part I), and these examples calibrate it.

### 18. Reader's guide to roadmaps

For an analyst assessing any quantum-hardware roadmap:

1. **Separate demonstrated from projected** (Part I) — credit peer-reviewed demonstrated results as facts; treat projections as claims.
2. **Check the track record** — did the company hit its past stated milestones? Slippage discounts future claims.
3. **Classify the milestone type** (Section 16) — peer-reviewed > count > composite-metric > press-release.
4. **Assess the metric** (File 22) — is it rigorous or favorably-framed? Is it logical (meaningful) or physical (limited) qubits?
5. **Ground in resource estimation** (File 18) — is the projected milestone consistent with the resource-estimated feasibility and the modality's scaling challenges (Files 3–7, 11)?
6. **Weight independent benchmarking** — has the claim been independently verified?
7. **Default to timeline skepticism** — aggressive near-term timelines commonly slip (Section 15).

Applying this guide separates credible roadmaps from marketing, and it is the roadmap-assessment counterpart to the algorithm checklist (File 13), the NISQ-application checklist (File 17), the resource-estimation interpretation guide (File 18), and the benchmarking rigor (File 22) — together forming the database's coherent, evidence-based framework for assessing quantum-computing claims.

### 19. Final summary

Quantum-hardware roadmaps are forward-looking claims requiring skeptical assessment via a durable framework: track record on past milestones, peer-reviewed vs. press-release, independent benchmarking, metric-shifting awareness, and demonstrated-vs-projected separation. The field's leaders by *demonstrated, peer-reviewed* results — Google (below-threshold error correction), IBM (qLDPC, utility, count milestones), Quantinuum (record QV, logical qubits), QuEra (48 logical qubits) — have the highest credibility for what they've shown; the companies with self-defined metrics (IonQ), retraction-marred programs (Microsoft topological), high-variance single-milestone bets (PsiQuantum), or opacity (Chinese programs) warrant more skepticism. The 2023–2024 field-wide reframing from physical-qubit count to logical-qubit count and application-feasibility crossovers (Files 9, 18) is the mature framing, and roadmaps should be assessed by logical targets and resource-estimated crossovers, not raw counts. Milestones should be classified by type (peer-reviewed > count > composite > press-release), grounded in track record and resource estimation, and read with default timeline skepticism given the industry's pattern of slippage. The durable lesson — credit demonstrated (peer-reviewed, independently-benchmarked) results and track record, discount projected milestones and press releases, and read all metrics with awareness of favorable framing — is the skeptical, evidence-based roadmap assessment this file models, connecting to the benchmarking rigor (File 22), competitive landscape (File 20), resource estimation (File 18), and honest path-to-utility assessment the database maintains throughout. Roadmaps reveal ambition; demonstrated results and track records reveal capability — and it is the latter, not the former, that an analyst should trust.

*Cross-references: hardware modalities and their scaling challenges (Files 3–7, 11); below-threshold error correction and qLDPC as roadmap milestones (File 9); resource estimation grounding feasibility crossovers (File 18); competitive/business positioning (File 20); benchmarking metrics and metric-shifting (File 22); classical rebuttals affecting supremacy claims (File 14); Chinese program opacity (File 21); NISQ-application skepticism tempering near-term claims (File 17); market-revenue reality (File 24).*
