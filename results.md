# Results

Run: 2026-07-13 · `google/gemma-3-4b-it` · `jlens@581d398` · T4 · 15 texts × 13 checkpoints ×
18-layer band (layers 12–29). Pre-registration and all design addenda: `prediction.md`.

> **Correction (2026-07-14 · re-analysis of the same run, no re-run).** The decay headline below —
> "no stated-vs-inferred decay difference; both collapse to baseline; neither persisted" — is a
> **normalization artifact and is superseded.** When each arm is compared to its *own* control across
> the filler (the comparison this document already uses at d0 and reintro, but never applied to the
> direct arm), the **verbatim trait persists** — 25–54% below its control at all ten filler
> checkpoints for 3 of 5 characters — while the inferred trait collapses within one sentence. That is
> registered decay outcome **#2** ("direct decays slower"), not the null. Superseded lines are flagged
> inline; full correction, table, and interpretation in **§5**. Everything else is left as first written.

## At a glance

- **A trait signal appears at the trigger for 4 of 5 characters** — the trait word ranks far better
  after a behaviour that *implies* it than after a matched one that implies nothing (its control) —
  then decays to baseline within one intervening sentence. (Some of that d0 signal may be topic-priming
  rather than entity attribution; see §3.)
- **No detectable decay difference between stated and inferred traits** — both collapse to baseline
  within one sentence. A floor-bounded, recency-dominated null (*neither* persisted), not
  demonstrated equivalence. **[Superseded — see §5 (2026-07-14). A normalization artifact: the
  *inferred* trait collapses, but the *verbatim* trait persists across the filler for 3/5. Registered
  outcome #2, not a null.]**
- **Re-mentioning the character reactivates the inferred trait above baseline in 3 of 5 cases** — a
  genuine directional signal, and the one effect not obviously reducible to recency, but a modest,
  deep-rank one (the trait becomes less buried, not prominent).
- **Peter (`patient`) produced no inference signal at all** — an informative failure, not a data
  problem.
- No statistics: n = 5, single-item medians. Everything below is descriptive.
- **Design caveat found post-hoc:** passive reads at sentence boundaries measure spontaneous
  saliency, not cued retrievability, so this design under-tests persistence. The reintroduction is
  the load-bearing measurement — and there, stated traits re-cue better than inferred (4/5). See §3.
  **[Refined in §5: stated actually beats inferred at *every* checkpoint, not only reintro; the
  reintro is load-bearing specifically for the inferred-vs-control *reactivation*.]**

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

Right after the behavioural sentence, the trait word ranks far better than in the control — the same
character under a matched behaviour that implies *no* trait — for everyone except Peter, whose fishing
sentence produced no signal. The comparison is inference-possible vs. nothing-to-infer, not two trait
strengths on one scale.

Reading that table without the statistics: **rank is just queue position** — how many of the model's
~262,000 words are ahead of `brave` in line to be said at that spot, so *lower = more active*. After
the fire sentence, `brave` sat at rank **66**; in the control it sits at **11,502** — roughly **170×
deeper in the queue**. That gap is the whole point. In the control, `brave` isn't "showing up weakly";
it is idling about where the word sits in *any* text that never mentions bravery. The control did its
job — it stayed at baseline.

*Why* does it idle at ~11,500 rather than the very bottom? Mostly **base frequency**: every word has a
default position set by how common it is, before any context (that, not any signal, is why the five
controls differ from one another — `curious` is simply a more expected word than `patient`). The
control also quietly absorbs the character's **role**, fixed in the shared opening (a clinic worker may
sit a touch nearer `brave` than a baker), and any **lexical adjacency** in the control behaviour
itself. All of which is exactly why the comparison is `inferred` vs `control` and never absolute rank:
the control soaks up the frequency floor and the priming, so the gap isolates the trigger.

### 1b. Across the filler: fast collapse, and no stated-vs-inferred difference

By distance 1–2 every curve falls into the thousands / tens-of-thousands and bounces
non-monotonically. A direct sign that these mid-filler values are at the **noise floor**: `inferred`
frequently drops *below its own control* (e.g. Simon d2: inferred 7360 vs control 3195 — the control
is stronger, which cannot happen if the trait were being actively held).

Consequently, the pre-registered decay comparison shows **no detectable difference between the stated
and inferred traits**: both fall to baseline within one sentence (the null outcome — option 3 of the
three we registered). **[Superseded — see §5 (2026-07-14): true only for the d0-normalized *shape*
metric and the *inferred* arm. The direct arm does not fall to baseline for 3/5 — that is registered
outcome #2. The `inferred < its own control` example cited just below is real; the missing symmetric
check is `direct < its own control`, which holds 10/10 across the filler for those 3.]** Two cautions on how to read this null, because it is easy to overclaim:

