# Rebuttal to Reviewer zDep24

Thank you for recognizing the importance of controlled attribution and for identifying the experiments needed to clarify the scope of our conclusions. We added evaluations on larger and newly generated graphs, strict graph-disjoint post-training, a 30-realization replication, and direct edge-level and conflict diagnostics. The results strengthen the benchmark contribution while narrowing our claims about scalability and post-training.

### W1 / Q1 — Evaluation on larger and more complex graphs

The submitted 5–11-node results cannot establish generalization to larger graphs. We therefore ran the full matched design on Child (20 nodes), two newly generated 25-node topologies, and Alarm (37 nodes), with 20 paired realizations per contrast. The principal data-bearing results for `+ SFT + GRPO` were:

| Graph | Real-name validity | Real-name F1 | Anonymous validity | Anonymous F1 |
|---|---:|---:|---:|---:|
| Child (20) | 100% | 0.070 | 100% | 0.104 |
| Chain-25 | 90% | 0.049 | 100% | 0.069 |
| Jungle-25 | 95% | 0.044 | 100% | 0.095 |
| Alarm (37) | 60% | 0.022 | 90% | 0.031 |

F1 includes every response, with invalid outputs assigned F1 = 0. Exact duplicate edges are collapsed because they do not change the predicted graph; self-loops, truncations, and other malformed outputs remain invalid. These negative capability results extend the diagnosis beyond the submitted graphs, but do not demonstrate successful large-graph recovery. We will add the full matched conditions, validity counts, and prompt lengths to Section 5.1 and restrict our conclusions to the evaluated models, representations, and graph sizes.

### W2 / Q2 — Post-training attribution and train–test independence

We performed strict leave-one-graph-out training. Each of Cancer, Earthquake, Asia, and Sachs was excluded from both SFT and GRPO; training restarted from Qwen3-4B-Thinking-2507; and every fold was repeated with seeds 42, 314, and 2718. After SFT, aggregate validity reached 100% in every held-out fold, compared with 69%–86% for Base.

The table reports how much GRPO changed the benefit of adding data after SFT. Positive values mean GRPO made data more helpful; negative values mean it made data less helpful.

| Held-out graph | Real names: GRPO effect [95% CI] | Anonymous: GRPO effect [95% CI] |
|---|---:|---:|
| Asia | −0.071 [−0.116, −0.028] | +0.028 [+0.003, +0.054] |
| Cancer | +0.003 [−0.056, +0.061] | +0.004 [−0.056, +0.069] |
| Earthquake | −0.025 [−0.074, +0.027] | +0.037 [+0.012, +0.064] |
| Sachs | −0.041 [−0.072, −0.011] | +0.018 [−0.013, +0.048] |

The mechanism diagnostics support the following narrower attribution:

| Candidate explanation | Diagnostic result | Conclusion |
|---|---|---|
| Learning the output contract | Held-out validity rises from 69%–86% for Base to 100% after SFT | Strongly supported |
| Memorizing the held-out graph during SFT | The validity gain persists when that graph is absent from SFT and GRPO | Not necessary for the format gain |
| Using semantic or pretraining priors | Names-only F1 remains substantially higher with real names than anonymous names | Strong name dependence; source unresolved |
| Learning verifier preferences or imitating targets | SFT uses gold graph targets, and no verifier-free target-matched checkpoint is available | Still confounded |
| Improving numerical integration through GRPO | Effects range from −0.071 to +0.037 and vary by graph and naming condition | Not consistently supported |

We will therefore recast post-training as a diagnostic application: SFT transfers output reliability to unseen graph families, but the current GRPO recipe does not consistently improve numerical-evidence integration beyond SFT. We will add the split manifests, all fold-seed runs, checkpoint comparisons, and paired intervals.

### W3 / Q3 — Statistical strength and qualitative evidence

We expanded the primary Sachs comparison to 30 paired data realizations and computed paired bootstrap confidence intervals and sign-flip tests. The table reports the effect of adding data: F1 with data minus F1 without data. Positive values indicate improvement; negative values indicate degradation. Every response is included, with invalid outputs assigned F1 = 0.

