# The maximum clause of Rosser–Schoenfeld 1962, Theorem 12

This separate solution proves the maximum of `ψ(x)/x` at 113 and an explicit uniqueness refinement.
The paper's printed maximum clause does not explicitly assert uniqueness. This is a formally
stronger formulation of the printed wording; no mathematical novelty is claimed. Equation (3.35)
remains in `RosserSchoenfeld1962.v1`; neither its conclusion nor `RS_prime.theorem_12` changes.

The main internal result is `RS12Max113.psi_strict_maximum`:

```lean
∀ {x : ℝ}, 0 < x → x ≠ 113 →
  113 * Chebyshev.psi x < x * Chebyshev.psi (113 : ℝ)
```

The endpoint `ψ(113)` stays symbolic. An integer LCM comparison and two exact rational logarithm intervals prove
`113 * 1.03882 < ψ(113)`. A finer logarithm majorant, verified from supplied factor and binary exponent witnesses, checks small integer
arguments other than 113 below that separating slope. A sharper seed at 600 supplies five units of savings, carried through
v1's existing finite checker to prove `ψ(n) + 5 ≤ 1.038829 * n` through 500000. This is
strictly below `1.03882 * n` on that interval. Above 500000, the recurrence and v1's
already proved equation (3.35), applied at `x/60`, give the strict bound directly.
The floor identity extends the finite integer bound to real arguments.

The original route ([pp. 77–78](https://doi.org/10.1215/ijm/1255631807)) uses Gram's
table through 2000, Theorem 18 and Table III for `2000 ≤ x < 50653`, Theorem 18 and
(4.12) for `50653 ≤ x ≤ 10^8`, and Table I above `10^8` (justified in Section 6).
This formal proof uses the same `1.03882` separator, but checks integers through 600,
carries the improved seed through the reused certificate to 500000, and uses the
recurrence with (3.35) at `x/60` for the tail. All replacement numerical checks are
kernel verified; the exact maximum remains `ψ(113)/113`.

The Lake project exposes v1's unchanged RS12 sources as a solution-only library using
a sibling `srcDir` derived from the Lake configuration directory, with the same pinned
LeanCert dependency. The source paths point to the original files; compiled outputs belong to the v2 project.
It does not require the entire v1 project, whose `Solution` module would shadow v2's
adapter in `lake env lean`. The v2 adapter imports neither v1's `Solution` nor either
node's generated `Challenge`. Only Mathlib is imported by the core conclusions.

`Max113/Private.lean` resolves private constants in the pinned v1 modules so the existing
kernel-checked certificate proofs can be reused without copying their data or editing
v1. It returns ordinary constant references; all applications are checked by the kernel.
`Max113/Shifted.lean` proves the stronger accumulator invariant. The 21 generated adapter
modules apply that theorem to the 1998 existing witness leaves and join their ranges.
A separate generated small certificate contains 600 factor/exponent triples in 12 blocks.
`Max113/SmallChecker.lean` proves its soundness by adapting v1's factor-witness argument.
The reuse intentionally relies on v1's private source interface, pinned and checked by the
provenance script.

From the repository root:

```sh
lake build
python3 scripts/ieantn.py gen-challenges --check
python3 scripts/ieantn.py fingerprint --check
python3 Solutions/RosserSchoenfeld1962.v2/scripts/generate_adapters.py --check
python3 Solutions/RosserSchoenfeld1962.v1/scripts/check_certificates.py "$HOME/src/PNT-current"
(cd Solutions/RosserSchoenfeld1962.v2 && lake build)
python3 scripts/ieantn.py progress RosserSchoenfeld1962.v2
```

For a fork whose committed receipts identify `teorth/IEANTN`, validate those receipts in
their canonical repository context without changing any saved remote:

```sh
GIT_CONFIG_COUNT=1 \
GIT_CONFIG_KEY_0=url.https://github.com/teorth/IEANTN.git.insteadOf \
GIT_CONFIG_VALUE_0=https://github.com/hakonlonmo/IEANTN.git \
python3 scripts/ieantn.py check
```

See `progress.yaml` for the source audit, branch-base dependency on PR #125, proof details,
and local validation results. Maintainer-side Comparator verification remains separate;
this contribution supplies no verification receipt.
