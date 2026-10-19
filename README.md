# TakeMeter

> 👋 **Start here**
>
> New to this repo? Read [RUNNING.md](RUNNING.md) first for setup instructions, notebook execution, baseline testing, and troubleshooting.
>
> Open `takemeter.ipynb` in VS Code and select the `.venv` Python kernel.

---

<!-- UNIT 5 — THE BUILD -->

## What This Does

TakeMeter is a machine learning text classification project that categorizes posts from the Reddit community r/learnprogramming into three categories: `question`, `opinion`, and `solution`.

The goal is to understand how a pretrained language model can be fine-tuned for text classification, evaluate its performance, identify classification errors, and improve the model based on evidence.

The project uses DistilBERT and evaluates model performance using accuracy, macro F1, class-specific F1 scores, and a confusion matrix.

---

## Label Taxonomy

### `question`

**Definition:** Posts where users ask for programming help, guidance, explanations, or learning resources.

**Example 1:** "How do you get better at coding without constantly following tutorials?"

**Example 2:** "Best resources to learn Git as a complete beginner?"

### `opinion`

**Definition:** Posts primarily expressing personal beliefs, concerns, or viewpoints about programming and technology.

**Example 1:** "What is the point of learning if AI can (or will eventually) do anything better than anyone"

**Example 2:** "IEEE Spectrum: AI Efficiency Could Cost Us the Next Generation of Experts"

### `solution`

**Definition:** Posts primarily sharing working solutions, practical tools, or learning resources rather than requesting help.

**Example 1:** "Free and open-source visual Git learning tools / resources"

**Example 2:** "im kinda proud of this solution i wrote even though it was an 'easy' i still felt like i grew a lot"

### The Hardest Boundary

**Which two labels:** `question` and `opinion`

**Decision rule:** If the primary purpose of a post is to request help or advice, classify it as `question`, even if it contains personal opinions or concerns. If the primary purpose is to express a viewpoint or argument and invite discussion, classify it as `opinion`.

---

## The Dataset

**Where the posts came from:** Reddit community r/learnprogramming.

**How I labelled them:** AI assistance was used to suggest labels for the collected posts. The dataset was reviewed, but the assignment's independent pre-AI labelling exercise of 20 posts was not completed.

**Counts per label:**

| Label     |   Count |    Share |
| --------- | ------: | -------: |
| question  |     121 |   69.94% |
| opinion   |      26 |   15.03% |
| solution  |      26 |   15.03% |
| **Total** | **173** | **100%** |

The dataset contains 173 posts, below the assignment's requested 200 posts.

The `question` category represents almost 70% of the dataset. This imbalance may cause the model to favor the majority class.

Many posts contain only their titles rather than complete post bodies. This reduces the context available to the classifier.

### Three Hard Cases

**1. AI and learning programming**

- **The post:** "What is the point of learning if AI can (or will eventually) do anything better than anyone"
- **Could have been:** `question` or `opinion`
- **I chose:** `opinion`, because the post primarily raises a viewpoint about AI's impact on learning programming rather than requesting a specific programming solution.

**2. Sharing a programming achievement**

- **The post:** "im kinda proud of this solution i wrote even though it was an 'easy' i still felt like i grew a lot"
- **Could have been:** `opinion` or `solution`
- **Existing label:** `solution`
- **Why it is difficult:** The post mentions a solution, but its main purpose may be sharing a personal achievement. This example needs manual review to determine whether `opinion` is more appropriate.

**3. Sharing an article about AI**

- **The post:** "IEEE Spectrum: AI Efficiency Could Cost Us the Next Generation of Experts"
- **Could have been:** `opinion` or `solution`
- **I chose:** `opinion`, because the title introduces a viewpoint about AI and expertise rather than presenting a direct programming solution.

These examples show why clear label definitions and manual review are important.

---

## The Training Run

**Base model:** `distilbert-base-uncased`

**Model type:** DistilBERT for sequence classification.

**Training settings:**

| Setting             | Value                               |
| ------------------- | ----------------------------------- |
| Model               | DistilBERT                          |
| Labels              | question, opinion, solution         |
| Epochs              | 3                                   |
| Learning rate       | 0.00002                             |
| Batch size          | 16                                  |
| Initial random seed | 42                                  |
| Additional seeds    | 7, 2024                             |
| Hardware            | CPU                                 |
| Framework           | PyTorch / Hugging Face Transformers |

**Anything I changed from the defaults, and why:** The project used three classification labels and repeated training with three random seeds to examine the stability of the evaluation results. Other changes from the starter defaults have not been independently verified.

**Split sizes:**

| Label     |   Train | Validation |   Test |
| --------- | ------: | ---------: | -----: |
| question  |      85 |         18 |     18 |
| opinion   |      18 |          4 |      4 |
| solution  |      18 |          4 |      4 |
| **Total** | **121** |     **26** | **26** |

