# Results

Run: 2026-07-13 · `google/gemma-3-4b-it` · `jlens@581d398` · T4 · 15 texts × 13 checkpoints ×
18-layer band (layers 12–29). Pre-registration and all design addenda: `prediction.md`.

## At a glance

- **Inference is real but immediate.** For 4 of 5 characters the inferred trait was strongly
  elevated over its matched control *at the trigger*, then decayed to baseline within one
  intervening sentence.
- **No directional decay difference between stated and inferred traits.** Both collapse fast; the
  pre-registered decay comparison is effectively a **recency-dominated null**.
- **Re-mentioning the character reactivates the inferred trait above baseline in 3 of 5 cases** — a
  genuine directional signal, and the one effect not obviously reducible to recency, but a modest,
  deep-rank one (the trait becomes less buried, not prominent).
- **Peter (`patient`) produced no inference signal at all** — an informative failure, not a data
  problem.
- No statistics: n = 5, single-item medians. Everything below is descriptive.

This document reports the **pre-registered comparison first**, then a clearly separated
**exploratory** section. The exploratory analysis does not modify the confirmatory result.

---

## 1. Confirmatory results (the pre-registered comparison)

The pre-registered measure is the **median rank (over the 18-layer band) of the single-token trait
adjective** (leading-space form), read at each checkpoint, in three conditions: `direct` (trait
stated), `inferred` (trait implied by behavior), `control` (trait never introduced). Lower rank =
stronger; vocabulary ≈ 262k. No directional prediction was registered. The reading rule fixed in
advance: **compare `inferred` against its own `control`**, and compare decay **shape**, not absolute
height (the `direct` arm's early strength is partly surface echo of the just-stated word).

### 1a. At the trigger (distance 0): inference is real for 4 of 5

| Character | Trait | `inferred` | `control` | Inference above baseline? |
|---|---|--:|--:|---|
| Nadia | brave | 66 | 11502 | strong |
| Simon | curious | 44 | 3867 | strong |
| Maria | generous | 768 | 15811 | strong |
| Otto | greedy | 1550 | 17145 | present, weaker |
| Peter | patient | 21658 | 24735 | **none** |

Right after the behavioral sentence, the trait is strongly more accessible than in the control that
never mentioned it — for everyone except Peter, whose fishing sentence produced no trait signal.

### 1b. Across the filler: fast collapse, and no stated-vs-inferred difference

By distance 1–2 every curve falls into the thousands / tens-of-thousands and bounces
non-monotonically. A direct sign that these mid-filler values are at the **noise floor**: `inferred`
frequently drops *below its own control* (e.g. Simon d2: inferred 7360 vs control 3195 — the control
is stronger, which cannot happen if the trait were being actively held).

Consequently, the pre-registered decay comparison shows **no reliable difference between the stated
and inferred traits**: both fall to baseline within one sentence. This is the null outcome (outcome
3 of the registered options) — consistent with a recency-dominated, "aggregate-at-query-time"
account rather than sustained representation.

### 1c. Reintroduction: present but weak

At the entity re-mention (a bare "[Name]", no trait content), median rank of the trait word:

| Character | `direct` | `inferred` | `control` | `inferred` beats `control`? |
|---|--:|--:|--:|:--|
| Maria | 13986 | 23261 | 30141 | yes |
| Nadia | 10085 | 8161 | 11474 | yes (beats direct too) |
| Otto | 1201 | 7772 | 14956 | yes |
| Simon | 603 | 2286 | 2031 | no (≈ control) |
| Peter | 34990 | 42770 | 44899 | no (no bump) |

The stated trait reactivates in 4 of 5 (all but Peter). The **inferred** trait exceeds its control in
**3 of 5** (Maria, Nadia, Otto) — a genuine directional signal, and notably the one effect here that
is *not* obviously reducible to recency (a bare name, with no trait content, should not selectively
raise the trait unless it remains bound to the entity). It is modest, though: the reactivation stays
deep in the ranking — the concept does not return to the top-20 (see E5) — and with n = 5 it is
suggestive, not established.

**Confirmatory summary.** Inference is readable at the moment of the behavior, does not persist
through intervening text (no stated-vs-inferred decay difference; recency dominates), and is
reactivated above baseline by re-naming the entity in a majority of cases — a genuine but modest,
deep-rank effect. One of five characters (Peter) showed no inference to begin with.

---

## 2. Exploratory results (not pre-registered)

Post-hoc, walled off from Section 1, does not change it. Full detail and the pre-declared concept
lexicons are in the `prediction.md` post-run addendum (E1–E6). In brief:

- **E1 — The trait rides on sibling words, mostly nouns.** A concept-lexicon scan (synonyms + noun
  forms, declared before scanning) found the concept at **rank 1 in 11–13 of 18 layers** at d0 for
  Maria (`donations, charity, kindness`), Nadia (`heroism, courage, bravery`), Simon
  (`curiosity, fascination`). The pre-registered adjective *undercounts* the inference — badly for
  Maria (`generous`=768 vs the concept at rank 1).
- **E2 — Reading at the sentence-final period biases toward sentence-openers** (`But, However, Every`,
  pronouns); trait tokens sit beneath that structural layer.
- **E3 — The collapse is concept-wide, not lexical.** The whole concept neighborhood leaves the
  top-20 within one sentence for every character; the exact-rank analysis agrees it decays to
  baseline. The trait is genuinely gone, not merely re-spelled.
- **E4 — The median did not hide a sub-band signal.** Best-layer beats control's best-layer at ≈50%
  of off-d0 checkpoints (chance). Structural note: d0 inference peaks mid-band (best layer ≈ 18–19
  of 34).
- **E5 — Reintroduction is a modest deep-rank shift, not a return to salience.** The concept does not
  re-enter the top-20 at re-mention (only Otto flickers). This tempers a stronger reading of 1c.
- **E6 — The two readouts are complementary.** The concept-scan sees Maria's strong-but-diffuse
  signal the adjective missed; the exact-rank analysis sees Otto's weak `greed` (≈1550, below the
  top-20 ceiling) the concept-scan missed. Neither alone suffices.

