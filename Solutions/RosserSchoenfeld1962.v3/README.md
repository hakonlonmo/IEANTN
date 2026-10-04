# Rosser–Schoenfeld 1962, equation (3.8)

The conclusion `RosserSchoenfeld1962.v3.equation_3_8` states exactly
`∀ x : ℝ, 41/2 ≤ x → 3*x/(5*Real.log x) < IEANTN.primeCounting (2*x) - IEANTN.primeCounting x`.
The prime counts include their upper endpoints. `Solution.lean` exports the matching
`challenge_equation_3_8` with no hypotheses from other IEANTN nodes.

The proof uses an elementary alternative to the paper's immediate Theorem 2 route:

* `Interval/Lower.lean` proves a weighted-factorial lower bound for ψ, adapting only
  elementary support from read-only PNT+ at `81eca5aa3fa4779883a9aeba18e49087c5366e0f`.
* `Interval/Counting.lean` relates the exact interval `(x,2x]` to its cardinality and θ sum.
* `Interval/Tail.lean` combines that bound, Mathlib's Costa–Pereira inequality, and the
  unchanged v1 proof of ψ(x) < 1.03883x, for x ≥ 4096. Its module docstring and
  `progress.yaml` give the exact cutoff inequalities and numerical margins.
* `Interval/FiniteCheck.lean` proves the soundness of square-root-bounded primality tests,
  witness-list ordering, endpoint checks, and the logarithmic comparison. `Data000` through
  `Data047` contain 3,356 witnesses covering `[41/2,4096)`. Primality checks run in batches
  of at most 64 with `decide +kernel`. `Interval/Finite.lean` assembles the 48 intervals.
* `Interval/Dyadic.lean` checks the secondary Erdős 306 specialization, taking the new
  conclusion as a hypothesis. It adapts the read-only PNT+ argument and imports no axiom.

The Lake project reuses the sibling v1 `RS12` library directly, including its actual proofs;
no v1 conclusion is assumed. It uses the same pinned LeanCert dependency and the repository's
Lean 4.34.0-rc2 toolchain. `provenance.json` inventories the 51 unchanged inherited sources;
it is a source inventory, not a verification receipt. The mathematical proof depends only on
Mathlib and this already-audited v1 proof/dependency library.

Build with `python3 scripts/build_sequential.py` from this directory (or any directory when
using the full script path). The sequential runner avoids concurrent compilation of memory-heavy
inherited certificates and finishes with an ordinary `lake build`. To reproduce the finite
sources, run `python3 scripts/generate_finite.py --check`. Python supplies data only; Lean
checks every asserted property. No external numerical result is trusted.

The source audit, rejected proof routes, dependencies, and validation record are in
`progress.yaml`. Comparator verification is a separate maintainer workflow; no receipt or
`lean-comparator` justification is created here. The pre-existing v1/v2 sources and the
read-only PNT+ checkout are unchanged.
