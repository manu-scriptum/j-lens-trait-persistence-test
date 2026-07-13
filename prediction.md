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
