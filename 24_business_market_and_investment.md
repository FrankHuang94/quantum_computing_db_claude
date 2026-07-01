# Business Context, Market Sizing, and Investment Landscape

> This file covers the business and investment dimension of quantum computing: market-size estimates (and their wide variance), the funding landscape, the SPAC-era public listings and aftermath, enterprise-customer engagement patterns, the talent market, and the post-quantum-cryptography migration as arguably the largest current quantum-adjacent economic activity. It is a "honesty/due-diligence" file emphasizing the gap between valuations/hype and current commercial reality. It builds on Files 17 (NISQ-application commercial reality), 18 (the long timeline), 20 (the vendor landscape), and 21 (national programs, PQC).

---

## Part I — Market Sizing and Its Variance

### 1. Market-size estimates

Published market-size forecasts for quantum computing vary *enormously* — often by an *order of magnitude or more* between sources — reflecting deep uncertainty:

- **The range:** forecasts from major consultancies/analyst firms span from single-digit billions to tens/hundreds of billions of dollars by 2030–2040, depending on assumptions.
- **The variance:** the wide variance reflects the *timeline uncertainty* (File 18 — when does useful fault tolerance arrive?) and the *pre-revenue reality* (Section 5). Forecasts making aggressive fault-tolerance-timeline assumptions project large markets; conservative ones project small.
- **Historical overoptimism:** market forecasts have *historically proven overoptimistic on near-term timelines* relative to actual delivered commercial revenue — a pattern common to emerging technologies (File 20).

The honest treatment: market forecasts should be viewed as a *calibration exercise* (illustrating the range and uncertainty) rather than presenting any single number as authoritative. The wide variance and historical overoptimism (File 20) mean *no single market-size number should be trusted* — the discipline is to understand *why* they vary (timeline assumptions, File 18) and to treat the large-market projections as *contingent* on useful fault tolerance arriving (File 18).

### 2. Why market sizing is hard

Market sizing is genuinely difficult because:

- **The technology is pre-commercial** (Section 5) — most "revenue" is R&D/government/pilot, not production, so extrapolating a market from current revenue is misleading.
- **The timeline is uncertain** (File 18) — the large market depends on useful fault tolerance, whose arrival is a shrinking-but-uncertain range (File 18).
- **The applications are uncertain** (File 17) — which applications deliver value (and when) is contested (File 17), so the addressable market is uncertain.
- **Definitional ambiguity** — "quantum computing market" may or may not include sensing (File 16), PQC (File 21), and services, inflating or deflating the number.

These difficulties (pre-commercial, uncertain timeline/applications, definitional ambiguity) explain the wide forecast variance (Section 1) and counsel treating market projections skeptically — the same skepticism the database applies to advantage claims (Files 14, 17) and roadmaps (File 19).

---

## Part II — Funding and Public Markets

### 3. The funding landscape

Quantum-computing funding comes from distinct sources (File 20):

