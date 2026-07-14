# What is this? — in plain language

*You don't need any background in AI, statistics, or interpretability to read this. If you finish and
want the rigorous version — every number, every caveat — [`results.md`](results.md) and
[`prediction.md`](prediction.md) are waiting.*

## The question, in one sentence

If a story **says** "Maria is generous," versus **shows** Maria quietly giving away the bread she
didn't sell, does a language model hold onto *generous* differently as the story moves on — and does
simply mentioning her name again, later, bring it back?

## Why bother asking

Here's the big thing hiding behind the small experiment. When one of these models reads a story, does
it keep something like a **mental note** — *Maria: generous* — alive in the back of its mind while it
reads on about the weather and the train station and the stray cat? Or does it hold nothing at all,
and just work the trait out from scratch each time the story happens to need it?

Whether there's a kind of internal *workspace* where a model holds a thought between the words is a
question the field genuinely hasn't settled. It's why people get excited — and occasionally
overexcited — about peering inside these things.

**This study does not answer that question.** It's a small, careful probe at one corner of it. But
it's a real corner, and by the end you'll see exactly why the honest answer is still "we can't tell
yet — but here's how we'd find out."

## What we actually did

Five invented characters, each with a trait: Maria (generous), Peter (patient), Nadia (brave), Simon
(curious), Otto (greedy). For each, we wrote the story three ways:

- **Say it:** "Maria is generous." The word is right there on the page.
- **Show it:** "Every night before closing, Maria brings the unsold bread to a shelter." The word
  *generous* never appears — you have to read between the lines.
- **Nothing:** a matched, humdrum routine that plants no trait at all. This is the baseline — how the
  trait word behaves when we simply never brought it up.

Then, for all three, the same ten sentences of deliberately dull filler (streets, trees, trains), and
finally the character's name once more with nothing revealing attached: "It is Tuesday. Maria ties
her shoelaces." At thirteen points along the way, we take a reading.

## How you "take a reading" of a model's mind — sort of

We use a tool (the **Jacobian lens**) that peeks at a hidden internal layer and asks, roughly: *right
now, how ready is the model to say the word "generous"?* Not "is it thinking about generosity" — we
can't see that directly — but the more modest "if it had to speak this instant, how near the front of
the line is that word?"

That "front of the line" is worth making concrete. The model carries a vocabulary of about
**262,000 words**, and at every moment they're queued by how likely it is to say each one next. We
report a word's **rank** — its place in that queue. Rank 3 means only two words are ahead of it: the
model is all but ready to say it. Rank 40,000 means it's buried and effectively idle. **Lower is
stronger.** That one number — where *generous* sits in the queue — is the whole measurement.

## What we found

- **"Show it" works — the model reads between the lines.** Right after the bread-and-shelter
  sentence, *generous* jumps toward the front of the queue for **4 of the 5** characters. The model
  inferred the trait from the behaviour, no word required. (This part is genuinely a little striking.)
- **…but it fades almost at once.** One sentence of filler later, the inferred trait is gone from the
  surface — back to idling where it sits in any story that never mentioned generosity.
- **"Say it" lingers far longer.** When the word was literally written down, it stays near the front
  through much of the filler — for most of the characters, the whole way. Not necessarily because the
  model is "holding the idea," but because the word is still sitting there in the text to glance back
  at. A *said* trait is a **sticky note you can re-read**; an *inferred* trait is a **conclusion you'd
  have to rebuild**.
- **Saying the name again tugs the inferred trait partway back.** Late in the story, "Maria" alone —
  no bread, no shelter, nothing but the name — pulls *generous* back up above baseline for 3 of 5. A
  hint that the trait stayed quietly attached to the *person*, not just to the words that introduced
  her.
- **Peter didn't work, and we're telling you.** Our "shows patience by fishing all morning" clue was
  too subtle; the model never inferred *patient* at all. An honest miss, left in the record rather
  than swept out of it.

## What it might mean (and where honesty kicks in)

The tidy picture: **say it and it's a re-readable note; show it and it's a conclusion that evaporates
and has to be rebuilt.** That fits everything above.

But here's the cliffhanger — the whole reason there's a next round. When re-mentioning Maria's name
tugs *generous* back, is that because the model *held* "generous" quietly in mind the entire time (a
real mental note), or because it *re-inferred* it on the spot from the bread-and-shelter scene it
still had in view? **From where we're standing, those two look identical.** We *watched* the model; we
never *asked* it. And you can't find out what someone's holding in mind by watching them read — you
have to put a question and see what comes out. That gap is the finding that matters most.

## The honest size of this

Five characters. One small open model. No statistics — every "4 of 5" describes what we saw, not a
proof that it holds in general. This is a **pilot**: a "huh, that's interesting, and here's exactly
what to do next," not a headline.

(We even changed our own headline once. An early version claimed *say it* and *show it* fade equally
fast. A closer look showed the *said* trait plainly doesn't — it persists across the filler. We
corrected it in the open; that story is [`results.md`](results.md) §5. Getting it wrong and fixing it
visibly is the job, not an embarrassment.)

## What's next

Three follow-ups, each aimed at a specific blind spot above:

1. **Stop watching, start asking.** At each step, actually put the question — "Is Maria generous?" —
   and read what comes back, instead of passively peeking. You can't measure what's remembered without
   a prompt.
2. **Add a rival.** Drop a second character with the opposite trait into the same story and see if the
   model **mixes them up** — does the *inferred* trait smear onto the wrong person more easily than the
   *said* one?
3. **Look for the trait as a flavour, not a word.** Instead of "how ready is it to say *generous*,"
   measure whether a "generosity direction" is lit up in the model's internal state even when no such
   word is anywhere near the surface. That's the most direct way to ask the held-in-mind question
   head-on.

## Where this sits in the wider work

A few papers, if you'd like to see the neighbourhood (all freely readable):

- **The tool itself** — [the Jacobian-lens write-up](https://www.neuronpedia.org/blog/jacobian-lens)
  introduces the "peek inside" method, and cheerfully calls the internal space a "global workspace."
  Whether that label is *earned* is exactly what we try not to lean on.
- **The honest counterweight** — Chalmers,
  [*Could a Large Language Model Be Conscious?*](https://arxiv.org/abs/2303.07103), lists a genuine
  workspace among the things today's models plausibly *lack*. Worth reading right after you take the
  bait two sections up.
- **Why we re-mention the name** —
  [*Do Language Models Track Entities Across State Changes?*](https://arxiv.org/abs/2605.30233) found
  models don't quietly track things sentence by sentence so much as pull everything together the moment
  a question makes it relevant — which is why we test at the re-mention, not only along the way.
- **How a trait gets fetched back later** —
  [*Mixing Mechanisms*](https://arxiv.org/abs/2510.06182) maps the different routes a model uses to
  retrieve a fact about a character across intervening text. This is the groundwork for follow-up #2.
- **Trait as a direction** —
  [*Persona Vectors*](https://arxiv.org/abs/2507.21509) shows traits can be read as a "direction" in a
  model's internal state. That's the method behind follow-up #3.

## Want the rigorous version?

[`prediction.md`](prediction.md) is what we committed to *before* running anything — so the target
couldn't be moved to fit the result. [`results.md`](results.md) is what actually came out, caveats and
all. Start with whichever your patience prefers.
