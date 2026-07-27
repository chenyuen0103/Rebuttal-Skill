# Paste-ready OpenReview responses

Copy only the text between the `COPY` markers into the corresponding OpenReview reply. The Area Chair’s concerns are addressed across the three reviewer responses. Each response is below 10,000 characters.

---

## Reviewer svky

<!-- COPY START: svky -->

Thank you for recognizing the value of separately evaluating semantic and statistical evidence. We will clarify the paper’s contribution, terminology, and scope.

### W1 — Novelty

CausalMix is an evaluation and dataset contribution, not a new causal-discovery algorithm. We do not claim novelty for supervised fine-tuning (SFT), replacing variable names with neutral labels, prompting with names but no numerical data, representation comparisons, or classical baselines individually. Our contribution is a matched attribution protocol: it changes variable names, numerical evidence, or representation one at a time while holding the remaining factors fixed. This identifies whether performance comes from names, data, representation, or their interaction—information that aggregate graph scores cannot provide. CausalMix supplies the datasets, simulator, prompt builders, classical baselines, and shared scoring needed to make these comparisons reproducible. We will center this contribution in the paper and present post-training as a case study of the benchmark, not as a new training method.

### W2 — Presentation and terminology

- A **semantic cue** is text associated with a variable, principally its original name and description. Anonymization replaces it with a neutral label such as `X1`.
- A **matched attribution test** changes one factor—names, numerical evidence, or representation—while fixing the graph, data realization and seed, observation/intervention budget, variable order, model settings, required input/output format, and scoring.

For example, a real-name/anonymized Sachs pair contains identical samples and differs only in labels such as Raf and Mek. Data/no-data pairs isolate responsiveness to numerical evidence; summary/tabular pairs compare statistical summaries with row-wise samples. We use **name-mediated support** to mean performance enabled by variable names, whether it comes from exact benchmark recall or broader knowledge about the named variables. Anonymization cannot distinguish those two sources.

### W3 — Treatment of semantic and numerical evidence

Our wording may have implied an unintended hierarchy. Semantic knowledge may encode expertise or misleading associations; numerical evidence depends on causal, sampling, and measurement assumptions. CausalMix measures both sources and does not treat data-only methods as gold standards. In controlled cells, all methods are scored against the known generating directed acyclic graph (DAG); the classical causal-discovery methods PC, GES, and ENCO are baselines under their assumptions. We will revise the relevant language.

### W4 — Dataset familiarity, age, and scale

The familiarity of Asia, Earthquake, Cancer, and Sachs increases the risk of pretraining recall. Their known DAGs and simulators nevertheless enable exact control over observational samples, intervention targets, samples per intervention, and seeds, allowing real-name and anonymized conditions to use identical data and budgets. We group both exact graph recall and general knowledge about named variables as **name-mediated support**, rather than data-driven causal discovery.

For the added evaluations, `Base` denotes Qwen3-4B-Thinking-2507 before our fine-tuning. **Format SFT** trains the model to produce structured responses containing gold graph targets; **reasoning SFT** then continues supervised training with gold answers, concise rationales, and teacher-generated rationales. We evaluated the model after both SFT phases on 20 paired realizations—independent data draws shared across label conditions—per comparison. The table reports data-present results after format SFT + reasoning SFT. **Validity** is the percentage of outputs that can be parsed as acyclic graphs; directed-edge **F1** measures recovery of the true directed edges, with invalid outputs assigned F1=0. Child and Alarm are established 20- and 37-node graphs; Chain-25 is a directed chain; and Jungle-25 is a denser hierarchical DAG with connections spanning up to two levels.

**Correction:** an earlier version of this table contained transcription errors in several cells. The values below are regenerated directly from our analysis output (`condition_summary.csv`; filter: `suite=graph_disjoint, protocol=outedges_json_guided, model=stage2, condition=data`, n=20 paired realizations per cell), reproducible with that exact filter:

| Graph | Original labels: validity / F1 | Anonymous labels: validity / F1 |
|---|---:|---:|
| Child (20) | 40% / 0.035 | 100% / 0.104 |
| Chain-25 | 60% / 0.038 | 100% / 0.069 |
| Jungle-25 | 65% / 0.044 | 100% / 0.095 |
| Alarm (37) | 5% / 0.002 | 50% / 0.026 |

Recovery is weaker than we previously reported, particularly with original labels; this reinforces rather than weakens the scope limitation below. The corresponding data-use gain (F1 with data minus F1 without data, invalid outputs scored 0) is:

| Graph | Original-label gain | Anonymous-label gain |
|---|---:|---:|
| Child (20) | −0.093 | +0.043 |
| Chain-25 | −0.015 | +0.003 |
| Jungle-25 | +0.014 | +0.058 |
| Alarm (37) | −0.097 | −0.005 |

Self-loops, truncations, and malformed outputs remain invalid and receive F1=0. Recovery remains weak on all four graphs, and part of the original-label shortfall reflects validity collapse under data-bearing prompts (e.g., Child original-label validity falls from 100% without data to 40% with data) rather than only weaker reasoning; we do not conflate the two. Chain-25 and Jungle-25 are synthetic categorical graphs with neutral variable labels. We also created five Child variants that retain the original names and each variable's in- and out-degree while replacing 12–16 of the graph's 25 edges and preserving acyclicity. Because data come from the rewired graphs, these tests place familiar names in conflict with numerical evidence. These controls test unfamiliar structures and name–data conflict, but cannot rule out all pretraining effects or replace evaluation in a new scientific domain. We therefore limit our conclusions accordingly. The released runner supports adding further graphs and simulators under the same controls.

Thank you again for the thoughtful feedback. We will use these suggestions to clarify CausalMix’s contribution, terminology, treatment of semantic and numerical evidence, and empirical scope in the revision.

<!-- COPY END: svky -->

---

## Reviewer mUVt

<!-- COPY START: mUVt -->

We appreciate the reviewer’s positive assessment and concrete suggestions. We completed evaluations that hold out an entire graph family, replicate the primary comparison, and test larger and newly generated graphs.

### W1 / Q1 — Post-training claim and held-out-graph evaluation

We used strict leave-one-graph-out (LOGO) training: each graph is evaluated only by models whose training data exclude that graph. `Base` denotes Qwen3-4B-Thinking-2507 before our fine-tuning. **Format supervised fine-tuning (format SFT)** first trains the model to produce structured responses containing gold graph targets. Starting from the resulting format-SFT checkpoint, **reasoning SFT** then continues supervised training with gold answers, concise rationales, and teacher-generated rationales. Each of Cancer, Earthquake, Asia, and Sachs was removed from both SFT phases, and each held-out-graph run was trained with seeds 42, 314, and 2718.

**Output validity** is the percentage of responses that can be parsed as acyclic graphs. Aggregate validity averages the real-name, anonymous (neutral-label), data, and no-data conditions:

| Held-out graph | Base | After format SFT | After format SFT + reasoning SFT |
|---|---:|---:|---:|
| Asia | 68.8% | 100% | 100% |
| Cancer | 86.3% | 100% | 100% |
| Earthquake | 76.3% | 100% | 100% |
| Sachs | 71.3% | 100% | 99.6% |

Directed-edge **F1** measures recovery of the true directed edges, with invalid outputs assigned F1=0. We define **data-use gain** as `F1(with data) − F1(without data)`. The next table reports how reasoning SFT changed this gain relative to format SFT, with 95% bootstrap confidence intervals (CIs):

| Held-out graph | Real names: reasoning SFT effect [95% CI] | Anonymous: reasoning SFT effect [95% CI] |
|---|---:|---:|
| Asia | −0.071 [−0.116, −0.028] | +0.028 [+0.003, +0.054] |
| Cancer | +0.003 [−0.056, +0.061] | +0.004 [−0.056, +0.069] |
| Earthquake | −0.025 [−0.074, +0.027] | +0.037 [+0.012, +0.064] |
| Sachs | −0.041 [−0.072, −0.011] | +0.018 [−0.013, +0.048] |

Format SFT therefore transfers output reliability to held-out graph families. The effects of reasoning SFT on numerical-evidence integration are mixed and do not establish consistent improvement. Because both phases use gold graph targets, content imitation remains possible. We will report all fold-seed runs and present these SFT experiments as a case study showing how CausalMix separates output reliability from numerical-evidence use, not as a new training contribution.

### W2 — Statistical power

We expanded the primary Sachs comparison from five to 30 paired realizations—independent data draws shared by the data and no-data conditions. Here, **data effect** means the data-use gain defined above. We report paired 95% bootstrap CIs and assign F1=0 to invalid outputs.

