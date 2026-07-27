# Issue board

## Interpretation legend

- **Direct:** stated explicitly by a reviewer.
- **Inferred:** likely underlying concern, strongly supported by wording.
- **UNCERTAIN:** plausible interpretation that should not be presented as reviewer intent without confirmation.

## Global themes

1. **External validity of the core benchmark finding:** four small, familiar graphs do not establish that semantic dominance or weak numerical-evidence use generalizes.
2. **Internal validity of post-training:** graph-family overlap makes the SFT gains compatible with memorization, format learning, or verifier preference learning.
3. **Statistical reliability:** five data realizations cannot support interpretation of small F1 deltas.
4. **Benchmark positioning and communication:** novelty, terminology, semantic-vs-data framing, model breadth, and resource accessibility need sharper treatment.

## Atomic concerns

| ID | Reviewer anchor | Surface comment | Underlying concern | Interpretation | Severity | Best response mode | Evidence needed |
|---|---|---|---|---|---|---|---|
| I01 | AC: “four small, widely known graphs containing 5–11 nodes” | Main claims rely on small familiar graphs. | The headline empirical conclusion may be an artifact of a tiny search space and benchmark exposure, not a property of LLM-assisted causal discovery. | Direct | Critical | New evidence + narrowed claim | Matched data-bearing tests on larger/less familiar graphs; explicit scope language |
| I02 | AC: names-only results “may stem partly from rote memorization” | Familiar variable names/graphs may be in pretraining. | Names-only recovery does not distinguish broad semantic reasoning from direct graph memorization. | Direct | Critical | Controlled diagnostic | Name/label permutation, less-common named graphs, and newly generated topology controls |
| I03 | AC: SFT train/eval overlap; “graph-disjoint evaluation is required” | Post-training validity is compromised. | Gains could come from graph memorization, output-format learning, or verifier-target imitation rather than evidence integration. | Direct | Critical | New graph-disjoint evaluation | Base-vs-FT on never-trained graphs and preferably leave-one-graph-out retraining |
| I04 | AC: few realizations/small differences | Effects may not be robust. | The paper's descriptive bootstrap can look more conclusive than five paired observations warrant. | Direct | Critical | More independent realizations + paired analysis | 20–30 paired data seeds for a prespecified subset; effect sizes/CIs and multiplicity-aware interpretation |
| I05 | AC: model coverage lower than expected | Benchmark generality is under-supported. | Findings may be family-specific rather than landscape-wide. | Direct | Major | Add targeted models or narrow generality | One additional independent frontier family if accessible; otherwise explicit family-bounded claim |
| I06 | AC: concepts/details require clearer definition; svky names “semantic cue” and “matched attribution problem” | Terminology is vague. | Readers cannot operationally reproduce the proposed protocol or understand what is held fixed. | Direct | Major | Clarification | One-sentence operational definitions plus a matched-cell example |
| I07 | svky: novelty limited; definition obvious; components/SFT exist | Contribution may be incremental. | Formalization and SFT alone are not novel enough; the unique value must be the joint controlled protocol and released matched artifacts. | Direct | Critical for svky | Reposition + comparison | Feature-by-feature related-work table showing which prior benchmark jointly supplies all controls; avoid claiming novelty for generic SFT |
| I08 | svky: paper is “unfair” to semantics and treats numerical data as gold standard | Framing discounts semantic evidence and data bias. | The evaluation may encode a normative hierarchy rather than measuring complementarity; classical output is not necessarily truth on real data. | Direct surface; inferred normative concern | Major | Clarification + concession | State that ground truth is available only in controlled simulations; semantics may help or mislead; data-only methods are matched references, not gold standards |
| I09 | svky: toy/outdated/semi-synthetic datasets unsuitable | Benchmark choices lack realism/current relevance. | Even apart from memorization, empirical relevance to modern scientific discovery is weak. | Direct | Major | New evidence or scope concession | Newer/less common graph(s), ideally a real scientific perturbation benchmark; otherwise clearly position these graphs as controlled attribution probes |
| I10 | svky: “latest benchmarks” requested | Add contemporary evaluation. | **UNCERTAIN:** reviewer may mean newer causal-network benchmarks, real perturbation datasets, or newer LLM-CD benchmarks; the requested target is unspecified. | UNCERTAIN | Major | Acknowledge ambiguity; choose defensible target | Prefer a recent network-inference/perturbation benchmark if integration is feasible; do not claim it answers all meanings of “latest” |
| I11 | mUVt: leave-one-graph-out suggested | SFT needs disjointness. | Same as I03, with a concrete minimum acceptable design. | Direct | Critical | Leave-one-graph-out experiment | Four held-out adapters if feasible, or clearly labeled graph-disjoint external evaluation if not |
| I12 | mUVt: larger lightweight matched evaluation requested | Attribution findings may not scale. | Larger names-only probes alone do not test numerical evidence use; the full matched contrast is what matters. | Direct | Critical | New matched experiment | Summary-only real/anon/names-only plus classical reference on Child and, if feasible, Alarm/Synthetic-25 |
| I13 | zDep: “limited direct evidence” that LLMs struggle to use numerical evidence | The interpretation outruns the measurements. | Small F1 deltas and format sensitivity do not by themselves prove inability to use data; metrics may miss partial or local evidence use. | Direct | Critical | Narrow claim + stronger sensitivity test | Use “limited/unstable gains in evaluated settings”; add intervention-response or counterfactual data-swap consistency test if feasible |
| I14 | zDep: stronger ablations/adversarial controls needed for post-training | Source of SFT gains is unidentified. | Parse validity and F1 improvements conflate formatting, memorization, semantics, and data use. | Direct | Major | Factorial ablation | Base vs format-only vs reasoning-SFT; real vs anon; correct vs shuffled/mismatched data; graph-disjoint test |
| I15 | zDep: statistical significance/qualitative cases | Small improvements may be noise. | Numerical uncertainty needs both sufficient replication and interpretable examples of changed edges. | Direct | Critical | Replication + case analysis | Paired CIs/randomization tests and a small prespecified edge-level audit |
| I16 | zDep: add Gemini/Claude/DeepSeek | Model roster omits influential independent families. | External validity across frontier models is uncertain. | Direct | Major | Targeted breadth or explicit limitation | Add 1–2 accessible families using the same small prespecified grid; API access/cost required |
| I17 | zDep: dataset URL inaccessible; svky says no dataset included | Artifact accessibility/track compliance is unclear. | Reviewers could not consistently verify the dataset contribution, weakening reproducibility and dataset-track credibility. | Direct | Critical/easy | Fix and document access | Verify anonymous URL from a clean session; provide immutable link, checksums, and precise contents |
| I18 | mUVt: limitations honest but unresolved | Transparency alone is insufficient. | Repeating limitations in rebuttal will not move the decision without at least one decisive validity experiment. | Inferred | Critical | Prioritize evidence | Complete P0 experiments rather than spending response budget restating caveats |
| I19 | mUVt strengths; zDep strengths; AC strengths | Attribution protocol is useful, focused, and potentially widely applicable. | The core contribution is viable if empirical overreach is repaired. | Direct | Positive anchor | Preserve and center | Lead with protocol value, then present targeted validity evidence |
| I20 | mUVt reproducibility positive; zDep conceptual reproducibility positive | Code/methods appear reproducible to some reviewers. | Artifact organization is a strength, but access failure creates inconsistency. | Direct | Positive/mixed | Preserve + repair link | Access audit and short artifact map |

