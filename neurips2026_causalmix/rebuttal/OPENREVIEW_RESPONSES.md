# Current OpenReview rebuttal responses with clarified tables

The prose and numerical results below reproduce the rebuttal responses posted on OpenReview on July 27, 2026. Table headers and captions have been clarified for readability. Copy only the text between the `COPY` markers into the corresponding OpenReview reply.

Throughout the tables:

- **Data-use gain** is `F1(with data) − F1(without data)`.
- **Reasoning-SFT effect** is `data-use gain after reasoning SFT − data-use gain after format SFT`.

---

## Reviewer svky

<!-- COPY START: svky -->

Thank you for recognizing the value of separately evaluating semantic and statistical evidence. To address the scale and familiarity concerns, we added larger and newly generated graphs plus a name–data conflict control; recovery remains weak, so we narrow rather than broaden our claims. We will also clarify the paper’s contribution and terminology.

### W1 — Novelty

CausalMix is an evaluation and dataset contribution, not a new causal-discovery algorithm. We do not claim novelty for supervised fine-tuning (SFT), replacing variable names with neutral labels, prompting with names but no numerical data, representation comparisons, or classical baselines individually. Our contribution is a matched attribution protocol: it changes variable names, numerical evidence, or representation one at a time while holding the remaining factors fixed. This identifies whether performance comes from names, data, representation, or their interaction—information that aggregate graph scores cannot provide. CausalMix supplies the datasets, simulator, prompt builders, classical baselines, and shared scoring needed to make these comparisons reproducible. We will center this contribution in the paper and present post-training as a case study of the benchmark, not as a new training method.

### W2 — Presentation and terminology

- A **semantic cue** is text associated with a variable, principally its original name and description. Anonymization replaces it with a neutral label such as `X1`.
- A **matched attribution test** changes one factor—names, numerical evidence, or representation—while fixing the graph, data realization and seed, observation/intervention budget, variable order, model settings, required input/output format, and scoring.

For example, a real-name/anonymized Sachs pair contains identical samples and differs only in labels such as Raf and Mek. Data/no-data pairs isolate responsiveness to numerical evidence; summary/tabular pairs compare statistical summaries with row-wise samples. We use **name-mediated support** to mean performance enabled by variable names, whether it comes from exact benchmark recall or broader knowledge about the named variables. Anonymization cannot distinguish those two sources. We will add these definitions and this matched example to the revised methodology.

### W3 — Treatment of semantic and numerical evidence

Our wording may have unintentionally implied a hierarchy between label semantics and data. We shall revise the text appropriately to address this unintended hierarchy. Semantic knowledge may encode expertise or misleading associations; numerical evidence depends on causal, sampling, and measurement assumptions. CausalMix measures both sources and does not treat data-only methods as gold standards. In controlled cells, all methods are scored against the known generating directed acyclic graph (DAG); the classical causal-discovery methods PC, GES, and ENCO are baselines under their assumptions. We will revise the relevant language.

### W4 — Dataset familiarity, age, and scale

The familiarity of Asia, Earthquake, Cancer, and Sachs increases the risk of pretraining recall. Their known DAGs and simulators nevertheless enable exact control over observational samples, intervention targets, samples per intervention, and seeds, allowing real-name and anonymized conditions to use identical data and budgets. We group both exact graph recall and general knowledge about named variables as **name-mediated support**, rather than data-driven causal discovery.

To address limited graph scale and possible reliance on familiar benchmark structures, we added 20–37-node evaluations, including the newly generated Chain-25 and Jungle-25, plus degree-preserving rewired Child variants that place familiar names in conflict with new numerical evidence. These controls probe, but cannot eliminate, pretraining familiarity.