The dataset was split into approximately 70% training, 15% validation, and 15% testing.

The test set contains only four `opinion` and four `solution` examples. Therefore, performance estimates for these categories are based on very small samples.

---

## How I Used AI

### Moment 1

- **What I asked for:** Help reviewing Reddit posts and identifying possible classification labels.
- **What came back:** Suggestions for the categories `question`, `opinion`, and `solution`.
- **What I changed:** I used these categories in the project and documented definitions, examples, and a decision rule.

### Moment 2

- **What I asked for:** Help correcting README Markdown formatting.
- **What came back:** A corrected README structure with consistent headings, examples, and decision rules.
- **What I changed:** I updated the README formatting and documentation.

### Moment 3

- **What I asked for:** Help understanding model training results, confusion matrix output, and Git commands.
- **What came back:** Explanations of evaluation metrics, model limitations, and steps for saving the project.
- **What I changed:** I documented the actual results and identified areas needing improvement.

**Pre-labelling disclosure:** AI assistance was used during dataset labelling. The required independent pre-AI labelling of 20 posts was not completed. The dataset may contain incorrectly assigned labels, which could affect model performance.

---

<!-- UNIT 6 — THE TEST -->

## Baseline vs. Trained

| Measure          | Baseline | Trained | Difference |
| ---------------- | -------- | ------- | ---------- |
| Overall accuracy | PENDING  | 0.692   | PENDING    |
| Macro F1         | PENDING  | 0.273   | PENDING    |
| F1 — `question`  | PENDING  | 0.818   | PENDING    |
| F1 — `opinion`   | PENDING  | 0.000   | PENDING    |
| F1 — `solution`  | PENDING  | 0.000   | PENDING    |

**What I predicted before I looked:** No prediction was recorded before examining the results, so a retrospective prediction is not reported.

**What the gap actually means:** The baseline comparison cannot be completed until the baseline metrics are available.

However, the trained model achieved 69.2% accuracy while failing to correctly identify any `opinion` or `solution` examples in the test set.

The test set contains 18 questions out of 26 posts, meaning a classifier that predicts `question` for every example would also achieve 69.2% accuracy.

This demonstrates why accuracy alone is insufficient for evaluating an imbalanced classification problem.

---

## Run Log — Before

The model was trained and evaluated using three random seeds.

| Measure          | Seed 42 | Seed 7 | Seed 2024 | Spread |
| ---------------- | ------: | -----: | --------: | -----: |
| Overall accuracy |   0.692 |  0.692 |     0.692 |  0.000 |
| Macro F1         |   0.273 |  0.273 |     0.273 |  0.000 |
| F1 — `question`  |   0.818 |  0.818 |     0.818 |  0.000 |
| F1 — `opinion`   |   0.000 |  0.000 |     0.000 |  0.000 |
| F1 — `solution`  |   0.000 |  0.000 |     0.000 |  0.000 |

All three seeds produced identical evaluation metrics.

This indicates that the observed failure to recognize minority classes was consistent across these runs.

### Evaluation Against the Five Criteria

| Criterion                   | Target | Seed 42 |  Seed 7 | Seed 2024 | Verdict |
| --------------------------- | ------ | ------: | ------: | --------: | ------- |
| Overall accuracy            | ≥ 0.70 |   0.692 |   0.692 |     0.692 | FAIL    |
| Question F1                 | ≥ 0.70 |   0.818 |   0.818 |     0.818 | PASS    |
| Opinion F1                  | ≥ 0.55 |   0.000 |   0.000 |     0.000 | FAIL    |
| Solution F1                 | ≥ 0.55 |   0.000 |   0.000 |     0.000 | FAIL    |
| High-confidence correctness | ≥ 0.75 | PENDING | PENDING |   PENDING | PENDING |

**Additional dataset constraint:** No label may exceed 70% of the dataset. The largest label, `question`, represents 69.94%, so this constraint passes numerically.

### Confusion Matrix

| True / Predicted | question | opinion | solution |
| ---------------- | -------: | ------: | -------: |
| **question**     |       18 |       0 |        0 |
| **opinion**      |        4 |       0 |        0 |
| **solution**     |        4 |       0 |        0 |

**My biggest off-diagonal number, and what it means:** Four actual `opinion` posts were incorrectly predicted as `question`. Another four actual `solution` posts were also incorrectly predicted as `question`.

The model predicted `question` for all 26 test examples.

This shows a strong bias toward the majority class.

---

## Verdicts and Diagnoses

| #   | Criterion                   | Target | Verdict | How I Decided                                  |
| --- | --------------------------- | ------ | ------- | ---------------------------------------------- |
| 1   | Overall accuracy            | ≥ 70%  | FAIL    | Achieved 69.2% across all seeds                |
| 2   | Question F1                 | ≥ 0.70 | PASS    | Achieved 0.818 across all seeds                |
| 3   | Opinion F1                  | ≥ 0.55 | FAIL    | Achieved 0.000 across all seeds                |
| 4   | Solution F1                 | ≥ 0.55 | FAIL    | Achieved 0.000 across all seeds                |
| 5   | High-confidence correctness | ≥ 75%  | PENDING | Exact criterion calculation needs verification |