## Coverage status

All substantive criticisms, questions, ratings, confidence statements relevant to strategy, and artifact-access comments in the supplied reviews are represented above. Positive comments are captured in I19–I20 because they determine the viable rebuttal framing.

---

# AUTHOR RESPONSE WORKSPACE

> **AUTHOR: Write your responses only in this section.** Keep the diagnostic issue board above unchanged. Each response field should contain short factual bullets, with compact tables where they make experimental comparisons clearer. You do not need polished rebuttal prose; the final response will be drafted from these notes plus verified experimental results.

For each issue, state what you agree or disagree with, facts only you can supply, the manuscript location if known, and the revision or claim narrowing you authorize. Leave unknown fields unchanged. Do not estimate unfinished experimental results.

### I01 — Small and familiar graphs (AC)

- **Question being asked:** Do the headline conclusions generalize beyond the four small, familiar 5–11-node graphs, or are they artifacts of graph size and benchmark familiarity?

We agree that the submitted four-graph slice cannot establish broad scale generalization. The new matched Child, Synthetic-25, and Alarm results directly test this limitation and support a narrower, capability-limited conclusion.

- **Your direct answer to this question:** The new results do not justify universal generalization, but they extend the diagnosis beyond the original four graphs. On Child (20 nodes), two newly generated Synthetic-25 topologies, and Alarm (37 nodes), graph recovery remained low and data-prompt validity deteriorated with scale. We therefore treat the larger-graph result as a negative capability finding rather than evidence that current prompt-based LLM causal discovery scales.
- **Facts/evidence already available:** The graph-disjoint suite evaluates Base, Stage 1, and Stage 2 on four graphs absent from both original SFT stages, using matched names-only, real-name data, anonymized data, and anonymized names-only cells with 20 paired realizations per contrast. Under the canonical controlled-decoding protocol, the principal Stage-2 data-bearing results are:

  | Graph | Nodes | Real-name validity | Real-name F1 | Anonymous validity | Anonymous F1 |
  |---|---:|---:|---:|---:|---:|
  | Child | 20 | 100.0% | 0.070 | 100.0% | 0.104 |
  | Chain-25 | 25 | 90.0% | 0.049 | 100.0% | 0.069 |
  | Jungle-25 | 25 | 95.0% | 0.044 | 100.0% | 0.095 |
  | Alarm | 37 | 60.0% | 0.022 | 90.0% | 0.031 |

  Exact duplicate directed edges are collapsed before scoring because they do not change the predicted graph. Self-loops, truncated responses, and other invalid outputs remain invalid and receive F1=0. Under the same scoring, the Stage-2-minus-final-Stage-1 data-gain changes were -0.092/-0.013 on Alarm, -0.022/-0.055 on Chain-25, +0.039/+0.005 on Child, and -0.025/+0.002 on Jungle-25 (real/anonymous). The effects were therefore mostly negative or near zero, with a modest positive Child real-name exception.
- **Current manuscript location:** Section 5.1 and Figure 2 currently provide the small-graph and names-only scale probes; Section 6 (Scope), Appendix B (Empirical scope), and Table 15 explicitly identify Child, Alarm, and Synthetic-25 as unevaluated scale tests in the submitted version.
- **Revision or claim narrowing authorized:** Add the matched larger-graph results and validity rates, replace the statement that these graphs were not run on the data-bearing grid, and restrict empirical conclusions to the evaluated models, representations, and graph sizes. Do not claim successful large-graph causal discovery.
- **Reviewer-specific nuance:** This directly answers the AC and mUVt request for a lightweight larger matched evaluation, but it does not establish generality across model families or realistic large scientific networks.

### I02 — Possible graph memorization (AC)