| Model | Real names: data effect [95% CI] | Anonymous: data effect [95% CI] |
|---|---:|---:|
| Base (Qwen3-4B-Thinking-2507) | −0.190 [−0.240, −0.138] | +0.230 [+0.207, +0.253] |
| GPT-5-mini | −0.087 [−0.104, −0.069] | +0.039 [+0.017, +0.062] |
| After format SFT | −0.169 [−0.223, −0.110] | +0.061 [+0.022, +0.100] |
| After format SFT + reasoning SFT | −0.240 [−0.297, −0.181] | +0.061 [+0.026, +0.096] |

All eight paired sign-flip randomization tests, which test whether the paired effects are centered at zero, have p ≤ 0.0055. Adding data consistently lowers F1 with real names but improves it with anonymous names. We will treat the remaining five-realization cells as descriptive; this replication supports the selected Sachs contrast, not every benchmark cell.

### W3 / Q2 / Q3 — Graph familiarity and scale

**Familiarity.** Chain-25 is a 25-node directed chain; Jungle-25 is a denser 25-node hierarchical graph. Both were generated for this evaluation. We also created five variants of the 20-node Child directed acyclic graph (DAG) that retain its variable names and each node's in- and out-degree while changing 12–16 of its 25 edges. On **disputed edges**, where the canonical and rewired Child graphs differ, the model after format SFT + reasoning SFT followed the rewired graph on 44.6% and the canonical graph on 43.6% with real names; anonymously, the rates were 41.4% and 43.6%. Thus, the supplied data did not create a consistent preference for the rewired structure. We use **name-mediated support** to mean performance enabled by variable names, whether it comes from exact benchmark recall or broader knowledge; these tests cannot distinguish those sources. These controls test unfamiliar topologies, but they do not provide a newly constructed named scientific domain or rule out broader pretraining exposure.

**Scale.** We evaluated the same real-name/anonymous and data/no-data comparisons on four larger graphs with 20 paired realizations per contrast. **Correction:** an earlier version of this table contained transcription errors; the values below are regenerated directly from `condition_summary.csv` (filter: `suite=graph_disjoint, protocol=outedges_json_guided, model=stage2, condition=data`, n=20), reproducible with that exact filter. The table reports data-present results after format SFT + reasoning SFT:

| Graph | Original labels: validity / F1 | Anonymous labels: validity / F1 |
|---|---:|---:|
| Child (20) | 40% / 0.035 | 100% / 0.104 |
| Chain-25 | 60% / 0.038 | 100% / 0.069 |
| Jungle-25 | 65% / 0.044 | 100% / 0.095 |
| Alarm (37) | 5% / 0.002 | 50% / 0.026 |

We also report the data-use gain (F1 with data minus F1 without data) at these graphs, since that is the estimand our central claim rests on and it was missing from the table above:

| Graph | Original-label gain | Anonymous-label gain |
|---|---:|---:|
| Child (20) | −0.093 | +0.043 |
| Chain-25 | −0.015 | +0.003 |
| Jungle-25 | +0.014 | +0.058 |
| Alarm (37) | −0.097 | −0.005 |

Invalid outputs, including self-loops and truncations, receive F1=0. The benchmark exposes low recovery and residual output failures at larger scales, and part of the original-label shortfall is validity collapse under data-bearing prompts rather than only weaker reasoning (e.g., Child original-label validity falls from 100% without data to 40% with data); it does not establish scalable LLM causal discovery.

Thank you again for the constructive suggestions. The requested experiments strengthen CausalMix’s evaluation under held-out, repeated, and larger-graph settings. We will incorporate them and clarify the scope of our conclusions in the revision.

<!-- COPY END: mUVt -->

---

## Reviewer zDep

<!-- COPY START: zDep -->

Thank you for recognizing the importance of separating performance derived from variable names from performance derived from numerical evidence. We added larger and newly generated graphs, training that excludes the evaluated graph family, a 30-realization replication, and direct edge-level analyses.

### W1 / Q1 — Larger and more complex graphs

We ran the matched original/anonymous-label and data/no-data design on four larger graphs with 20 paired realizations per contrast. We evaluated Qwen3-4B after format SFT, which uses structured responses with gold graphs, and a reasoning-SFT continuation with gold answers and rationales. The table reports data-present results after both phases. **Validity** is the percentage of parseable acyclic graphs; directed-edge **F1** scores true directed edges, with invalid outputs assigned F1=0. Child and Alarm have 20 and 37 nodes; Chain-25 and Jungle-25 are newly generated sparse and hierarchical DAGs.

