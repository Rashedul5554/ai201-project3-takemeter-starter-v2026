# TakeMeter

**Status:** Work in progress. The prepared dataset contains 24 unlabeled entries. Independent labeling, further collection, training, and evaluation are pending.

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

TakeMeter is a planned text classifier for comments from Reddit’s r/technology community. It will sort comments into `analysis`, `hot_take`, and `reaction` based on the way they communicate. The goal is to distinguish supported explanations, strong judgments without substantial support, and immediate emotional responses. These labels describe communication style; they do not establish whether a claim is true.



---

## Label Taxonomy

Community: **r/technology**. These six genuine comments come from the discussion [AI can cost more than human workers now](https://www.reddit.com/r/technology/comments/1swsnn6/ai_can_cost_more_than_human_workers_now/). These taxonomy examples and source links were retained from the existing README; they were not reverified during this update. Labels describe how the comments communicate; they do not certify that a commenter’s claims are true.

### `analysis`

**Definition:** A comment that explains a technology-related issue using specific facts, examples, statistics, or clear supporting reasoning.

**Example 1:**

> This is what is going to burst the bubble eventually.
> The technology needs to replace whole teams/workforces to justify the actual cost. And I just don’t think it’s ever going to get there - at least not in its current form.

[Source comment by u/redish6](https://www.reddit.com/r/technology/comments/1swsnn6/comment/oiiiknv/)

**Why this label:** The comment supports its prediction with reasoning about the workforce replacement needed to justify costs.

**Example 2:**

> I assume you mean "wasn't" charging the real cost not "isn't". Netflix has been consistently profitable since 2014.
>
> The other companies are profitable as well, though with Uber there is obviously more to it (the cost to society, etc.).

[Source comment by u/ireadoldpost](https://www.reddit.com/r/technology/comments/1swsnn6/comment/oiihqsn/)

**Why this label:** The comment supports a correction with a specific, checkable claim about profitability since 2014.

### `hot_take`

**Definition:** A comment that expresses a strong opinion or judgment about a technology-related issue without providing substantial supporting evidence.

**Example 1:**

> And it's still not being charged at the actual cost.

[Source comment by u/thekk_](https://www.reddit.com/r/technology/comments/1swsnn6/comment/oihxpyl/)

**Why this label:** The comment makes a confident claim about pricing without offering evidence or an explanation.

**Example 2:**

> And it's still not doing the work of the people it's supposed to be replacing.

[Source comment by u/GenericFatGuy](https://www.reddit.com/r/technology/comments/1swsnn6/comment/oii167k/)

**Why this label:** The comment makes a confident judgment about performance without offering supporting examples or reasoning.

### `reaction`

**Definition:** A comment that primarily expresses an immediate feeling, such as surprise, excitement, frustration, or concern, about technology-related news or events.

**Example 1:**

> Now comes the part where we throw our heads back and laugh.

[Source comment by u/sgtsausagepants](https://www.reddit.com/r/technology/comments/1swsnn6/comment/oii0eo5/)

**Why this label:** The main purpose is to express amusement in response to the news.

**Example 2:**

> surprise surprise surprise

[Source comment by u/Sorry-Climate-7982](https://www.reddit.com/r/technology/comments/1swsnn6/comment/oii0o1z/)

**Why this label:** The comment expresses sarcastic surprise in response to the news without developing a substantive claim.

### The hardest boundary

**Which two labels:** `hot_take` and `reaction`.

**The decision rule to apply consistently:** First, choose `analysis` when a comment explains an issue using specific supporting facts, examples, statistics, or clear reasoning—even when its tone is emotional. A number or fact merely mentioned in passing does not qualify unless it supports the explanation. Otherwise, choose `hot_take` when the main purpose is to assert an unsupported claim or judgment; choose `reaction` when the main purpose is to express an immediate feeling about the news or event. For a comment containing both, identify its central message: a substantive claim about the issue takes `hot_take`, while a brief emotional or sarcastic response without a developed claim takes `reaction`. Do not classify by tone or length alone.

If a comment fits none of these definitions, set it aside for review rather than forcing a label. Use the same rule when selecting examples and later labeling the dataset.


---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** I pasted 25 entries from seven r/technology discussions into my project conversation. The prepared `labels.csv` contains 24 entries after excluding one apparent Ozempic advertisement. The discussions concern:

- [Bank of America hiring contractors](https://www.reddit.com/r/technology/comments/1x6700d/bank_of_america_hired_1000_contractors_to_patch/)
- [ICE domestic surveillance](https://www.reddit.com/r/technology/comments/1x5thwn/ice_planning_100_million_nightmare_domestic_spy/)
- [BlackRock and data-center investment](https://www.reddit.com/r/technology/comments/1x5v797/blackrocks_larry_fink_warns_a_data_center/)
- [El-Sayed’s AI platform](https://www.reddit.com/r/technology/comments/1x5wce2/elsayed_releasing_his_platform_on_ai_calls_it_an/)
- [SpaceX limiting Starlink service](https://www.reddit.com/r/technology/comments/1x60zu3/spacex_limits_starlink_service_for_ukraines/)
- [Ukrainian soldiers’ access to Starlink](https://www.reddit.com/r/technology/comments/1x5nj77/ukrainian_soldiers_in_combat_have_access_to/)
- [Peter Thiel and AI critics](https://www.reddit.com/r/technology/comments/1x5lcon/peter_thiel_escalates_war_on_ai_critics_accuses/)

The CSV preserves the comment bodies as pasted, including wording, punctuation, links, Markdown, and internal line breaks. Post headings, comment-number markers, and separator whitespace were removed. These texts were recovered from the conversation rather than independently verified on Reddit. Two apparent article excerpts remain included for suitability review: data row 2 (ICE relationships) and data row 16 (Ukrainian soldiers/Starlink). The ICE excerpt starts with “o identify” exactly as pasted. No missing text was reconstructed.

**How I labelled them:** Labeling is pending. Every `label` field is blank and every `note` field contains `cold`. The `cold` marker records the intended labeling workflow; it does not mean independent labeling has already happened. I must assign the first 20 labels myself before training, using the taxonomy above. The final four entries are also unlabeled.

Earlier AI responses offered comment-specific classification guidance, although they did not fill the CSV labels. This assistance is disclosed below. I should check the course’s independence requirement before claiming that examples already discussed with AI qualify as cold labels; fresh comments without AI guidance may be needed for the independent set.

**Counts per label:**

| Label | Assigned count | Share of labeled entries |
|---|---|---|
| `analysis` | 0 | Not yet applicable |
| `hot_take` | 0 | Not yet applicable |
| `reaction` | 0 | Not yet applicable |
| **Total labeled** | **0** | **Not yet applicable** |

There are **24 unlabeled entries**. Label balance cannot be evaluated yet. My collection target is 200 usable labeled comments; the supplied template lists 150 as its minimum. At 24 entries, the dataset is below both. Additional collection is still required, with more needed if excerpts or other unsuitable entries are excluded. The six taxonomy examples above are separate from these 24 entries and are not included in this count.

**Three hard cases**

Pending my own labeling decisions. I will complete the three records below after assigning labels; no decisions or personal reflections have been invented for this draft.

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**2.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**3.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** Pending confirmation from the project notebook; no training run has been supplied.

**Settings:** Pending the actual run: epochs, learning rate, batch size, seed, and hardware.

**Anything I changed from the defaults, and why:** Pending the actual run. No changes are claimed.

**Split sizes:** Pending. Keep the completed dataset in one CSV and let the notebook create the train/validation/test splits. Record the actual split sizes and test counts per label after running it.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1 — Preparing the dataset file**

- *What I asked for:* Prepare `labels.csv` from the Reddit comments I pasted.
- *What came back:* A CSV with `text,label,note`, containing 24 entries after excluding the apparent advertisement. Labels were left blank and notes set to `cold`. CSV parsing and exact text recovery were checked.
- *What I changed:* No student edits or completed labels have been reported yet. I still need to review excerpt suitability and complete the independent labeling requirement.

**Moment 2 — Preparing this README**

- *What I asked for:* Prepare my README using the existing template and current project progress.
- *What came back:* A draft overview, collection details, current counts, AI-use disclosure, and explicit pending fields for work that has not been completed.
- *What I changed:* Pending my review. I must add my own difficult labeling decisions, actual training settings, results, and reflections before submission.

**Pre-labelling disclosure:** AI did not assign labels in the prepared CSV. Earlier conversation responses provided comment-specific classification hints and explanations of the taxonomy. AI also helped prepare the taxonomy materials, dataset file, and README draft. Blank labels do not erase that earlier guidance. I will not claim that AI supplied my independent decisions or that those decisions are complete. Any later AI-assisted labeling must be disclosed and reviewed by me.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

**Unit 6 status:** Pending. The sections below retain the submission template. No measurements, predictions, agreement decisions, or improvement results have been supplied. Write the prediction before viewing results.

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `analysis` |  |  |  |
| F1 — `hot_take` |  |  |  |
| F1 — `reaction` |  |  |  |

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

| true \ predicted | `analysis` | `hot_take` | `reaction` |
|---|---|---|---|
| `analysis` |  |  |  |
| `hot_take` |  |  |  |
| `reaction` |  |  |  |

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

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

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