- **Question being asked:** Do the names-only results reflect transferable semantic reasoning, or memorization of familiar graph structures and variable-name associations?

The current protocol measures how much predictions depend on semantic labels; it does not determine whether that dependence comes from transferable reasoning, broad prior knowledge, or direct memorization. We will make that distinction explicit.



- **Your direct answer to this question:** The current controls show that names materially influence predictions, but they cannot identify whether that influence comes from transferable domain knowledge, direct pretraining memorization, or plausible semantic priors. We therefore interpret names-only performance as name-mediated support, not proof of semantic reasoning.
- **Facts/evidence already available:** Anonymization holds graph and sampled data fixed while replacing variable names with neutral labels. Strict LOGO removes each held-out graph from both SFT stages, and the conflict suite pairs familiar Child names with five newly rewired data-generating graphs. These tests rule out SFT-set graph overlap as the sole explanation for format gains, but they cannot audit pretraining exposure. In LOGO names-only cells, final Stage 1 F1 averaged 0.498 with real names versus 0.157 anonymously, and Stage 2 averaged 0.489 versus 0.134, demonstrating strong name dependence.
- **Current manuscript location:** Section 5.1 and Figures 2 and 10 report names-only and anonymization results; Section 6 and Appendix B (Interpreting semantic controls) already state that anonymization does not distinguish domain knowledge from memorization.
- **Revision or claim narrowing authorized:** Replace any language equating names-only recovery with transferable semantic reasoning by “name-mediated support,” add the conflict and LOGO evidence, and state explicitly that pretraining memorization remains unresolved.
- **Reviewer-specific nuance:** The new tests address post-training leakage and semantic-data conflict, but a training-corpus audit or newly constructed named scientific domain would be required to separate broad knowledge from pretraining memorization.

### I03 — Non-disjoint SFT evaluation (AC)

- **Question being asked:** Do the post-training gains persist on graph families absent from both SFT stages, rather than coming from graph memorization, format learning, or verifier imitation?

- **Your direct answer to this question:** Post-training reliably improves output validity on held-out graph families, but it does not consistently improve numerical-evidence integration beyond the final Stage-1 checkpoint. The original post-training result should therefore be interpreted primarily as improved structured-output reliability.
- **Facts/evidence already available:** We completed immediate graph-disjoint evaluation on Child, Alarm, and two new Synthetic-25 graphs, plus strict LOGO retraining for Cancer, Earthquake, Asia, and Sachs with training seeds 42, 314, and 2718. In LOGO, final Stage 1 reached 100% validity in every aggregate condition, compared with 56%–93% for Base. The Stage-2-minus-Stage-1 data-gain effects were, for real names, -0.071 (Asia), +0.003 (Cancer), -0.025 (Earthquake), and -0.041 (Sachs), and, anonymously, +0.028, +0.004, +0.037, and +0.018. Graph-disjoint effects were between -0.055 and +0.005.
- **Current manuscript location:** Section 5.2 and Figure 4 contain the submitted overlapping-graph post-training result; Appendix F documents both training stages. The graph-disjoint and LOGO experiments are new and not in the submitted manuscript.
- **Revision or claim narrowing authorized:** Add the strict split construction, all four folds and three training seeds, and paired intervals. Recast post-training as an example application that improves format reliability; remove the conclusion’s claim that it provides evidence of broadly improved numerical integration.
- **Reviewer-specific nuance:** This directly satisfies the requested graph-disjoint and leave-one-graph-out designs even though the outcome is mixed or negative for numerical reasoning.

### I04 — Too few realizations and small effects (AC)

- **Question being asked:** Are the reported small performance differences stable across independent data realizations, or could they be sampling noise from only five runs?

- **Your direct answer to this question:** Thirty paired Sachs realizations show that the broad semantic-condition effects are stable, while the small incremental Stage-2-over-Stage-1 effect is not distinguishable from zero. Five-run estimates should therefore not be used to support small-delta claims.
- **Facts/evidence already available:** The 30-seed paired Sachs estimates are:

  | Contrast: data minus no data | Real names, mean [95% CI] | Anonymous, mean [95% CI] |
  |---|---:|---:|
  | Base | -0.190 [-0.240, -0.138] | +0.230 [0.207, 0.253] |
  | GPT-5-mini | -0.087 [-0.104, -0.069] | +0.039 [0.017, 0.062] |
  | + SFT | -0.169 [-0.223, -0.110] | +0.061 [0.022, 0.100] |
  | + SFT + GRPO | -0.240 [-0.297, -0.181] | +0.061 [0.026, 0.096] |

  Paired sign-flip tests accompany these estimates.
- **Current manuscript location:** Section 4 states that data-bearing cells use five realizations; Section 5.1 and Appendix E.3, Tables 10–11, report descriptive five-run bootstrap intervals; Appendix B acknowledges the power limitation.
- **Revision or claim narrowing authorized:** Add the 30-seed paired estimates and tests for the prespecified Sachs slice, retain five-run results as descriptive elsewhere, and avoid claiming that five seeds suffice for small effects in general.
- **Reviewer-specific nuance:** The replication resolves uncertainty for the selected Sachs contrasts, not for every graph, format, and model cell in the benchmark.

### I05 — Limited model coverage (AC)

- **Question being asked:** Does the observed semantic-versus-numerical evidence pattern generalize beyond the limited model families evaluated in the paper?