- **Private venture capital:** total disclosed VC investment into quantum-computing companies by year — with the funding environment *sensitive to broader deep-tech/speculative-technology investment sentiment cycles* (a notable slowdown in quantum-specific VC in certain years correlating with broader VC market conditions, *not* quantum-specific technical setbacks).
- **Corporate R&D:** the tech giants (IBM, Google, Microsoft, AWS, Intel) and Honeywell (Quantinuum) fund from corporate budgets — the deepest, most stable capital (File 20).
- **Government funding:** national programs (File 21) — increasingly significant, and in some cases (PsiQuantum's Australian government investment, File 20, 21) *dominant* for specific companies, distinct from and sometimes exceeding private VC.
- **Public markets:** SPAC listings (Section 4) provided public capital.

The funding mix (private VC, corporate R&D, government, public markets) shapes the sector's resilience: corporate-backed and government-backed players (giants, Quantinuum, PsiQuantum) have the most stable funding, while VC-dependent and public-pure-plays face sentiment-driven volatility. Government funding's growing role (File 21) is a notable trend, sometimes dominating private capital for specific companies.

### 4. The SPAC era and aftermath

The **2021–2022 SPAC wave** — IonQ, Rigetti, D-Wave going public via special-purpose-acquisition-company mergers (File 20) — was a distinctive capital-raising pattern:

- **The listings:** provided public capital and visibility during a hot market.
- **The aftermath:** subsequent *share-price performance was volatile* (File 20, Section 6), and these companies experienced *dilution and follow-on-raise* pressures as they burn capital toward long-horizon profitability (File 18).
- **The lesson:** the SPAC episode illustrates the *gap between public-market-enthusiasm timing and underlying-technology-maturity timing* — the companies went public when the technology was (and remains) years from useful commercial revenue (File 18), creating a mismatch fueling volatility.

The SPAC era and its aftermath (File 20) is a cautionary case study in taking a long-horizon deep-tech sector public before commercial maturity — a pattern relevant to the sector's public-market dynamics and to assessing the risks of quantum-computing investment (Section 7).

### 5. The revenue reality

The essential business caveat (File 20): **total industry-wide quantum-computing revenue remains very small relative to valuations and funding**, and nearly all of it is *R&D-services, government-contract, and early-pilot-program* revenue — *not* recurring production-workload revenue (File 20, Section 18).

- **The scale:** sector-wide revenue is in the hundreds of millions of dollars annually (estimated), most of it non-production.
- **The gap:** aggregate valuations and funding (tens of billions) vastly exceed current revenue — pricing *future* potential (File 18), not current traction.
- **The closest to production:** D-Wave's annealing customers (File 17) — but with contested advantage (File 17).

The revenue reality is the single most important business caveat (File 20): the sector is *pre-revenue at scale*, and any business/investment reading must grapple with the gap between long-horizon potential (File 18) and current pre-commercial reality. This is not a criticism (early deep-tech sectors are normally pre-revenue) but an essential calibration against hype (File 17, 20).

---

## Part III — Customers, Talent, and the PQC Economy

### 6. Public-market volatility

The publicly-traded pure-play quantum stocks (IONQ, RGTI, QBTS, and others, File 20) have exhibited *very high share-price volatility*, often correlated with *general speculative-technology sentiment cycles* rather than company-specific technical milestones:

- Share prices have swung dramatically on macro sentiment, AI-adjacency hype (File 20), and occasionally milestone news.
- The volatility reflects the gap between *long-horizon* technology maturity (File 18) and *short-horizon* market sentiment — a recurring feature of emerging-technology public markets.
- This pattern is worth noting for investment-research context (with the standard *not-investment-advice* caveat).

Public-market volatility (File 20) is a defining feature of the quantum-stock sector — sentiment-driven, high-variance, decoupled from technical milestones — reflecting the mismatch between the long technology timeline (File 18) and short-term market dynamics. Investors and analysts should expect and account for this volatility.

### 7. Enterprise customer engagement

Characterizing *what enterprise "quantum adoption" actually consists of today* (File 17, 20) provides a grounded, non-speculative picture:

- **Cloud-access subscriptions:** internal R&D teams exploring quantum via IBM/AWS/Azure — the most common, modest-revenue.
- **Joint research/pilot partnerships:** vendor + enterprise co-developing a use case, with named milestones but limited public disclosure — capability-building, hedging (File 17).
- **Production-workload contracts:** a *smaller number*, concentrated in D-Wave's annealing customer base (File 17), contested advantage.
- **Consulting/services:** algorithm and pilot services (QC Ware, formerly Zapata, File 20).

The enterprise landscape is dominated by *R&D exploration and pilots* — companies building capability and hedging future risk (File 17), *not* deploying production quantum workloads. This grounded picture (distinct from press-release framing implying imminent deployment) is essential for assessing the sector's true commercial traction (File 17, 20). Enterprise "adoption" today is *preparation*, not *deployment*.

### 8. The talent market

The PhD-level quantum-talent market (File 20) has distinctive dynamics:

- **Scarcity and concentration:** a small pool of PhD-level hardware/algorithms talent, concentrated in a few academic lineages (File 20), with intense competition among companies.
- **AI/ML competition:** the AI/ML industry (File 20) competes for the same quantitative talent, offering higher compensation and faster impact — a challenge for quantum-talent retention.
- **Pipeline maturation:** the recent (2020s) emergence of *dedicated quantum-focused graduate and undergraduate degree programs* — the field's talent pipeline maturing beyond pure physics-PhD-to-industry transitions.
- **Cross-pollination:** with AI/ML (ML decoders, File 9; NVIDIA's entry, File 20) — bringing techniques and competition.

The talent market — scarce, concentrated, AI/ML-competitive, but with a maturing pipeline — is a key competitive factor (File 20) and a real constraint on the sector's growth. A company's ability to attract and retain talent (via compensation, research freedom, mission) is a competitive asset (File 20), and the maturing degree-program pipeline is a positive long-term signal.

### 9. The PQC-migration economy

A crucial, frequently-overlooked point (File 21): the **post-quantum-cryptography (PQC) migration** (File 21) is arguably the *largest current dollar-value quantum-computing-adjacent economic activity*:

- **The activity:** enterprises and governments spending on cryptographic audits, library updates, hybrid classical/PQC deployment, and crypto-agility (File 21) — a substantial, already-underway, multi-year global effort.
- **The distinction:** this is technically a *classical-cryptography response* to the *anticipated* quantum threat (File 21), *not* quantum-technology spending — but it is driven by quantum computing (Shor's algorithm, File 13; the HNDL threat, File 21).
- **The scale:** the PQC migration's spending likely *exceeds* actual quantum-computing revenue (Section 5), making it the largest "quantum" economic activity — an important, overlooked distinction for market sizing (Section 1).

The PQC-migration economy (File 21) is a striking illustration of quantum computing shaping economic activity *today* — years before a useful quantum computer exists — via the *defensive* response to its *anticipated* threat. For any total market-sizing exercise (Section 1), distinguishing quantum-*technology* spending (small, pre-revenue) from quantum-*adjacent* spending (PQC migration, larger) is essential — the biggest near-term "quantum" economic activity is defensive classical cryptography, not quantum technology.

---

## Part IV — Hype Cycles, PQC Economics, and Assessment

### 10. The hype-cycle dynamics

Quantum computing has moved through *hype cycles* (Files 17, 20) — a pattern the disciplined analyst must recognize:

- **The pattern:** media coverage, investor enthusiasm, and vendor marketing periodically outrun demonstrated results (File 17), followed by disillusionment when advantage fails to materialize on schedule, then renewed hype on a new milestone.
- **The drivers:** genuine technical progress (File 9) mixed with overstated near-term-advantage claims (File 17), AI-adjacency hype (File 20), and the long gap between technology and commercial maturity (File 18).
- **The healthy response:** neither uncritical enthusiasm nor blanket dismissal, but *evidence-based, milestone-tracked assessment* (Files 17, 19, 22) — crediting genuine progress (below-threshold error correction, File 9) while skeptically scrutinizing advantage and timeline claims (Files 14, 17).

The hype-cycle dynamics (File 17) are a defining feature of the sector's *business* dimension — driving funding swings (Section 3), public-market volatility (Section 6), and periodic disillusionment. Recognizing and resisting the hype cycle (via the database's skeptical frameworks, Files 14, 17, 19, 22) is essential to sound business assessment, and the sector's credibility (and funding stability) depends on the community holding claims to evidence-based standards (File 17).

### 11. The insurance/risk framing for PQC

Returning to the PQC migration (Section 9; File 21) from a *risk/insurance* angle: the substantial, already-underway enterprise/government spending on PQC migration (Section 9) is, economically, a form of *insurance* against the anticipated quantum threat:

- **The premium:** the cost of PQC migration (audits, updates, hybrid deployment, File 21) — paid *now*.
- **The insured risk:** the future compromise of encrypted data by a CRQC (File 18) — via the HNDL threat (File 21), a *present* risk for long-term-sensitive data.
- **The rationale:** because the migration takes years (File 21) and the HNDL threat is present (File 21), the "insurance" must be bought *now*, before the CRQC arrives — a rational response to a future risk with present consequences (File 21).

The PQC-migration-as-insurance framing (File 21) clarifies why it is the largest current quantum-adjacent economic activity (Section 9): organizations are paying the "premium" now to insure against a future-but-present-consequence risk. This is a *rational, risk-driven* economic activity (not hype-driven), and it is the clearest example of quantum computing driving substantial economic activity *today* — a striking, frequently-overlooked point for market sizing (Section 1) and business assessment.

### 12. Worked market-sizing analysis

Illustrate the market-sizing difficulty (Sections 1–2) with a worked decomposition. A "quantum computing market" estimate might combine:

- **Quantum-computing hardware/cloud revenue:** small (hundreds of millions, Section 5), pre-revenue at scale — the *core* market.
- **Quantum software/services:** modest (Qiskit/PennyLane ecosystems, TKET, Q-CTRL, consulting, File 20).
- **Quantum sensing (File 16):** a *separate, more mature* market (atomic clocks, magnetometers, gravimeters) — often *included* in "quantum technology" market sizing, inflating the number.
- **PQC migration (File 21):** large (Section 9), but *classical cryptography*, not quantum technology — sometimes included, greatly inflating the number.

Depending on which components are included (core computing only, vs. + sensing + PQC + services), the "market" ranges from *small* (core computing, pre-revenue) to *large* (all-inclusive). This decomposition explains the order-of-magnitude forecast variance (Section 1): the forecasts differ partly on *what they count*. The honest analysis (Section 2) *disaggregates* — core quantum-computing revenue is small and pre-revenue (File 18); quantum-adjacent activity (sensing, PQC) is larger but distinct — rather than citing a single all-inclusive number. This disaggregation is the disciplined approach to market sizing.

### 13. Cross-sector investment comparison

Comparing quantum-computing investment to other emerging-tech sectors (File 20) calibrates the risk profile:

- **Versus AI:** AI has *near-term revenue* (deployed products) that quantum lacks; quantum is a longer-horizon, higher-uncertainty bet with no near-term-revenue anchor (File 20). Quantum investment is more speculative.
- **Versus biotech:** quantum shares biotech's *long timelines, binary technical risk, and milestone-driven* profile — a useful analogy for the pure-plays' "long path to a binary outcome" (File 20).
- **Versus fusion:** the closest analogy — transformative potential, long uncertain timeline, capital-intensive, government + private funding, repeated "N years away" predictions (File 20).

Quantum-computing investment is thus a *long-horizon, binary-risk, capital-intensive, milestone-driven, pre-revenue* bet — more like biotech or fusion than near-term-revenue AI (File 20). This calibration (File 20) frames the risk profile: it requires patient, risk-tolerant capital prepared for a long, uncertain journey, and it is unsuitable for those expecting near-term returns. Understanding this profile is essential for sound investment assessment (with the not-investment-advice caveat).

### 14. Glossary

- **Market-size variance:** the order-of-magnitude range in forecasts, reflecting timeline/application/definitional uncertainty.
- **SPAC:** special-purpose-acquisition-company merger — how IonQ/Rigetti/D-Wave went public (2021–2022).
- **Revenue reality:** the sector is pre-revenue at scale; most "revenue" is R&D/government/pilot.
- **Public-market volatility:** sentiment-driven, high-variance quantum-stock behavior decoupled from milestones.
- **Enterprise engagement:** mostly cloud-access R&D and pilots (capability-building), not production deployment.
- **Talent market:** scarce, concentrated, AI/ML-competitive, with a maturing degree-program pipeline.
- **PQC-migration economy:** the (classical) cryptography-migration spending, arguably the largest quantum-adjacent economic activity.
- **Hype cycle:** the recurring enthusiasm-disillusionment pattern the disciplined analyst resists.
- **Quantum-adjacent:** sensing (File 16), PQC (File 21), services — distinct from core quantum-computing revenue.

---

## Part V — Investment Assessment, Reader's Guide, and Summary

### 15. Assessing the investment case honestly

For readers using this database for investment-research context (with the standard *not-investment-advice* caveat), the honest investment assessment (File 20) grapples with:

- **The pre-revenue reality** (Section 5) — the sector is not yet a commercial business at scale.
- **The long timeline** (File 18) — useful fault tolerance is plausibly 2030s+, a long horizon for returns.
- **The modality uncertainty** (File 18) — which approach wins is unknown, so single-company bets carry modality risk.
- **The high volatility** (Section 6) — sentiment-driven, decoupled from milestones.
- **The concentration of value** in well-capitalized players (giants, Quantinuum, PsiQuantum, File 20) vs. higher-risk smaller players.
- **The hedged options** — platforms (Microsoft, AWS) and picks-and-shovels (Q-CTRL, Riverlane, File 20) benefit regardless of modality.

The disciplined investment view: the *technology* is real and progressing (File 9), but the *commercial timeline* is long and uncertain, the *public-market dynamics* are volatile, and the *modality outcome* is unresolved — a high-risk, long-horizon, patient-capital sector requiring careful, risk-aware, long-horizon assessment (File 20). This structural characterization (not advice) frames the investment profile honestly.

### 16. The technology-vs-business timeline mismatch

The defining business insight (File 20): *the technology and the business are on different timelines*. The *technology* is progressing genuinely and measurably (below-threshold error correction, dozens of logical qubits, File 9). The *business* is pre-revenue at scale (Section 5), competing for position ahead of a future market whose arrival (File 18) is uncertain, amid volatility and hype cycles. This mismatch — real technical progress, long uncertain commercial timeline — is the essential business calibration, and it explains the sector's dynamics: valuations pricing future potential (Section 5), volatility from the timeline gap (Section 6), hype cycles from the mismatch (Section 10), and consolidation as capital tests the long path (File 20). Holding both truths — real progress *and* long timeline — is the disciplined business assessment.

### 17. Reader's guide to business assessment

For an analyst assessing the quantum-computing business/market:

1. **Distinguish core computing from quantum-adjacent** (Section 12) — core computing is small/pre-revenue; sensing (File 16) and PQC (File 21) are larger but distinct.
2. **Treat market forecasts skeptically** (Section 1) — order-of-magnitude variance, historical overoptimism; understand *why* they vary.
3. **Apply the revenue-reality caveat** (Section 5) — the sector is pre-revenue at scale; discount production-deployment framing.
4. **Recognize the hype cycle** (Section 10) — credit real progress, discount overstated advantage/timeline claims.
5. **Assess funding resilience** (Section 3) — corporate/government-backed players are more stable than VC/public pure-plays.
6. **Note the PQC economy** (Section 9) — the largest current quantum-adjacent activity, a rational insurance response (Section 11).
7. **Calibrate the investment profile** (Section 13) — long-horizon, binary-risk, like biotech/fusion, not near-term-revenue AI.

Applying this guide grounds business assessment in evidence and honest calibration, resisting both hype and dismissal — the disciplined approach the database maintains, and the business-dimension counterpart to the algorithm (File 13), NISQ-application (File 17), roadmap (File 19), and benchmarking (File 22) frameworks.

### 18. Summary

The business and investment dimension of quantum computing is defined by a *mismatch between long-horizon technology and short-horizon markets*: the technology is progressing genuinely (File 9), but the sector is *pre-revenue at scale* (most "revenue" is R&D/government/pilot, not production), with market forecasts varying by an order of magnitude (reflecting timeline, application, and definitional uncertainty) and historically proving overoptimistic. Funding comes from private VC (sentiment-sensitive), corporate R&D (stable), government (growing, sometimes dominant), and public markets (the volatile 2021–2022 SPAC wave and its dilution-pressured aftermath). Enterprise engagement is dominated by R&D exploration and pilots (capability-building, hedging), not production deployment — with D-Wave's contested-advantage annealing customers the closest to production. The talent market is scarce, concentrated, and AI/ML-competitive, with a maturing degree-program pipeline. Crucially, the *post-quantum-cryptography migration* (File 21) — a classical-cryptography response to the anticipated quantum threat, driven by the HNDL risk — is arguably the *largest current quantum-adjacent economic activity*, a rational insurance response paid now against a future-but-present-consequence risk, and an essential distinction for market sizing (core quantum-computing revenue is small; quantum-adjacent activity including sensing and PQC is larger but distinct). The sector's hype cycles (recurring enthusiasm-disillusionment) demand evidence-based assessment, and its investment profile (long-horizon, binary-risk, capital-intensive, like biotech/fusion) requires patient, risk-tolerant, well-calibrated capital. The disciplined business assessment — disaggregating core from adjacent, treating forecasts skeptically, applying the revenue-reality caveat, recognizing the hype cycle, and holding both the real technical progress and the long uncertain commercial timeline in view — is the honest calibration this file contributes, completing the commercial and financial picture of a transformative technology whose physics, hardware, algorithms, and resources the database's technical files develop, and whose long, uncertain, but real path from laboratory to industry the strategic files (Files 19, 20, 21, 25) together document.

*Cross-references: NISQ-application commercial reality and the hype cycle (File 17); the long fault-tolerance timeline (File 18); the vendor landscape, revenue reality, SPAC dynamics, and talent (File 20); PQC migration and national programs (File 21); quantum sensing as a mature adjacent market (File 16); D-Wave annealing customers (File 17); the classical-comparison and benchmarking discipline underlying honest assessment (Files 14, 22); roadmap credibility (File 19).*

---

## Appendix — Business Reference and Cross-Cutting Observations

### The business dimension at a glance

| Aspect | Reality |
|---|---|
| Market-size forecasts | Order-of-magnitude variance; historically overoptimistic on near-term |
| Sector revenue | Hundreds of millions/year (est.), mostly R&D/government/pilot |
| Aggregate valuation/funding | Tens of billions — pricing future potential (File 18), not current traction |
| Funding sources | Corporate R&D (stable), private VC (sentiment-sensitive), government (growing), public (volatile SPACs) |
| Public-market behavior | High volatility, sentiment-driven, decoupled from milestones |
| Enterprise engagement | Mostly cloud-access R&D and pilots (capability-building), not production |
| Closest to production revenue | D-Wave annealing (contested advantage) |
| Largest quantum-adjacent activity | PQC migration (classical cryptography response, File 21) |
| Talent market | Scarce, concentrated, AI/ML-competitive, maturing pipeline |
| Investment profile | Long-horizon, binary-risk — like biotech/fusion, not near-term-revenue AI |

### Three business observations to carry forward

1. **Disaggregate the "market."** Core quantum-computing revenue is small and pre-revenue (File 18); "quantum technology" market sizes often inflate the number by including sensing (File 16) and PQC migration (File 21), which are larger but distinct. Always ask *what is being counted*.

2. **The PQC-migration economy is the largest current quantum-adjacent activity** (File 21) — a rational, insurance-like, present-tense response (audits, library updates, hybrid deployment) to a future-but-present-consequence threat (harvest now, decrypt later), even though it is *classical* cryptography, not quantum technology. This is the clearest example of quantum computing driving substantial economic activity *today*.

3. **Technology and business are on different timelines** — real, measurable technical progress (below-threshold error correction, dozens of logical qubits, File 9) alongside a long, uncertain, pre-revenue-at-scale commercial timeline (File 18). Holding both truths is the essential business calibration, and it explains the sector's valuations (pricing future potential), volatility (the timeline gap), hype cycles (the mismatch), and consolidation (capital testing the long path).

### Final business takeaway

The quantum-computing business is an early-stage, pre-revenue-at-scale, capital-intensive, talent-concentrated, hype-cycle-prone, geopolitically-entangled deep-tech sector whose commercial realization (File 18) is a long-horizon, uncertain prospect — requiring patient, risk-tolerant, well-calibrated capital, and best assessed by disaggregating core from adjacent activity, treating forecasts skeptically, applying the revenue-reality caveat, recognizing the hype cycle, and holding both the real technical progress and the long commercial timeline in view. The disciplined business assessment (this file) complements the technical (Files 2–18) and strategic (Files 19–23, 25) analyses to complete the honest, evidence-based picture of quantum computing's path from laboratory to industry that the database provides.

### Extended note: the investment-thesis structure

An investment thesis for the sector must reconcile several structural facts. First, the *addressable market* is large *if* useful fault tolerance arrives (File 18) — the transformative applications (cryptanalysis, quantum simulation, File 13) have enormous economic value — but that arrival is a shrinking-but-uncertain 2030s-and-beyond prospect (File 18). Second, *current revenue* is minimal and non-production (Section 5), so near-term financials do not support the valuations, which price the future option. Third, *modality uncertainty* (File 18) means single-company bets carry the risk that the company's modality does not win the fault-tolerant race — mitigated by the platforms (Microsoft, AWS) and picks-and-shovels firms (Q-CTRL, Riverlane, File 20) that are hedged across modalities. Fourth, *volatility* (Section 6) means the public pure-plays are sentiment-driven, poorly correlated with technical milestones. And fifth, *survival* through the long pre-revenue phase favors the well-capitalized (giants, Quantinuum, PsiQuantum, File 20). The resulting thesis structure — a long-dated option on a transformative technology, with modality and timeline risk, high volatility, and survival gated by capital — is more like a venture/biotech/fusion bet than a conventional growth-equity investment (Section 13), and it demands patient, risk-tolerant, diversified, long-horizon capital. This structural characterization (not investment advice) is the honest frame for anyone assessing the sector financially, and it follows directly from the technology's long timeline (File 18) and pre-revenue reality (Section 5).

### One-line business summary

Quantum computing is a pre-revenue-at-scale, long-horizon, high-variance deep-tech sector whose valuations price a future (2030s+, File 18) fault-tolerant market — best assessed by disaggregating core computing from quantum-adjacent activity (sensing File 16, PQC migration File 21), treating market forecasts and advantage claims skeptically (Files 14, 17, 22), applying the revenue-reality caveat, and holding both the real technical progress (File 9) and the long, uncertain commercial timeline (File 18) in view.

### Extended note: distinguishing quantum from quantum-adjacent

The single most clarifying move for quantum-computing market assessment is to *distinguish quantum-technology spending from quantum-adjacent spending*. Core quantum-*computing* revenue (hardware, cloud access) is small and pre-revenue at scale (Section 5). But "quantum" market figures often bundle in: quantum *sensing* (File 16), a mature, deployed, revenue-generating market with a *different* technology and maturity; and the *post-quantum-cryptography migration* (File 21), a large but *classical-cryptography* activity driven by (not consisting of) quantum computing. Including these inflates the "quantum market" by orders of magnitude and conflates fundamentally different activities. The disciplined market analysis disaggregates: core quantum computing (small, pre-revenue, File 18), quantum sensing (mature, File 16), PQC migration (large, classical, File 21), and quantum software/services (modest) — each assessed separately, with the "quantum computing" market specifically being the small, future-contingent one. This disaggregation explains the order-of-magnitude forecast variance (Section 1) and is the essential first step in any honest market-sizing exercise — a discipline that, like the classical-comparison rigor for advantage claims (File 14), guards against the conflation and inflation that pervade the sector's business narratives.
