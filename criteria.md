# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. Overall accuracy

<!-- Your criterion. It must name a number. -->

The classifier achieves at least 70% accuracy on the held-out test set in each of the three Unit 6 runs.
The model correctly labels at least 70% of held-out comments in each of the three Unit 6 runs.

**Why this target:**

Some comments describe both the difficult early days of quitting and improvements later on. Those will be harder to classify.
I chose 70% because I want the model to get most comments right, including some of these less straightforward examples.

---

## 2. Performance on each label

<!-- Your criterion. -->

Each label has an F1 score of at least 0.60 on the held-out test set in each of the three Unit 6 runs.


**Why this target:**

I want the model to recognize all three labels. Mixed-effects comments may be harder because they describe both benefits and difficulties. I chose 0.60 as the minimum acceptable score for each label so that none gets overlooked.

---

## 3. Label balance

Each label makes up at least 20% and no more than 60% of the final labeled dataset.

<!-- Your criterion. -->

**Why this target:**

Comments describing only benefits may be easier to find than comments describing mixed effects. I want enough examples of each label for the model to learn all three. With 200 comments, this means at least 40 examples per label.

---

## 4. Labeling consistency

<!-- Your criterion. -->

When I relabel 20 randomly selected comments without looking at my original labels or AI suggestions, at least 17 receive the same label.

**Why this target:**

A comment about early withdrawal followed by improvement could be mistaken for benefit_only instead of mixed_effects. Matching at least 17 out of 20 would give me some evidence that I can apply my decision rules consistently.

---

## 5. Recognizing mixed effects

<!-- Your criterion. -->

The model identifies at least 60% of the comments labeled mixed_effects in the held-out test set in each of the three Unit 6 runs.

**Why this target:**

A comment may describe a difficult first week and improvements later. The model needs to recognize both parts instead of focusing only on the improvement. I want it to catch at least six out of ten mixed-effects comments.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
