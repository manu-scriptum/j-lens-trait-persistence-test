# Pre-registration: does a verbatim-stated trait decay differently than an inferred one?

Committed before any code is written. This is a genuinely new question, separate from the
negation-framing project (`jacobian-lens-experiment`) — it does not attempt to salvage or
extend that project's thesis.

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

## Addendum 2026-07-13 — expansion to a five-character series

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

## Addendum 2026-07-13 — control design: why a clean control is the hard part

The control arm went through three forms before this one; the reasoning is recorded here because
"what counts as a neutral control" is genuinely the hardest decision in the design.

The control's job is to give the baseline rank of the trait word when the trait was **never
introduced** — the floor against which a decay curve or a reintroduction bump is read. For that
floor to mean anything, the control should resemble the two test conditions in everything *except*
the trait.

That is where it gets hard: **there is no trait-neutral sentence in an absolute sense.** Any
concrete sentence about a person invites *some* inference — a bare biographical fact implies
stability or rootedness; a vivid action implies whatever disposition it looks like. So "a control
that implies no trait" is unreachable. The achievable bar is weaker and more precise: a control
that **does not preferentially imply the specific trait word we read**. A routine that makes Maria
look mildly orderly is fine, because "orderly" is not "generous" and will not inflate the rank of
*generous*.

Two earlier forms were rejected:
1. A bare copula — "[Name] is thirty-five." — matches the *direct* arm's frame but is
   conspicuously short and static against the inferred arm's vivid multi-clause behavior; it
   controls the wrong thing.
2. A longer biographical fact — "[Name] is thirty-five and has worked in the town for the past
   nine years." — fixes the length but is still a static *fact*, whereas the inferred arm is an
   episodic *behavior*. Fact and behavior differ in kind, so the baseline would not be matched to
   the arm it most needs to anchor.

Settled form: a **per-character, role-obligatory morning routine** — episodic like the inferred
arm, mundane, and chosen to avoid both the character's target trait and any lexical echo of the
character's inferred scene:

- Maria: "Every morning Maria unlocks the bakery and sorts the flour delivery by the back door."
- Peter: "Every morning Peter sweeps the sawdust from the workshop floor and lays out his tools."
- Nadia: "Every morning Nadia checks the clinic's supply cupboard and signs the register at the desk."
- Simon: "Every morning Simon raises the shutter and lines up the tickets for the day's repairs."
- Otto: "Every morning Otto unlocks the depot gates and marks the arriving crates against the delivery sheet."

Stated residual: these routines imply generic conscientiousness ("does their job"), which is not
trait-free — but it is orthogonal to the five target words (generous / patient / brave / curious /
greedy), which is the only neutrality the control actually requires. Otto's routine deliberately
involves checking crates against a sheet, **not** counting money, so it does not brush against his
greedy inferred scene. One sentence each; the 14-period structure is unchanged.

## Note on novelty and prior work

We are not claiming this question is unprecedented. The general shape — does an inferred property
persist differently from a stated one — has plausibly been gestured at in the entity-tracking and
binding literature (see the literature notes compiled for this project). What is distinctive here
is the *instrument*: reading trait persistence through the Jacobian lens, a workspace-style readout
that did not exist, or was not available outside a very limited setting, until recently. The
contribution we can honestly claim is the application and a clean, reproducible measurement — not
the invention of the question. This is deliberately modest: a shallow search does not prove
nonexistence, and if a direct precedent surfaces, this note gets updated rather than defended.

## Addendum 2026-07-13 (post-run): exploratory analysis and follow-up design spec

**Status: POST-HOC / EXPLORATORY.** Written after the run, after seeing the data. It does **not**
modify the pre-registered comparison. The confirmatory result (pre-registered rank of the single
trait adjective) is reported in `results.md`; this section records
exploratory observations and the design lessons they imply for a future run, kept here so the
reasoning that motivates the follow-up is dated and auditable.

Run environment: `gemma-3-4b-it`, `jlens@581d398`, T4; 15 texts × 13 checkpoints × 18-layer band
(layers 12–29). Row counts verified (`results` 4914, `top20` 70200) — completeness confirmed by
arithmetic.

