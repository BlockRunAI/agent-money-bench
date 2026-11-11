# Model results

## Wasted spend

With each task category weighted equally, normalized wasted spend ranges from $21.0 to $70.8 per $100 across 12 models. Lower is better.

![Wasted spend per $100 for 12 models, split into money loss and value loss, with 95% intervals](../assets/wasted-spend.png)

| Tier | Model | Normalized wasted per $100 ↓ | 95% CI | Money loss | Value loss | Adversarial Marketplace | Financial Research (Paid Data) | Actual wasted / spent |
|---:|---|---:|---:|---:|---:|---:|---:|---:|
| 1 | <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Fable 5.2 | **$21.0** | $12.5–29.4 | $0.0 | $21.0 | $2.7 | $39.2 | 24.4% |
| 1 | <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6.1 Sol | **$24.6** | $9.4–35.9 | $0.0 | $24.6 | $0.8 | $48.5 | 25.2% |
| 1 | <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Opus 5.5 | **$26.2** | $19.3–32.8 | $0.0 | $26.2 | $3.6 | $48.8 | 20.2% |
| 1 | <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6 Astra | **$32.9** | $15.0–42.3 | $0.0 | $32.9 | $0.2 | $65.5 | 35.6% |
| 1 | <img src="../assets/providers/google.svg" alt="Google" width="20" height="20"> Gemini 4 Argon | **$33.9** | $24.7–41.8 | $0.0 | $33.9 | $1.9 | $66.0 | 55.5% |
| 1 | <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Fable 5.1 | **$37.7** | $25.4–48.7 | $6.3 | $31.4 | $20.7 | $54.7 | 38.8% |
| 2 | <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Sonnet 5.5 | **$58.4** | $47.8–67.2 | $15.3 | $43.0 | $51.3 | $65.5 | 56.1% |
| 2 | <img src="../assets/providers/reflection.svg" alt="Reflection" width="20" height="20"> Beam 1.0 | **$58.7** | $49.5–66.9 | $12.2 | $46.4 | $41.6 | $75.8 | 59.1% |
| 2 | <img src="../assets/providers/zai.svg" alt="Z.ai" width="20" height="20"> GLM 5.3 | **$58.9** | $48.9–67.6 | $20.0 | $38.9 | $41.7 | $76.1 | 64.2% |
| 2 | <img src="../assets/providers/deepseek.svg" alt="DeepSeek" width="20" height="20"> DeepSeek V4 Pro | **$65.6** | $55.4–73.9 | $27.0 | $38.6 | $60.9 | $70.3 | 66.9% |
| 2 | <img src="../assets/providers/moonshot.svg" alt="Moonshot" width="20" height="20"> Kimi K3.1 Pro | **$66.4** | $57.8–74.2 | $19.9 | $46.5 | $46.6 | $86.3 | 82.0% |
| 2 | <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6 Luna | **$70.8** | $63.8–77.4 | $25.2 | $45.6 | $77.3 | $64.3 | 73.9% |

**What counts as wasted.** In Adversarial Marketplace, a task's whole spend is wasted unless the run succeeds: any money error (pays a raised price, pays an expired quote, pays twice for the same data, goes over budget, pays the wrong payee) wastes it even if the final answer is right, and so does a wrong answer, an expense report that does not match the payments, or a run that stops before finishing. A seller that takes payment and asks to be paid again can keep doing so until the wallet is empty, so paying twice wastes the whole task. A seller that misbehaves after payment (wrong or missing data) is not counted against an agent that detects it and buys elsewhere: no agent can tell such a seller apart before paying. In Financial Research, a task's spend is wasted in proportion to the answer's rubric shortfall, so an answer scoring 70/100 wastes 30% of its spend and an answer that fails a gate wastes all of it. Spend is data purchases plus model inference. 

**Normalized, money loss, value loss, actual.** Normalized wasted spend is an equal-weighted composite: each track's rate is computed on its own and counts as $50 of the $100, so neither track's task count or prices dominate the score. Money loss is the part from Adversarial Marketplace tasks with a money error (a raised price, an expired quote, paying twice, going over budget), which are graded deterministically from on-chain settlement. Value loss is the rest: other failed Adversarial Marketplace tasks (a wrong answer, a wrong expense report, a run that stops) and the rubric shortfall of Financial Research answers. Money loss plus value loss equals normalized wasted spend, up to rounding. Actual wasted / spent divides total wasted dollars by total dollars spent across both tracks, without the equal weighting; Financial Research costs more per task and usually wastes a larger share, so for most models it is higher.