- **Your direct answer to this question:** We cannot establish landscape-wide generality beyond the evaluated GPT, Qwen, and Llama families. The new reliable replication adds GPT-5-mini but not an independent frontier family, so the claim must remain family-bounded.
- **Facts/evidence already available:** The submitted paper evaluates ten models from GPT-5, Llama-3.1, Qwen2.5, and Qwen3. The rebuttal adds a complete 30-seed GPT-5-mini Sachs slice. Three Ministral reasoning variants were attempted on Sachs and Child but produced 0 valid rows in the controlled grid, and the DeepSeek-R1-Distill-Llama-8B run has only two completed order seeds; neither is suitable evidence for the semantic-versus-data pattern.
- **Current manuscript location:** Section 4 lists the model roster; Table 4 and Figure 8 report validity and names-only coverage; Appendix B (Model and method coverage) already limits interpretation.
- **Revision or claim narrowing authorized:** State that the phenomenon is demonstrated only for the evaluated GPT/Qwen/Llama slices, report failed structured-output coverage separately if space permits, and remove universal language about modern LLMs.
- **Reviewer-specific nuance:** GPT-5-mini improves replication but not architectural independence. Invalid Ministral outputs and an incomplete Llama-distilled DeepSeek run do not answer the reviewer’s request for Gemini, Claude, or frontier DeepSeek.

### I06 — Terminology and operational definitions (AC, svky)

- **Question being asked:** What exactly are a “semantic cue” and a “matched attribution test,” and which graph, data, seed, scoring, and prompt factors are held fixed?

Thank you for pointing this out. We will define these terms explicitly in the revision.

A **semantic cue** is variable-associated text, i.e., the original variable names and any provided descriptions. Anonymization replaces these with neutral labels such as `X1` and `X2`.

A **matched attribution test** changes one evidence source while holding fixed the graph, sampled data, data seed, sample/intervention budget, variable order, model settings, prompt instructions, output format, and scoring:

- Original vs. anonymized names isolates semantic cues.
- Data vs. no data isolates numerical evidence.
- Summary vs. tabular input isolates data representation.

For example, the real-name and anonymized Sachs conditions use identical samples and settings; only the variable names differ. We will add these definitions and a matched-cell example to the paper.


- **Your direct answer to this question:** A semantic cue is variable-associated text, principally the original variable names and any descriptions. A matched attribution test changes one evidence source while fixing the graph, sampled data, data seed, sample and intervention budgets, variable order, model settings, prompt instructions, output representation, and scoring.
- **Facts/evidence already available:** Original versus anonymized names isolates name-mediated support; data versus names-only isolates the response to numerical evidence; summary versus tabular holds evidence fixed while changing serialization. For example, the real-name and anonymized Sachs cells use identical samples and settings and differ only in whether names such as Raf and Mek are replaced by neutral labels.
- **Current manuscript location:** Section 2 defines the evidence channels; Section 3, especially Design principles, Cells and cell families, and Prompt conditions, gives the operational matched-cell construction; Table 16 lists controlled axes.
- **Revision or claim narrowing authorized:** Add the two one-sentence definitions and one matched Sachs example to the main text, and consistently use “name-mediated support” where the mechanism is not identified.
- **Reviewer-specific nuance:** The definitions should describe observable controls rather than implying that anonymization reveals the origin of a model’s knowledge.

### I07 — Novelty and contribution positioning (svky)

- **Question being asked:** What is genuinely novel beyond an intuitive formal definition, existing evaluation components, and generic supervised fine-tuning?

We appreciate this concern. CausalMix is an evaluation and dataset contribution, not a new algorithm. Its novelty is source attribution: it measures separately how variable meanings, numerical data, and their interaction affect causal discovery. Existing aggregate scores cannot make this distinction. CausalMix provides the controlled datasets, protocol, simulator, and tools needed to perform this analysis reproducibly. Fine-tuning is an example application, not a novel training method.

- **Your direct answer to this question:** CausalMix is an evaluation and dataset contribution, not a new causal-discovery algorithm or a claim that generic SFT is novel. Its distinctive contribution is a joint matched protocol that attributes performance to variable semantics, numerical evidence, their interaction, and data representation while using the same graph, samples, and scorer.
- **Facts/evidence already available:** The release provides fixed graph/data pairings, names-only and anonymized controls, observational and interventional budgets, summary and tabular representations, matched PC/GES/ENCO references, deterministic seeds, prompt builders, scoring code, and training-data utilities. The submitted Appendix C Table 1 already compares which related benchmark families support these controls jointly.
- **Current manuscript location:** The Abstract and Introduction list the two contributions; Section 2 motivates the unmatched gap; Section 3 defines the suite; Appendix C and Table 1 provide the feature-by-feature related-work comparison.
- **Revision or claim narrowing authorized:** Lead with the controlled attribution protocol and released matched artifacts, describe fine-tuning only as a diagnostic application, and avoid novelty claims for causal SFT, individual controls, or the formal definition alone.
- **Reviewer-specific nuance:** For svky, the strongest response is the joint-control comparison and concrete released artifact, not an argument that semantic/data decomposition itself is conceptually surprising.

### I08 — Semantic evidence versus numerical data framing (svky)

- **Question being asked:** Does the paper unfairly treat numerical data as the gold standard and discount the value of semantic evidence or the biases in numerical datasets?

We agree that both evidence sources may be valuable and complementary; the benchmark is intended to attribute their contributions, not rank one as intrinsically superior.