For these evaluations, `Base` denotes Qwen3-4B-Thinking-2507 before our fine-tuning. **Format SFT** trains the model to produce structured responses containing gold graph targets; **reasoning SFT** then continues supervised training with gold answers, concise rationales, and teacher-generated rationales. We evaluated the model after both SFT phases on 20 paired realizations per comparison: 4 fixed column-order seeds × 5 independent data draws per order. Within each realization, the same draw is shared across label conditions. The table reports data-present results after format SFT + reasoning SFT. **Validity** is the percentage of outputs satisfying the structured-output contract and parseable at the expected graph size; acyclicity is evaluated separately. Directed-edge **F1** measures recovery of the true directed edges, with invalid outputs assigned F1=0. Child and Alarm are established 20- and 37-node graphs; Chain-25 is a directed chain; and Jungle-25 is a denser hierarchical DAG with connections spanning up to two levels.

Because an edge list represents a set, exact repeated copies of the same directed edge are collapsed before scoring; they neither add an edge nor invalidate the graph. Self-loops, unknown endpoints, malformed edges, and truncations remain invalid. All values below use this scoring rule and aggregate 20 runs per cell. ENCO uses the same 20 datasets and numerical budget (1,000 observational samples and 10 samples per intervention):

**Larger-graph recovery.**

| Graph (nodes) | ENCO F1 | LLM validity: original labels (%) | LLM F1: original labels | LLM validity: anonymous labels (%) | LLM F1: anonymous labels |
|---|---:|---:|---:|---:|---:|
| Child (20) | 0.652 | 100 | 0.070 | 100 | 0.104 |
| Chain-25 | 0.609 | 90 | 0.049 | 100 | 0.069 |
| Jungle-25 | 0.703 | 95 | 0.044 | 100 | 0.095 |
| Alarm (37) | 0.677 | 60 | 0.022 | 90 | 0.031 |

Recovery remains weak despite substantially higher parse validity. The corresponding data-use gain (F1 with data minus F1 without data, invalid outputs scored 0) is:

**Data-use gain.**

| Graph (nodes) | Original labels: ΔF1 from adding data | Anonymous labels: ΔF1 from adding data |
|---|---:|---:|
| Child (20) | 0.000 | +0.005 |
| Chain-25 | 0.000 | 0.000 |
| Jungle-25 | +0.003 | 0.000 |
| Alarm (37) | −0.133 | 0.000 |

**Takeaway:** ENCO reaches F1=0.609–0.703, confirming that the simulated numerical signal is recoverable under a matched data budget. The LLM remains at 0.022–0.104 despite 90–100% validity on most cells, and its data-use gain is approximately zero except for a negative original-label effect on Alarm; that effect partly reflects Alarm's lower 60% data-present validity because invalid outputs receive zero. Chain-25 and Jungle-25 are synthetic categorical graphs with neutral variable labels. We also created five Child variants that retain the original names and each variable's in- and out-degree while replacing 12–16 of the graph's 25 edges and preserving acyclicity. Because data come from the rewired graphs, these tests place familiar names in conflict with numerical evidence. These controls test unfamiliar structures and name–data conflict, but cannot rule out all pretraining effects or replace evaluation in a new scientific domain. We therefore limit our conclusions accordingly.

Thank you again for the thoughtful feedback. We will use these suggestions to clarify CausalMix’s contribution, terminology, treatment of semantic and numerical evidence, and empirical scope in the revision.

<!-- COPY END: svky -->

---

## Reviewer mUVt

<!-- COPY START: mUVt -->

We appreciate the reviewer’s positive assessment and concrete suggestions. We completed strict held-out-family training, a 30-realization replication, and tests on larger and newly generated graphs. The held-out results show transferable output reliability but mixed numerical-evidence gains, while larger-graph recovery remains weak; we narrow the claims accordingly.

### W1 / Q1 — Post-training ablation and held-out-graph evaluation

We used strict leave-one-graph-out (LOGO) training, so train and test graph families do not overlap: each graph is evaluated only by models whose training data exclude it. `Base` denotes Qwen3-4B-Thinking-2507 before our fine-tuning. **Format supervised fine-tuning (format SFT)** first trains the model to produce structured responses containing gold graph targets. Starting from that checkpoint, **reasoning SFT** continues training with gold answers and rationales. Each of Cancer, Earthquake, Asia, and Sachs was removed from both phases, and every fold was trained with seeds 42, 314, and 2718.

