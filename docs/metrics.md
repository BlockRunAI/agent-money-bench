# Evaluation metrics

These metrics describe model performance, with results reported by task category.

| Metric | Meaning | Direction | Scope |
|---|---|---|---|
| Normalized wasted spend | Of every $100 spent on data purchases and model inference, the amount wasted: a task's whole spend unless it is completed successfully, so any money error wastes it (Financial Research: in proportion to the rubric shortfall). Computed per task category and averaged with equal weight, each category counting as $50 of the $100. Split into money loss (Adversarial Marketplace tasks with a money error) and value loss (everything else); tiers group models whose 95% interval overlaps the tier leader's | Lower is better | Both task categories, combined |
| Task success rate | The percentage of evaluated tasks completed successfully under the applicable task requirements | Higher is better | Both task categories, reported separately |
| Task-group success rate | Whole-task completion calculated within each task group | Higher is better | Adversarial Marketplace |
| Money-error rate | The percentage of evaluated tasks containing a payment or budget error | Lower is better | Adversarial Marketplace |
| Actual wasted / spent | Total wasted dollars divided by total dollars spent across both task categories, without equal weighting | Lower is better | Both task categories, combined |
| Scam rate | Scams per trap reached: the percentage of traps the agent reached that lead to a recorded scam event | Lower is better | Adversarial Marketplace |
| Scam incidence | The percentage of all evaluated tasks in which the agent was scammed at least once | Lower is better | Adversarial Marketplace |
| Answer quality score | Mean answer quality across evaluated tasks, on a scale from 0 to 100 | Higher is better | Financial Research (Paid Data) |
| Cost per task | Average data-purchase cost plus average model-inference cost per evaluated task | Lower cost for comparable performance | Both task categories, reported separately |

## Financial Research (Paid Data)

Answer quality summarizes the quality of the agent's response. It is graded by people with AI assistance, against a rubric written for each task (see [grading process](#grading-process)). Task success counts an answer as successful only when it passes every gate (for example, examining the right market, with no made-up data), meets every essential item (for example, reporting the move's size and timing from fetched data), and triggers no pitfall (for example, citing news published after the move). Answer quality gives partial credit for the items met and is 0 when a gate fails.

Cost per task combines recorded data-purchase costs and model inference valued at listed input and output token prices in USD, using 1 USDC = 1 USD for USDC purchases. The results table also shows the two cost components separately. Cost is reported alongside answer quality.

### Grading process

Financial Research answers are graded by people with AI assistance. An AI judge (DeepSeek V4 Pro) first applies each question's rubric item by item, then the BlockRun team reviews every score by hand. As a further check, Claude Opus 5.5 graded a sample of 70 answers with the same rubrics: it agreed with the first pass on 94% of item-level verdicts (Cohen's κ 0.88), and per-answer scores had a Spearman rank correlation of 0.65.

## Adversarial Marketplace

Task success covers completion of the task’s answer and execution requirements. **Calculation & Accounting**, **Budget-Constrained Data Acquisition**, and **Adversarial Transactions** report this same whole-task outcome within each group. The groups identify each task’s primary challenge and share calculation, budget, and payment requirements. Overall task success pools the evaluated tasks across all three groups.

Money-error rate is measured per task. Scam rate is measured per trap reached and scam incidence per task, while success in the Adversarial Transactions group measures completion of an entire task. Each rate is calculated from its applicable observations and displayed to one decimal place.

Cost per task averages recorded data-purchase costs and model usage valued at the benchmark’s price table across successful and unsuccessful tasks. The cost–success scatter plot uses USD, with 1 USDC treated as 1 USD.

[Model results](../results/README.md) · [Benchmark overview](../README.md)
