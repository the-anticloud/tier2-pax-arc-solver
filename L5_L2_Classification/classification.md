# L5 Narrow / L2 General Classification — PAX_ARC_SOLVER
**Platform:** Anticloud | **PAX:** 27B | **IP:** USPTO pending 2026, Anticloud FZ LLE

## L5 Narrow
PAX_ARC_SOLVER operates at L5 Narrow: it applies PAX 27B's reasoning to structured abstract reasoning tasks (ARC challenge format, combinatorial optimization, constraint satisfaction). It does not attempt free-form generation — it validates every output against the problem constraints before returning.

## L2 General
L2 General means PAX_ARC_SOLVER is invocable for any structured reasoning problem across all deployment domains: surgical planning constraint satisfaction, robotics path planning optimization, security policy rule conflict resolution.

## PAX Integration
PAX 27B generates candidate solutions; PAX_ARC_SOLVER validates them against constraint sets and re-prompts PAX with failure feedback in a loop until a valid solution is found or budget is exhausted.

## AIOSS Audit Relevance
Every reasoning trace (problem hash + solution hash + constraint validation result + iterations) produced by PAX_ARC_SOLVER is appended to the AIOSS chain.
Chain formula: H_n = SHA3-256(H_{n-1} || entry_hash_n || timestamp_n)
Tamper-evident, air-gap verifiable, no cloud dependency.

## Regulatory
ISO/IEC 25010 (correctness, reliability), NIST AI RMF 1.0 (trustworthy AI)
