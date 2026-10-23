# TakeMeter

**Unit 5 status:** The repository contains 200 labeled candidate comments, five acceptance criteria, a configured and executed training notebook, and saved `results.json` and `test_split.csv`. The seed-42 run achieved 0.633 accuracy and 0.583 macro F1. These results and the dataset are committed. Before final submission, the boundary-case drafts, labeling provenance, first-20 independence requirement, and suitability of two apparent article excerpts still need review. Training completion does not establish assignment completeness.

**Environment:** Python 3.13.15 in the project `.venv`; PyTorch 2.14.1; Apple Silicon GPU (MPS). The reported environment check passed 9 checks with 0 failures. The saved notebook has no error outputs.

**Scope:** This repository contains the Unit 5 build and ongoing Unit 6 testing. The baseline comparison, three-seed before-results, and staff agreement script run are complete. The agreement used AI-generated comparison labels; acceptance of that workflow for the independent-labeling exercise remains unresolved. Final diagnoses remain incomplete; the earlier five-epoch after-log is now verified. An earlier three-to-five-epoch experiment is documented below with its Unit 5 history; acceptance of its reuse for Unit 6 is not confirmed.

**Repository:** [TakeMeter project](https://github.com/Rashedul5554/ai201-project3-takemeter-starter-v2026)

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

TakeMeter is a text classifier for comments from Reddit’s r/technology community. It classifies comments into `analysis`, `hot_take`, and `reaction` based on the way they communicate. The goal is to distinguish supported explanations, strong judgments without substantial support, and immediate emotional responses. These labels describe communication style; they do not establish whether a claim is true.



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

**Labeling-workflow disclosure:** Earlier AI responses offered comment-specific guidance for the original collection. The first-20 independent-labeling requirement still needs verification; it cannot be inferred from a `cold` marker. The original 24 `note` values remain `cold` as previously saved; notes for the later 176 remain blank because the submission did not specify how those labels were produced. These markers still need review before submission; training has already been completed. If any submitted labels were generated or suggested by another AI tool, disclose that assistance and the student review performed.

**Counts per label in the repository’s validated 200-entry file:**

| Label | Count | Share |
|---|---|---|
| `analysis` | 83 | 41.5% |
| `hot_take` | 62 | 31.0% |
| `reaction` | 55 | 27.5% |
| **Total** | **200** | **100%** |

All 200 entries have a valid label, and each label is within the planned 20%–45% dataset-balance range. Formatting and normalized-text uniqueness were checked. These checks establish structure and counts, not label correctness. The collection target has been reached in candidate entries. The two apparent article excerpts in original data rows 2 and 16 remain included pending suitability review; replace excluded entries before claiming 200 usable examples. The six taxonomy examples are separate from these 200 records.

**Three boundary-case drafts — student review pending**

These cases use previously submitted labels and reasons. AI selected the examples and formulated the alternative-label comparisons; they are not a claim that the student independently considered those alternatives or personally found these exact examples difficult. Confirm or revise the account before submitting it as personal reflection. No dataset labels were changed during this README review.

### Case 1 — data row 4

> Honestly given how everything this administration and it's sympathizers have done to date is absolute trash quality execution, I'm not super worried that this garbage can't be wrecked by some armchair hacktivist.
> I think the part everyone forgets about. We'll be able to FOIA all the ICE employment records soon enough. These mouth-breathers will be papered everywhere.

**Possible labels:** `hot_take` or `analysis`.

**Why the boundary matters:** The supplied reasoning emphasizes sweeping judgments about execution, while acknowledging the explanation about FOIA requests.

**Recorded decision and supplied rationale:** The saved student choice is `hot_take`: the supplied rationale treated the generalized political judgment as dominant. Clear reasoning can still qualify as analysis without citations, so this remains a boundary to review.

### Case 2 — data row 8

> I'm glad to see this happening, though I am sad to see it becoming a partisan issue (EDIT: though it's not entirely a partisan divide, and El-Sayed's opponent has also called for AI regulation which is good). A ban on recursive self improvement would go a LONG way towards slowing things down enough that researchers can try to improve safety. AI should not be building AI.
> A national ban/pause isn't enough in the long term though, we need real international cooperation between the US and China.

**Possible labels:** `analysis` or `reaction`.

**Why the boundary matters:** The supplied reasoning identifies policy proposals and an argument about slowing recursive improvement, despite expressions of gladness and sadness.

**Recorded decision and supplied rationale:** The saved student choice is `analysis`: the supplied rationale focuses on the explanation for a pause and international cooperation, rather than the emotional wording.

### Case 3 — data row 17

> Starlink being cut, unfortunately, is not a surprise.
> But, it sounds like the US is intentionally withholding intel about incoming strikes. Intel they USED to provide to Ukraine.
> That is fucking vile. I know none of us are surprised that Trump is a Russian puppet. But this is, by far, the biggest demonstration of just how deep into their pockets he is.
> The US administration is now using Ukrainian civilian lives as leverage to act in Russian interests.
> Americans, for the love of dignity, do your part and VOTE.

**Possible labels:** `reaction` or `hot_take`.

**Why the boundary matters:** The supplied reasoning emphasizes anger and an impassioned plea to vote; the comment also makes substantive political accusations.

**Recorded decision and supplied rationale:** The saved student choice is `reaction`: the supplied rationale treated emotion as dominant. Under the stated rule, emotional language alone is insufficient; the general accusations make hot_take a defensible alternative, so this choice still needs review.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`.

**Settings:** 3 epochs, learning rate `2e-5`, batch size 16, maximum input length 128 tokens, seed 42 for the Unit 5 run. Extra Unit 6 runs used seeds 7 and 2024. Device: Apple Silicon GPU (MPS); PyTorch 2.14.1.

**Changes from defaults:** No changes to the listed starter training hyperparameters. The label list was set to `analysis`, `hot_take`, and `reaction`; student identification fields were filled in.

**Seed-42 split sizes:** 139 training, 31 validation, 30 test entries. The notebook performed a stratified split with nominal 70/15/15 proportions; integer rounding produced the counts below.

| Label | Train | Validation | Test |
|---|---|---|---|
| `analysis` | 57 | 13 | 13 |
| `hot_take` | 43 | 10 | 9 |
| `reaction` | 39 | 8 | 8 |
| **Total** | **139** | **31** | **30** |

**Initial held-out results:** Accuracy 0.633 (19/30 correct), macro F1 0.583. F1: `analysis` 0.703, `hot_take` 0.500, `reaction` 0.545. The most-confident third scored 0.800 accuracy versus 0.600 for the least-confident third, with only 10 examples in each group; this is limited evidence about confidence quality.

Training and evaluation finished despite warnings about the notebook progress widget, newly initialized classification weights, and MPS pinned-memory support. The successful outputs are stored in `results.json` and `test_split.csv`. The test labels contain only 13, 9, and 8 examples respectively, so per-label scores have substantial sampling uncertainty.

### Unit 5 bonus: completed second training run

**Status:** The experiment was declared in README commit `8cd185c` before implementation. The five-epoch run is complete; the comparison and supporting files are being prepared for commit. This addresses the second-run stretch feature; bonus credit remains subject to instructor review.

**Single change:** Increase `EPOCHS` from 3 to 5 to test whether additional training helps the classifier learn the label boundaries. Improvement is a hypothesis, not a promised result.

**Controls:** Keep the same 200-comment dataset and row order, seed 42, `distilbert-base-uncased`, learning rate `2e-5`, batch size 16, maximum input length 128 tokens, label mapping, split procedure, and Apple Silicon MPS device. Verify that the training, validation, and test memberships match the original seed-42 split. No dataset or label edits are part of this experiment. Increasing epochs necessarily increases the number of optimizer steps under the existing training procedure.

**Evidence preservation:** Preserve the original `results.json` and `test_split.csv` before running. Save the bonus output separately as `results_bonus_epochs5.json` and retain the executable notebook or script configuration that produced it. Preserve the existing Unit 6 before-results as well.

**Planned comparison:** Report original and bonus accuracy, macro F1, and F1 for each label, with differences calculated from unrounded results. Report the actual outcome even if performance worsens. Original scores are accuracy 0.633, macro F1 0.583, analysis F1 0.703, hot_take F1 0.500, and reaction F1 0.545.

**Interpretation:** This is one predeclared comparison, not repeated tuning against the test set. The held-out set has already been evaluated, so disclose that fact and do not present the bonus evaluation as a new untouched test. Do not claim the bonus point until the run, comparison, and repository evidence are complete; grading is determined by the instructor.


**Completed comparison — seed 42:** Both evaluations used the exact same 30 test entries in the same order, verified by parsing the saved test CSVs. The original result file is unchanged. The configured training change was epochs from 3 to 5; model, seed, label mapping, learning rate, batch size, max length, and device remain the same. The dataset and split code were retained; no original training-membership file was saved, so identical training/validation membership follows from keeping those inputs and the deterministic split unchanged rather than from a saved-membership comparison.

| Measure | Original: 3 epochs | Bonus: 5 epochs | Difference |
|---|---|---|---|
| Overall accuracy | 0.633 | 0.767 | +0.133 |
| Macro F1 | 0.583 | 0.743 | +0.160 |
| F1 — `analysis` | 0.703 | 0.812 | +0.110 |
| F1 — `hot_take` | 0.500 | 0.750 | +0.250 |
| F1 — `reaction` | 0.545 | 0.667 | +0.121 |

**What changed:** Correct test predictions increased from 19 to 23 out of 30 (+4), an accuracy increase of 13.3 percentage points. Macro F1 increased by 0.160. Hot-take recall increased from 3/9 to 6/9; reaction recall increased from 3/8 to 4/8. Analysis recall remained 13/13, while analysis predictions decreased from 24 to 19, reducing overprediction of that label. Differences above use unrounded metrics.

**Evidence:** `results_original_epochs3.json` and `test_split_original_epochs3.csv` preserve the original run; `results_bonus_epochs5.json` and `test_split_bonus_epochs5.csv` contain the seed-42 bonus evaluation. The executed configuration is in `takemeter.ipynb`. The later three-seed cell also ran and had reused the bonus filename; its three records are preserved separately in `results_three_seeds_bonus_epochs5.json`. The original Unit 6 before-results are retained. This extra run is disclosed rather than presented as part of the single-seed predeclared comparison.

**Limit:** These gains describe one small, previously evaluated split. They do not establish significance, new-thread generalization, or that every three-seed criterion is met. No further test-guided tuning was performed as part of documenting this comparison.


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
- *What I changed:* I supplied the dataset labels and ran the notebook. AI added verified settings/results and drafted three boundary-case discussions from my submitted reasons. My review of those discussions and labeling provenance remains pending; AI assistance is not presented as independent student reflection.

**Pre-labelling disclosure:** The initial CSV had blank labels. I later supplied labels for all 200 entries, which AI entered unchanged. Earlier conversation responses provided comment-specific classification hints and explanations of the taxonomy. AI also helped prepare the taxonomy materials, dataset file, and README draft. AI collection assistance and earlier classification guidance are disclosed separately from the labels I submitted. This record does not establish that the first-20 independence requirement has been met. The provenance of any labels generated with another tool still needs to be recorded accurately. Any later AI-assisted labeling must be disclosed and reviewed by me.

**Moment 3 — Unit 6 agreement and troubleshooting**

- *What I asked for:* Help running the baseline and agreement scripts, resolving an empty staff file and an inactive virtual environment, and updating the README from the actual output.
- *What came back:* Environment activation instructions, a command pointing to the TF-supplied answer file, and an explanation of the 30/30 agreement result and its limitations.
- *What I changed:* I activated the project environment, ran the scripts locally, and committed `agreement_results.json`. AI helped draft this documentation. The comparison labels were AI-generated before receipt of the staff key; the result is not described as independent student labeling.

## Unit 5 pre-submission checklist

- [x] Community exploration and label taxonomy files are present.
- [x] Three label definitions, six taxonomy examples, and a boundary rule are documented.
- [x] Five numeric criteria with reasons are recorded in `criteria.md`.
- [x] One `labels.csv` contains 200 valid labels with no normalized-text duplicates.
- [x] No label exceeds the brief’s 70% cap; current shares also meet the project’s stricter 20%–45% target.
- [x] Initial training completed; `results.json` and `test_split.csv` are tracked in Git.
- [x] More than four project commits are present.
- [ ] Review the three AI-drafted boundary cases and make the personal account accurate.
- [ ] Confirm first-20 independence and accurately document the labeling method in `note` and this README. The current file has 24 `cold` notes and 176 empty notes; this is not evidence of compliance.
- [ ] Decide whether the apparent article excerpts in data rows 2 and 16 are suitable. If the dataset changes, regenerate affected training results and report that change rather than silently altering the dataset behind existing results.
- [ ] Verify the inherited taxonomy examples and source links if source verification is required.
- [ ] Submit the existing repository URL through the course submission page.

The staff answer key, agreement report, baseline comparison, three-seed analysis, and improvement experiment are not Unit 5 prerequisites according to `RUNNING.md`.

---

<details>
<summary>Unit 6 — testing progress and remaining work</summary>

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Extra work has been retained for later; it is not a Unit 5 prerequisite.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

**Unit 6 status:** The baseline comparison, three-seed before-evaluation, seed-42 confusion matrix, and agreement script run are complete. The agreement run matched all 30 staff labels using AI-generated comparison labels; it does not establish independent student labeling. Final diagnoses, an accepted resolution of the independent-labeling exercise, remain pending; the earlier five-epoch after-log is verified. The earlier three-to-five-epoch experiment is documented below; its reuse for Unit 6 is not confirmed. No advance student prediction is recorded; none has been reconstructed after seeing the results.

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.467 | 0.633 | +0.167 |
| Macro F1 | 0.454 | 0.583 | +0.128 |
| F1 — `analysis` | 0.609 | 0.703 | +0.094 |
| F1 — `hot_take` | 0.333 | 0.500 | +0.167 |
| F1 — `reaction` | 0.421 | 0.545 | +0.124 |

**What I predicted before I looked:** No prediction has been provided for this record. If one was recorded before viewing results, insert that original prediction; otherwise disclose that it was not recorded.
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:** The zero-shot baseline (`facebook/bart-large-mnli`) used the definitions in `label_definitions.txt` and the same 30 held-out comments as the seed-42 trained model. The trained model got 19/30 correct versus 14/30 for the baseline: five additional correct classifications, a 16.7-percentage-point accuracy gain, and a macro-F1 gain of 0.128. Differences are calculated before rounding. All three per-label F1 values are higher for the trained model on this split. This is a benefit on one small held-out set, not proof of a stable improvement across seeds or unseen threads; seed 7 trained accuracy was only 0.433. The trained model still misses the predeclared performance criteria across the three runs.
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.70 in each seed | 0.633 | 0.433 | 0.533 | MISSED |
| 2. F1 — analysis | ≥ 0.65 in each seed | 0.703 | 0.605 | 0.625 | MISSED |
| 3. F1 — hot_take | ≥ 0.60 in each seed | 0.500 | 0.000 | 0.182 | MISSED |
| 4. F1 — reaction | ≥ 0.60 in each seed | 0.545 | 0.000 | 0.588 | MISSED |
| 5. Full-dataset balance | Each label 20%–45%; checked once | N/A | N/A | N/A | MET on current dataset |

Criterion 5 is evaluated once on the full reviewed dataset; seed columns do not apply to that criterion. The validated repository file meets the balance target: 41.5% analysis, 31.0% hot_take, and 27.5% reaction. Recalculate if the dataset changes.

Accuracy ranges from 0.433 to 0.633, a spread of 0.200 (20 percentage points). Macro F1 ranges from 0.202 to 0.583. The results are sensitive to the split and initialization; these runs alone do not identify the cause.

### Confusion matrix — seed 42, 30 held-out entries

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | `analysis` | `hot_take` | `reaction` |
|---|---|---|---|
| `analysis` | 13 | 0 | 0 |
| `hot_take` | 6 | 3 | 0 |
| `reaction` | 5 | 0 | 3 |

**My biggest off-diagonal number, and what it means:** Six true `hot_take` comments were predicted as `analysis`, while zero true `analysis` comments were predicted as `hot_take`. Five true reactions were also called analysis. In this run, 24 of 30 predictions were analysis, showing overprediction of that label.
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.70 in every seed | MISSED | Minimum observed value: 0.433; not all seeds reach the target. |
| 2 | F1 — analysis | ≥ 0.65 in every seed | MISSED | Minimum observed value: 0.605; not all seeds reach the target. |
| 3 | F1 — hot_take | ≥ 0.60 in every seed | MISSED | Minimum observed value: 0.000; not all seeds reach the target. |
| 4 | F1 — reaction | ≥ 0.60 in every seed | MISSED | Minimum observed value: 0.000; not all seeds reach the target. |
| 5 | Full-dataset balance | Each label 20%–45% | MET | Shares are 41.5%, 31.0%, and 27.5% in the current 200 entries. |

**Diagnoses — preliminary; agreement result available with provenance limitations**

- **Accuracy:** All three runs are below 0.70. Accuracy varies by 20 percentage points. This establishes unstable measured performance, not a proven cause.
- **Analysis F1:** Seed 42 reaches 0.65, but seeds 7 and 2024 do not. In seed 7, every test comment is predicted as analysis; recall is 1.0 but precision is only 0.433.
- **Hot-take F1:** All three runs miss 0.60. Seed 42 sends six of nine hot takes to analysis; seed 7 finds none. The model is not reliably separating supported explanation from unsupported judgment.
- **Reaction F1:** All three runs miss 0.60. Seed 42 sends five of eight reactions to analysis; seed 7 finds none. The small test support also limits precision of this measurement.

Possible contributors include limited training examples, inconsistent boundaries, short training, and correlated comments from the same threads appearing across row-level splits. None has been isolated experimentally. The completed comparison found 100% AI–staff agreement on the separate staff set. It cannot identify inconsistencies in my own 200 labels or rule them out. A review of the training labels and training/validation behavior is still needed to strengthen the causal diagnosis. The earlier three-to-five-epoch comparison is documented below as a test of training duration, with its original history retained. The earlier five-epoch Unit 5 bonus results provide evidence that training duration can affect these scores, but are not presented as a newly selected Unit 6 experiment. Preserve the current targets and before-results.


<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

**Reference set and taxonomy:** The comparison used all 30 posts in `data/staff_posts.csv` and the staff definitions and decision rules in `data/staff_taxonomy.md`. The authentic staff answer file was received from my teaching fellow and saved as `data/data_staff_labels.csv`.

**Labeling provenance:** At my request, AI assigned all 30 labels in `my_staff_labels.csv` before the staff answer file was obtained. Reasons are recorded in `staff_labeling_notes.md`. These are AI-generated comparison labels, not my independently assigned answers. Acceptance of this workflow for the required independent-labeling exercise remains unresolved with the teaching fellow.

**Command used:**

```bash
source .venv/bin/activate
python agreement.py --staff-labels data/data_staff_labels.csv
```

**Results:** 30 posts compared, 30 agreed, 0 disagreed, and **100% agreement**. The script wrote `agreement_results.json`; the terminal confirms it was committed in `f3eac92`.

**Disagreements and adjudication:** There were no disagreements in this comparison, so there are no disagreement cases to adjudicate or sort into the three categories. No disagreement examples have been invented.

**Interpretation and limitations:** This measures AI–staff agreement on these 30 posts. It does not establish my independent labeling consistency, prove that my 200 training labels are consistent, or show that the staff set was easy. It also does not establish that the trained model learned the intended boundaries: its separate evaluation still shows overprediction of `analysis`. Because the comparison contains no disagreements and used AI-generated labels, it provides no direct evidence that my own labeling caused the model's errors. The staff set and my training data also use different taxonomies and communities, which limits transfer of this result.

---

## The Improvement

**Change tested:** Increase training from 3 to 5 epochs while retaining the dataset and row order, model, learning rate, batch size, maximum input length, split procedure, and seeds 42, 7, and 2024.

**Reason:** The original runs overpredicted `analysis`; seed 7 predicted every test post as that label. Additional training was tested as a possible way to improve separation between labels. This is a training-duration hypothesis, not proof that the labels are consistent or that too little training explains every miss.

**Experiment history:** This experiment was performed earlier as the Unit 5 bonus and is reported here transparently. The original results are preserved in `results_three_seeds_before.json`, and the five-epoch results are preserved in `results_three_seeds_bonus_epochs5.json`. It is not a newly performed Unit 6 experiment. Acceptance of reusing this earlier experiment for Unit 6 has not been confirmed with the teaching fellow.

**Evidence and remaining work:** Both existing JSON files are preserved unchanged. The complete three-seed score comparison is verified in the after-log below. The required `results_three_seeds_after.json` has not yet been supplied or verified; no empty, renamed, or fabricated result file is presented as a new run.

### Run Log — After: earlier five-epoch experiment

These values were checked against `results_three_seeds_bonus_epochs5.json` and compared with `results_three_seeds_before.json`. Both files record seeds 42, 7, and 2024, 30 test examples per seed, and MPS. The JSON summaries do not contain full training settings, dataset hashes, or test-post membership; the controlled-change description relies on the documented experiment and retained notebook, not on these summaries alone.

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.70 in each seed | 0.767 | 0.600 | 0.567 | MISSED |
| 2. F1 — analysis | ≥ 0.65 in each seed | 0.812 | 0.722 | 0.690 | MET |
| 3. F1 — hot_take | ≥ 0.60 in each seed | 0.750 | 0.333 | 0.400 | MISSED |
| 4. F1 — reaction | ≥ 0.60 in each seed | 0.667 | 0.500 | 0.500 | MISSED |
| 5. Full-dataset balance | Each label 20%–45%; checked once | N/A | N/A | N/A | MET on unchanged dataset |

Criterion 5 uses the previously verified counts of 83 analysis, 62 hot_take, and 55 reaction examples, conditional on retaining that same dataset. The result JSON files do not independently verify dataset counts.

**Did it help, and how do I know:** Accuracy improved in all three seeds: 0.633 to 0.767 for seed 42 (+13.3 percentage points), 0.433 to 0.600 for seed 7 (+16.7 points), and 0.533 to 0.567 for seed 2024 (+3.3 points). Mean accuracy across the three runs rose from 0.533 to 0.644, and mean macro F1 rose from 0.416 to 0.597. These are descriptive averages across overlapping splits, not a pooled independent test or the basis for the verdicts.

Analysis F1 now reaches 0.65 in every seed, changing that criterion from MISSED to MET. Overall accuracy, hot-take F1, and reaction F1 still miss their targets because each threshold must hold in every seed. Reaction F1 actually fell from 0.588 to 0.500 for seed 2024, so the improvement was not uniform across labels. Accuracy still spans 20 percentage points across seeds, from 0.567 to 0.767; the experiment did not remove instability.

The comparison supports additional training as helpful under the documented setup, but does not prove that all failures were caused by too few epochs or that the labels are consistent. This is the earlier Unit 5 experiment, not a newly performed Unit 6 run; acceptance of its reuse remains unconfirmed. No `results_three_seeds_after.json` has been generated by this review.

**Notebook output fix:** Section 6 now uses `RUN_LABEL = "after"`, constructs `results_three_seeds_after.json`, and stops if that output already exists. The code fix was committed as `3be0a45`; it does not itself constitute a completed training run. Preserve both existing result files.

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

The initial runs miss all four performance criteria across the required seeds. In the earlier five-epoch experiment, analysis F1 meets its target across all seeds, while accuracy, hot-take F1, and reaction F1 remain below target in at least one seed. They overpredict analysis, and seed 7 fails to predict the other labels entirely. Baseline comparison and the AI–staff agreement run are complete. Acceptance of the independent-labeling workflow and reuse of the earlier experiment, and final evidence-based diagnoses remain pending; the earlier five-epoch after-log is now verified; no claim is made that the current model is ready for use.

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:** The intended labels separate supported explanations, unsupported judgments, and immediate feelings. The seed-42 matrix instead shows that many hot takes and reactions are absorbed into analysis, so the intended boundaries have not been learned reliably.
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
       [x] results.json and test_split.csv committed manually after the notebook writes them
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

</details>