- **Your direct answer to this question:** No hierarchy is assumed: semantic knowledge may help, mislead, or encode useful expert information, while numerical data are informative only under explicit causal and sampling assumptions. CausalMix measures complementarity and attribution rather than treating a data-only method as epistemic truth.
- **Facts/evidence already available:** All simulated and semi-synthetic benchmark cells are scored against a known generating DAG. PC, GES, and ENCO are matched data-only references under their assumptions, not gold standards on real scientific data. The names-only and real-versus-anonymized results demonstrate that semantics can be strongly useful; the conflict tests demonstrate that it can also oppose the supplied DGP.
- **Current manuscript location:** Section 2 (Identifiability and references) states that the semantic channel may help or mislead; Section 5.3 compares methods by regime; Section 6 discusses combining evidence channels; Appendix B states the causal assumptions.
- **Revision or claim narrowing authorized:** Replace language suggesting numerical evidence is inherently superior with language about attributable complementarity, and explicitly distinguish known simulation ground truth from classical reference outputs.
- **Reviewer-specific nuance:** The rebuttal should agree with the value of semantic expertise while explaining why a controlled benchmark still needs ground-truth-generating graphs to diagnose information use.

### I09 — Toy, outdated, or semi-synthetic datasets (svky)

- **Question being asked:** Why are small, semi-synthetic, or established benchmark graphs appropriate evidence, and what empirical scope can they legitimately support?


We chose these graphs for three reasons:

1. **Reliable ground truth.** Real-world causal structures are rarely known with certainty. Simulated and semi-synthetic data provide exact graphs and controlled noise, helping distinguish method errors from uncertain ground truth. Sachs is derived from a real biological system, while our controlled cells use generated samples from a known benchmark parameterization.

2. **Feasible matched evaluation.** Small graphs allow us to test all data formats and budgets under identical conditions. Numerical prompts grow rapidly with the number of variables and samples, and larger settings can exceed current context limits. Our released benchmark includes larger graphs to support future methods with more scalable representations.

3. **Comparison with prior work.** These are established benchmarks in data-driven and LLM-based causal discovery. ENCO [20], our numerical baseline, evaluates Cancer, Earthquake, Asia, Sachs, Child, and Alarm. Recent LLM-based evaluations also use these graphs, including CausalBench [43], CausalGraphBench [3], and Causal-LLM [32].

Accordingly, our experiments support source-attribution conclusions for the evaluated models, small graphs, and data regimes. They do not establish generalization to larger, unfamiliar, or broader real-world settings.

Our goal is to establish the problem, controlled evaluation protocol, and initial benchmark dataset. Broader graph coverage is an important extension. We will release the codebase so researchers can readily add new graphs, datasets, and simulators.

**Reference:** Phillip Lippe, Taco Cohen, and Efstratios Gavves (2022), “Efficient Neural Causal Discovery without Acyclicity Constraints.”
- **Your direct answer to this question:** These graphs are appropriate as controlled attribution probes because they provide exact ground truth and permit matched interventions, but they cannot support broad claims about modern scientific discovery. The new larger and newly generated tests strengthen the stress test while reinforcing, rather than removing, the scope limitation.
- **Facts/evidence already available:** The original slice contains Cancer, Earthquake, Asia, and Sachs; Sachs represents a real biological system, while the controlled benchmark cells use generated samples from a fixed known parameterization. The rebuttal adds full matched Qwen3-4B evaluations on Child, Alarm, and two new Synthetic-25 topologies. Performance and validity remain poor at larger scale. CausalMix V1 releases 14 graph instances and the generator needed to add further domains.
- **Current manuscript location:** Section 3 (Released instance), Section 6 (Scope), Appendix B (Empirical scope and Small-graph identifiability), and Table 15 describe the graph inventory and token burden.
- **Revision or claim narrowing authorized:** Report the larger matched results, call the original graphs controlled attribution probes, and explicitly state that the work does not establish effectiveness on contemporary high-dimensional or unconstrained real-world discovery tasks.
- **Reviewer-specific nuance:** The negative larger-graph result is still informative for the benchmark’s diagnostic purpose, but it is not a substitute for a newer real perturbation benchmark.

### I10 — Request for “latest benchmarks” (svky)

- **Question being asked:** Can the method be evaluated on a contemporary benchmark, and which interpretation of “latest benchmark” is scientifically appropriate here?

The framework can be extended to other data generators, but extensibility is not evidence that a contemporary benchmark has already been evaluated.

- **Your direct answer to this question:** The phrase “latest benchmark” is underspecified. We added newly generated and larger graph controls because they directly test the reviewers’ size and familiarity concerns, but we did not add a contemporary real perturbation benchmark and will not claim that we did.
- **Facts/evidence already available:** The graph-disjoint suite covers Child, Alarm, Chain-25, and Jungle-25 with matched names-only/data and real/anonymized conditions. The public runner is graph- and dataset-extensible, but no licensed contemporary external perturbation dataset was selected and integrated during the rebuttal.
- **Current manuscript location:** Section 3 describes configurable graph and data inputs; Section 6 and Appendix B define the present empirical scope; Appendix G inventories released graphs and generation assets.
- **Revision or claim narrowing authorized:** Present the new synthetic/larger tests as targeted validity controls, state that evaluation on contemporary real perturbation benchmarks remains future work, and avoid implying that extensibility equals completed validation.
- **Reviewer-specific nuance:** This is a clarification and scoped concession because the reviewer did not identify a specific benchmark whose graph, interventions, and ground truth support the matched protocol.

### I11 — Leave-one-graph-out evaluation (mUVt)

- **Question being asked:** Does post-training improve performance when an entire graph family is excluded from training and used only for leave-one-graph-out evaluation?