- It is **"neither persisted," not "persisted equally."** With both arms on the floor after d1, there
  is almost no dynamic range in which a provenance difference *could* appear, so the null does not
  establish equivalence — it reflects that neither trait was represented, not that the two were
  represented identically.
- With **n = 5 and no equivalence test**, we can say a difference was *not detected*, not that none
  exists. (Peter contributes nothing to this comparison, having produced no inferred signal at all.)

So the defensible statement: the mode of introduction — stated outright vs. inferred from behavior —
made **no detectable difference to persistence, in a regime where nothing much persisted.** It is a
real, registered result that could have gone the other way; it is also a floor-bounded, underpowered
null. Both are true. And a deeper caveat applies (§3): reading at sentence-final periods measures
spontaneous saliency, not cued retrievability, so this comparison is only weakly informative about
persistence in the first place.

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

These observations are the basis for the follow-up design spec in `prediction.md` (concept-set targets,
noun forms, content-position reads, deeper-than-top-20 storage, d0 stimulus screening, and a
KV-ablation test for cache-vs-reconstruction).

---

## 3. Interpretation — stated carefully

What this measures is **trait-concept accessibility in the residual stream via the J-lens** — *not*
"the global workspace" in any theory-laden sense. The result is neutral on whether the J-space is a
genuine workspace (that is a discriminant-validity question this design cannot touch).

**On "is the workspace a cache?" — this design mostly cannot tell, and an earlier draft of this
section over-read it.** A cache is defined by *retrievability on demand*, and the only way to test one
is to *query* it. But eleven of our thirteen checkpoints are passive reads at sentence-final periods,
where the disposition-to-say is "begin a new sentence" — a trait adjective is contextually
near-impossible there whether or not it is still bound to the entity. So the decay to baseline across
the filler measures **spontaneous saliency turnover, not eviction**, and is only weakly informative
about persistence as such. The one checkpoint that actually re-queries the entity without surface echo
or fresh topic — the reintroduction — is the design's only genuine (if weak) cache-read, and it shows
modest retrieval above baseline (inferred > control in 3 of 5), which a fully-evicted trait could not.
Properly weighted, the little valid probing we have leans *weakly toward a dormant, retrievable
binding* — not the transient "blackboard" the passive decay superficially suggested. Net: we cannot
separate cache from blackboard here; if anything the evidence tilts to dormant-cache.

**A stated-vs-inferred difference does appear — but in retrievability, not persistence.** At the
reintroduction (the one cued checkpoint), the *stated* trait re-cues better than the *inferred* one in
4 of 5 (all but Nadia: direct 13986/10085/1201/603/34990 vs inferred 23261/8161/7772/2286/42770).
There is a concrete mechanistic candidate for this, and it may be the sharpest thing the study points
to. The stated trait leaves a **literal `generous` token** in the sequence — a cached position whose
key/value the model can attend back to and re-surface directly. The inferred trait is **never
tokenized**: `generous` exists only as a transient latent (which is exactly why the readout was the
only place it ever appeared), so what the cache holds is the *behaviour* (`bread, shelter`), and
re-cueing the trait means **re-inferring it from the scene**, not re-reading a symbol. Re-read vs.
re-derive — and a symbol lookup is cheaper and cleaner than re-running an inference, which is precisely
why stated retrieves better. This is the dissociation the pre-registered *decay* comparison failed to
find; it was hiding in the wrong checkpoint. (n = 5; a lead with a mechanism, not a result.)

**A confound we under-weighted: topic-priming vs. entity attribution.** At the inferred trigger,
concepts like `donations/charity` may surface because the sentence we just read was *about a shelter*
— topic saturation — rather than because "generous" became a property of the *Maria* entity. Maria is
the starkest case: the shelter *is* the charity topic. The design cannot separate "we just discussed
charity" from "the model now attributes generosity to Maria" **at d0**. But the reintroduction *does*
separate them, and this is the correction: it is **topic-free** — "It is Tuesday. Maria ties her
shoelaces" contains no shelter, no charity, nothing but the name — so any inferred > control *there*
cannot be topic-priming; it can only be the trait returning from the entity itself. That happens in 3
of 5. So the topic confound bites the *magnitude of the d0 signal*, but the topic-independent
reintroduction is the clean check, and it comes back positive in a majority — real (if modest) evidence
of genuine entity-binding, not just a momentary charity echo. Read together: d0 is part-topic, the
reintroduction isolates the binding part, and the binding part is there.