### Confirmatory result, one line (full writeup in results.md)
The pre-registered target (single-token trait adjective, e.g. `generous`) ranked far better than its
no-trait control — a matched behaviour implying nothing — **only at distance 0**, then decayed to
≈baseline within one filler sentence, with only a modest deep-rank improvement at re-mention.
Peter/`patient` produced no d0 signal.

### Exploratory observations (not pre-registered)

- **E1 — The trait is carried by sibling lexicalizations (nouns/synonyms) that the pre-registered
  adjective undercounts.** Concept-lexicon scan (pre-declared sets, below) over the top-20: at d0 the
  concept sits at **rank 1 in 11–13 of 18 layers** for Maria (`donations, charity, kindness`), Nadia
  (`heroism, courage, bravery, risking`), Simon (`curiosity, fascination`). For Maria the
  pre-registered `generous`=768 badly understated an inference actually at rank 1 under `donations`.
  Nouns dominate.
- **E2 — Reading at the sentence-final period biases toward sentence-openers.** The top-20 at every
  trigger-end is dominated by discourse-continuation tokens (`But, However, Despite, Every`, pronouns)
  — the readout predicts the *next sentence's start*; trait tokens sit beneath that structural layer.
- **E3 — The collapse is concept-wide, not lexical.** Across d1–d10 the entire concept neighborhood
  leaves the top-20 for every character and condition, and the single-token exact rank independently
  decays to the tens-of-thousands. Both analyses agree: the trait is genuinely gone by the first
  filler sentence, not merely re-spelled.
- **E4 — The median did not hide a persistent sub-band signal.** Best-layer (min over 18) beats the
  control's best-layer at ≈50% of off-d0 checkpoints — chance. Collapse is not a median artifact. One
  structural note: d0 inference peaks mid-band (best layer ≈18–19 of 34, ~54% depth) for all four
  characters that inferred.
- **E5 — Re-mention reactivation is modest and deep-rank, not a return to salience.** At the entity
  re-mention the single-token rank improves (e.g. Maria inferred 47872→23261) but the concept does
  **not** re-enter the top-20 (only Otto flickers: `greed`, one layer). Real but small — "less
  buried," not "reactivated to prominence." (This tempers an over-warm reading made mid-analysis.)
- **E6 — The two readouts are complementary, each blind in one direction.** The concept-scan (top-20)
  sees Maria's strong-but-diffuse signal the single adjective missed, but is blind to Otto's weak
  `greed` (real at rank ≈1550, below the top-20 ceiling), which the exact-rank analysis caught.
  Neither alone suffices.

Pre-declared concept sets used in E1/E3 (declared before scanning; mildly contaminated in that a few
tokens — `donations, charity, curiosity, heroism, patience` — had been seen in the earlier top-20
spot-check): generous {generous, generosity, giving, gives, gave, charity, charitable, donations,
donation, donate, kind, kindness, selfless}; patient {patient, patience, calm, calmly, unhurried,
tolerant, waited, waiting}; brave {brave, bravery, courage, courageous, heroism, hero, fearless,
risking, risked}; curious {curious, curiosity, inquisitive, fascination, fascinated, wonder,
intrigued}; greedy {greedy, greed, stingy, miserly, selfish, hoard, hoarding, avarice, grasping}.

### Design spec for a future run (a fresh pre-registration, not a change to this one)

A post-hoc reconsideration (see `results.md` §3) reframes what a follow-up should do. The core error
to fix: **passive reads at sentence boundaries measure spontaneous saliency, not retrievability — you
cannot test a cache without querying it.** Two redesigns follow, then refinements.

**Redesign 1 — cued retrieval at each distance (the primary change).** Stop passively reading periods.
At each distance *k*, run a separate sequence `[opening + trigger + filler(1..k)]` followed by a
**probe** that licenses the trait — an open slot ("Maria is ___") or a two-alternative forced choice
("Is Maria generous or stingy? Maria is ___") — and read the trait concept-set's rank *at the probe*.
Every checkpoint is then a cache-read at a content-appropriate position. This separates the three
states the current design smears together: spontaneously active (blackboard), dormant-but-retrievable
(cache), and truly gone (eviction) — and makes the stated-vs-inferred decay question actually
answerable. Include a no-trait control probe to baseline the slot.