- **Your direct answer to this question:** Strict leave-one-graph-out training preserves the format-validity improvement but does not yield a consistent Stage-2 numerical-evidence gain beyond Stage 1. The post-training claim must therefore be narrowed.
- **Facts/evidence already available:** We constructed four folds holding out Cancer, Earthquake, Asia, or Sachs from both stages, trained each fold from the base Qwen3-4B model, and repeated training with seeds 42, 314, and 2718. Cancer was already absent from the original Stage-2 source, which the split manifest records. Final Stage 1 achieved 100% aggregate validity. Incremental Stage-2-over-Stage-1 data gain was:

  | Held-out graph | Real names, mean [95% CI] | Anonymous, mean [95% CI] |
  |---|---:|---:|
  | Asia | -0.071 [-0.116, -0.028] | +0.028 [0.003, 0.054] |
  | Cancer | +0.003 [-0.056, 0.061] | +0.004 [-0.056, 0.069] |
  | Earthquake | -0.025 [-0.074, 0.027] | +0.037 [0.012, 0.064] |
  | Sachs | -0.041 [-0.072, -0.011] | +0.018 [-0.013, 0.048] |
- **Current manuscript location:** Section 5.2 and Figure 4 report the submitted non-disjoint post-training experiment; Appendix F describes its data. Strict LOGO is new rebuttal evidence.
- **Revision or claim narrowing authorized:** Add fold construction, removed-row counts, three training seeds, Base/Stage-1/Stage-2 results, and paired intervals; state that LOGO supports transferable format learning but not a broad numerical-integration claim.
- **Reviewer-specific nuance:** This implements the exact design suggested by mUVt rather than relying only on evaluation of an adapter trained on overlapping families.

### I12 — Larger matched attribution evaluation (mUVt)

- **Question being asked:** Do the matched names-only, real-name, anonymized-data, and data-only attribution findings hold on larger and more complex graphs?


- **Your direct answer to this question:** The larger matched tests reproduce the difficulty of extracting useful numerical signal, but current Qwen3-4B prompting does not provide reliable large-graph recovery. The result supports the benchmark’s diagnostic value, not successful scale generalization.
- **Facts/evidence already available:** Child (20 nodes), Chain-25, Jungle-25, and Alarm (37 nodes) were evaluated with matched names-only, real-name summary, anonymized summary, and anonymized names-only conditions. Each paired contrast has 20 realizations and includes Base, the submitted Stage-1 checkpoint, final Stage 1, and Stage 2. After collapsing exact duplicate directed edges, Stage-2 real-name data validity ranged from 60% on Alarm to 100% on Child, and validity-inclusive data-bearing F1 was 0.022–0.104. Self-loops and truncated responses remain invalid. Stage-2-minus-final-Stage-1 data-gain effects were mostly negative or near zero; Child real names were the modest positive exception (+0.039). The full validity/F1 table appears under I01.
- **Current manuscript location:** Figure 2 currently contains only names-only Child/Alarm probes; Section 6, Appendix B, and Table 15 explicitly state that a full larger matched grid was not included in the submission.
- **Revision or claim narrowing authorized:** Add a compact table of larger matched results and prompt validity, update the scope text, and state that scalable data representations remain an open challenge.
- **Reviewer-specific nuance:** The 68k-plus-token numerical prompts and serialization difficulty are part of the evaluated interface’s present limitation, but we should report validity separately rather than treating invalid outputs as proof of causal inability.

### I13 — Direct evidence of numerical-evidence use (zDep)

- **Question being asked:** What direct evidence shows that LLMs use numerical evidence only weakly or unstably, rather than the aggregate metric missing partial or local evidence use?

- **Your direct answer to this question:** Edge-level analysis shows that models do respond locally to numerical evidence, so the correct conclusion is not that they ignore data. The response is unstable: many edges change, but harmful changes exceed beneficial corrections in nearly every evaluated slice.
- **Facts/evidence already available:** Among matched valid Stage-2 outputs, the edge transitions are:

  | Evaluation | Edge changed | Beneficial | Harmful | Net benefit |
  |---|---:|---:|---:|---:|
  | Graph-disjoint, real names | 25.3% | 4.8% | 18.5% | -13.6 pp |
  | Graph-disjoint, anonymous | 37.7% | 5.4% | 29.8% | -24.4 pp |
  | LOGO, real names | 28.1% | 11.4% | 15.0% | -3.6 pp |
  | LOGO, anonymous | 25.5% | 9.8% | 12.3% | -2.5 pp |
  | Sachs, 30 seeds, anonymous | 42.2% | 10.9% | 25.5% | -14.7 pp |

  The conflict test gives a complementary diagnostic:

  | Names | Change in data-following | Change in unresolved choices |
  |---|---:|---:|
  | Real | -1.6 pp | +6.0 pp |
  | Anonymous | -7.9 pp | +14.5 pp |
- **Current manuscript location:** Section 5.1 and Figures 3 and 10 currently report aggregate data and anonymization contrasts. The edge-transition and five-rewire conflict analyses are new.
- **Revision or claim narrowing authorized:** Add edge-change, beneficial, harmful, net-benefit, and conflict-following rates; revise “limited data use” to “partial local responsiveness with unstable and frequently detrimental integration.”
- **Reviewer-specific nuance:** This directly addresses the possibility that aggregate F1 hides offsetting local effects: such partial effects exist, but they are more often harmful than beneficial. The invalid shuffled-data placebo must not be used.

### I14 — Stronger ablations and adversarial controls (zDep)

- **Question being asked:** Which factor causes the post-training gains: output-format learning, memorization, semantic priors, verifier preference, or genuine numerical-evidence integration?