**Strength vs. sophistication — and the best hint at a *held* concept.** Keep two things separate.
First, "stated beats inferred" is not "verbatim is better": at the reintroduction the two arms do
different work. Stated = resolve the coreference, then *look up* a stored token. Inferred = resolve the
coreference, retrieve the behavioural scene, and *re-infer* the trait from it — a multi-step
reconstruction triggered by a bare name. So the inferred arm is **modest in strength but far higher in
mechanistic sophistication**; its weakness is the price of real reconstruction, not a failure. Second,
the tantalising part: a topic-free trait that vanishes from the readout and then returns when only the
*entity* is re-cued is exactly the observable you would want if you were hunting for a **held concept**
— a latent property carried across intervening text and reactivated on demand. This is the best hint in
the dataset for such a thing. But *hint*, not evidence, for a precise reason: "recoverable" is
consistent with **two** mechanisms this design cannot tell apart — a genuinely *held* trait latent
(stored, reactivated) versus a *held scene* from which the trait is *recomputed* (nothing trait-specific
stored, only the behaviour). Their observables here are identical. Separating them is exactly what the
KV-ablation / differential-fragility follow-up is for: a held latent survives ablation of the behaviour
tokens and resists interference; pure re-inference dies with the scene. And even a confirmed held latent
would show a *persistent entity-bound trait representation* — a real, modest object — not a "global
workspace" in the theory-laden sense; that is a further, separate claim this line cannot reach. So:
right paradigm, best hint so far, and a clean test already specified that would move it from hint to
evidence.

**Why does the (spontaneous) decay show no stated-vs-inferred difference? (Speculative.)** Two
readings. *Deflationary:* after one filler sentence neither trait is spontaneously expressible, and
you cannot see a provenance difference in a signal that is absent (the floor effect in §1b) — the
parsimonious account, not rule-out-able here. *Substantive:* spontaneous saliency is provenance-blind (both fade alike), while cued retrieval is not
(stated re-cues better) — because the two conditions leave *different things* in the cache: the stated
trait a re-readable token, the inferred trait only a behavioural scene to re-infer from (see above).
The KV-ablation follow-up separates these, and the asymmetry sharpens it: in the **inferred** arm,
ablate the *behaviour* tokens' keys/values and watch whether any retrieval survives — **nothing
survives** = the trait was pure re-inference (it lived only in those tokens); **some survives** = a
genuine latent trace was stored at the position where the trait was assembled, independent of the
scene. In the **stated** arm, ablating just the `generous` token isolates how much of retrieval is the
symbol itself. That is the deep cache-vs-blackboard question, made answerable per arm.

---

## 4. Limitations

- **n = 5, single-item medians, no statistics.** Every count ("3 of 5") is descriptive, not a test.
- One 4B open model, one lens, one filler block, one town. A mechanistic first look.
- Four non-negative traits and one negative — not a balanced valence design.
- The pre-registered single-token target is a conservative, noisy proxy (see E1/E6).
- The readout is prediction-flavoured (fitted on wikitext) and read at sentence-final positions
  (see E2) — both bias what surfaces.
- **Passive reads cannot test a cache.** A cache is a retrievability property; measuring it requires
  *querying* at each checkpoint, not passively reading a period. This design does neither except,
  accidentally, at the reintroduction — so the decay curve is only weakly informative about
  persistence (see §3). The follow-up fixes this with cued retrieval.
- **Single entity.** With one character per text, the design cannot test binding fidelity or
  interference — whether an inferred trait stays attached to the right entity when a competing entity
  is present.
- **Control-trigger topic differs** from the inferred trigger's, so `inferred` vs `control` is not
  perfectly matched even absent the trait; some d0 difference may be topic, not trait (see §3).
- **The control baseline is not perfectly clean, and the shared opening carries uncontrolled
  associations.** Absolute control ranks are dominated by the target word's base frequency, and
  additionally absorb (a) **role-priming** from the shared opening (a clinic worker sits nearer `brave`;
  a repair-counter worker nearer `curious`), (b) **lexical adjacency** in the control behaviour (Peter's
  "lays out his tools" leans methodical ~ patient; Otto's "marks crates against the delivery sheet"
  leans tallying ~ greedy), and (c) the opening naming a **real city, "Lyon,"** which carries its own
  associations — gastronomy, and (trait-adjacently for Nadia) a WWII Resistance / bravery history. All
  of these sit in the *shared* opening or are second-order, so they apply across conditions and largely
  cancel in the `inferred` vs `control` gap — but they are uncontrolled inputs with no upside, absolute
  control numbers are not trait presence, and a cleaner design would neutralise the roles, the control
  vocabulary, and the setting (drop the real place-name for a neutral or invented one) without breaking
  the structural match.

