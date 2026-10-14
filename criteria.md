# Acceptance criteria — TakeMeter

Five criteria defining what "working" means for this classifier, set in Unit 5 before training. These are targets, not measured results.

The classifier uses three labels: `analysis`, `hot_take`, and `reaction` for comments from r/technology.

Criteria 1–4 are evaluated separately on the held-out set in each run using seeds **42, 7, and 2024**. A criterion is met only if its threshold is reached in all three runs; averaging the runs does not count. Criterion 5 is checked once on the full final labeled dataset before splitting it.

---

## 1. Overall accuracy

The trained classifier must achieve held-out accuracy of at least **0.70 in each of the three seed runs**.

**Why this target:** The classifier needs to distinguish supported explanations, unsupported judgments, and immediate feelings in r/technology comments. Getting at least seven out of ten comments correct would make it useful for an initial sorting pass, while still requiring human review.

---

## 2. Performance on `analysis`

The trained classifier must achieve held-out F1 of at least **0.65 for `analysis` in each of the three seed runs**.

**Why this target:** This label has concrete signals, including supporting facts, examples, statistics, and clear reasoning. A higher target is appropriate because the classifier should recognize that support even when a comment also contains a strong opinion or emotional language.

---

## 3. Performance on `hot_take`

The trained classifier must achieve held-out F1 of at least **0.60 for `hot_take` in each of the three seed runs**.

**Why this target:** Unsupported judgments can resemble emotional reactions, making this one of the hardest categories. F1 measures both whether predicted hot takes belong in this category and whether the model finds the actual hot takes.

---

## 4. Performance on `reaction`

The trained classifier must achieve held-out F1 of at least **0.60 for `reaction` in each of the three seed runs**.

**Why this target:** Brief or sarcastic reactions may express feelings without explicit emotion words. This target allows for that ambiguity while requiring the model to learn the distinction between expressing an immediate feeling and asserting an unsupported judgment.

---

## 5. Dataset balance

In the final labeled dataset, **each of `analysis`, `hot_take`, and `reaction` must represent between 20% and 45% of all examples**, inclusive.

**Why this target:** All three communication styles matter to this classifier. Keeping every label within this range limits dominance by one category and gives the model meaningful exposure to the difficult `hot_take`–`reaction` boundary. If I collect 200 examples, this means 40–90 examples per label; this is a planned range, not a claim about existing counts.

---

<!-- UNIT 6 — preserve the original targets above.

If a criterion is broken because it cannot be measured, keep its original
wording and add a revision underneath with an explanation of why it was
unmeasurable. Do not overwrite or delete the original criterion.

Do not lower a measurable target because the model missed it. Record the
miss, diagnose its cause, attempt an improvement, and report the result.

For criteria 1–4, report all three seed results and mark MET only when every
run reaches the stated threshold. For criterion 5, report the actual count
and percentage for each label in the full final dataset.
-->
