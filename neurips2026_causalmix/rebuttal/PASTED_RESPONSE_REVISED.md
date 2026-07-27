Thank you for recognizing the importance of controlled attribution. We added larger and newly generated graphs, graph-disjoint post-training, a 30-realization replication, and edge-level diagnostics. The new results support the attribution protocol while narrowing our claims about scalability, post-training, and numerical-evidence use.

### 1 — Larger-graph evaluation

We evaluated the full matched design on Child (20 nodes), two newly generated 25-node graphs, and Alarm (37 nodes), using 20 realizations per condition and the `+ SFT + GRPO` checkpoint. Validity is the percentage of responses that can be parsed as causal graphs. F1 is averaged over all responses, with invalid outputs assigned F1 = 0. Exact duplicate edges are collapsed before scoring; self-loops and truncated outputs remain invalid.

| Graph | Real names: validity / F1 | Anonymous: validity / F1 |
|---|---:|---:|
| Child (20) | 100% / 0.070 | 100% / 0.104 |
| Chain-25 | 90% / 0.049 | 100% / 0.069 |
| Jungle-25 | 95% / 0.044 | 100% / 0.095 |
| Alarm (37) | 60% / 0.022 | 90% / 0.031 |

These are negative capability results: structured-output validity remains moderate to high after deduplication, but graph-recovery F1 is low. The results extend the diagnostic beyond the submitted graphs; they do not demonstrate scalable LLM causal discovery. We will state this scope explicitly.

### 2 — Graph-disjoint post-training

To test whether post-training transfers beyond graphs seen during training, we performed strict leave-one-graph-out evaluation. Each of Cancer, Earthquake, Asia, and Sachs was excluded from both SFT and GRPO. Training restarted from Qwen3-4B-Thinking-2507, and each fold was repeated with seeds 42, 314, and 2718.

After SFT, every held-out fold reached 100% aggregate output validity, compared with 69%–86% for the Base model. Thus, SFT transfers the required output format to unseen graph families. However, GRPO did not consistently improve the model's use of numerical data beyond SFT.

The table below reports the additional effect of GRPO on the data gain, where data gain is F1 with data minus F1 without data. Positive values mean GRPO made data more helpful; negative values mean it made data less helpful.

| Held-out graph | Real names: GRPO effect [95% CI] | Anonymous: GRPO effect [95% CI] |
|---|---:|---:|
| Asia | −0.071 [−0.116, −0.028] | +0.028 [+0.003, +0.054] |
| Cancer | +0.003 [−0.056, +0.061] | +0.004 [−0.056, +0.069] |
| Earthquake | −0.025 [−0.074, +0.027] | +0.037 [+0.012, +0.064] |
| Sachs | −0.041 [−0.072, −0.011] | +0.018 [−0.013, +0.048] |

The graph-disjoint results support a more precise interpretation of the post-training experiment:

| Possible mechanism | Evidence | Conclusion |
|---|---|---|
| Learning the output contract | SFT yields 100% validity on held-out graphs | Supported |
| Memorizing a held-out graph during SFT | The test graph is absent from SFT and GRPO | Ruled out for that fold |
| Using semantic priors | Real-name and anonymous results differ | Still possible and measured by the benchmark |
| Learning verifier preferences or imitating graph targets | SFT uses gold graph targets | Still possible |
| Improving numerical-data use through GRPO | Effects are small, mixed, or negative | Not consistently supported |

We will therefore revise the claim: SFT transfers structured-output reliability, but the current GRPO recipe does not reliably improve numerical-evidence integration beyond SFT. Because SFT uses gold graph targets, it is not a schema-only control; content imitation and verifier preference remain possible. We will present post-training as a diagnostic application of CausalMix and release the split manifests, seeds, checkpoint comparisons, and paired intervals.

### 3 — Replication and uncertainty

Five realizations are insufficient for strong conclusions about small effects. We therefore expanded the prespecified Sachs comparison to 30 paired realizations and computed paired bootstrap confidence intervals and sign-flip tests.

The table reports the effect of adding numerical data: F1 with data minus F1 without data. Positive values mean data improved recovery; negative values mean it reduced recovery. Every response is included, with invalid outputs assigned F1 = 0.