**Output validity** is the percentage of responses satisfying the structured-output contract and parseable at the expected graph size; acyclicity is evaluated separately. Exact repeats of the same directed edge are collapsed before scoring. Aggregate validity averages the real-name, anonymous (neutral-label), data, and no-data conditions:

**LOGO output validity.**

| Held-out graph | Base validity (%) | After format SFT: validity (%) | After reasoning SFT: validity (%) |
|---|---:|---:|---:|
| Asia | 71.3 | 100 | 100 |
| Cancer | 86.3 | 100 | 100 |
| Earthquake | 76.3 | 100 | 100 |
| Sachs | 73.8 | 100 | 100 |

Directed-edge **F1** measures recovery of the true directed edges, with invalid outputs assigned F1=0. We define **data-use gain** as `F1(with data) − F1(without data)`. The next table reports how reasoning SFT changed this gain relative to format SFT, with 95% bootstrap CIs over 60 paired predictions per graph and naming condition: 20 data pairs (4 fixed column-order seeds × 5 independent data draws per order) × 3 training seeds.

**Reasoning-SFT effect.**

| Held-out graph | Original names: change in data-use gain [95% CI] | Anonymous labels: change in data-use gain [95% CI] |
|---|---:|---:|
| Asia | −0.071 [−0.116, −0.028] | +0.028 [+0.003, +0.054] |
| Cancer | +0.003 [−0.056, +0.061] | +0.004 [−0.056, +0.069] |
| Earthquake | −0.025 [−0.074, +0.027] | +0.037 [+0.012, +0.064] |
| Sachs | −0.036 [−0.067, −0.008] | +0.018 [−0.013, +0.049] |

**Takeaway:** Format SFT transfers output reliability to held-out graph families, but reasoning SFT has mixed numerical-evidence effects. Because both phases use gold graph targets, content imitation remains possible. Together with the newly generated larger graphs in W3, this addresses training-family overlap and direct topology familiarity without claiming that pretraining familiarity is eliminated. We will report all fold-seed runs and present post-training as a diagnostic case study, not a new training contribution.

### W2 — Statistical power

We expanded the primary Sachs comparison from five to 30 paired realizations—independent data draws shared by the data and no-data conditions. Here, **data effect** means the data-use gain defined above. We report paired 95% bootstrap CIs and assign F1=0 to invalid outputs.

**Sachs replication.**

| Model | Original names: ΔF1 from adding data [95% CI] | Anonymous labels: ΔF1 from adding data [95% CI] |
|---|---:|---:|
| Base (Qwen3-4B-Thinking-2507) | −0.183 [−0.234, −0.130] | +0.230 [+0.207, +0.252] |
| GPT-5-mini | −0.087 [−0.104, −0.069] | +0.039 [+0.017, +0.062] |
| After format SFT | −0.091 [−0.143, −0.040] | +0.061 [+0.023, +0.100] |
| After format SFT + reasoning SFT | −0.089 [−0.168, −0.010] | +0.061 [+0.025, +0.096] |

**Takeaway:** All eight paired sign-flip tests have p ≤ 0.0403: adding data lowers F1 with real names but improves it anonymously in this Sachs slice. We treat the remaining five-realization cells as descriptive; this replication supports the prespecified contrast, not every benchmark cell.

### W3 / Q2 / Q3 — Graph familiarity and scale

**Familiarity.** Chain-25 is a 25-node directed chain; Jungle-25 is a denser 25-node hierarchical graph. Both were generated for this evaluation. We also created five variants of the 20-node Child directed acyclic graph (DAG) that retain its variable names and each node's in- and out-degree while changing 12–16 of its 25 edges. On disputed edges, where the canonical and rewired Child graphs differ, the model after format SFT + reasoning SFT followed the rewired graph on 45.0% and the canonical graph on 43.6% with real names; anonymously, the rates were 41.4% and 43.6%. Thus, the supplied data did not create a consistent preference for the rewired structure. We use **name-mediated support** to mean performance enabled by variable names, whether it comes from exact benchmark recall or broader knowledge; these tests cannot distinguish those sources. These controls test unfamiliar topologies, but they do not provide a newly constructed named scientific domain or rule out broader pretraining exposure.

