# Pre-registration: does a verbatim-stated trait decay differently than an inferred one?

Committed before any code is written. This is a genuinely new question, separate from the
negation-framing project (`jacobian-lens-experiment`) — it does not attempt to salvage or
extend that project's thesis. Not pushed to a remote yet; local-only for now.

## Background and motivation

The negation-framing project found that `honest` ranked strongly under a prompt that never
uses the word, only describes truth-telling behavior — inference, not echo. That raised a
further, more general question with no clear answer in the literature we could find (one
paper's full text checked directly, plus a broader search — see chat log; not an exhaustive
review): **does a concept that must be inferred persist differently in the J-space than a
concept that is simply stated outright, as more text intervenes?**

One relevant, verified finding from adjacent work (entity tracking in language models,
arXiv:2605.30233): *"LMs do not incrementally track world states across tokens or
query-relevant states across layers, but simply aggregate relevant information in parallel
at the last token when the query becomes evident."* This motivates reading out at points
where the entity is re-mentioned, not only via smooth distance decay.

## Model, lens, layer band

Identical to `jacobian-lens-experiment`: `google/gemma-3-4b-it`, lens file
`gemma-3-4b-it/jlens/Salesforce-wikitext/gemma-3-4b-it_jacobian_lens.pt` from
`neuronpedia/jacobian-lens` (no revision needed), 35-90% depth band. No spider check or
walkthrough reproduction repeated here — that pipeline validation already happened twice
(the negation-framing project, and the Room exploratory notebook) on this exact model/lens
pairing. Text fed as raw text only; no chat template involved (this question is about
document-level structure, not system-prompt framing).

## Target concept

`generous` — a trait deliberately unrelated to honesty/lying, to keep this cleanly separate
from the prior project's thesis and its still-open valence confound. Both bare (`generous`)
and leading-space (` generous`) forms will be checked for single-token status in Colab
before the sweep runs, same as before; dropped if not single-token, not substituted.

## The two conditions (verbatim, fixed in advance)

Both share an identical opening sentence, then diverge:

**Direct/verbatim:**
> Maria runs the corner bakery in a small town outside Lyon. Maria is generous.

**Inferred:**
> Maria runs the corner bakery in a small town outside Lyon. Every evening, Maria packs up the bread that didn't sell and drives it to the shelter across town before deciding what to keep for herself.

Both are then followed by an **identical** ten-sentence neutral filler block (fixed in
advance, deliberately unrelated to character or generosity), then an **identical**
reintroduction sentence:

1. The town has one main street lined with plane trees that lose their leaves in early October.
2. A weekly market sets up in the square every Wednesday morning before the shops open.
3. The nearest train station is a fifteen-minute walk past the old stone bridge.
4. Local weather this time of year tends to bring short rain showers in the late afternoon.
5. The bakery's ovens are relined with fresh brick every few years to keep the heat even.
6. A small hardware store sits two doors down, run by a family that has owned it for decades.
7. Traffic on the main road slows considerably during the summer tourist season.
8. The town council meets on the first Tuesday of each month in the old schoolhouse.
9. A stray cat has taken to sleeping in the doorway of the shuttered cinema.
10. The river that runs along the edge of town floods rarely, but the bridge was built high just in case.

**Reintroduction sentence (identical for both conditions):** "It is Tuesday. Maria ties her shoelaces." (finalized — see addendum 2026-07-13 below; an earlier draft read "Maria answers the phone.")

This sentence is not claimed to be topically neutral in an absolute sense — no sentence
is. It is chosen only to avoid directly naming or describing the trait itself (unlike an
earlier draft, "Maria takes a walk," dropped for its health/exercise association, which sits
closer to trait-adjacent territory than "ties her shoelaces" does). If some other,
unrelated concept co-occurs at this position in either condition, that is not treated as
contamination of the test — `generous` surfacing (or not) at this point is the question of
interest regardless of what else is present.

## Reading plan

For each condition, one continuous cumulative raw text (opening + trigger + filler +
reintroduction). Positions read, both conditions, same 35-90% band:

- **Distance 0:** final token of the trigger sentence.
- **Distance 1-10:** final token of each successive filler sentence, in order.
- **Reintroduction — entity mention:** the token for "Maria" in the reintroduction sentence.
- **Reintroduction — sentence end:** final token of "...answers the phone."