The KV-ablation causal test rides on top of this, and a **structural asymmetry between the arms makes
it sharp** (this is the key design update — it matters substantially, not cosmetically). The stated
trait leaves a **literal trait token** in the sequence — a cached position the model can attend back to
and re-read. The inferred trait is **never tokenized**; it existed only as a transient latent, so the
cache holds only the *behaviour* tokens, and re-cueing the trait means **re-inferring it from the
scene**, not re-reading a symbol. The two arms therefore demand different ablations:
- **Inferred arm:** ablate the *behaviour* tokens' keys/values. **No** cued retrieval survives → the
  trait was pure re-inference (it lived only in those tokens, nothing was stored). **Some** survives →
  a genuine latent trace persisted at the position where the trait was assembled, independent of the
  scene — a real dormant binding.
- **Stated arm:** ablate just the trait token's keys/values, to isolate how much of retrieval is the
  re-readable symbol versus the surrounding context.

Reading the two arms together separates *re-read a symbol* from *re-derive from a scene* — which is the
actual mechanism behind the stated > inferred retrievability lead, and the deep version of the
cache-vs-blackboard question, made answerable per arm. (Credit: this asymmetry was missed in the
original framing — the inferred trait's never-being-a-token was the thing hiding in plain sight.)

**Feasibility / scope — the ablation is out of the read-only lab.** The ablation is an *intervention*,
not a readout: it needs attention/KV-patching scaffolding (forward hooks or a library like `nnsight`)
**and its own validation** — proving the ablation removed the intended trace and did not just break the
model. That is beyond the read-only lens rig here; park the causal ablation as a proper-lab /
agentic-run step, not the immediate next run. **What is in reach is its observational shadow —
differential fragility.** The symbol-vs-re-inference distinction predicts a symbol lookup is *robust*
(a fixed, sharp cached token) while re-inference from a *buried or contested* scene is *fragile*. So
stress both arms with reads only — cued retrieval at growing distance, plus the competing-entity
interference of Redesign 2 — and see which retrieval mode breaks first. If `inferred` craters faster
than `stated` as distance and interference pile on, that is the observational fingerprint of the same
mechanism. Correlational, not causal — it *motivates* the ablation rather than replacing it — but fully
runnable in the little lab. **Primary next run = cued retrieval + interference + this fragility read;
the KV-ablation is the eventual causal confirmation, elsewhere.**

**Redesign 2 — interference / binding fidelity with a competing entity (the more novel question).**
Persistence-over-time may be the wrong axis; the place stated and inferred plausibly dissociate is
**binding fidelity under load.** Add a second character with a conflicting trait ("Maria is generous…
Bruno is stingy…"), then probe at distance ("Is Maria generous or stingy?"). Pre-registered question,
no direction: does the *inferred* trait mis-bind (Maria inherits Bruno's trait, or vice versa) more
than the *stated* trait, and does mis-binding rise with distance / number of intervening entities?
This connects to the binding literature (Mixing Mechanisms) rather than re-confirming the
entity-tracking null, and the single-entity design here was structurally blind to it.

**Redesign 3 — the trait-direction probe (the representation test, and the primary held-concept arm).**
Every lens above — logit, tuned, J — reads the same basis: *disposition to say the token*. A concept
can be **represented as a feature while never being disposed to be said**, and no vocabulary-projection
readout can see that. So to ask whether a trait is *held between the tokens*, read it as a **direction**,
not a word. Fit a linear "generous" direction from contrastive activations (persona-vector style:
trait-implying vs matched neutral contexts), validate it on held-out text, then read the residual
stream's **projection onto that direction, continuously across the whole filler**, not just at
checkpoints. The decisive contrast:
- Direction stays **lit** across the filler while the token rank sits on the floor → the trait is
  **held but unexpressed** — a held concept the vocabulary readouts were structurally blind to. This is
  the strongest positive result available for held concepts, and only this probe can produce it.
- Direction goes **dark** too → only the behavioural scene is retained; the trait genuinely isn't held,
  and the reintroduction is re-inference.

This adjudicates *held latent vs re-inference* observationally and continuously — for the held-concept
question specifically it is cleaner than the KV-ablation, and being a readout (a linear probe, no
intervention) it is in reach of the little lab. Cost: fitting the probe is a supervised step (labelled
contrastive activations) with its own validity checks — does the direction capture the *entity-bound
trait* rather than mere topic, and does it generalise off the fitting set. Treat a positive result as
"represented as a linear feature," not automatically as a "global-workspace held concept" — still the
separate, harder claim.

**The three arms answer three different questions, and keeping them distinct is the point.**
*Expression* (vocabulary readout — is it disposed to be said), *retrievability* (cued retrieval — is it
recoverable on demand), *representation* (direction probe — is it held as a feature). This study only
ever measured the first; the follow-up adds the other two, and most of the confusion in the original
writeup came from reading an *expression* measurement as if it answered *representation*.

**Refinements (carried over from the exploratory notes):**
1. Pre-register a concept *set* (synonyms + noun form) per trait; primary metric = best exact-rank over
   the set — E1/E6. Prefer noun lexicalizations (`patience, curiosity, heroism`).
2. Save exact rank at any depth (not just top-20) for the whole set — E3/E6.
3. **Screen every inferred trigger for a strong d0 concept signal before running** — Peter (null) and
   Otto (weak) would have been caught. You cannot measure the decay of a signal that was never there.
4. **Disentangle topic from attribution** (the Maria/shelter problem): the inferred behaviour must not
   saturate the readout with the trait's *topic*. Either match the control's topic to the inferred
   trigger's, or read only where the topic has receded and the entity is the sole cue. *And neutralise
   priming one level up:* the character's **role** (fixed in the shared opening), the **control
   behaviour's own vocabulary**, and even the **setting** can sit near the trait's neighbourhood (nurse
   ~ brave, tallying ~ greedy — and the shared opening named a real city, **Lyon**, carrying gastronomy
   and a Resistance/bravery history). Design openings, controls, and setting to be lexically/semantically
   distant from the trait — including **dropping real place-names for a neutral or invented setting** —
   while keeping the control a plausible structural match. (Base frequency dominates the baseline, so
   this is a second-order cleanup, not a rescue.)
5. Stronger, more stereotyped negative-trait behaviours; more items before any quantitative claim.

**Behavioural cross-check (optional, strongest ground truth).** Independently of the lens, end with a
trait-diagnostic completion and test whether stated vs inferred changes the model's *output* — this
sidesteps the readout-position problem entirely.

**Standing lens baseline (include from now on — it is free).** Alongside the J-lens, read the same
checkpoints with the **logit lens**: the model's own unembedding (`lm_head`) applied to the
intermediate residual streams. No download, no training — it runs on activations already being captured
— so there is no reason to omit it, ever. It buys two things for nothing: (1) **robustness** — if the
crude logit lens shows the same pattern, the finding is not a J-lens artifact; and (2) a **discriminant
test of the J-lens itself**, via the `{direct, inferred} × {logit, J-lens}` contrast — if the logit lens
surfaces the *direct* (expressed) trait but not the *inferred* (latent-only) one while the J-lens
surfaces both, that is the J-lens doing its claimed job of reading unexpressed latent content; if all
lenses agree, the J-lens buys nothing on this task. Caveat: all three are vocabulary-projection
transports, so this is a readout-robustness / discriminant check, **not** a representation probe — it
cannot see a held-but-unexpressed concept (that is the direction-probe's job). A **tuned lens** would
sharpen the discriminant test but needs one trained for `gemma-3-4b-it` specifically (only a public 27B
lens was found, wrong size); add it only if one can be sourced or trained.

### Honesty caveats
n=5, single-item medians, no statistics — every "count" is descriptive, not a test. Top-20 is a hard
ceiling that censors weak/decayed signal. The concept lexicon is exploratory and pre-declared but
mildly contaminated. None of this modifies the confirmatory comparison.
