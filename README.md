# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

I built TakeMeter to classify comments from Reddit’s r/QuitVaping community by how people describe the effects of quitting vaping. 
It sorts comments into benefit_only, difficulty_only, and mixed_effects. The labels describe what the commenter reports; they do not 
verify whether a health claim is medically correct.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### Community and topic

Community: r/QuitVaping on Reddit.

Question: How do people describe the effects of quitting vaping?

### `label_one`

**Definition:**
- benefit_only: Describes an improvement attributed to quitting vaping, without describing a difficulty caused by quitting.

**Example 1:**
>oh absolutely i quit a couple months ago and noticed how much my stress has decreased. i ultimately quit because of the health scares it was causing when it would make my chest hurt.

**Example 2:**
>Since quitting 5 months ago my anxiety has improved so much it genuinely feels life changing, I had insomnia for months before quitting because of similar symptoms to you, gasping for air trying to get to sleep, I am soo much calmer now and have achieved things I didn't think possible because anxiety is that much less of a barrier these days

### `label_two`

**Definition:**
- difficulty_only: Describes a difficulty attributed to quitting vaping, without describing an improvement.

**Example 1:**
>I'm early in my journey (3 Days) but have already been struggling with sleep when usually my head's hits the pillow and I'm out. The last couple of nights I have simple been reading books to eventually get to the point of sleepiness.

**Example 2:**
>That’s very reassuring. It’s the main reason I relapse every time. I can handle everything else but the insomnia breaks me every single time. By week 2 I’m becoming a walking zombie. So miserable

### `label_three`

**Definition:**
- mixed_effects: Describes both an improvement and a difficulty attributed to quitting vaping, including an early difficulty followed by recovery.

**Example 1:**
>I was fine at first, then all of a sudden I couldn't sleep more than an hour at a time. But it felt like I'd been asleep for hours! So I started incorporating these micro naps and working my life around them... Only for it to end and now I can sleep right through the night and still want more. So I think I'm still cycling through varieties of sleep issues, hopefully it'll settle eventually! Currently about 2.5 months free."

**Example 2:**
>63 days here the insomnia passed at week5 and turned into 10 hours of sleep, day 63 back to insomnia

### Decision rules

1. If both a benefit and a difficulty are described, use mixed_effects.
2. If only a benefit is described, use benefit_only.
3. If only a difficulty is described, use difficulty_only.
4. Problems experienced while vaping do not count as difficulties caused by quitting.
5. Do not assume that weight change or another neutral change is positive or negative. 
6. Flag unclear comments for review rather than forcing a label.
7. Exclude comments that contain no described effect of quitting.
8. Classify the post description; do not judge whether a health claim is medically correct.

### The hardest boundary

**Which two labels:** benefit_only and mixed_effects

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->

If a post describes both an improvement and a difficulty attributed to quitting vaping, label it mixed_effects, even if the difficulty happened earlier and has since improved. Use benefit_only when it describes an improvement without a difficulty attributed to quitting. Problems experienced while vaping do not count as difficulties caused by quitting.
---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**

I collected 200 distinct comments from Reddit’s public r/QuitVaping community about the effects of quitting vaping.


**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->

I labeled the first 30 comments without any AI suggestions and marked the first 20 as cold. For the remaining comments,
I reviewed AI suggestions and accepted or changed them using my taxonomy. I recorded the assistance in the note column.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| benefit_only | 91 | 45.5% |
| difficulty_only | 67 | 33.5% |
| mixed_effects | 42 | 21.0% |
| **Total** | **200** | **100%** |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
>*The post:* “63 days here the insomnia passed at week5 and turned into 10 hours of sleep, day 63 back to insomnia”
>
> *Could have been:* difficulty_only or mixed_effects
>
> *I chose, because:* I chose mixed_effects because the comment describes both insomnia and a period of recovery. My rule
> counts earlier difficulties followed by improvement as mixed, even when the difficulty returns.

**2.**
> *The post:* “i quit with the patch as well. the first few days i had high anxiety.”
>
> *Could have been:* difficulty_only or excluded as coping advice
>
> *I chose, because:* I chose difficulty_only because the writer describes anxiety during the first few days of quitting.
> The rest of the comment offers advice about nicotine patches, but it still includes a reported difficulty and no clear benefit.

**3.**
> *The post (excerpt):* “Since quitting 5 months ago my anxiety has improved so much … I had insomnia for months before quitting”
>
> *Could have been:* benefit_only or mixed_effects
>
> *I chose, because:* I chose benefit_only because the anxiety improved after quitting, while the insomnia happened before quitting. 
> Mentioning a difficulty does not make a comment mixed unless that difficulty is attributed to quitting.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** distilbert-base-uncased

**Settings:** <!-- epochs, learning rate, batch size, seed -->
- Epochs: 3
- Learning rate: 0.00002
- Batch size: 16
- Maximum token length: 128
- Seed: 42
- Device: CPU
- PyTorch version: 2.14.1+cpu

**Anything I changed from the defaults, and why:**

I kept the default training settings. I changed LABELS to match my three categories and the CSV path to data/labels.csv.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->

With seed 42, I have 139 training comments, 31 validation comments, and 30 test comments.

The test set contains:
- benefit_only: 14
- difficulty_only: 10
- mixed_effects: 6

With only 6 mixed_effects comments in the test set, a few mistakes could change that label’s score substantially
between seeds.


### First-run results

The model correctly classified 14 of the 30 test comments.

| Measure | Result |
|---|---:|
| Test accuracy | 46.7% |
| Macro F1 | 0.212 |
| benefit_only F1 | 0.636 |
| difficulty_only F1 | 0.000 |
| mixed_effects F1 | 0.000 |

The model predicted benefit_only for every test comment. It completed training, but it has not learned to distinguish
the three labels. The first run missed my accuracy, per-label F1, and mixed_effects recall targets.

The evaluation metrics are saved in results.json, and the held-out comments are saved in test_split.csv.

---


## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Help setting up the notebook for my vaping dataset.
- *What came back:* Instructions to replace the practice labels with my three labels and point the notebook to data/labels.csv.
- *What I changed:* I updated LABELS to benefit_only, difficulty_only, and mixed_effects, and corrected the CSV path.

**Moment 2**

- *What I asked for:* Help checking my reviewed CSV before training.
- *What came back:* AI identified blank labels, invalid label names, empty rows, and the number of usable comments.
- *What I changed:* I corrected the labels, resolved the blank entries, removed empty rows, and added another real comment
  to reach 200 examples.

**Pre-labelling disclosure:**

AI helped me develop the taxonomy and provided suggested labels and explanations for the collected comments. I labeled the first
30 comments without viewing those suggestions and marked the first 20 as cold. For the remaining comments, I read the text,
reviewed the suggestions, and accepted or changed the labels using my written rules. I recorded AI assistance in the note column.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->

---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [x] criteria.md has five numbered criteria, each naming a NUMBER
       [x] Each has a reason underneath tied to your data or taxonomy
       [x] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [x] No label above 70%
       [x] All five unit 5 sections have real content
       [x] Label Taxonomy includes the decision rule for your hardest boundary
       [x] The Dataset includes three hard cases
       [x] results.json and test_split.csv committed (the notebook does this)
       [x] At least four new commits
       [x] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