---

## 5. Correction (2026-07-14): the decay null is a normalization artifact

**Status: post-hoc re-analysis of the same run — no new data.** This corrects the *prose* headline of
§1b and the summary. It does **not** alter the pre-registered comparison as executed (that comparison
measured d0-normalized *shape* and genuinely found no shape difference; see below).

**What the headline said.** §1b and "At a glance" reported "no stated-vs-inferred decay difference —
both fall to baseline within one sentence ... neither persisted."

**Why that over-generalized.** The pre-registered decay metric normalized each condition to its *own*
distance-0 and compared shape. That metric is dominated by the d0 echo it was built to neutralise: the
direct arm starts at rank ≈ 3 (pure surface echo of the just-stated word), so its multiplicative drop
across the filler is enormous, and "no direct-slower *shape*" follows trivially — while saying nothing
about where the curve *lands*. The comparison the rest of this document relies on
(`arm` vs its *own* control, used at d0 in §1a and at reintro in §1c) was never run on the direct arm
across the filler. Running it, over d1–d10:

| Character | direct < its control | median direct/control | inferred < its control | median inferred/control |
|---|:--:|:--:|:--:|:--:|
| Nadia | 10/10 | 0.75 | 2/10 | 1.10 |
| Simon | 10/10 | 0.46 | 1/10 | 1.47 |
| Otto  | 10/10 | 0.60 | 5/10 | 1.03 |
| Maria | 5/10  | 1.01 | 0/10 | 1.13 |
| Peter | 8/10  | 0.91 | 8/10 | 0.94 |

(ratio < 1 = the trait word sits below its frequency floor = persisting; median over d1–d10;
regenerable from `stickiness_summary.csv`.)

For **Nadia, Simon, Otto (3/5)** the verbatim trait word stays 25–54% below its own control at **all
ten** filler checkpoints — sustained verbatim persistence, not collapse. Their inferred arm is at or
above its floor (ratio ≥ 1.0) the whole way. **Maria** is the anomaly (and our running example): her
direct arm barely persists (1.01) and her inferred arm is *worse* than its control throughout (1.13).
**Peter** produced no usable signal in either arm (both ≈ floor), consistent with his d0 null.

**Corrected statement.** The inferred trait collapses to baseline within one filler sentence; the
verbatim trait does not — it persists across the entire filler for 3 of 5. There *is* a
stated-vs-inferred decay difference: **registered outcome #2 ("the direct condition decays slower"),
not #3.** The pre-registered *shape* comparison is not wrong as executed — d0-normalized shapes do not
differ — but the prose translated "no shape difference" into "both collapse / neither persisted,"
which the each-arm-vs-control check refutes. The corrected, narrower null: no *inferred* persistence,
and no difference in *d0-normalized shape*.

**Two riders.**
- This *strengthens* the §3 mechanism ("stated leaves a re-readable token; inferred was never
  tokenised"). Filler-wide verbatim persistence is the strongest evidence for it in the dataset —
  stronger than the reintro-only evidence §3 leaned on.
- It reframes the reintroduction. "Stated re-cues better than inferred" holds at essentially *every*
  checkpoint, filler included — the direct arm never left — so the reintro is **not** special for the
  stated > inferred contrast. It stays load-bearing only for the **inferred-vs-control reactivation**
  (§1c): the inferred arm climbs from below-floor mid-filler back above its own control at re-mention
  in 3/5. That signal is untouched.

**One open fork (not resolved here).** Direct persistence could be trait-specific *holding* or generic
*token-echo* — any word that appeared in the trigger might get the same downstream rank boost via
repetition/copy bias. Both are genuine stated-vs-inferred dissociations, but they mean different
things. Cheaply testable in the follow-up with a neutral content-word tracer carried in the trigger;
flagged, not settled.

---

## Files

- `prediction.md` — pre-registration + dated addenda (design, control rationale, novelty, post-run
  exploratory notes + follow-up design spec).
- `workspace_stickiness.ipynb` — the experiment (pinned environment).
- `stimuli.csv` — the 15 exact texts fed in, with checkpoint tokens (provenance).
- `stickiness_results.csv` — full raw sweep (4914 rows). `stickiness_top20.csv` — top-20 vocabulary
  per checkpoint (70200 rows). `stickiness_summary.csv` — median/best rank per cell.

The per-character decay-curve figure is not shipped; it is regenerable by running the notebook's
final plotting cell against `stickiness_summary.csv`.