**Scale.** We evaluated the same real-name/anonymous and data/no-data comparisons on four larger graphs with 20 paired realizations per contrast. Exact repeated copies of a directed edge are collapsed before scoring because edge lists represent sets; other contract failures remain invalid. All LLM values below use this scoring rule and aggregate 20 runs per cell. ENCO uses the same 20 datasets and numerical budget. The table reports data-present LLM results after format SFT + reasoning SFT:

**Larger-graph recovery.**

| Graph (nodes) | ENCO F1 | LLM validity: original labels (%) | LLM F1: original labels | LLM validity: anonymous labels (%) | LLM F1: anonymous labels |
|---|---:|---:|---:|---:|---:|
| Child (20) | 0.652 | 100 | 0.070 | 100 | 0.104 |
| Chain-25 | 0.609 | 90 | 0.049 | 100 | 0.069 |
| Jungle-25 | 0.703 | 95 | 0.044 | 100 | 0.095 |
| Alarm (37) | 0.677 | 60 | 0.022 | 90 | 0.031 |

We also report the data-use gain (F1 with data minus F1 without data) at these graphs, since that is the estimand our central claim rests on and it was missing from the table above:

**Data-use gain.**

| Graph (nodes) | Original labels: ΔF1 from adding data | Anonymous labels: ΔF1 from adding data |
|---|---:|---:|
| Child (20) | 0.000 | +0.005 |
| Chain-25 | 0.000 | 0.000 |
| Jungle-25 | +0.003 | 0.000 |
| Alarm (37) | −0.133 | 0.000 |

**Takeaway:** ENCO reaches F1=0.609–0.703 under the matched data budget, whereas LLM F1 remains 0.022–0.104 and its data-use gain is nearly zero except for a negative original-label Alarm effect. The latter partly reflects Alarm's 60% data-present validity because invalid outputs receive zero. High parse validity therefore does not imply successful recovery; these tests do not establish scalable LLM causal discovery.

Thank you again for the constructive suggestions. The requested experiments strengthen CausalMix’s evaluation under held-out, repeated, and larger-graph settings. We will incorporate them and clarify the scope of our conclusions in the revision.

<!-- COPY END: mUVt -->

---

## Reviewer zDep

<!-- COPY START: zDep -->

Thank you for recognizing the importance of separating variable-name and numerical evidence. We added larger/newly generated graphs, held-out post-training ablations, a 30-run replication, and edge-level analyses; they show transferable format reliability but weak and unstable numerical recovery.

### W1 / Q1 — Larger and more complex graphs

We ran the original/anonymous-label × data/no-data design on four 20–37-node graphs over 20 matched runs (4 column orders × 5 independent draws). The table reports data-present Qwen3-4B results after format and reasoning SFT. Validity means contract-compliant output at the expected graph size; acyclicity is separate, and invalid outputs receive F1=0. Exact duplicate edges are collapsed, while self-loops, unknown endpoints, malformed edges, and truncations remain invalid. All values below use this scoring rule and aggregate 20 runs per cell. ENCO uses the same 20 datasets and numerical budget.

**Larger-graph recovery.**

| Graph (nodes) | ENCO F1 | LLM validity: original labels (%) | LLM F1: original labels | LLM validity: anonymous labels (%) | LLM F1: anonymous labels |
|---|---:|---:|---:|---:|---:|
| Child (20) | 0.652 | 100 | 0.070 | 100 | 0.104 |
| Chain-25 | 0.609 | 90 | 0.049 | 100 | 0.069 |
| Jungle-25 | 0.703 | 95 | 0.044 | 100 | 0.095 |
| Alarm (37) | 0.677 | 60 | 0.022 | 90 | 0.031 |

This does not contrast data-present against no-data at these graphs, which is the estimand our central claim rests on. That contrast (data-use gain, invalid=0) is:

**Data-use gain.**

| Graph (nodes) | Original labels: ΔF1 from adding data | Anonymous labels: ΔF1 from adding data |
|---|---:|---:|
| Child (20) | 0.000 | +0.005 |
| Chain-25 | 0.000 | 0.000 |
| Jungle-25 | +0.003 | 0.000 |
| Alarm (37) | −0.133 | 0.000 |