These observations are the basis for the v3 design spec in `prediction.md` (concept-set targets,
noun forms, content-position reads, deeper-than-top-20 storage, d0 stimulus screening, and a
KV-ablation test for cache-vs-reconstruction).

---

## 3. Interpretation — stated carefully

What this measures is **trait-concept accessibility in the residual stream via the J-lens** — *not*
"the global workspace" in any theory-laden sense. The result is neutral on whether the J-space is a
genuine workspace (that is a discriminant-validity question this design cannot touch).

On "is the workspace a cache?": our readout does **not** behave like a persistent store — the trait
is not held across intervening text; it looks reconstructed on cue. The reintroduction effect —
genuine in 3 of 5, but modest — is consistent with reconstruction from the ordinary key/value cache
when the name recurs, rather than with workspace-level storage. This leans toward a transient "blackboard" over a "cache," but it
is a hint, not a verdict — we measured a *disposition-to-say* readout, not workspace contents, so
absence from the readout is not absence from the model. A causal test (ablate the trait sentence's
KV, see whether the reintroduction bump survives) would settle it; that is a v3 goal.

---

## 4. Limitations

- **n = 5, single-item medians, no statistics.** Every count ("3 of 5") is descriptive, not a test.
- One 4B open model, one lens, one filler block, one town. A mechanistic first look.
- Four non-negative traits and one negative — not a balanced valence design.
- The pre-registered single-token target is a conservative, noisy proxy (see E1/E6).
- The readout is prediction-flavoured (fitted on wikitext) and read at sentence-final positions
  (see E2) — both bias what surfaces.

---

## Files

- `prediction.md` — pre-registration + dated addenda (design, control rationale, novelty, post-run
  exploratory notes + v3 spec).
- `workspace_stickiness.ipynb` — the experiment (pinned environment).
- `stimuli.csv` — the 15 exact texts fed in, with checkpoint tokens (provenance).
- `stickiness_results.csv` — full raw sweep (4914 rows). `stickiness_top20.csv` — top-20 vocabulary
  per checkpoint (70200 rows). `stickiness_summary.csv` — median/best rank per cell.

The per-character decay-curve figure is not shipped; it is regenerable by running the notebook's
final plotting cell against `stickiness_summary.csv`.