| Model | Real names: data effect [95% CI] | Anonymous: data effect [95% CI] |
|---|---:|---:|
| Base | −0.190 [−0.240, −0.138] | +0.230 [+0.207, +0.253] |
| GPT-5-mini | −0.087 [−0.104, −0.069] | +0.039 [+0.017, +0.062] |
| `+ SFT` | −0.169 [−0.223, −0.110] | +0.061 [+0.022, +0.100] |
| `+ SFT + GRPO` | −0.240 [−0.297, −0.181] | +0.061 [+0.026, +0.096] |

The naming-condition pattern is stable: adding data lowers F1 with real names but improves it with anonymous names. We will not use the remaining five-realization cells to support claims about small differences.

Edge-level inspection also reveals behavior hidden by aggregate F1. Across 59 valid real-name Sachs leave-one-graph-out pairs, Jnk–P38 and Erk–P38 were corrected 36 times each, with 2 and 0 regressions; PIP2–PKA and Mek–Plcg regressed 30 times each, with 0 and 1 corrections; and Erk–PKA showed both 18 corrections and 26 regressions. Thus, local corrections can be offset by harmful changes elsewhere.

### Q4 — Direct evidence of numerical-evidence use

The edge-level audit changes our wording: the models do not ignore data. They respond locally, but harmful changes exceed beneficial corrections in nearly every evaluated slice. Among matched valid `+ SFT + GRPO` pairs:

| Evaluation | Edges changed | Beneficial | Harmful | Net benefit |
|---|---:|---:|---:|---:|
| Graph-disjoint, real names | 25.3% | 4.8% | 18.5% | −13.6 pp |
| Graph-disjoint, anonymous | 37.7% | 5.4% | 29.8% | −24.4 pp |
| Leave-one-graph-out, real names | 28.1% | 11.4% | 15.0% | −3.6 pp |
| Leave-one-graph-out, anonymous | 25.5% | 9.8% | 12.3% | −2.5 pp |
| Sachs, 30 realizations, anonymous | 42.2% | 10.9% | 25.5% | −14.7 pp |

A complementary conflict test on five rewired Child graphs reduced data-following by 1.6 percentage points with real names and 7.9 points anonymously, while increasing unresolved choices by 6.0 and 14.5 points. Thus, aggregate F1 hides partial local evidence use, but that use is unstable and frequently detrimental. We will replace “models ignore data” with **partial local responsiveness with unstable numerical-evidence integration** and report edge-change, correction, regression, and net-benefit rates alongside graph-level F1.

We also attempted an intact-versus-shuffled control, but all shuffled prompts exceeded the 131,072-token context limit. Because this did not produce a valid matched comparison, we will report the limitation without interpreting the test.

### W4 / Q5 — Model-family coverage

The submitted evaluation covers ten models from GPT-5, Llama-3.1, Qwen2.5, and Qwen3, and the rebuttal adds a 30-realization GPT-5-mini replication. We also conducted preliminary matched Sachs checks on two additional models. DeepSeek-R1-Distill-Llama-8B produced empty graphs from variable names alone and self-loops in both data-present cells under one tested variable ordering. Mistral-Nemo produced repeated edges and self-loops in its real-name, names-only output.

These checks were incomplete, so we exclude them from quantitative comparisons and treat them as output-contract failures under the current interface, not evidence of general causal-discovery inability. We therefore scope our conclusions to the evaluated GPT, Qwen, and Llama models. The released model adapter will allow additional families to be evaluated under the same matched protocol.

### F1 — Dataset URL and access

Thank you for reporting the inaccessible URL. We verified unauthenticated access to the public dataset repository and confirmed that its files, including the largest artifacts, resolve to downloadable URLs. The dataset is available at [https://huggingface.co/datasets/mixcausalbench/anonymous-data](https://huggingface.co/datasets/mixcausalbench/anonymous-data). We will place this exact link prominently in the paper and add a file inventory, checksums, access date, and exact download and regeneration commands.