**Takeaway:** ENCO reaches F1=0.609–0.703 under the matched budget, confirming recoverable numerical signal. Even at 90–100% LLM validity on most cells, F1 remains 0.022–0.104 and data-use gains are approximately zero except for a negative original-label Alarm effect; that effect partly reflects Alarm's 60% data-present validity because invalid outputs receive zero. We do not claim successful large-graph LLM recovery.

### W2 / Q2 — Post-training ablations and train–test independence

We agree that identifying the source of post-training gains requires explicit ablations. We therefore crossed three training stages—`Base`, format SFT, and format SFT + reasoning SFT—with a 2×2 information-source ablation: original versus anonymous labels and data versus no data. We evaluated this factorial design under leave-one-graph-out (LOGO) training, so every evaluated graph was excluded from both SFT phases. `Base` denotes Qwen3-4B-Thinking-2507 before our fine-tuning. Each of Cancer, Earthquake, Asia, and Sachs was held out in turn, and every fold was trained with seeds 42, 314, and 2718. Aggregate validity averages the four information-source conditions:

**LOGO output validity.**

| Held-out graph | Base validity (%) | After format SFT: validity (%) | After reasoning SFT: validity (%) |
|---|---:|---:|---:|
| Asia | 71.3 | 100 | 100 |
| Cancer | 86.3 | 100 | 100 |
| Earthquake | 76.3 | 100 | 100 |
| Sachs | 73.8 | 100 | 100 |

We define **data-use gain** as `F1(with data) − F1(without data)`. The next table reports how reasoning SFT changed this gain relative to format SFT, with 95% bootstrap CIs over 60 paired predictions per graph and naming condition: the 20 data pairs above × 3 training seeds.

**Reasoning-SFT effect.**

| Held-out graph | Original names: change in data-use gain [95% CI] | Anonymous labels: change in data-use gain [95% CI] |
|---|---:|---:|
| Asia | −0.071 [−0.116, −0.028] | +0.028 [+0.003, +0.054] |
| Cancer | +0.003 [−0.056, +0.061] | +0.004 [−0.056, +0.069] |
| Earthquake | −0.025 [−0.074, +0.027] | +0.037 [+0.012, +0.064] |
| Sachs | −0.036 [−0.067, −0.008] | +0.018 [−0.013, +0.049] |

**Ablation summary.**

| Hypothesis | Held-out evidence | What the evidence supports |
|---|---|---|
| Output-contract learning | LOGO validity increases from 71.3%–86.3% for Base to 100% after format SFT. | Supported for held-out-graph output validity. |
| Training-graph memorization | The validity gain persists when the evaluated graph is excluded from both SFT phases. | Not necessary for the validity gain. |
| Semantic dependence | In the LOGO names-only condition, F1 falls from 0.498 to 0.157 after anonymizing labels for format-SFT models and from 0.489 to 0.134 for reasoning-SFT models. | Strong name dependence; its pretraining source remains unresolved. |
| Numerical integration | Reasoning-SFT effects on data-use gain vary in sign across graphs and naming conditions. | Small and inconsistent. |
| Gold-target/content imitation | Both SFT phases contain gold graph targets; no schema-only control is available. | Unresolved. |

**Takeaway:** These ablations identify transferable output reliability as the most robust gain and rule out evaluated-family overlap as necessary for it, but do not show consistent numerical-integration improvement. The rewired-Child ablation in Q4 adds an adversarial name–data conflict. Because both SFT phases contain gold graph targets, format learning, content imitation, and pretraining familiarity are not fully separated; we do not attribute these supervised-checkpoint gains to verifier optimization or claim complete mechanistic identification.

### W3 / Q3 — Statistical and qualitative evidence

We expanded the primary Sachs comparison to 30 paired realizations—independent data draws shared by the data and no-data conditions. Here, **data effect** means the data-use gain defined above. We report paired 95% bootstrap CIs and assign F1=0 to invalid outputs.

**Sachs replication.**

