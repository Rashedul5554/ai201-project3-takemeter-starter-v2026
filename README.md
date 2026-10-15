# TakeMeter

**Status:** A completed 200-entry dataset has been prepared with student-submitted labels. The most recent local check of the repository’s `labels.csv` still showed only 24 entries, so replacement with the completed file and a new count check are pending. Suitability review, labeling-workflow verification, training, and evaluation are also pending.

**Environment check:** The reported `python test.py` run passed 9 checks with 0 failures and 1 warning. Python 3.13.15 and the project virtual environment were active, required packages imported, and PyTorch reported Apple Silicon GPU (MPS) support. The baseline model has not yet been downloaded; the check estimates about 1.6 GB on its first run. Passing these checks confirms the environment, not the dataset or model performance.

**Before the next commit:** Replace the older root `labels.csv` with the completed 200-entry file, confirm 200 rows and zero blank labels, and save this README. The counts below describe the prepared 200-entry file until that local replacement is verified.

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

Community: **r/technology**. The six taxonomy examples below were supplied as comments from the discussion [AI can cost more than human workers now](https://www.reddit.com/r/technology/comments/1swsnn6/ai_can_cost_more_than_human_workers_now/). These taxonomy examples and source links were retained from the existing README; they were not reverified during this update. Labels describe how the comments communicate; they do not certify that a commenter’s claims are true.

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

**Where the posts came from:** The initial collection contained 25 pasted entries from seven r/technology discussions. One apparent Ozempic advertisement was removed, leaving 24 entries. AI then collected 20 additional entries from four thread pages and another 156 from six thread pages. The final collection contains 200 distinct candidate texts; these thread groups overlap and should not be added together as unique discussion counts.

The initial 24 texts retain their pasted wording, Markdown, and internal newlines after removing entry headings and separator whitespace. The later 176 use accessible indexed Reddit page text; original raw Markdown and whitespace were not available for verification. Source records are in `additional_comment_sources.csv` and `additional_156_sources.csv`. The initial seven source discussions are:

- [Bank of America contractors](https://www.reddit.com/r/technology/comments/1x6700d/bank_of_america_hired_1000_contractors_to_patch/)
- [ICE surveillance](https://www.reddit.com/r/technology/comments/1x5thwn/ice_planning_100_million_nightmare_domestic_spy/)
- [BlackRock and data centers](https://www.reddit.com/r/technology/comments/1x5v797/blackrocks_larry_fink_warns_a_data_center/)
- [El-Sayed’s AI platform](https://www.reddit.com/r/technology/comments/1x5wce2/elsayed_releasing_his_platform_on_ai_calls_it_an/)
- [SpaceX limiting Starlink](https://www.reddit.com/r/technology/comments/1x60zu3/spacex_limits_starlink_service_for_ukraines/)
- [Ukrainian soldiers’ Starlink access](https://www.reddit.com/r/technology/comments/1x5nj77/ukrainian_soldiers_in_combat_have_access_to/)
- [Peter Thiel and AI critics](https://www.reddit.com/r/technology/comments/1x5lcon/peter_thiel_escalates_war_on_ai_critics_accuses/)

 The convenience sample is concentrated on browsers and data centers and is not representative of all r/technology discussion. Some replies require parent context. The 156-entry batch excluded visible quote blocks, unresolved links, obvious bot notices, removed bodies, normalized duplicates, and bodies shorter than 30 characters. This length filter can underrepresent short reactions. Visible author names do not establish human authorship, and factual claims were not independently verified.

**How I labelled them:** I supplied labels for the initial 24 entries and subsequently submitted labels for the remaining 176. AI matched these labels to the original texts and entered them unchanged. One submitted comment had a stray bracket and quote-escaping differences; the original dataset text was retained. No new classification decisions were made during this transfer. The student-supplied labels were preserved rather than independently validated as correct.

**Labeling-workflow disclosure:** Earlier AI responses offered comment-specific guidance for the original collection. The first-20 independent-labeling requirement still needs verification; it cannot be inferred from a `cold` marker. The original 24 `note` values remain `cold` as previously saved; notes for the later 176 remain blank because the submission did not specify how those labels were produced. These markers need review under the actual course/notebook convention before training. If any submitted labels were generated or suggested by another AI tool, disclose that assistance and the student review performed.

**Counts per label in the prepared 200-entry file:**

| Label | Count | Share |
|---|---|---|
| `analysis` | 83 | 41.5% |
| `hot_take` | 62 | 31.0% |
| `reaction` | 55 | 27.5% |
| **Total** | **200** | **100%** |

All 200 entries have a valid label, and each label is within the planned 20%–45% dataset-balance range. Formatting and normalized-text uniqueness were checked. These checks establish structure and counts, not label correctness. The collection target has been reached in candidate entries. The two apparent article excerpts in original data rows 2 and 16 remain included pending suitability review; replace excluded entries before claiming 200 usable examples. The six taxonomy examples are separate from these 200 records.

**Three hard cases — student completion required**

I have supplied labels for all 200 entries and reasons for the original 24. I still need to select three cases that I personally found difficult and document the alternative labels considered; no personal reflections have been invented for this draft.

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

**Settings:** Epochs, learning rate, batch size, and seed are pending the actual run. The environment check reports Apple Silicon GPU (MPS) availability; actual training-device use must be confirmed in the notebook.

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
- *What I changed:* I subsequently supplied labels for all 200 entries. AI entered these choices without altering them. I still need to review excerpt suitability and confirm the independent labeling requirement.

**Moment 2 — Preparing this README**

- *What I asked for:* Prepare my README using the existing template and current project progress.
- *What came back:* A draft overview, collection details, current counts, AI-use disclosure, and explicit pending fields for work that has not been completed.
- *What I changed:* Pending my review. I must add my own difficult labeling decisions, actual training settings, results, and reflections before submission.

**Pre-labelling disclosure:** The initial CSV had blank labels. I later supplied labels for all 200 entries, which AI entered unchanged. Earlier conversation responses provided comment-specific classification hints and explanations of the taxonomy. AI also helped prepare the taxonomy materials, dataset file, and README draft. AI collection assistance and earlier classification guidance are disclosed separately from the labels I submitted. This record does not establish that the first-20 independence requirement has been met. The provenance of any labels generated with another tool still needs to be recorded accurately. Any later AI-assisted labeling must be disclosed and reviewed by me.

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
| 1. Overall accuracy | ≥ 0.70 in each seed | Pending | Pending | Pending | Pending |
| 2. F1 — analysis | ≥ 0.65 in each seed | Pending | Pending | Pending | Pending |
| 3. F1 — hot_take | ≥ 0.60 in each seed | Pending | Pending | Pending | Pending |
| 4. F1 — reaction | ≥ 0.60 in each seed | Pending | Pending | Pending | Pending |
| 5. Full-dataset balance | Each label 20%–45%; checked once | N/A | N/A | N/A | Pending final review |

Criterion 5 is evaluated once on the full reviewed dataset; seed columns do not apply to that criterion. The prepared file currently meets the balance target, pending confirmation of the repository file and any suitability exclusions.

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
| 1. Overall accuracy | ≥ 0.70 in each seed | Pending | Pending | Pending | Pending |
| 2. F1 — analysis | ≥ 0.65 in each seed | Pending | Pending | Pending | Pending |
| 3. F1 — hot_take | ≥ 0.60 in each seed | Pending | Pending | Pending | Pending |
| 4. F1 — reaction | ≥ 0.60 in each seed | Pending | Pending | Pending | Pending |
| 5. Full-dataset balance | Each label 20%–45%; checked once | N/A | N/A | N/A | Pending final review |

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