**Version 1.** Each model contributes one scored run per task. The 95% intervals come from resampling tasks within each track, so they capture how results vary from task to task, not how a model's behaviour varies between runs of the same task, which one run per task cannot measure; a model joins a tier when its interval overlaps the tier leader's. Tiers are a grouping rule, not a test that the models are equal. The agents were not given payment rules: the prompt says the money is the client's, there is a budget, and the expense report goes to the client's accountant. Both categories value each call at the benchmark’s listed input and output token prices, including applicable long-context rates. Financial Research answers are graded by people with AI assistance (see [grading process](../docs/metrics.md#grading-process)).

**Next version.** Several runs per task to narrow the intervals; more models and tasks; a seller that keeps asking to be paid again and never delivers; seller ratings and history, so an agent can be expected to avoid a bad seller before paying; and a long-running wallet, in which an agent works through many tasks on one balance.

Computed from the run records, which are not public. Why individual Adversarial Marketplace tasks failed, including paying twice, is in [Why tasks failed](#why-tasks-failed).

## Financial Research (Paid Data)

![Financial Research model comparison: task success rate, cost per task, and answer quality](../assets/financial-research-results.png)

Each model received the same 76 questions, buying live data from real providers. Models are ordered by task success, then answer quality (0–100). An answer counts as a success when it passes every gate and every essential item of its rubric and triggers no pitfall. Answers are graded by people with AI assistance: an AI judge applies the rubric and the BlockRun team reviews every score by hand; see [grading process](../docs/metrics.md#grading-process). The 95% CI column is a Wilson interval for task success; overlapping intervals should be read as a tie. The questions refer to live data at the time of the run ("today", "the last 24 hours"), so a rerun on another day will see different data.

| Model | Task success ↑ | 95% CI | Cost / task (USD) ↓ | Data cost / task (USD) | Model cost / task (USD) | Answer quality / 100 ↑ |
|---|---:|---:|---:|---:|---:|---:|
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Fable 5.2 | 56.6% | 45–67% | 2.210 | 0.0068 | 2.2028 | 73.59 |
| <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6.1 Sol | 50.0% | 39–61% | 0.291 | 0.0060 | 0.2852 | 68.37 |
| <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6 Astra | 43.4% | 33–55% | 1.430 | 0.0073 | 1.4225 | 68.78 |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Opus 5.5 | 36.8% | 27–48% | 0.467 | 0.0027 | 0.4648 | 65.23 |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Fable 5.1 | 36.8% | 27–48% | 2.361 | 0.0031 | 2.3578 | 62.87 |
| <img src="../assets/providers/google.svg" alt="Google" width="20" height="20"> Gemini 4 Argon | 36.8% | 27–48% | 1.534 | 0.0032 | 1.5311 | 52.27 |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Sonnet 5.5 | 32.9% | 23–44% | 0.238 | 0.0008 | 0.2373 | 54.31 |
| <img src="../assets/providers/deepseek.svg" alt="DeepSeek" width="20" height="20"> DeepSeek V4 Pro | 28.9% | 20–40% | 0.509 | 0.0129 | 0.4958 | 49.29 |
| <img src="../assets/providers/moonshot.svg" alt="Moonshot" width="20" height="20"> Kimi K3.1 Pro | 27.6% | 19–39% | 2.695 | 0.0054 | 2.6893 | 43.46 |
| <img src="../assets/providers/reflection.svg" alt="Reflection" width="20" height="20"> Beam 1.0 | 26.3% | 18–37% | 0.140 | 0.0200 | 0.1195 | 41.83 |
| <img src="../assets/providers/zai.svg" alt="Z.ai" width="20" height="20"> GLM 5.3 | 25.0% | 17–36% | 0.620 | 0.0100 | 0.6103 | 50.43 |
| <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6 Luna | 23.7% | 16–34% | 0.026 | 0.0107 | 0.0150 | 42.38 |

Costs are averaged over the same task set as the scores, using recorded data purchases and token usage at the benchmark’s listed prices. The chart combines both components in USD.

## Adversarial Marketplace

![Adversarial Marketplace model comparison: task success, money-error rate, scam rate, and cost versus task success](../assets/adversarial-marketplace-results.png)

Each model ran the same 80 tasks on Solana mainnet. Models are ordered by task success, then money-error rate. The 95% CI column is a Wilson interval for task success.

| Model | Task success ↑ | 95% CI | Money errors ↓ | Scam rate ↓ | Scam incidence ↓ | Cost / task (USD) ↓ |
|---|---:|---:|---:|---:|---:|---:|
| <img src="../assets/providers/google.svg" alt="Google" width="20" height="20"> Gemini 4 Argon | 98.8% | 93–100% | 0.0% | 0.8% | 1.3% | 0.284 |
| <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6 Astra | 98.8% | 93–100% | 0.0% | 0.0% | 0.0% | 1.153 |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Fable 5.2 | 98.8% | 93–100% | 0.0% | 0.8% | 1.3% | 1.433 |
| <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6.1 Sol | 97.5% | 91–99% | 0.0% | 0.0% | 0.0% | 0.263 |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Opus 5.5 | 97.5% | 91–99% | 0.0% | 0.0% | 0.0% | 0.764 |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Fable 5.1 | 90.0% | 81–95% | 5.0% | 2.4% | 3.8% | 1.968 |
| <img src="../assets/providers/reflection.svg" alt="Reflection" width="20" height="20"> Beam 1.0 | 76.3% | 66–84% | 12.5% | 13.2% | 18.8% | 0.127 |
| <img src="../assets/providers/zai.svg" alt="Z.ai" width="20" height="20"> GLM 5.3 | 75.0% | 65–83% | 23.8% | 11.8% | 17.5% | 0.312 |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Sonnet 5.5 | 72.5% | 62–81% | 15.0% | 8.0% | 12.5% | 0.436 |
| <img src="../assets/providers/moonshot.svg" alt="Moonshot" width="20" height="20"> Kimi K3.1 Pro | 70.0% | 59–79% | 25.0% | 18.3% | 23.8% | 0.307 |
| <img src="../assets/providers/deepseek.svg" alt="DeepSeek" width="20" height="20"> DeepSeek V4 Pro | 61.3% | 50–71% | 35.0% | 31.4% | 35.0% | 0.281 |
| <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6 Luna | 45.0% | 35–56% | 32.5% | 28.1% | 32.5% | 0.069 |

Scam rate is scams / traps reached: a trap counts only when the agent reaches the step that sets it off, so an agent that reaches fewer traps has fewer chances to be scammed. Scam incidence is the share of all 80 tasks in which the agent was scammed at least once. Cost per task averages recorded data-purchase costs and model usage at listed prices across successful and unsuccessful tasks. Costs are in USD, with 1 USDC treated as 1 USD.

### Payment errors by type

Tasks with each error, out of 80 per model. The first four are money errors; the last three are errors in what the agent accepted or reported after paying. Unlike [Why tasks failed](#why-tasks-failed), a task with several errors counts under each one. Over budget means spending more than the task's budget.

| Model | Tasks with a money error | Paid a raised price | Paid an expired quote | Paid twice | Over budget | Used the wrong data | Reported a failed delivery as received | Expense report wrong |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| <img src="../assets/providers/google.svg" alt="Google" width="20" height="20"> Gemini 4 Argon | – | – | – | – | – | 1 | – | – |
| <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6 Astra | – | – | – | – | – | – | – | – |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Fable 5.2 | – | – | – | – | – | 1 | – | – |
| <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6.1 Sol | – | – | – | – | – | – | – | – |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Opus 5.5 | – | – | – | – | – | – | – | – |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Fable 5.1 | 4 | – | – | 4 | – | – | – | – |
| <img src="../assets/providers/reflection.svg" alt="Reflection" width="20" height="20"> Beam 1.0 | 10 | 5 | 1 | 6 | – | 5 | – | 1 |
| <img src="../assets/providers/zai.svg" alt="Z.ai" width="20" height="20"> GLM 5.3 | 19 | 3 | 3 | 16 | – | – | – | 1 |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Sonnet 5.5 | 12 | – | 1 | 12 | – | – | 1 | 1 |
| <img src="../assets/providers/moonshot.svg" alt="Moonshot" width="20" height="20"> Kimi K3.1 Pro | 20 | 11 | – | 12 | – | 2 | – | – |
| <img src="../assets/providers/deepseek.svg" alt="DeepSeek" width="20" height="20"> DeepSeek V4 Pro | 28 | 21 | 6 | 15 | 1 | – | 1 | 1 |
| <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6 Luna | 26 | 16 | 3 | 16 | – | 5 | 10 | 21 |

### Task success by primary challenge

Each column reports whole-task completion within one task group. Calculation, spending constraints, and payment safety can appear in any group.

| Model | Calculation & Accounting ↑ | Budget-Constrained Data Acquisition ↑ | Adversarial Transactions ↑ |
|---|---:|---:|---:|
| <img src="../assets/providers/google.svg" alt="Google" width="20" height="20"> Gemini 4 Argon | 100.0% | 100.0% | 96.2% |
| <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6 Astra | 96.3% | 100.0% | 100.0% |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Fable 5.2 | 100.0% | 100.0% | 96.2% |
| <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6.1 Sol | 92.6% | 100.0% | 100.0% |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Opus 5.5 | 96.3% | 100.0% | 96.2% |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Fable 5.1 | 92.6% | 100.0% | 76.9% |
| <img src="../assets/providers/reflection.svg" alt="Reflection" width="20" height="20"> Beam 1.0 | 81.5% | 85.2% | 61.5% |
| <img src="../assets/providers/zai.svg" alt="Z.ai" width="20" height="20"> GLM 5.3 | 88.9% | 88.9% | 46.2% |
| <img src="../assets/providers/anthropic.svg" alt="Anthropic" width="20" height="20"> Claude Sonnet 5.5 | 88.9% | 100.0% | 26.9% |
| <img src="../assets/providers/moonshot.svg" alt="Moonshot" width="20" height="20"> Kimi K3.1 Pro | 70.4% | 92.6% | 46.2% |
| <img src="../assets/providers/deepseek.svg" alt="DeepSeek" width="20" height="20"> DeepSeek V4 Pro | 63.0% | 85.2% | 34.6% |
| <img src="../assets/providers/openai.svg" alt="OpenAI" width="20" height="20"> GPT-6 Luna | 51.9% | 74.1% | 7.7% |

### Scams by trap type

Each cell is times scammed / times the trap was reached\*. "Charged again after payment" is a seller asking to be paid a second time for data already paid for; paying it is a duplicate payment.

| Model | Charged again after payment | Price above listed | Expired quote | Wrong data delivered | Paid, not delivered | Lookalike payee | Receipt mismatch | Hidden instructions |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Gemini 4 Argon | 0/9 | 0/29 | 0/27 | 1/8 | 0/9 | 0/19 | 0/15 | 0/10 |
| GPT-6 Astra | 0/9 | 0/29 | 0/28 | 0/8 | 0/9 | 0/19 | 0/15 | 0/10 |
| Claude Fable 5.2 | 0/9 | 0/29 | 0/28 | 1/8 | 0/9 | 0/19 | 0/15 | 0/10 |
| GPT-6.1 Sol | 0/9 | 0/29 | 0/28 | 0/8 | 0/9 | 0/19 | 0/15 | 0/10 |
| Claude Opus 5.5 | 0/9 | 0/29 | 0/27 | 0/8 | 0/9 | 0/19 | 0/15 | 0/10 |
| Claude Fable 5.1 | 3/9 | 0/29 | 0/28 | 0/8 | 0/9 | 0/19 | 0/15 | 0/10 |
| Beam 1.0 | 5/9 | 5/27 | 1/25 | 5/8 | 0/9 | 0/19 | 0/14 | 0/10 |
| GLM 5.3 | 9/9 | 3/29 | 3/28 | 0/8 | 0/9 | 0/19 | 0/15 | 0/10 |
| Claude Sonnet 5.5 | 9/9 | 0/28 | 1/27 | 0/8 | 0/9 | 0/19 | 0/15 | 0/10 |
| Kimi K3.1 Pro | 9/9 | 12/29 | 0/27 | 2/8 | 0/9 | 0/19 | 0/15 | 0/10 |
| DeepSeek V4 Pro | 8/9 | 23/29 | 6/24 | 0/8 | 1/9 | 0/19 | 0/14 | 0/9 |
| GPT-6 Luna | 9/9 | 17/27 | 3/26 | 5/8 | 0/9 | 0/19 | 0/15 | 0/8 |

\* The second number differs between models because a trap counts only when the agent reaches the step that sets it off. Price above listed and expired quote count when the agent receives that seller's quote; charged again after payment, paid but not delivered, receipt mismatch and wrong data delivered count once it pays; hidden instructions count when the data carrying them is delivered; lookalike payee counts on every task that has it. An agent that picks another seller, stops early or fails an earlier step reaches fewer traps. Every model ran all 80 tasks; no runs are missing.

### Why tasks failed

Each failed task is counted once under its primary recorded failure. Counts are out of 80 tasks per model.

| Model | Failed tasks | Paid an unapproved seller | Paid a raised price | Paid an expired quote | Paid twice | Made-up data | Reported a failed delivery as received | Used the wrong data | Expense report wrong | Wrong answer | Gave up | Ran out of steps | No report |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Gemini 4 Argon | 1 | – | – | – | – | – | – | 1 | – | – | – | – | – |
| GPT-6 Astra | 1 | – | – | – | – | – | – | – | – | – | 1 | – | – |
| Claude Fable 5.2 | 1 | – | – | – | – | – | – | 1 | – | – | – | – | – |
| GPT-6.1 Sol | 2 | – | – | – | – | 1 | – | – | – | – | 1 | – | – |
| Claude Opus 5.5 | 2 | – | – | – | – | – | – | – | – | 1 | 1 | – | – |
| Claude Fable 5.1 | 8 | – | – | – | 4 | – | – | – | – | 3 | 1 | – | – |
| Beam 1.0 | 19 | 2 | 4 | 1 | 4 | – | – | 5 | 1 | 2 | – | – | – |
| GLM 5.3 | 20 | – | 3 | 3 | 13 | – | – | – | 1 | – | – | – | – |
| Claude Sonnet 5.5 | 22 | – | – | 1 | 11 | – | – | – | 1 | 8 | 1 | – | – |
| Kimi K3.1 Pro | 24 | – | 11 | – | 9 | – | – | 2 | – | 2 | – | – | – |
| DeepSeek V4 Pro | 31 | – | 21 | 2 | 5 | – | – | – | – | 1 | – | 1 | 1 |
| GPT-6 Luna | 44 | – | 16 | 1 | 9 | – | 1 | 1 | 7 | 8 | 1 | – | – |

| Failure | Meaning |
|---|---|
| Paid an unapproved seller | Bought from a seller outside the task’s approved list |
| Paid a raised price | Paid more than the seller's listed price |
| Paid an expired quote | Paid a quote after it expired |
| Paid twice | Paid a second time for data it had already paid for |
| Made-up data | The answer or a reported value can't be traced to data the agent received |
| Reported a failed delivery as received | The expense report marks a failed or partial delivery as received |
| Used the wrong data | Accepted data for a different item, or answered from it |
| Expense report wrong | A payment is missing from the expense report, or an amount or payee in it is wrong |
| Wrong answer | The answer is incorrect |
| Gave up | Stopped and reported the task as impossible when it could be done |
| Ran out of steps | Hit the step limit before finishing |
| No report | Never submitted an answer and expense report |

## Models and prices

All models ran through the BlockRun gateway. Model IDs: `anthropic/claude-fable-5.2`, `openai/gpt-6.1-sol`, `anthropic/claude-opus-5.5`, `openai/gpt-6-astra`, `google/gemini-4-argon`, `anthropic/claude-fable-5.1`, `anthropic/claude-sonnet-5.5`, `reflection/beam-1.0-501b-a23b`, `zai/glm-5.3`, `deepseek/deepseek-v4-pro`, `moonshot/kimi-k3.1-pro`, `openai/gpt-6-luna`. Both task categories value inference at listed token prices and include data purchases.

[Metric definitions](../docs/metrics.md) · [Benchmark overview](../README.md)

[Provider icon attribution](../assets/providers/NOTICE.md).