At each position: rank and log-probability of every surviving single-token form of
`generous`, within the band. Distance is measured in *sentences from the trigger*, not
absolute token position — the two triggers are different lengths, and narrative distance,
not raw position, is what "decay" should mean here. Also recorded at each position (raw
CSV, not part of the pre-registered comparisons): the top-20 J-lens vocabulary, exploratory
only, no target list, same spirit as the Room notebook.

## Prediction: none

No directional prediction is made, for either comparison below. This is not a two-sided
hedge across a preferred outcome — there is no prior belief about which way either
comparison will go. What is fixed in advance is the analysis plan and the stimuli, not an
expected result. Every outcome listed below is of equal interest; none is a "win" or a
"loss" for anything.

**Decay curve:** one of three outcomes, all reportable —
1. The inferred condition's rank decays slower (stays lower/stronger for longer) than the
   direct condition's, across distances 0-10.
2. The direct condition's rank decays slower than the inferred condition's.
3. No reliable difference in decay rate between conditions (null result; consistent with
   the "aggregate at query time" finding above, which would predict decay is dominated by
   general position/recency rather than by how the concept was introduced).

**Reintroduction bump:** one of four outcomes, all reportable —
1. `generous`'s rank improves (gets stronger) at the reintroduction point, roughly equally,
   in both conditions.
2. The bump is larger for the inferred condition (consistent with the trait being bound to
   the entity as a derived property, reactivated by any mention of her, independent of
   surface wording).