- **Your direct answer to this question:** The most robust post-training effect is structured-output and contract learning, accompanied by strong name-based semantic priors. SFT-family memorization is not necessary for the format gain, but pretraining memorization and verifier preference cannot be fully separated. Genuine numerical-evidence integration is at most small and inconsistent.
- **Facts/evidence already available:** The mechanism diagnostics support the following attribution:

  | Candidate explanation | Diagnostic result | Conclusion |
  |---|---|---|
  | Output-format learning | LOGO validity rises from 56%–93% for Base to 100% with + SFT; + SFT + GRPO remains near 100%. | Strongly supported. |
  | SFT graph memorization | Validity gains persist on graph-disjoint tests and strict LOGO folds. | Not necessary for the format gain. |
  | Semantic priors/pretraining familiarity | LOGO names-only F1 is 0.498 vs. 0.157 with + SFT and 0.489 vs. 0.134 with + SFT + GRPO, real versus anonymous. | Strong name dependence; the source of that knowledge remains unresolved. |
  | Verifier preference | No verifier-free, target-matched checkpoint is available. | Confounded with format/content imitation. |
  | Numerical-evidence integration | Graph-disjoint GRPO increments over SFT are -0.055 to +0.005, while conflict following is inconsistent. | At most small and unstable. |

  The intended intact-versus-shuffled audit is unusable because all 160 shuffled prompts were 138k–142k tokens and exceeded the 131,072-token context limit.
- **Current manuscript location:** Section 5.2 and Figure 4 present the aggregate submitted post-training result; Appendix F describes Stage 1 as format and graph-target SFT and Stage 2 as reasoning-SFT continuation. The factorial, LOGO, and conflict results are new.
- **Revision or claim narrowing authorized:** Attribute the supported gain to output reliability, report semantic dependence, state that SFT overlap is not the sole cause, and explicitly leave verifier preference and pretraining memorization unresolved. Remove claims that Stage 2 establishes numerical integration.
- **Reviewer-specific nuance:** Stage 1 contains gold graph targets and is therefore not a perfectly pure schema-only control. A verifier-free target-matched run or a schema-transcoding control would be required for complete causal separation.

### I15 — Statistical significance and qualitative cases (zDep)

- **Question being asked:** Are the improvements statistically reliable, and can representative edge-level cases show how predictions change when numerical evidence changes?

- **Your direct answer to this question:** The 30-seed paired analysis makes the principal Sachs contrasts statistically interpretable, while showing that the incremental Stage-2 gain over final Stage 1 is not reliable. Edge-level transitions provide interpretable examples of both corrections and regressions.
- **Facts/evidence already available:** The 30-seed table under I04 shows that Stage-2 data-minus-no-data F1 is reliable within each naming condition, while incremental Stage-2-over-Stage-1 gain is not. In 59 valid real-name Sachs LOGO pairs, representative edge transitions were:

  | Unordered edge pair | Beneficial corrections | Harmful regressions | Interpretation |
  |---|---:|---:|---|
  | Jnk–P38 | 36 | 2 | Usually improved with data. |
  | Erk–P38 | 36 | 0 | Consistently improved in valid pairs. |
  | PIP3–PKA | 7 | 34 | Usually degraded with data. |
  | PIP2–PKA | 0 | 30 | Degraded without observed corrections. |
  | Mek–Plcg | 1 | 30 | Usually degraded with data. |
  | Erk–PKA | 18 | 26 | Both corrected and broken across runs, illustrating instability. |
- **Current manuscript location:** Appendix E.3, Tables 10–11, contains the submitted five-run bootstrap intervals; Section 4 and Appendix B describe their limitations. The 30-seed tests and qualitative transition audit are new.
- **Revision or claim narrowing authorized:** Add paired CIs, sign-flip tests, valid-pair counts, and a small prespecified table of beneficial and harmful edge cases. Interpret expanded intervals rather than isolated five-run point estimates.
- **Reviewer-specific nuance:** The edge examples are diagnostics, not independent hypothesis tests; aggregate paired estimates remain primary, and invalid pairs must be reported rather than silently discarded.

### I16 — Gemini, Claude, or DeepSeek coverage (zDep)

- **Question being asked:** Does the phenomenon hold for influential independent model families such as Gemini, Claude, DeepSeek, or another modern reasoning family?

We did not obtain reliable controlled Gemini, Claude, or frontier-DeepSeek results during the rebuttal period.

- **Your direct answer to this question:** We do not have reliable controlled results for Gemini, Claude, or a frontier DeepSeek model and therefore cannot claim that the phenomenon holds across those families.
- **Facts/evidence already available:** The submitted evaluation covers GPT, Qwen, and Llama. The rebuttal adds a complete GPT-5-mini replication. Three Ministral reasoning variants produced no valid graphs in the controlled Sachs/Child grid, and DeepSeek-R1-Distill-Llama-8B completed only two order seeds; these runs are insufficient for a family-level conclusion.
- **Current manuscript location:** Section 4 lists the model roster, and Appendix B (Model and method coverage) already notes omitted closed-source systems and family-specific behavior.
- **Revision or claim narrowing authorized:** Restrict all empirical claims to the evaluated model families, list Gemini/Claude/frontier-DeepSeek coverage as future work, and emphasize that the released model adapter supports adding them under identical controls.
- **Reviewer-specific nuance:** A Llama-based DeepSeek distillation should not be presented as independent frontier-DeepSeek coverage, and zero-valid-output Ministral runs cannot establish the semantic-versus-numerical pattern.

### I17 — Dataset URL and artifact accessibility (zDep, svky)

- **Question being asked:** Can reviewers reliably access the dataset and code artifacts, and is it clear exactly what files, seeds, checksums, and instructions are provided?

