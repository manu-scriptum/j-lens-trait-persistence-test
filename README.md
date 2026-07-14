# j-lens-trait-persistence-test

*(New to this? See [**our primer**](PRIMER.md) — what this is, what we found, and why it might matter,
in plain language with no background assumed.)*

Does a character trait that is **stated outright** ("Maria is generous") stay accessible in a
language model's internal state differently from one that must be **inferred from behavior**
("Maria brings the unsold bread to a shelter"), as unrelated text intervenes? And does merely
**re-mentioning the character** later — with no trait-relevant content — reactivate the trait?

Measured by reading the trait word out of the model's residual stream with the **Jacobian lens**
(J-lens), across a neutral filler block, for five characters.

> **Status: run complete (2026-07-13).** Pre-registered in `prediction.md` before any code; the run
> and analysis are reported in [`results.md`](results.md). This README describes the design; the
> findings summary is below.

## Findings at a glance

- **Inference is real but immediate.** For 4 of 5 characters the inferred trait was strongly elevated
  over its matched control *at the trigger*, then decayed to baseline within one intervening sentence.
- **No stated-vs-inferred decay difference** — both collapse fast; the pre-registered decay
  comparison is a **recency-dominated null**. **[Superseded — see [`results.md`](results.md) §5
  (2026-07-14): a normalization artifact. The *inferred* trait collapses within one sentence, but the
  *verbatim* trait persists 25–54% below its control across the whole filler for 3/5
  (Nadia/Simon/Otto) — registered decay outcome #2, not a null.]**
- **Re-mentioning the character reactivates the inferred trait above baseline in 3 of 5** — a genuine
  directional signal (the one effect not obviously recency), but modest and deep-rank.
- **Peter (`patient`) produced no inference at all** — an informative failure.
- n = 5, single-item medians, no statistics. Full report and caveats: [`results.md`](results.md).

## The question, precisely

No directional prediction is made (see `prediction.md`). Two comparisons are of equal interest:

- **Decay:** how the trait word's rank changes as 10 neutral sentences intervene — for a stated
  trait vs. an inferred one.
- **Reintroduction:** whether a bare later mention of the character's name reactivates the trait.

## Design at a glance

Five characters × three conditions, sharing an identical mundane-town filler block and an identical
reintroduction sentence. Only the trigger differs.

| Character | Trait | Valence |
|-----------|----------|---------|
| Maria | generous | + |
| Peter | patient | + |
| Nadia | brave | + |
| Simon | curious | + |
| Otto | greedy | − |

- **direct** — "[Name] is [trait]." (verbatim)
- **inferred** — a behavior that implies the trait; the trait word never appears.
- **control** — a trait-neutral role-obligatory routine (baseline for the trait word's rank when the
  trait was never introduced). Why a clean control is the hard part of the design is written up in
  `prediction.md` (the control-design addendum).

The trait word's rank/log-probability is read at 13 checkpoints (end of trigger, end of each of the
10 filler sentences, the reintroduction name-mention, and the reintroduction sentence end), across
the 35–90% depth band, using `google/gemma-3-4b-it` and the corresponding J-lens.

## Reproducing

Runs on a Colab **T4 GPU** (no local GPU needed).

1. Open `workspace_stickiness.ipynb` in Colab; set `Runtime → Change runtime type → T4 GPU`.
2. `google/gemma-3-4b-it` is gated: accept the license on Hugging Face, then add your HF token as a
   Colab secret named `HF_token` (key icon in the left sidebar; enable notebook access).
3. Run top to bottom. `jlens` is pinned to a fixed commit; the model/library versions actually used
   are printed into the output cell for the record.

Two things to actually check in the output, not skip past:
- the **single-token check** — all five trait words must come back single-token, or that trait is
  swapped and re-registered in `prediction.md` before continuing;
- the **entity-mention token dump** — confirm the name lookup lands on a real name token for all 15
  texts before trusting any curve.

### Outputs

- `stickiness_results.csv` — full raw sweep (character × condition × checkpoint × layer × target).
- `stickiness_top20.csv` — exploratory top-20 J-lens vocabulary at each checkpoint (not part of the
  pre-registered comparison; also the check for wrong-target cases).
- `stickiness_summary.csv` — median/best rank per (character, condition, checkpoint, target).
- `stickiness_decay_curves.png` — per-character decay curves for the three conditions.

## Reading the results (fixed in advance)

- **Median rank across the band is the primary statistic** (best rank is secondary).
- Compare decay **shape**, each condition normalized to its own distance-0 — **not** absolute
  heights. The direct arm's early points partly reflect surface echo of the just-stated word, which
  for that arm *is* the verbatim persistence being measured; it does not make the two curves' heights
  comparable.
- The reintroduction bump is read **within-condition** (distance-10 → reintroduction).
- Five characters, non-independent layer samples: **no statistics** — direction and consistency are
  reported descriptively, not tested.

## Scope and honesty

This is a small, exploratory measurement on one open model (Gemma-3-4B-IT) through one interpretability
tool. It probes **trait-concept accessibility in the residual stream via the J-lens** — not "the
global workspace" in any theory-laden sense; that framing is contested and not something this setup
can establish. On novelty: the general question may have been gestured at in the entity-tracking and
binding literature; what is distinctive here is applying the J-lens to it. The claim is the
application and a clean, reproducible measurement, not the invention of the question — and if a direct
precedent surfaces, that note gets updated rather than defended.

## Files

- `prediction.md` — the pre-registration and all dated addenda (read this first).
- `workspace_stickiness.ipynb` — the experiment.
- `LICENSE` (MIT) — code. `LICENSE-docs` (CC BY 4.0) — the writeup and data.