| Model | Original names: ΔF1 from adding data [95% CI] | Anonymous labels: ΔF1 from adding data [95% CI] |
|---|---:|---:|
| Base (Qwen3-4B-Thinking-2507) | −0.183 [−0.234, −0.130] | +0.230 [+0.207, +0.252] |
| GPT-5-mini | −0.087 [−0.104, −0.069] | +0.039 [+0.017, +0.062] |
| After format SFT | −0.091 [−0.143, −0.040] | +0.061 [+0.023, +0.100] |
| After format SFT + reasoning SFT | −0.089 [−0.168, −0.010] | +0.061 [+0.025, +0.096] |

**Takeaway:** All eight paired sign-flip tests have p ≤ 0.0403: adding data lowers F1 with real names but improves it anonymously in this Sachs slice. We treat the remaining five-realization cells as descriptive.

At the edge level, a **correction** changes an incorrect prediction to the true edge state, while a **regression** changes a correct prediction to an incorrect state. Across 60 valid real-name Sachs LOGO pairs, data corrected Jnk–P38 and Erk–P38 36 times each, but regressed PIP2–PKA and Mek–Plcg 31 times each. These cases show how local corrections and regressions can offset each other in graph-level F1.

### Q4 — Direct numerical-evidence response

After format SFT + reasoning SFT, we compared predictions with and without data on every variable pair among paired-valid outputs. A **correction** changes an edge state toward the ground truth, a **regression** changes one away from it, and **net corrections** is their rate difference. “Scale suite” denotes Child, Chain-25, Jungle-25, and Alarm; LOGO is defined in W2/Q2.

**Edge-level response.**

| Evaluation condition | Usable pairs | Edge states changed (%) | Corrected (%) | Regressed (%) | Net corrections (pp) |
|---|---:|---:|---:|---:|---:|
| Scale suite, original labels | 69/80 | 18.7 | 5.7 | 11.4 | −5.7 |
| Scale suite, anonymous labels | 78/80 | 34.0 | 5.0 | 26.7 | −21.7 |
| Strict LOGO, original names | 240/240 | 28.2 | 11.4 | 15.0 | −3.6 |
| Strict LOGO, anonymous labels | 240/240 | 25.5 | 9.8 | 12.3 | −2.5 |
| Sachs, 30 realizations, anonymous labels | 30/30 | 42.2 | 10.8 | 25.5 | −14.7 |

**Takeaway:** As an adversarial ablation, five Child variants preserve names and node degrees while changing 12–16 of 25 edges. On disputed pairs, adding data reduced following of the rewired graph by 1.0 percentage point with real names and 7.9 anonymously, while increasing unresolved choices by 5.4 and 14.5 points. Across the broader paired-valid analyses, data change many local decisions but regressions often offset or exceed corrections. We therefore replace “models ignore data” with **partial local responsiveness with unstable numerical-evidence integration**. We do not interpret a shuffled-data placebo whose prompts exceeded the context limit.

### W4 / Q5 — Model coverage

The submission evaluates ten GPT-5/Llama/Qwen models, and the rebuttal adds a 30-run GPT-5-mini replication. During the rebuttal period, we also completed interim 10-run evaluations of Granite-3.2-8B and DeepSeek-V2-Lite-Chat (an actual DeepSeek-family checkpoint) on Child, Sachs, and larger graphs; 20-run replications are ongoing. We report these interim results only descriptively and do not use them for confidence intervals or small-effect claims. We will include the completed cross-family coverage, uncertainty estimates, and failure-mode analysis in the revised manuscript. Separately, three Ministral reasoning checkpoints produced 0% strict validity on the completed Child/Sachs grid, and preliminary DeepSeek-R1-Distill-Llama-8B outputs were invalid. We treat these as protocol-compatibility failures, not causal-capability evidence; main inferential conclusions remain scoped to GPT/Qwen/Llama.

### Formatting concern — Dataset access

On July 26, 2026, we verified through unauthenticated access that the dataset repository listed in the submission is public and ungated. We will make this access information explicit in the revised artifact documentation.

Thank you again for the thoughtful feedback. We will incorporate these results and scope clarifications in the revision.

<!-- COPY END: zDep -->