Thank you for reporting this. We verified the artifact on July 24th 2026 from two unauthenticated browsers and separate networks; the direct URL opened without login and the complete archive downloaded successfully.

- **Your direct answer to this question:** The anonymous repository and dataset are currently accessible, but the revision should make access independently verifiable with an exact link, command-line instructions, a file inventory, and checksums.
- **Facts/evidence already available:** On July 24, 2026, we verified access from two unauthenticated browsers on separate networks and downloaded the complete archive. The dataset is available at https://huggingface.co/datasets/mixcausalbench/anonymous-data, and the README provides an hf download command plus regeneration commands. The repository documents configs, graph assets, generated summaries, prompt builders, scoring, and tests.
- **Current manuscript location:** Section 3 (Released instance) describes the fixed release; Appendix G inventories graph and dataset assets. The repository README contains installation, quick-start, paper-run, data-download, and regeneration commands.
- **Revision or claim narrowing authorized:** Put the direct dataset URL and repository link in the rebuttal and manuscript, add an archive SHA-256 and per-directory inventory, and state the access date and unauthenticated verification procedure.
- **Reviewer-specific nuance:** Do not ask reviewers to contact us for basic access. Their inconsistent experience should be treated as an artifact-documentation failure even though the link now works.

### I18 — Limitations acknowledged but unresolved (mUVt)

- **Question being asked:** What new evidence actually resolves the acknowledged limitations, rather than merely restating them?

- **Your direct answer to this question:** The rebuttal adds decision-relevant evidence rather than only repeating caveats: larger matched graphs, strict graph-disjoint and LOGO post-training, 30-seed replication, and direct edge/conflict diagnostics. Where those tests fail to support the original interpretation, we narrow the claim.
- **Facts/evidence already available:** Completed suites include four larger/unseen graphs, four LOGO folds with three training seeds, 30 paired Sachs seeds for Base/Stage 1/Stage 2 and GPT-5-mini, five rewired Child conflict graphs, and paired edge-transition analyses. They show transferable format reliability, low larger-graph recovery, statistically stable semantic-condition effects, and unstable numerical responses.
- **Current manuscript location:** Section 6 and Appendix B state the submitted limitations; Sections 5.1–5.2 contain the empirical claims that the new experiments qualify.
- **Revision or claim narrowing authorized:** Add the new tables and diagnostics, update the limitation text from untested to tested where appropriate, and remove unsupported claims of scalable or broadly transferable numerical integration.
- **Reviewer-specific nuance:** Negative results are not presented as resolving the capability limitation positively; they resolve the evidentiary ambiguity and establish the correct scope of the paper’s claims.

### I19 — Positive anchor: value of the attribution protocol (AC, mUVt, zDep)

- **Question being asked:** What is the central value of the attribution protocol that should remain the paper’s main contribution after narrowing unsupported empirical claims?

- **Your direct answer to this question:** The durable contribution is the matched attribution protocol: it determines whether a causal-discovery score is supported by variable semantics, numerical evidence, their interaction, or representation choices rather than treating one aggregate score as evidence of data use.
- **Facts/evidence already available:** Reviewers recognize the usefulness of names-only, anonymized, hybrid, and matched data-only comparisons. CausalMix packages those controls with fixed graph/data pairings, observational and intervention budgets, multiple representations, shared scoring, released seeds, classical references, and extensible runners. The new experiments demonstrate that this protocol detects format learning, semantic reliance, scaling failure, and harmful evidence responses that aggregate F1 would obscure.
- **Current manuscript location:** The Abstract and Introduction state the benchmark contribution; Section 3 defines matched cells and references; Appendix C Table 1 positions the joint controls; the Conclusion summarizes the intended protocol value.
- **Revision or claim narrowing authorized:** Lead the rebuttal and revision with the evaluation/dataset contribution, present model findings as demonstrations of the protocol, and make post-training secondary and explicitly preliminary.
- **Reviewer-specific nuance:** This positive anchor is shared by the AC, mUVt, and zDep and remains defensible even after narrowing the empirical model claims.

### I20 — Positive/mixed reproducibility assessment (mUVt, zDep)

- **Question being asked:** Which reproducibility artifacts are already available, and how will the inconsistent dataset-access experience reported by reviewers be repaired?


The dataset link and codebase were verified as accessible without authentication on July 24th 2026.

- **Your direct answer to this question:** The release already contains the benchmark builder, configs, graph assets, generated summaries, prompt renderers, model and classical runners, scoring code, tests, and lightweight result summaries. We will repair the inconsistent access experience by providing a direct immutable artifact map and checksums rather than relying on prose assurances.
- **Facts/evidence already available:** The README documents installation, smoke tests, canonical paper commands, result regeneration, data download from mixcausalbench/anonymous-data, and large-data regeneration. Access was verified without authentication on July 24, 2026, from two browsers and networks. Checkpoint hashes and exact rebuttal run manifests are available locally; the public archive checksum and compact file manifest still need to be added to the response package.
- **Current manuscript location:** Section 3 describes the released instance; Appendices D–H document protocols, metrics, training, graph inventory, and prompts; the repository README supplies executable commands and directory layout.
- **Revision or claim narrowing authorized:** Add a one-paragraph artifact map with exact links, contents, seeds, access date, archive SHA-256, and minimal reproduce/download commands; retain the positive reproducibility assessment while acknowledging and repairing the reported access failure.
- **Reviewer-specific nuance:** mUVt found the organization reproducible while zDep and svky reported access problems. The response should reconcile these observations with verifiable access details, not dismiss either report.
