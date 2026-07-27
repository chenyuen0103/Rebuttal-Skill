Thank you for recognizing the importance of the controlled attribution question and for identifying the experiments needed to delimit the conclusions. We added larger matched graphs, strict graph-disjoint post-training, a 30-seed replication, and direct edge/conflict diagnostics. These results strengthen the benchmark contribution while requiring us to narrow the capability and post-training claims.

### 1. Larger matched evaluation

We agree that four 5–11-node graphs cannot support broad scale generalization. We added the full matched design on Child (20 nodes), two newly generated 25-node topologies, and Alarm (37 nodes), with 20 paired realizations per contrast. Under canonical controlled decoding, the principal data-bearing results for `+ SFT + GRPO` were:

| Graph | Real validity | Real F1 | Anonymous validity | Anonymous F1 |
|---|---:|---:|---:|---:|
| Child (20) | 100% | 0.070 | 100% | 0.104 |
| Chain-25 | 90% | 0.049 | 100% | 0.069 |
| Jungle-25 | 95% | 0.044 | 100% | 0.095 |
| Alarm (37) | 60% | 0.022 | 90% | 0.031 |

Exact duplicate edges are collapsed because they do not change the predicted graph. Self-loops, truncated responses, and other invalid outputs remain invalid with F1=0. These are negative capability results: they extend the diagnosis beyond the submitted graphs but do not establish successful large-graph recovery. Long numerical serialization and structured-output difficulty are limitations of the evaluated interface, so we report validity separately rather than interpreting invalid outputs as proof of causal inability. We will add the full matched conditions, validity counts, and prompt lengths to Section 5.1 and restrict conclusions to the evaluated models, representations, and graph sizes.

### 2. Graph-disjoint post-training and mechanism attribution

We ran strict leave-one-graph-out training: each of Cancer, Earthquake, Asia, and Sachs was excluded from both SFT and GRPO, training restarted from Qwen3-4B, and every fold was repeated with seeds 42, 314, and 2718. With `+ SFT`, aggregate validity reached 100% versus 69%–86% for Base. The GRPO increments over SFT data gain were:

| Held-out graph | Real names, mean [95% CI] | Anonymous, mean [95% CI] |
|---|---:|---:|
| Asia | −0.071 [−0.116, −0.028] | +0.028 [0.003, 0.054] |
| Cancer | +0.003 [−0.056, 0.061] | +0.004 [−0.056, 0.069] |
| Earthquake | −0.025 [−0.074, 0.027] | +0.037 [0.012, 0.064] |
| Sachs | −0.041 [−0.072, −0.011] | +0.018 [−0.013, 0.048] |

The mechanism diagnostics support the following narrower attribution:

| Candidate explanation | Diagnostic result | Conclusion |
|---|---|---|
| Output-contract learning | LOGO validity rises from 69%–86% for Base to 100% with `+ SFT`; `+ SFT + GRPO` remains near 100%. | Strongly supported. |
| SFT graph memorization | The validity gain persists when the held-out graph is absent from both stages. | Not necessary for the format gain. |
| Semantic/pretraining priors | LOGO names-only F1 is 0.498 real versus 0.157 anonymous with `+ SFT`, and 0.489 versus 0.134 with `+ SFT + GRPO`. | Strong name dependence; source unresolved. |
| Verifier preference | No verifier-free, target-matched checkpoint is available. | Still confounded with format/content imitation. |
| Numerical integration | Graph-disjoint GRPO increments over SFT range from −0.055 to +0.005. | Small and inconsistent. |

SFT contains gold graph targets and is therefore not a pure schema-only control. We cannot fully separate verifier preference or pretraining familiarity and will not claim otherwise. We will recast post-training as a diagnostic application that transfers output reliability, not as evidence of broadly improved numerical integration.

### 3. Repetitions, uncertainty, and qualitative cases

