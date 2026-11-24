# On the Riemann Hypothesis: A Supplement on the Failure of Claude's Proof

**中文标题：关于黎曼猜想——Claude 证明不成立的补充**

---

## What this repository is

A line-by-line source audit of the claimed Lean 4 proof of the Riemann Hypothesis published as
[`CocoSgt/riemann-hypothesis-lean`](https://github.com/CocoSgt/riemann-hypothesis-lean) (a mirror of
Levent Alpöge's formalization "with Claude").

It is written as a **supplement** that can be placed after an existing critique: every conclusion of that
critique is kept, and four things are added — four pieces of evidence it did not have, a verdict on its
open A/B question, corrections to six statements that could be dismissed with a single sentence, and a
line-by-line checkable evidence table.

## What it claims

Two independent reasons why **this artifact has not proved the Riemann Hypothesis**:

1. **The load-bearing premise is unproven.** The whole 175,187-file, 909,822-import development reduces to
   a single proposition, `UpperTheory.GalStar.Row0b recT` — a Fontaine–Mazur-type local–global Langlands
   statement, strictly stronger than RH — for which the repository supplies only a `def`, never a proof.
2. **Critical conditions are hardcoded to true.** When a hard analytic input is unavailable, the code
   substitutes a vacuously true predicate: `anyGeometry := fun _ _ _ => True` for geometric existence,
   `semipos/nefFib/arNef := fun _ => True` in the only concrete `ShimX` witness, and
   `UnusedSlot72 := True` behind the Siegel–Eisenstein simple-pole slot whose "proof" is `trivial`.

## What it does **not** claim

- It does **not** claim the Riemann Hypothesis is false.
- It does **not** claim the anonymous/other-side author fabricated anything; it claims only that **the proof
  Claude produced does not hold**, i.e. that this repository has not proved RH.
- It does **not** rely on a compile error. Nothing was compiled: the copy contains **0** `.olean` files,
  `.lake` has no `build` directory, and mathlib ships without a `.lake` cache. The argument is a source-level
  argument, reproducible with a text editor.

## The four layers

| Layer | Mathematics it is supposed to carry | What the code actually says | Where |
|---|---|---|---|
| Geometric existence | "this divisor is realized by a geometric object" | `fun _ _ _ => True` | `Definitions\AutomorphicMembership\OverIx.lean:19` |
| Semipositivity and nefness | positivity of a Hermitian line bundle (the hardest analytic input) | `semipos/nefFib/arNef := fun _ => True` | `Proofs\FoundationProofs\ExistsProvisionalShimX.lean` |
| Literature theorems (60 fields) | 60 published theorems | 139 of 177 filling lemmas never use their hypothesis; the critical slot is `trivial` | `ThirdPlaceSeries\EveryCharacterOfLiteratureOfVWin.lean:666–726` |
| Entry point | "a proof with no hypotheses" | never compiled | — |

Take any one layer and "this repository proved RH" fails. With all four at once it is not a contested proof
but a formal artefact in which the mathematical content has been replaced by `True`.

## Repository contents

| Path | Language | Content |
|---|---|---|
| `docs/supplement-en.md` / `.docx` | English | The supplement (this repo's main text) |
| `docs/supplement-zh.md` / `.docx` | 中文 | The same supplement in Chinese |
| `docs/audit-zh.md` / `.docx` | 中文 | A longer standalone audit with the full evidence chain and an appendix of 22 evidence rows |
| `LICENSE` | — | CC BY 4.0 |

## Selected evidence

```
-- Definitions\AutomorphicMembership\OverIx.lean:19
def anyGeometry : GeoP := fun _ _ _ => True

-- Proofs\FoundationProofs\ExistsProvisionalShimX.lean  (fields :236–238 of DataDIV.lean:159)
semipos := fun _ => True
nefFib  := fun _ => True
arNef   := fun _ => True

-- ThirdPlaceSeries\EveryCharacterOfLiteratureOfVWin.lean:505–506
theorem UpperTheory.LiteratureTheorems.siegel_eisenstein_simple_pole
    (O : OpenLiteratureStatements) : GlobalTheory.Published.UnusedSlot72 := trivial
-- where, at LocalTypeDictionary\DiscreteSpectrumResidualClassification.lean:7
abbrev GlobalTheory.Published.UnusedSlot72 : Prop := True
-- while the real statement sits next door in four other bundles, e.g.
-- PublishedTheorems\SiegelWeil₂\SiegelEisensteinSimplePoleOfDoublingMethod.lean:14–19
```

## How it was checked

- Copy audited: `lean.tar.gz` extracted to `D:\LiManCX\lean`; SHA-256
  `9631b4f020199e580ba86178cb56c6e9b09f690c2369a80dc4334aa93f49c7fc`, **byte-identical** to the hash
  published in the mirror's README.
- Scale: 175,187 `.lean` files, 909,822 `import` lines (matches the mirror's own figures).
- Escape hatches scanned across the whole tree: `sorry` 0, `admit` 0, `axiom`/`constant`/`opaque` 0,
  `unsafe` 0, `native_decide` 0, `@[extern]`/`implemented_by` 0, `debug.skipKernelTC` 0.
- Build state: no `.git`, no `.github`, `.lake` contains only `packages`, `*.olean` count 0.
- Every quotation in the supplement is a real source line, with the file's actual line number.

## Reproduce the decisive facts

```bash
# vacuously true geometry predicate and its consumers
rg -n "anyGeometry" src

# the degenerate ShimX witness and its three hardcoded positivity fields
rg -n "semipos|nefFib|arNef" src/Development/AbelianVarietyTheory/Proofs/FoundationProofs/ExistsProvisionalShimX.lean

# the flattened slot and the real statement of the same theorem
rg -n "UnusedSlot72" src
rg -n "SiegelEisensteinSimplePole" src
```

## Citing

```
On the Riemann Hypothesis: A Supplement on the Failure of Claude's Proof.
https://github.com/youbaiyun/riemann-hypothesis-claude-proof-does-not-hold
```

## License

Text and documents: **CC BY 4.0** — see [`LICENSE`](LICENSE). Reuse with attribution.
