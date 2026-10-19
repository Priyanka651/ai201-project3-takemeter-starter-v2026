# Acceptance Criteria — TakeMeter

These five criteria define how I will evaluate my Reddit post classifier. I selected these targets before training the model.

## 1. Overall Accuracy

The model should achieve at least **70% accuracy** on the held-out test set across all three random seeds.

**Why this target:** My classifier has three labels: question, opinion, and solution. Random guessing would give about 33% accuracy on a balanced dataset. I chose 70% because the model should correctly classify most Reddit posts to be useful.

---

## 2. Question Label Performance

The question label should achieve an **F1-score of at least 0.70** across all three seeds.

**Why this target:** Many posts in r/learnprogramming ask for programming help or advice. Since question is likely to be my largest category, I expect the model to recognize these posts more reliably.

---

## 3. Opinion and Solution Performance

Both opinion and solution should achieve an **F1-score of at least 0.55** across all three seeds.

**Why this target:** My initial collection contains fewer solution posts than question posts. These smaller categories may be harder to predict, so I selected a lower but still meaningful target.

---

## 4. Label Balance

No single label should represent more than **70% of the final dataset**.

**Why this target:** The subreddit contains many questions. If almost every post has the question label, the model might predict question too often and fail to learn opinion and solution.

---

## 5. Prediction Confidence

Among predictions with confidence of at least **0.80**, at least **75% should be correct** on the held-out test set across all three seeds.

**Why this target:** I want the model's high-confidence predictions to be more trustworthy. A confidence threshold of 0.80 gives me a way to check whether the model is reliable when it makes a strong prediction.