We expanded the primary Sachs comparison to 30 paired data realizations and computed paired bootstrap confidence intervals and sign-flip tests. The table includes every response: valid graphs receive their measured F1, while invalid outputs receive F1=0.

| Data minus no data | Real names, mean [95% CI] | Anonymous, mean [95% CI] |
|---|---:|---:|
| Base | −0.190 [−0.240, −0.138] | +0.230 [0.207, 0.253] |
| GPT-5-mini | −0.087 [−0.104, −0.069] | +0.039 [0.017, 0.062] |
| + SFT | −0.169 [−0.223, −0.110] | +0.061 [0.022, 0.100] |
| + SFT + GRPO | −0.240 [−0.297, −0.181] | +0.061 [0.026, 0.096] |

The naming-condition effect is stable: adding data lowers F1 with real names but improves F1 with anonymous names. We will not use five-run cells to support claims about small differences. In 59 valid real-name Sachs LOGO pairs, Jnk–P38 and Erk–P38 were beneficially corrected 36 times each, with 2 and 0 harmful regressions, whereas PIP2–PKA and Mek–Plcg regressed 30 times each, with 0 and 1 corrections. Erk–PKA exhibited both 18 corrections and 26 regressions. These cases illustrate the offsetting local behavior hidden by aggregate F1.

### 4. Direct evidence of numerical-evidence response

The edge-level audit changes our wording: the models do not ignore data. They respond locally, but harmful changes exceed beneficial corrections in nearly every evaluated slice. Among matched valid `+ SFT + GRPO` pairs:

| Evaluation | Edges changed | Beneficial | Harmful | Net benefit |
|---|---:|---:|---:|---:|
| Graph-disjoint, real | 25.3% | 4.8% | 18.5% | −13.6 pp |
| Graph-disjoint, anonymous | 37.7% | 5.4% | 29.8% | −24.4 pp |
| LOGO, real | 28.1% | 11.4% | 15.0% | −3.6 pp |
| LOGO, anonymous | 25.5% | 9.8% | 12.3% | −2.5 pp |
| Sachs 30-seed, anonymous | 42.2% | 10.9% | 25.5% | −14.7 pp |

A complementary conflict test on five rewired Child graphs reduced data-following by 1.6 pp with real names and 7.9 pp anonymously, while increasing unresolved choices by 6.0 and 14.5 pp. Thus, aggregate F1 hides partial local evidence use, but that use is unstable and frequently detrimental. We will replace “models ignore data” with **partial local responsiveness with unstable numerical-evidence integration**. We attempted an intact-versus-shuffled control, but all shuffled prompts exceeded the 131,072-token context limit; we will not interpret that invalid test.

### 5. Model coverage

The submitted evaluation contains ten models from GPT-5, Llama-3.1, Qwen2.5, and Qwen3, and the rebuttal adds a complete 30-seed GPT-5-mini replication. We agree that reliable Gemini, Claude, or frontier-DeepSeek results would strengthen external validity, but we did not obtain them and therefore cannot make a landscape-wide claim. Preliminary DeepSeek-R1-Distill-Llama-8B and Mistral-Nemo checks produced invalid causal graphs under the current inference setup, so we exclude them from quantitative comparisons. We will restrict all empirical conclusions to the evaluated GPT/Qwen/Llama slices and identify independent-family evaluation as future work.

### 6. Dataset access

Thank you for reporting the inaccessible URL. We verified the dataset on July 24, 2026, from two unauthenticated browsers on separate networks and downloaded the complete archive. The [dataset is available here](https://huggingface.co/datasets/mixcausalbench/anonymous-data). We will put this exact link in the paper and add a file inventory, archive SHA-256, access date, and minimal download/regeneration commands.

Overall, the added tests do not turn the negative capability result into a positive one. They establish the appropriate scope: CausalMix diagnoses format learning, name-mediated support, scaling failures, and unstable local responses that an aggregate graph score would conflate. We will make the benchmark and protocol contribution primary and remove unsupported claims of scalable or broadly transferable numerical integration.
