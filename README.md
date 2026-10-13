<!-- solve-problem:53fadb52-56eb-4ed7-a6bb-69cd81ca0597 -->
# Bayesian Sybil reserve prices in proof auctions

A [Tangle Solve problem](https://solve.tangle.tools/problems/53fadb52-56eb-4ed7-a6bb-69cd81ca0597).

## What is the problem?

In markets for zero-knowledge proofs (ZKPs), provers sell proof generation as a service. Prooφ (Wang et al., 2025) proposes a double-auction market mechanism where users submit proof requests and provers bid to fulfill them; the mechanism matches demand and supply and clears prices.

The open problem concerns **Sybil/fake-name manipulation of reserve prices**: a prover (or user) can fabricate multiple fake identities and submit shill bids, effectively creating a hidden, self-serving price floor (or ceiling) that inflates the clearing price or manipulates matching. The task, formally:

- **Setting:** A double auction with $n$ users with valuations $v_i \sim F$, $m$ provers with costs $c_j \sim G$ (independent private values), a truthful (or strategyproof) matching-and-pricing rule $M$. Provers may create $k \geq 1$ Sybil identities with arbitrary reported costs, at zero or negligible cost.
- **Part A (characterization):** Under a common prior $(F, G)$, characterize the Bayesian-Nash optimal fake-identity bidding strategy for a prover, and the resulting expected gain in profit/utility relative to truthful single-identity participation.
- **Part B (design):** Design a mechanism (reserve-price rule, identity-conditional pricing, or payment adjustment) that is (i) incentive compatible for truthful identities, (ii) **false-name-proof** — no prover gains in expectation from Sybils — or (iii) at minimum bounds the expected Sybil gain by a stated function of $k$, $F$, $G$.

The exact formalization in Prooφ's 2025 revision is sketched as a future direction, so the precise objective (full false-name-proofness vs. bounded expected gain) is partly an interpretive choice.

## Why does it matter?

Proof markets are being actively deployed (zkRollups, zkCoprocessors, private smart contracts), and Prooφ is a concrete proposal for pricing ZK proving as a commodity. If provers can freely fabricate price floors via fake identities, quoted clearing prices inflate, honest provers are crowded out, and the mechanism's advertised truthfulness guarantees become vacuous in practice. A solution would give deployable payment rules robust to pseudonymous collusion — relevant also to false-name manipulation in any double-sided marketplace on permissionless blockchains where identities are free.

## What's already known?

- **Prooφ (arXiv:2404.06495)** gives the base mechanism: a double auction for proofs with proof-size-aware matching, strategyproof pricing, and welfare/truthfulness analysis — but under a single-identity assumption; its revision explicitly flags Sybil reserve-price manipulation as open.
- **False-name-proofness literature:** Yokoo, Sakurai and Watanabe's line of work (combinatorial auctions, e.g., their 2004 EC paper and subsequent journal versions) shows false-name-proofness is strictly stronger than strategyproofness, and that many efficient mechanisms (even VCG) fail it. Known false-name-proof mechanisms typically sacrifice efficiency (e.g., the "LDS" mechanism achieving 1/2-approximation in some settings). (Verify exact bounds before citing.)
- **Double auctions:** Classic strategyproof double auctions (McAfee 1992 trade-reduction mechanism) are the natural base; whether trade reduction survives Sybil attacks appears unstudied.
- No published result we are aware of characterizes Bayesian optimal shill bidding in double auctions with distributional knowledge, or gives a false-name-proof double auction with bounds in the Prooφ setting.

## What would count as solving it?

1. **Characterization theorem:** For given $(F, G)$ and mechanism $M$ (start with McAfee-style trade reduction or Prooφ's rule), a closed-form or tight asymptotic expression for the expected utility gain of a prover deploying $k$ Sybil identities under optimal Bayesian play.
2. **Lower bound (impossibility):** A proof that no budget-balanced, individually rational double auction in this setting can be fully false-name-proof, or a characterization of what must be sacrificed.
3. **Construction:** A mechanism with a proven bound $\mathbb{E}[\text{Sybil gain}] \leq \varepsilon(k, F, G)$ — ideally $\varepsilon \to 0$ as $k$ grows or a constant independent of $k$ — while retaining truthfulness for honest identities and approximate efficiency/budget balance.
4. Any of (1)–(3) instantiated concretely on Prooφ's mechanism counts as a referee-acceptable resolution.

## How to start

1. Read Yokoo–Sakurai–Watanabe on false-name-proof combinatorial auctions first (definitions and impossibility techniques transfer directly), then McAfee's 1992 trade-reduction double auction.
2. Reconstruct Prooφ's payment rule from arXiv:2404.06495 (§ on the double auction / pricing) and compute, for a single prover, the expected gain from one Sybil identity with the cheapest possible shill cost — this is a finite-dimensional Bayesian optimization even in the simplest uniform-prior case.
3. The main obstacle: free identity creation means standard single-parameter IC analysis doesn't apply; expect to weaken to *false-name-proofness in expectation* or use identity-count-dependent payments (which provers can evade by splitting).
4. Consider charging per-identity participation fees or making payments depend on rank statistics that Sybils cannot cheaply improve — rank-based trade reduction is a promising starting mechanism.

## Key references

- Wang, Zhou, Yaish, Zhang, Fisch, Livshits (2025), *Prooφ: A ZKP Market Mechanism*, arXiv:2404.06495. https://arxiv.org/abs/2404.06495 — Designs the double-auction ZK proof market this problem attacks; the revision lists the Sybil reserve-price question as future work.
- Yokoo, Sakurai, Watanabe (2004), *The effect of false-name bids in combinatorial auctions: New fraud in Internet auctions*, Games and Economic Behavior. — Establishes that false-name-proofness strictly exceeds strategyproofness and gives impossibility results for efficient false-name-proof combinatorial auctions. (Verify exact venue/year.)
- McAfee (1992), *A dominant strategy double auction*, Journal of Economic Theory. — Canonical trade-reduction strategyproof double auction; the natural baseline mechanism to test for Sybil robustness.
- Ieong, Shoham, Syrgkanis et al., work on shill/false-name bidding in position auctions and beyond — analyzes when splitting or merging bids helps in single-dimensional auctions. (Verify; relevant for the characterization half.)

## Warnings

- The problem originates as an informal "future direction," so the precise objective (exact false-name-proofness vs. bounded expected gain vs. equilibrium analysis) is yours to fix — state your formalization explicitly.
- Don't assume provers' Sybils are detectable; in permissionless settings identity creation is free and unauthenticated, and any mechanism relying on identity verification trivializes the problem.
- Full false-name-proofness + efficiency + budget balance is likely impossible by analogy to the combinatorial-auction literature; don't chase all three at once.
- Beware of "Sybils never help" intuitions from single-sided auctions: in double auctions, shill bids can manipulate both the match set and both sides' prices, so the analysis is genuinely two-sided.

---
*`BCGT-000183` · unverified*

## Acceptance guidance

1. **Characterization theorem:** For given $(F, G)$ and mechanism $M$ (start with McAfee-style trade reduction or Prooφ's rule), a closed-form or tight asymptotic expression for the expected utility gain of a prover deploying $k$ Sybil identities under optimal Bayesian play.
2. **Lower bound (impossibility):** A proof that no budget-balanced, individually rational double auction in this setting can be fully false-name-proof, or a characterization of what must be sacrificed.
3. **Construction:** A mechanism with a proven bound $\mathbb{E}[\text{Sybil gain}] \leq \varepsilon(k, F, G)$ — ideally $\varepsilon \to 0$ as $k$ grows or a constant independent of $k$ — while retaining truthfulness for honest identities and approximate efficiency/budget balance.
4. Any of (1)–(3) instantiated concretely on Prooφ's mechanism counts as a referee-acceptable resolution.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/53fadb52-56eb-4ed7-a6bb-69cd81ca0597?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000183.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000183)