### Diagnoses

**1. Class imbalance**

The dataset contains 121 `question` examples but only 26 examples each for `opinion` and `solution`.

The model may have learned to favor the majority class.

**2. Minority classes were not recognized**

All 26 test posts were predicted as `question`, causing both minority-class F1 scores to be zero.

**3. Small test samples**

The test set contains only four examples each for `opinion` and `solution`. A single corrected prediction would have a substantial effect on their metrics.

**4. Possible labelling problems**

Some posts are difficult to classify based on their titles alone. Ambiguous or incorrect labels may make it harder for the model to learn meaningful distinctions.

**5. Stable results do not guarantee a good model**

All three seeds produced the same scores. This confirms consistent behavior across the tested seeds, but that behavior was consistently poor for the minority classes.

---

## Agreement Report

**Agreement rate:** PENDING — independent 30-example agreement evaluation has not been completed or its results have not been supplied.

### Disagreements

**1.**

- **The post:** PENDING
- **Staff said / I said:** PENDING
- **My call, and why:** PENDING
- **Which pile:** PENDING

**2.**

- **The post:** PENDING
- **Staff said / I said:** PENDING
- **My call, and why:** PENDING
- **Which pile:** PENDING

**What the pattern in my disagreements tells me:** The agreement report is needed before determining whether the main labelling problem is confusion between `question` and `opinion`, confusion between `opinion` and `solution`, or inconsistent application of the taxonomy.

The current model results alone cannot establish the exact source of label disagreement.

---

## The Improvement

**What I changed:** PENDING — no completed improvement experiment has been documented.

**Which diagnosis pointed at it:** The initial evaluation identified majority-class bias and possible label ambiguity as the main issues to investigate.

A possible next experiment is to review ambiguous labels and address class imbalance, then retrain and compare the results using the same evaluation procedure.

### Run Log — After

| Measure          | Seed 42 | Seed 7  | Seed 2024 | Spread  |
| ---------------- | ------- | ------- | --------- | ------- |
| Overall accuracy | PENDING | PENDING | PENDING   | PENDING |
| Macro F1         | PENDING | PENDING | PENDING   | PENDING |
| F1 — `question`  | PENDING | PENDING | PENDING   | PENDING |
| F1 — `opinion`   | PENDING | PENDING | PENDING   | PENDING |
| F1 — `solution`  | PENDING | PENDING | PENDING   | PENDING |

**Did it help, and how do I know:** PENDING — an improvement experiment must be completed before making this comparison.

---

## What's Still Broken

The initial trained model does not successfully distinguish all three categories.

Although overall accuracy is 69.2%, the model predicts `question` for every test example.

The `opinion` and `solution` categories both have an F1 score of zero.

Other outstanding issues include:

- The dataset contains 173 posts instead of the requested 200.
- The dataset is imbalanced.
- Some labels may need manual review.
- The baseline comparison is incomplete.
- The agreement report is incomplete.
- A documented improvement experiment and after-results are still required.
- The dataset's independent pre-AI labelling requirement was not completed.

**The gap between what I meant my labels to capture and what the model learned:** The intended taxonomy separates requests for help, personal viewpoints, and practical solutions. However, the model learned a prediction pattern dominated by `question`, rather than reliably distinguishing the three purposes.

This demonstrates the importance of balanced data, consistent labelling, and class-specific evaluation metrics.

---

## Submission Checklist — Unit 5

- [x] `criteria.md` contains five numbered, measurable criteria with reasons.
- [ ] `labels.csv` contains the requested minimum number of real posts (currently 173).
- [x] `labels.csv` contains columns `text`, `label`, and `note`.
- [x] No label represents more than 70% of the dataset.
- [x] README includes project description, label taxonomy, dataset, training details, and AI usage.
- [x] Label taxonomy includes definitions, examples, and a decision rule.
- [x] Dataset section includes three hard cases for review.
- [ ] `results.json` and `test_split.csv` committed to Git.
- [ ] At least four new Git commits verified.
- [ ] GitHub repository URL submitted.

## Submission Checklist — Unit 6

- [ ] Baseline vs. Trained comparison completed.
- [x] Run Log — Before across three seeds.
- [x] Confusion matrix as a Markdown table.
- [ ] All five criteria fully evaluated.
- [x] Diagnoses documented for failed accuracy and class-specific F1 criteria.
- [ ] Agreement Report with disagreements explained.
- [ ] One improvement and Run Log — After.
- [x] What's Still Broken section.
- [ ] Required JSON results committed.
- [ ] At least four new Git commits verified.
- [ ] Same repository URL submitted.

---

📖 **How to run this project:** [RUNNING.md](RUNNING.md)