| Model | Real names: data effect [95% CI] | Anonymous: data effect [95% CI] |
|---|---:|---:|
| Base | −0.190 [−0.240, −0.138] | +0.230 [+0.207, +0.253] |
| GPT-5-mini | −0.087 [−0.104, −0.069] | +0.039 [+0.017, +0.062] |
| `+ SFT` | −0.169 [−0.223, −0.110] | +0.061 [+0.022, +0.100] |
| `+ SFT + GRPO` | −0.240 [−0.297, −0.181] | +0.061 [+0.026, +0.096] |

The naming-condition pattern is stable: adding data helps when names are anonymized but hurts when real names are present. We will treat the remaining five-realization cells as descriptive rather than conclusive.

Edge-level inspection also reveals changes hidden by graph-level F1. Across 59 valid real-name Sachs leave-one-graph-out pairs:

| Edge | Corrections | Regressions |
|---|---:|---:|
| Jnk–P38 | 36 | 2 |
| Erk–P38 | 36 | 0 |
| PIP2–PKA | 0 | 30 |
| Mek–Plcg | 1 | 30 |
| Erk–PKA | 18 | 26 |

Thus, an unchanged or lower total F1 can conceal genuine local corrections that are offset by harmful changes elsewhere.

### 4 — Direct response to numerical evidence

The edge audit changes our interpretation. The models do respond to data locally, but harmful changes generally outnumber beneficial corrections. For each matched pair, “beneficial” means an incorrect edge became correct after adding data, while “harmful” means a correct edge became incorrect. Net benefit is the beneficial rate minus the harmful rate.

| Evaluation | Edges changed | Beneficial | Harmful | Net benefit |
|---|---:|---:|---:|---:|
| Graph-disjoint, real names | 25.3% | 4.8% | 18.5% | −13.6 pp |
| Graph-disjoint, anonymous | 37.7% | 5.4% | 29.8% | −24.4 pp |
| Leave-one-graph-out, real names | 28.1% | 11.4% | 15.0% | −3.6 pp |
| Leave-one-graph-out, anonymous | 25.5% | 9.8% | 12.3% | −2.5 pp |
| Sachs, 30 realizations, anonymous | 42.2% | 10.9% | 25.5% | −14.7 pp |

We also tested five degree-preserving rewired Child graphs, where familiar names and the supplied data support different structures. Adding data did not shift predictions consistently toward the rewired generating graph: data-following decreased by 3.0 percentage points with real names and increased by 2.0 points anonymously. Semantics-following increased by 0.5 and 4.0 points, respectively.

We will therefore replace “models ignore data” with the more accurate conclusion: **models show partial local responsiveness but integrate numerical evidence unstably and often detrimentally**. We will report changed-edge, beneficial-change, harmful-change, and net-benefit rates alongside graph-level F1.

We attempted shuffled-data controls, but the shuffled prompts exceeded the model's 131,072-token context limit. Because these runs were not valid matched comparisons, we will report the limitation rather than interpret them.

### 5 — Model coverage

The submission evaluates ten models from the GPT-5, Llama-3.1, Qwen2.5, and Qwen3 families, and the rebuttal adds 30 GPT-5-mini realizations. Our conclusions are limited to these evaluated model families.

Preliminary DeepSeek and Mistral checks were not reliable enough for quantitative comparison. DeepSeek-R1-Distill-Llama-8B produced self-loops in both data-present conditions of its second realization. Mistral-Nemo produced duplicate edges and self-loops in a names-only condition and raised a tokenizer warning. We will document these failure modes but exclude the runs from aggregate claims.

### 6 — Dataset access

We verified the dataset link from two unauthenticated networks and successfully downloaded the archive: [CausalMix dataset](https://huggingface.co/datasets/mixcausalbench/anonymous-data). In the revision, we will place this link prominently in the main text and add a file inventory, SHA-256 checksum, access date, and exact download and regeneration commands.

Overall, the additional experiments sharpen rather than broaden our conclusions: CausalMix supports controlled attribution of semantic and numerical evidence; SFT improves output reliability on unseen graph families; current GRPO does not consistently improve numerical-data use; and larger-graph recovery remains weak. We will revise the paper to reflect these narrower, evidence-supported claims.