3. The bump is larger for the direct condition (consistent with the literal word being
   what's re-associated with her name).
4. No bump in either condition (a bare entity mention, with no implicit query for the
   trait, does not reactivate it).

## Limitations, stated in advance

- One target concept, one character, one town — a first look, not a full experiment, same
  caveat as the negation-framing project's original prediction.
- The reintroduction sentence is not topically neutral in an absolute sense (see above);
  only that it avoids directly naming or describing the trait.
- The "aggregate at query time" finding is borrowed from a different paper on a different
  question (entity state tracking under explicit operations); it has not been verified
  against this lens/model pairing specifically, and is cited here as motivating context for
  the reading plan, not as an established property of this exact setup.
- Small open model (Gemma-3-4B-IT), one document pair — a mechanistic look, not a claim
  about language models generally.
- This is new inference, not a re-analysis of existing data — unlike the recency-controlled
  re-test in the other project, this requires an actual Colab run.

## Addendum 2026-07-13 (before any run): reintroduction sentence finalized

The reintroduction sentence is changed from "It is Tuesday. Maria answers the phone." to
**"It is Tuesday. Maria ties her shoelaces."** No data has been run at the time of this
change — this is still pre-registration, edited before any Colab execution; git history
preserves the original wording. Rationale: "answers the phone" carries a faint but avoidable
social/communicative valence — a phone call implies another person, a request, a possible
interaction — whereas "ties her shoelaces" is a self-contained physical action with less
unavoidable connotation, while equally avoiding any naming or description of generosity.
This applies the same selection criterion that dropped the earlier "Maria takes a walk"
draft (health/exercise association): prefer the most inert available action when it is
equally easy to write. The structure is unchanged — still two sentences, still 14 total
periods — so the checkpoint logic is unaffected.

Also logged (implementation, not stimulus): the entity-mention checkpoint in the notebook
was hardened to locate the last "Maria" by character offset rather than by exact
token-string equality, so a subword tokenization of the name (e.g. "Mar" + "ia") cannot
silently produce an empty match and crash the checkpoint lookup.

## Addendum 2026-07-13 (v2): expansion to a five-character series

Before any run; supersedes the single-character (Maria-only) design above. Motivation: one
item cannot separate a real verbatim-vs-inferred difference from character-specific noise,
and one positive trait cannot separate "inference persists" from "positive valence persists."
Five characters give five replications of the same contrast; including one negative-valence
trait gives a first, deliberately unbalanced look at whether any persistence pattern tracks
inference or merely valence (connecting to the still-open valence confound in
`jacobian-lens-experiment`). This is explicitly not a balanced factorial (four non-negative
traits, one negative) — a first look, not a valence experiment.

### The five characters (verbatim, fixed in advance)

Each: a one-sentence opening ending "… in a small town outside Lyon", then one of three
triggers, then the shared filler and reintroduction below.

| Name | Trait | Valence | Role (opening) | Direct | Inferred (trait never named) |
|---|---|---|---|---|---|
| Maria | generous | + | corner bakery | Maria is generous. | Every night before closing up, Maria brings part of the unsold bread to a shelter. |
| Peter | patient | + | carpentry workshop | Peter is patient. | Peter can fish the lake all morning without a single bite and still call it time well spent. |
| Nadia | brave | + | health clinic | Nadia is brave. | When fire broke out in the stairwell, Nadia went back inside twice to carry out the people who could not move. |
| Simon | curious | + | repair counter | Simon is curious. | Simon takes the back off every broken radio he finds, just to see how the parts fit together. |
| Otto | greedy | − | packing depot | Otto is greedy. | Otto counts his money twice a night and never once picks up a bill. |

### Three conditions per character

- **Direct** — "[Name] is [trait]." (verbatim statement)
- **Inferred** — the behavior sentence above; the trait word never appears.
- **Control** — "[Name] is thirty-five and has worked in the town for the past nine years."
  A trait-neutral baseline for the trait word's rank when the trait was never introduced.
  One sentence, longer than a bare copula so it is not conspicuously short against the
  inferred arm; it is a static biographical fact rather than an episodic behavior — a
  residual mismatch with the inferred arm's form, accepted as good enough since the control
  mainly supports the trait-word floor at the (identical) filler and reintroduction
  checkpoints.

5 characters × 3 conditions = 15 texts, one Colab run.

### Shared filler (replaces the ten-sentence block in the body above)

Mundane town geography/infrastructure/weather — no entity tied to any character, no commerce,
no charity/need, no valence, checked against all five inferred scenes for lexical echoes:

1. The town's main street runs in a straight line from the north gate to the central square.
2. A row of plane trees stands along the pavement, tall enough to shade the whole street by midsummer.
3. The central square is paved with grey cobblestones laid in a fan pattern.
4. A stone fountain in the middle of the square has three tiers and runs from spring until autumn.
5. The nearest railway station sits at the far end of a long avenue past the clock tower.
6. Trains to the regional capital leave twice in the morning and once in the early afternoon.
7. The weather in this region turns from dry summers to grey, rain-heavy winters.
8. A ridge of low hills marks the eastern edge of the district, covered in scrub and loose rock.
9. The oldest building still standing is a squat administrative hall with a tiled roof.
10. A single road bridge crosses the river at the southern edge, wide enough for one lane of traffic.

One noted residual: Peter's inferred sentence has him fishing "the lake" while filler #10
mentions "the river" — different words, both water features. Judged negligible (a filler
mention of a bridge over a river would at most re-cue the fishing *situation*, not the word
"patient"); recorded here rather than silently ignored.

### Shared reintroduction (generalizes the shoelaces sentence)

"It is Tuesday. [Name] ties his/her shoelaces." — trait-neutral, self-contained; "Tuesday"
no longer collides with the filler (the council sentence is gone).

### Reading plan and analysis (updated)

Checkpoints, band, and per-position readout unchanged, applied per character per condition:
distance 0 (trigger end), distances 1–10 (filler-sentence ends), reintro entity-mention (last
occurrence of that character's name, by character offset), reintro sentence end. Period count
is 14 for every text; the assert is retained.

- **Primary statistic: median rank across the band** (best rank kept as secondary).
- **Decay is compared as shape, each condition normalized to its own distance-0 — not as
  absolute rank level.** The direct arm's early points partly reflect surface echo of the
  just-stated trait word (unavoidable, and for the direct arm that echo *is* the verbatim
  persistence we want to measure), so absolute heights of the direct and inferred curves are
  not treated as comparable; only decay shapes and the within-condition reintroduction change
  are.
- **Reintroduction bump** is read within-condition (distance-10 → reintro), unaffected by
  differing starting heights.
- Exploratory top-20 vocabulary at every checkpoint (not pre-registered) doubles as the check
  for wrong-target cases (e.g. a scene surfacing "kind" rather than "generous").

### Prediction: still none

No directional prediction, per character or in aggregate, for either the decay comparison or
the reintroduction bump. The negative item (Otto/greedy) is included to see whether any
pattern tracks inference or valence; no direction is predicted for that either. Fixed in
advance: the stimulus set, the conditions, the checkpoints, the analysis plan — not any
expected result.

### Limitations (in addition to the single-item version's)

- Five characters, one trait each; four non-negative and one negative — not a balanced
  valence design. A first look, not a factorial.
- Trait words differ in frequency/tokenization; cross-character absolute ranks are not
  directly comparable — another reason the analysis is within-character and shape-based.
- Any trait word that fails the single-token check is swapped and re-registered before the
  readout, never silently substituted.