**Correction:** these values fix earlier transcription errors and come directly from `condition_summary.csv` (`suite=graph_disjoint`, `protocol=outedges_json_guided`, `model=stage2`, `condition=data`, n=20):

| Graph | Original labels: validity / F1 | Anonymous labels: validity / F1 |
|---|---:|---:|
| Child (20) | 40% / 0.035 | 100% / 0.104 |
| Chain-25 | 60% / 0.038 | 100% / 0.069 |
| Jungle-25 | 65% / 0.044 | 100% / 0.095 |
| Alarm (37) | 5% / 0.002 | 50% / 0.026 |

This does not contrast data-present against no-data at these graphs, which is the estimand our central claim rests on. That contrast (data-use gain, invalid=0) is:

| Graph | Original-label gain | Anonymous-label gain |
|---|---:|---:|
| Child (20) | −0.093 | +0.043 |
| Chain-25 | −0.015 | +0.003 |
| Jungle-25 | +0.014 | +0.058 |
| Alarm (37) | −0.097 | −0.005 |

Self-loops, truncations, and malformed outputs remain invalid. Recovery is weak, and some original-label losses reflect validity collapse under data-bearing prompts (e.g., Child falls from 100% without data to 40% with data). With F1 near the floor (0.002–0.104), we treat these deltas as directional, not precise, and do not claim successful large-graph recovery.

### W2 / Q2 — Post-training ablations and train–test independence

We agree that identifying the source of post-training gains requires explicit ablations. We therefore crossed three training stages—`Base`, format SFT, and format SFT + reasoning SFT—with a 2×2 information-source ablation: original versus anonymous labels and data versus no data. We evaluated this factorial design under leave-one-graph-out (LOGO) training, so every evaluated graph was excluded from both SFT phases. `Base` denotes Qwen3-4B-Thinking-2507 before our fine-tuning. Each of Cancer, Earthquake, Asia, and Sachs was held out in turn, and every fold was trained with seeds 42, 314, and 2718. Aggregate validity averages the four information-source conditions:

| Held-out graph | Base | After format SFT | After format SFT + reasoning SFT |
|---|---:|---:|---:|
| Asia | 68.8% | 100% | 100% |
| Cancer | 86.3% | 100% | 100% |
| Earthquake | 76.3% | 100% | 100% |
| Sachs | 71.3% | 100% | 99.6% |

We define **data-use gain** as `F1(with data) − F1(without data)`. The next table reports how reasoning SFT changed this gain relative to format SFT, with 95% bootstrap confidence intervals (CIs):

| Held-out graph | Real names: reasoning SFT effect [95% CI] | Anonymous: reasoning SFT effect [95% CI] |
|---|---:|---:|
| Asia | −0.071 [−0.116, −0.028] | +0.028 [+0.003, +0.054] |
| Cancer | +0.003 [−0.056, +0.061] | +0.004 [−0.056, +0.069] |
| Earthquake | −0.025 [−0.074, +0.027] | +0.037 [+0.012, +0.064] |
| Sachs | −0.041 [−0.072, −0.011] | +0.018 [−0.013, +0.048] |

| Candidate explanation | Ablation result | Conclusion |
|---|---|---|
| Output-contract learning | LOGO validity increases from 68.8%–86.3% for Base to 100% after format SFT. | Strongly supported. |
| Training-graph memorization | The validity gain persists when the evaluated graph is excluded from both SFT phases. | Not necessary for the validity gain. |
| Semantic dependence | LOGO names-only F1 is 0.498 versus 0.157 after format SFT and 0.489 versus 0.134 after reasoning SFT, original versus anonymous labels. | Strong name dependence; its pretraining source remains unresolved. |
| Numerical integration | Reasoning-SFT effects on data-use gain vary in sign across graphs and naming conditions. | Small and inconsistent. |

Taken together, these ablations identify transferable output reliability as the most robust post-training gain and rule out overlap with the evaluated graph family as necessary for that gain. They do not show consistent improvement in numerical-evidence integration. As an adversarial complement, the topology-preserving rewired-Child ablation in Q4 tests whether predictions follow supplied data when familiar names conflict with a new generating graph. Because both SFT phases contain gold graph targets, these ablations cannot fully separate format learning from content imitation or pretraining familiarity. The evaluated checkpoints support claims about the supervised phases only; we do not attribute their gains to verifier-based optimization or claim complete mechanistic identification.

### W3 / Q3 — Statistical and qualitative evidence

We expanded the primary Sachs comparison to 30 paired realizations—independent data draws shared by the data and no-data conditions. Here, **data effect** means the data-use gain defined above. We report paired 95% bootstrap CIs and assign F1=0 to invalid outputs.

| Model | Real names: data effect [95% CI] | Anonymous: data effect [95% CI] |
|---|---:|---:|
| Base (Qwen3-4B-Thinking-2507) | −0.190 [−0.240, −0.138] | +0.230 [+0.207, +0.253] |
| GPT-5-mini | −0.087 [−0.104, −0.069] | +0.039 [+0.017, +0.062] |
| After format SFT | −0.169 [−0.223, −0.110] | +0.061 [+0.022, +0.100] |
| After format SFT + reasoning SFT | −0.240 [−0.297, −0.181] | +0.061 [+0.026, +0.096] |

All eight paired sign-flip randomization tests, which test whether paired effects are centered at zero, have p ≤ 0.0055. Adding data consistently lowers F1 with real names but improves it with anonymous names. We will treat the remaining five-realization cells as descriptive.

At the edge level, a **correction** changes an incorrect prediction to the true edge state, while a **regression** changes a correct prediction to an incorrect state. Across 59 valid real-name Sachs LOGO pairs, data corrected Jnk–P38 and Erk–P38 36 times each, but regressed PIP2–PKA and Mek–Plcg 30 times each. These cases show how local corrections and regressions can offset each other in graph-level F1.

### Q4 — Direct numerical-evidence response

After format SFT + reasoning SFT, we compared predictions with and without data on every variable pair. A **beneficial** change corrects an edge state, a **harmful** change breaks one, and **net benefit** is their rate difference. “Larger unseen graphs” denotes Child, Chain-25, Jungle-25, and Alarm; LOGO is defined in W2/Q2.

| Evaluation | Edges changed | Beneficial | Harmful | Net benefit (percentage points) |
|---|---:|---:|---:|---:|
| Larger unseen graphs, original labels | 25.3% | 4.8% | 18.5% | −13.6 |
| Larger unseen graphs, anonymous | 37.7% | 5.4% | 29.8% | −24.4 |
| Strict leave-one-graph-out, real names | 28.1% | 11.4% | 15.0% | −3.6 |
| Strict leave-one-graph-out, anonymous | 25.5% | 9.8% | 12.3% | −2.5 |
| Sachs, 30 realizations, anonymous | 42.2% | 10.9% | 25.5% | −14.7 |

As an adversarial ablation, five Child variants preserve names and node degrees while changing 12–16 of 25 edges. On disputed pairs, adding data reduced following of the rewired graph by 1.6 percentage points with real names and 7.9 anonymously, while increasing unresolved choices by 6.0 and 14.5 points. We therefore replace “models ignore data” with **partial local responsiveness with unstable numerical-evidence integration**. We do not interpret a shuffled-data placebo whose prompts exceeded the context limit.

### W4 / Q5 — Model coverage

The submission evaluates ten models from GPT-5, Llama-3.1, Qwen2.5, and Qwen3; the rebuttal adds a 30-realization GPT-5-mini replication. Additional family checks were not quantitatively usable: three Ministral reasoning checkpoints produced 0% strict validity across the Child/Sachs 2×2 grid, Mistral-Small-3.1-24B failed to load because of a package-version incompatibility, and preliminary DeepSeek-R1-Distill-Llama-8B outputs contained invalid graphs. These are interface failures, not evidence about causal ability. The DeepSeek checkpoint is also Llama-distilled, not independent DeepSeek coverage. We therefore scope reliable conclusions to GPT/Qwen/Llama and do not claim landscape-wide generality.

### Formatting concern — Dataset access

On July 26, 2026, an unauthenticated request to [the dataset repository](https://huggingface.co/datasets/mixcausalbench/anonymous-data) returned HTTP 200. It is public and ungated at commit `63988a99f9f6bdc97a7dd2aa1ef26b1716a44b6a` with 33 files. We will add the commit and directory manifest to the artifact documentation.

Thank you again for the thoughtful feedback. The experiments you suggested provide stronger evidence for CausalMix’s central contribution by revealing how numerical evidence affects local predictions and how attribution patterns change with graph scale. We will incorporate these results and clarify their scope in the revision.

<!-- COPY END: zDep -->
