# Reviews for “CausalMix: Defining and Benchmarking Hybrid Causal Discovery”

> Verbatim transcription from the supplied OpenReview export. Only Markdown presentation has been added; review wording is unchanged.

## At a glance

- [Meta-review — Area Chair Kf9w](#meta-review--area-chair-kf9w)
- [Official review — Reviewer svky](#official-review--reviewer-svky)
- [Official review — Reviewer mUVt](#official-review--reviewer-muvt)
- [Official review — Reviewer zDep](#official-review--reviewer-zdep)

---

## Meta-review — Area Chair Kf9w

**Submission:** 3307  
**Posted:** 21 Jul 2026, 10:20  
**Modified:** 23 Jul 2026, 12:23  
**Visible to:** Senior Area Chairs, Area Chairs, Authors, Reviewers Submitted, Program Chairs, Area Chair Kf9w  
**Revision status:** Revisions

### Meta-review

Scientific Contribution. CausalMix formalizes hybrid causal analysis, in which models receive both semantic information about variables and numerical observations or interventions. The controlled evaluation distinguishes between "names-only," "data-only," and combined scenarios to determine whether LLM performance relies on semantic prior knowledge or the actual use of numerical evidence. Experiments suggest that LLMs reconstruct substantial graph structures from variable names, whereas numerical data offer only limited and format-dependent benefits. The paper also presents a preliminary, verifier-based fine-tuning approach.

Strengths. The controlled attribution model addresses a key methodological issue in LLM-based causal analysis: high graph reconstruction scores do not necessarily prove that a model utilized the provided data. Comparing scenarios involving purely semantic information, hybrid data, and classical data is useful and potentially widely applicable. Comparisons with classical causal analysis algorithms and a generally transparent discussion of limitations represent further strengths.

Main Criticisms. The primary criticism is that the key conclusions rely largely on four small, widely known graphs containing 5–11 nodes. These graph structures and variable names may already be present in the model's pre-training data; consequently, the strong results observed in the "names-only" scenarios may stem partly from rote memorization. Thus, the findings do not demonstrate that the observed behavior generalizes to larger, less common, or newly generated causal structures.

Furthermore, the fine-tuning experiment lacks sufficient validity: the training and evaluation phases utilize overlapping graph families, meaning that improvements could result from memorizing structures, output formats, or the verifier's preferences rather than from an improved integration of semantic and numerical evidence. A graph-disjoint evaluation is required.

After all, many experimental cells contain only a few realizations, and some of the reported differences are small. More robust uncertainty estimates and repeated runs are necessary before these effects can be interpreted as robust. Furthermore, model coverage is lower than expected for a benchmark-based claim, and some key concepts and methodological details require clearer definition.

---

## Official review — Reviewer svky

**Title:** Review of paper 3307  
**Posted:** 25 Jun 2026, 23:04  
**Modified:** 23 Jul 2026, 11:25  
**Visible to:** Program Chairs, Senior Area Chairs, Area Chairs, Reviewers Submitted, Authors, Reviewer svky  
**Revision status:** Revisions

### Summary

The paper studies hybrid causal discovery methods (LLM + statistical methods) in three aspects: (1) formally define a hybrid causal discovery method; (2) introduce an evaluation frame that checks the effectiveness of LLM and statistical-based components separately; (3) fine-tune an LLM to complete causal discovery tasks. Additionally, the work introduces a simulator to generate benchmarks for hybrid causal discovery method evaluation. The work evaluates hybrid methods based on several LLM models and several statistical-based causal discovery methods by their evaluation frame, concluding that pure LLM-based methods perform well on several small datasets, while statistical-based methods are stronger for larger-scale tasks. They also show that task-driven fine-tuning can significantly improve the performance of LLM-based methods.

**Contribution Type Check:** Yes

### Strengths

The paper formalises a frame for the hybrid causal discovery methods to evaluate the performances of the statistical-based component and the semantic-based component separately. Such an evaluation process would be helpful to improve the hybrid causal discovery methods.

### Weaknesses

The paper’s novelty is limited. It may try to shape the evaluation frame as a standard for hybrid methods’ evaluation, however, many of the components have been used in existing works. The hybrid method’s definition is also obvious. Fine-tuning LLMs to handle causal discovery tasks also exists in previous works.

The presentation needs improvement. I could not clearly understand every detail of the method, e.g., many terms do not have an explanation. To name a few: “semantic cue”, “matched attribution problem”. I believe the authors need to significantly revise the methodology to explicitly describe the components instead of only provide intuition with such vague terms.

Another concern is the paper’s unfair attitude towards the semantic component (e.g., Lines 104-112). My understanding is that the authors tend to believe that the numerical data is the gold standard for causal discovery, but the semantic evidences are not that reliable. However, in practice, a numerical dataset also suffers from bias, and in contrast, semantic evidence may help debias the result. For instance, LLMs provide suggestions based on a vast domain research training dataset, potentially more reliable than a single dataset.

The authors use several toy datasets, especially, the Asia, Earthquake, and Cancer. These datasets may be out of date, and moreover, some of them contain semi-synthetic data, hence not suitable for evaluating LLM-based methods. The paper also shows that a simple strategy to use pure LLM-based methods can achieve good performances. This may be because of LLMs’ memory of the corresponding datasets.

### Assessment details

**Ethical Concerns:** No or very minor ethics concerns only  
**Reproducibility:** Yes  
**Dataset Assessment:** NA - no dataset included

### Limitations

I would recommend that the authors add experiments on the latest benchmarks to test the performances.

### Questions

See the weakness section.

### Score

**Rating:** 2: Reject: For instance, a paper with technical flaws, weak evaluation, inadequate reproducibility and incompletely addressed ethical considerations.  
**Confidence:** 4: You are confident in your assessment, but not absolutely certain. It is unlikely, but not impossible, that you did not understand some parts of the submission or that you are unfamiliar with some pieces of related work.

### Paper Formatting Concerns

No concern.

**LLM Policy:** Yes  
**Code Of Conduct Acknowledgement:** Yes  
**Responsible Reviewing Acknowledgement:** Yes

> [!IMPORTANT]
> **Draft rebuttal — Reviewer svky**
>
> **W1 — Limited novelty of the framework, definition, and fine-tuning approach**
>
> _Response:_ We agree that neither generic SFT nor the individual controls are new algorithms. CausalMix is an evaluation and dataset contribution. Its distinctive contribution is the **joint matched attribution protocol**: it changes variable semantics, numerical evidence, or representation one at a time while preserving the graph, samples, seed, budgets, prompt contract, and scorer. An aggregate graph score cannot reveal whether a model used names, data, or their interaction; the new edge-level results show why that distinction matters—models often change edges when data are added, but harmful changes can exceed beneficial corrections.
>
> _Evidence / revision:_ Reposition the Abstract, Introduction, and Conclusion around the controlled protocol and released matched artifacts; use Appendix C, Table 1 for the feature-by-feature related-work comparison; present SFT only as a diagnostic application, not a novel training method.
>
> **W2 — Unclear presentation and undefined terms such as “semantic cue” and “matched attribution problem”**
>
> _Response:_ Thank you; we will replace the intuitive wording with operational definitions. A **semantic cue** is variable-associated text, principally the original variable names and descriptions; anonymization replaces it with neutral labels such as `X1`. A **matched attribution test** changes one evidence source while holding fixed the graph, sampled data and seed, observation/intervention budget, variable order, model settings, instructions, representation, and scorer. Thus, a real-name/anonymized Sachs pair contains identical numerical samples and differs only in the labels Raf, Mek, etc. Data/no-data pairs isolate the response to numerical evidence, and summary/tabular pairs isolate serialization.
>
> _Evidence / revision:_ Add these definitions and the Sachs matched-cell example to Sections 2–3, and consistently use “name-mediated support” when anonymization cannot identify the source of the model’s knowledge.
>
> **W3 — Perceived unfair framing of semantic evidence relative to numerical data**
>
> _Response:_ We agree that neither source is intrinsically privileged. Semantic knowledge can supply valuable expertise or encode misleading associations; numerical evidence is informative only under causal, sampling, and measurement assumptions. CausalMix measures their attribution and complementarity rather than treating a data-only method as epistemic truth. In controlled simulations, all methods are scored against the known generating DAG; PC, GES, and ENCO are matched references under their assumptions, not gold standards for real scientific data. Indeed, the strong real-versus-anonymous names-only gap shows that semantics can be useful.
>
> _Evidence / revision:_ Revise Lines 104–112 and Sections 2/5.3/6 to remove hierarchical language, distinguish generating-graph ground truth from classical reference predictions, and discuss how semantics can either complement or conflict with sampled evidence.
>
> **W4 — Toy or potentially memorized datasets: Asia, Earthquake, and Cancer**
>
> _Response:_ These graphs provide exact ground truth and make a fully matched factorial evaluation feasible, so they are useful controlled attribution probes; they also remain common in recent LLM-CD evaluations, including CausalBench [1], CausalGraphBench [2], and Causal-LLM [3]. We agree they cannot establish broad real-world or pretraining-independent generalization. We added 20-realization matched evaluations on Child (20 nodes), Alarm (37), and two newly generated 25-node topologies. After collapsing exact duplicate directed edges, Stage 2 (after GRPO) validity-inclusive data-bearing F1 was only 0.022–0.104, with validity falling to 60% for real-name Alarm prompts because self-loops and truncated responses remain invalid. The new topologies reduce direct benchmark-graph familiarity, but no black-box experiment can audit all pretraining exposure. We therefore interpret names-only performance as name-mediated support, not proof of transferable semantic reasoning.
>
> _Evidence / revision:_ Add the larger/new-topology results and validity rates to Section 5.1; revise Section 6 and Appendix B to limit the empirical claim to the tested models, representations, and graph regimes.
>
> **L1 — Request for experiments on newer benchmarks**
>
> _Response:_ “Latest benchmark” could refer to a modern LLM-CD suite or to a recent real perturbation dataset. We chose newly generated and larger matched graphs because they directly test the size and familiarity concerns while retaining known ground truth and matched interventions. We did **not** integrate a contemporary external perturbation benchmark during the rebuttal and will not present the new synthetic controls as if we had. The runner and released 14-graph instance support such extensions, but extensibility is not completed validation.
>
> _Evidence / revision:_ State this scope explicitly in Section 6/Appendix B and identify contemporary real perturbation evaluation as future work. Add the exact public dataset URL and artifact inventory: https://huggingface.co/datasets/mixcausalbench/anonymous-data.
>
> **Q1 — Questions refer back to the weakness section**
>
> _Response:_ Addressed individually in W1–W4. In summary, we will center the joint attribution protocol, define every controlled factor operationally, treat semantics and data as complementary evidence sources, and narrow all generalization and SFT claims to what the new graph-disjoint, larger-graph, and repeated-seed evidence supports.

> **References**
>
> [1] Yu Zhou, Xingyu Wu, Beicheng Huang, Jibin Wu, Liang Feng, and Kay Chen Tan. *CausalBench: A Comprehensive Benchmark for Causal Learning Capability of LLMs*. 2024.
>
> [2] Nikolay Babakov, Ehud Reiter, and Alberto Bugarín. *CausalGraphBench: a Benchmark for Evaluating Language Models capabilities of Causal Graph discovery*. 2025.
>
> [3] Amartya Roy, N Devharish, Shreya Ganguly, and Kripabandhu Ghosh. *Causal-LLM: A Unified One-Shot Framework for Prompt- and Data-Driven Causal Graph Discovery*. 2025.


---

## Official review — Reviewer mUVt

**Title:** Interesting ablation study with a flawed SFT experiment design  
**Posted:** 24 Jun 2026, 19:02  
**Modified:** 23 Jul 2026, 11:25  
**Visible to:** Program Chairs, Senior Area Chairs, Area Chairs, Reviewers Submitted, Authors, Reviewer mUVt  
**Revision status:** Revisions

### Summary

The paper studies hybrid causal discovery. In this setting, a method uses both variable meanings and numerical data to recover a causal graph. The authors argue that current LLM evaluations often mix these signals. A model may score well because it knows what variable names imply, not because it used the data. The main contribution is CAUSALMIX, a benchmark suite that controls this issue. It compares names-only prompts, real-name prompts with data, anonymized prompts with the same data, different data formats, and classical data-only baselines. The authors evaluate several LLMs on graphs such as Cancer, Earthquake, Asia, and Sachs. They find that LLMs recover substantial structure from names alone. Adding data gives limited and format-sensitive gains. Classical methods remain stronger when the statistical setting fits their assumptions. The paper also includes a preliminary fine-tuning experiment for Qwen3-4B. This improves output validity and some graph-recovery scores.

**Contribution Type Check:** Yes

### Strengths

The paper makes a clear and useful contribution to LLM-based causal discovery. Its main strength is the attribution strategy. The authors show that a high graph-recovery score is not enough, since an LLM may recover edges from variable names rather than from the supplied data. The work is significant because it exposes a key limitation of current LLM-CD methods. The reported gains often appear to come largely from semantic priors rather than robust use of observational or interventional data. This is an important diagnostic for the field.

The empirical evaluation is broad. The authors test many baselines, including LLM-based methods and conventional causal discovery methods such as PC, GES, and ENCO. This makes the comparisons more informative than evaluating LLMs in isolation.

The authors are appropriately transparent about limitations. In particular, they acknowledge the limited statistical power in parts of the study and treat some findings as preliminary rather than definitive. This makes the contribution more credible.

### Weaknesses

The SFT experiment has a weak train/test split. The training data uses the same graph families that are later evaluated. Stage 1 includes Cancer, Earthquake, Asia, and Sachs. Stage 2 includes Earthquake, Asia, and Sachs. The evaluation is again on Cancer, Earthquake, Asia, and Sachs. This makes it hard to tell whether the model learned to use numerical evidence, or simply learned the graph structures, output format, and verifier preferences. A graph-disjoint evaluation would be needed to support stronger claims.

The limitation on statistical power is real. Many repeated cells use only five data realizations. The bootstrap intervals are useful, but the authors state that they are descriptive rather than high-powered tests. This weakens claims about small F1 differences, such as data gains, format effects, anonymization drops, and model comparisons. The broad patterns are still useful, but small effects should be interpreted cautiously.

The studied causal graphs are small and well-known. Cancer and Earthquake have only five nodes, Asia has eight, and Sachs has eleven. These are common benchmark graphs, so they may already be familiar to LLMs through pretraining. It is therefore somewhat expected that GPT-style models can recover structure from variable names alone. The paper would be stronger if it tested whether the same pattern holds on larger, less popular, or newly constructed causal graphs.

### Assessment details

**Ethical Concerns:** No or very minor ethics concerns only  
**Reproducibility:** Yes  
#### Reproducibility Comments

The authors provide the anonymous codebase (through complementary material in openreview) and dataset (hf). I did not run them manually but everything seems pretty organized and straightforward to reproduce with API keys.

**Dataset Assessment:** Yes

#### Dataset Comments

See above.

### Limitations

Overall, I would say the limitations are honestly discussed, but not fully resolved. The paper is more of a benchmark and evaluation-protocol contribution. It is weaker as (formal) evidence about current LLMs’ general ability to use causal data or about the effectiveness of post-training.

### Questions

Could the authors clarify the intended claim of the SFT experiment? Since training and evaluation use the same graph families, gains may reflect simply memorization. A small leave-one-graph-out evaluation would make this much stronger.

The names-only result may be affected by the popularity of Cancer, Earthquake, Asia, and Sachs. Can the authors test less common or newly constructed named graphs? This would help separate semantic reasoning from benchmark memorization.

The main matched experiments are on small graphs. Can the authors add a lightweight matched evaluation on a larger graph, even with fewer models or summary-only prompts? This would clarify whether the attribution findings scale.

### Score

**Rating:** 4: Borderline accept: Technically solid paper where reasons to accept outweigh reasons to reject, e.g., limited evaluation. Please use sparingly.  
**Confidence:** 3: You are fairly confident in your assessment. It is possible that you did not understand some parts of the submission or that you are unfamiliar with some pieces of related work. Math/other details were not carefully checked.

### Paper Formatting Concerns

N/A

**LLM Policy:** Yes  
**Code Of Conduct Acknowledgement:** Yes  
**Responsible Reviewing Acknowledgement:** Yes

> [!IMPORTANT]
> **Draft rebuttal — Reviewer mUVt**
>
> **W1 — Non-disjoint SFT train/test split and possible memorization**
>
> _Response:_ We agree this is the decisive validity test and implemented the suggested strict leave-one-graph-out (LOGO) design. Each of Cancer, Earthquake, Asia, and Sachs was excluded from both training stages—SFT and GRPO; training restarted from Qwen3-4B-Thinking-2507 and was repeated with seeds 42, 314, and 2718. Final Stage 1 (after SFT) reached 100% aggregate output validity in every held-out fold versus 69%–86% for Base, showing that the structured-output gain transfers beyond trained graph families. However, Stage 2 (after GRPO) did not consistently improve numerical-data use beyond Final Stage 1 (after SFT):
>
> | Held-out graph | GRPO increment over SFT data gain, real [95% CI] | Anonymous [95% CI] |
> |---|---:|---:|
> | Asia | -0.071 [-0.116, -0.028] | +0.028 [0.003, 0.054] |
> | Cancer | +0.003 [-0.056, 0.061] | +0.004 [-0.056, 0.069] |
> | Earthquake | -0.025 [-0.074, 0.027] | +0.037 [0.012, 0.064] |
> | Sachs | -0.041 [-0.072, -0.011] | +0.018 [-0.013, 0.048] |
>
> _Evidence / revision:_ Add the split manifests, all four folds, three training seeds, checkpoint comparisons, and paired intervals to Section 5.2/Appendix F. Attribute transferable output-contract learning to SFT, and remove the claim that the subsequent GRPO stage establishes broadly improved numerical integration.
>
> **W2 — Low statistical power from only five data realizations per cell**
>
> _Response:_ We expanded the prespecified Sachs slice to 30 paired data realizations and computed paired bootstrap intervals and sign-flip tests:
>
> | Data minus no data | Real names, mean [95% CI] | Anonymous, mean [95% CI] |
> |---|---:|---:|
> | Base | -0.190 [-0.240, -0.138] | +0.230 [0.207, 0.253] |
> | GPT-5-mini | -0.087 [-0.104, -0.069] | +0.039 [0.017, 0.062] |
> | + SFT | -0.169 [-0.223, -0.110] | +0.061 [0.022, 0.100] |
> | + SFT + GRPO | -0.240 [-0.297, -0.181] | +0.061 [0.026, 0.096] |
>
> The broad naming-condition dependence is stable, whereas the incremental effect of GRPO over Final Stage 1 is not distinguishable from zero. We therefore do not claim that five seeds suffice for small effects.
>
> _Evidence / revision:_ Add the 30-seed estimates and tests to Section 5.1/Appendix E.3; retain the remaining five-run grid as descriptive and avoid robust interpretations of small deltas. The replication resolves this Sachs slice, not every model/graph/format cell.
>
> **W3 — Small, popular causal graphs and uncertain generalization**
>
> _Response:_ We added a matched 20-realization evaluation on Child, two newly generated 25-node graphs, and Alarm. Under canonical controlled decoding, Stage 2 (after GRPO) results were:
>
> | Graph | Real validity / F1 | Anonymous validity / F1 |
> |---|---:|---:|
> | Child (20) | 100% / 0.070 | 100% / 0.104 |
> | Chain-25 | 90% / 0.049 | 100% / 0.069 |
> | Jungle-25 | 95% / 0.044 | 100% / 0.095 |
> | Alarm (37) | 60% / 0.022 | 90% / 0.031 |
>
> Exact duplicate directed edges are collapsed before scoring because they do not change the predicted graph. Self-loops, truncated responses, and other invalid outputs remain invalid and receive F1=0. The results extend the diagnostic beyond the submitted four graphs but show poor recovery and residual structured-output failures on Alarm, not successful scale generalization. The new topologies and anonymization reduce direct familiarity, but pretraining memorization remains unresolved.
>
> _Evidence / revision:_ Add the matched cells and validity/F1 table to Section 5.1; report prompt-length/serialization limitations separately; restrict conclusions to evaluated settings.
>
> **Q1 — What claim is intended for the SFT experiment, and can it be tested graph-disjointly?**
>
> _Response:_ The intended revised claim is: SFT transfers the required structured-output contract to unseen graph families, but the subsequent GRPO stage does not consistently improve numerical-evidence integration beyond Final Stage 1 (after SFT). The strict four-fold, three-seed LOGO results in W1 provide the requested graph-disjoint test.
>
> _Evidence / revision:_ Recast Figure 4 and the Conclusion accordingly, with post-training secondary to the benchmark contribution.
>
> **Q2 — Can we test less common or newly constructed named graphs?**
>
> _Response:_ Partially. Chain-25 and Jungle-25 are newly generated topologies, and the graph-disjoint suite also includes Child and Alarm. Their matched results are reported in W3. We also paired familiar Child names with five rewired generating graphs to test semantic–data conflict. These controls address SFT-family overlap and benchmark-topology familiarity, but we did not construct a new named scientific domain and cannot rule out pretraining-derived priors.
>
> _Evidence / revision:_ Release the graph files and seeds, add the results to the graph inventory, and use “name-mediated support” rather than “semantic reasoning.”
>
> **Q3 — Can we add a matched evaluation on a larger graph?**
>
> _Response:_ Yes. We added the full matched real-name/anonymized and data/no-data design on four 20–37-node graphs with 20 paired realizations per contrast. As W3 shows, the negative result is informative for diagnostic scope but does not establish that current prompt-based methods scale.
>
> _Evidence / revision:_ Add the compact table, exact protocol, validity counts, and validity-inclusive scoring to the revision.


---

## Official review — Reviewer zDep

**Title:** review  
**Posted:** 24 Jun 2026, 05:30  
**Modified:** 23 Jul 2026, 11:25  
**Visible to:** Program Chairs, Senior Area Chairs, Area Chairs, Reviewers Submitted, Authors, Reviewer zDep  
**Revision status:** Revisions

### Summary

The paper primarily investigates whether causal graph recovery in LLM-based causal discovery relies on semantic priors or on the underlying data itself. To this end, the authors propose CAUSALMIX, a benchmark framework that systematically evaluates and analyzes model behavior by independently controlling variable semantics, data availability, and data representations. Experimentally, the paper compares several LLM families, including GPT, Qwen, and Llama, as well as classical causal discovery methods. The paper maintains a clear and focused research objective.

**Contribution Type Check:** Yes

### Strengths

Focused and well-motivated contribution: The paper concentrates on a fundamental question in causal graph recovery: what is the primary source of performance gains, semantic priors or numerical data? This issue extends beyond causal discovery and is broadly relevant to many LLM-based reasoning tasks. Therefore, the core contribution is meaningful and sufficiently important.

Well-designed benchmark framework: To rigorously evaluate the contribution of each information source, CAUSALMIX introduces four controlled experimental settings. The benchmark systematically isolates different factors and analyzes their effects through detailed F1-based comparisons. Overall, the evaluation protocol is carefully designed and logically structured.

Meaningful empirical findings: The experimental results suggest that many existing LLM-based causal discovery (LLM-CD) methods may rely more heavily on semantic priors than on genuine utilization of numerical evidence for causal graph recovery. This finding provides valuable insights into the limitations of current approaches and may inspire new directions for future research.
### Weaknesses

Limited persuasiveness of the graph structures used in the evaluation: The main experiments are conducted on graphs containing only 5–11 nodes. Such graphs are relatively small, with limited structural complexity and a constrained search space, yet the paper draws its primary conclusions from these settings. Although the authors release larger graphs as part of the benchmark, they do not perform sufficiently extensive evaluations on them. As a result, the propose of the paper is that current LLM-based causal discovery methods rely primarily on semantic information rather than numerical evidence remains insufficiently supported.

The verifier-based post-training approach is not fully convincing: As acknowledged by the authors, the training graphs and evaluation graphs are not completely disjoint. Furthermore, the paper lacks sufficiently strong ablation studies, adversarial evaluations, or controlled experiments to isolate the source of the observed improvements. Therefore, it is difficult to determine whether the reported gains truly stem from the proposed training strategy or from other confounding factors.

The experimental evidence is not sufficiently strong: Many of the reported performance differences are relatively small (e.g., F1 improvements of 0.05–0.10). It is unclear whether these differences reflect genuine performance gains or simply model variance and randomness. The paper lacks sufficiently thorough statistical analysis, additional experiments, or qualitative case studies to demonstrate that the observed improvements are meaningful and robust.

Insufficient diversity of evaluated models: As a benchmark paper, an important objective is to demonstrate the generality of its core findings across a broad range of LLMs. Therefore, evaluations should ideally cover both open-source and closed-source models with diverse architectures and capabilities. However, the current study only includes GPT, Qwen, and Llama families, which do not adequately represent either the classical models that have shaped the field or several state-of-the-art frontier models. As a result, it remains unclear whether the observed phenomenon is a general characteristic of modern LLMs or merely specific to the selected model families. Expanding the evaluation to include additional models would substantially strengthen the benchmark's conclusions and external validity.

### Assessment details

**Ethical Concerns:** No or very minor ethics concerns only  
**Reproducibility:** Yes  
#### Reproducibility Comments

The paper provides a relatively detailed description of the benchmark construction process, evaluation protocol, experimental settings, and metrics, which would allow readers to reproduce most of the methodology at a conceptual level.

**Dataset Assessment:** Partly

#### Dataset Comments

Unable to access the URL

### Limitations

yes

### Questions

Evaluation on larger graphs: The main conclusions are derived from relatively small graphs. Could the authors provide additional experiments on larger graphs to further validate their findings? Such evaluations would help assess whether the observed patterns generalize to more complex causal structures and would strengthen the evidence supporting the paper’s claims.

Indirectly ensuring train-test independence in post-training: The verifier-based post-training results may be affected by the overlap between training and evaluation graphs. Could the authors explore mechanisms to better ensure train-test independence, for example by controlling specific nodes, replacing semantically irrelevant variables, or modifying graph components while preserving overall structure? Such experiments would provide stronger evidence that the observed gains arise from the proposed training strategy rather than memorization.

Statistical significance analysis: Many reported improvements are relatively small in magnitude. Could the authors provide statistical significance tests, confidence intervals, or additional repeated runs to demonstrate that these improvements are attributable to the proposed method rather than random variation in model behavior?

Evidence supporting the claim that LLMs struggle to utilize numerical evidence: The propose of the paper is that current LLMs have difficulty leveraging numerical evidence for causal discovery. However, the paper provides limited direct evidence supporting this conclusion. Could the authors provide additional experiments, analyses, or references from prior literature to substantiate this claim? Otherwise, the motivation and interpretation of several findings may be less convincing.

Broader coverage of LLMs: The benchmark evaluation focuses primarily on GPT, Qwen, and Llama families. However, several influential frontier models, such as Gemini, Claude, and DeepSeek, are not included. As a benchmark paper, a broader model coverage would be important for demonstrating that the identified phenomenon is prevalent across the current LLM landscape. Without such evidence, it remains unclear whether the observed behavior is a general limitation of modern LLMs or merely a characteristic of the selected model families.

### Score

**Rating:** 3: Borderline reject: Technically solid paper where reasons to reject, e.g., limited evaluation, outweigh reasons to accept, e.g., good evaluation. Please use sparingly.  
**Confidence:** 4: You are confident in your assessment, but not absolutely certain. It is unlikely, but not impossible, that you did not understand some parts of the submission or that you are unfamiliar with some pieces of related work.

### Paper Formatting Concerns

One of the URLs provided in the paper could not be accessed during review, which made it difficult to verify the corresponding resource.

**LLM Policy:** Yes  
**Code Of Conduct Acknowledgement:** Yes  
**Responsible Reviewing Acknowledgement:** Yes

> [!IMPORTANT]
> **Draft rebuttal — Reviewer zDep**
>
> **W1 — Insufficient evaluation on larger and more complex graphs**
>
> _Response:_ We agree that the submitted 5–11-node results cannot support broad scale generalization. We added the full matched design on Child (20 nodes), two newly generated 25-node graphs, and Alarm (37 nodes), with 20 paired realizations per contrast. Recovery remained low and validity worsened with scale; this extends the diagnosis beyond the original graphs but is a negative capability result, not evidence of successful large-graph LLM causal discovery.
>
> _Evidence / revision:_ Add the validity/F1 table in Q1 to Section 5.1; revise Section 6 and Appendix B to restrict claims to the evaluated models, representations, and graph sizes.
>
> **W2 — Post-training evidence does not isolate the source of improvement**
>
> _Response:_ The new diagnostics identify structured-output learning as the most robust gain, not numerical-reasoning improvement:
>
> | Candidate explanation | New diagnostic | Supported conclusion |
> |---|---|---|
> | Output-contract learning | In strict LOGO, validity rises from 69%–86% for Base to 100% with + SFT; + SFT + GRPO remains near 100%. | Strongly supported. |
> | Training-graph memorization | Validity gains persist when the held-out graph is excluded from both SFT and GRPO. | Not necessary for format gain. |
> | Semantic/pretraining priors | LOGO names-only F1: + SFT, 0.498 real vs. 0.157 anonymous; + SFT + GRPO, 0.489 vs. 0.134. | Strong name dependence; source unresolved. |
> | Verifier preference | No verifier-free, target-matched checkpoint is available. | Still confounded with format/content imitation. |
> | Numerical integration | Graph-disjoint GRPO increment over SFT data-gain effects range from -0.055 to +0.005. | Small and inconsistent. |
>
> Final Stage 1 (after SFT) includes gold graph targets, so it is not a pure schema-only control. We therefore cannot fully separate verifier preference or pretraining familiarity and will not claim otherwise.
>
> _Evidence / revision:_ Add the strict LOGO and graph-disjoint ablations to Section 5.2/Appendix F. Recast SFT as a diagnostic example that improves output reliability and remove the conclusion’s claim of broadly improved numerical integration.
>
> **W3 — Small effects without sufficiently strong statistical or qualitative evidence**
>
> _Response:_ We expanded the primary Sachs slice to 30 paired data realizations and added paired bootstrap intervals and sign-flip tests. The broad semantic-condition effects are stable, whereas the incremental effect of GRPO over Final Stage 1 (after SFT) includes zero. We also audited edge transitions: some edges are repeatedly corrected by data while others repeatedly regress, demonstrating local responsiveness but unstable integration.
>
> _Evidence / revision:_ Add the quantitative table in Q3 and the representative edge cases there to Section 5.1/Appendix E.3; treat the remaining five-run cells as descriptive and do not interpret isolated 0.05–0.10 differences as robust.
>
> **W4 — Limited diversity of evaluated LLM families**
>
> _Response:_ We agree that broader independent-family coverage would improve external validity. The submission evaluates ten models from GPT-5, Llama-3.1, Qwen2.5, and Qwen3, and the rebuttal adds a complete 30-seed GPT-5-mini replication. We also conducted preliminary matched Sachs checks with DeepSeek-R1-Distill-Llama-8B and Mistral-Nemo-Instruct-2407. DeepSeek returned self-loops in both data-present conditions in its second realization. Mistral returned duplicate and self-loop edges in a names-only, real-name condition and also raised a tokenizer-compatibility warning. We stopped both runs and exclude them from quantitative comparisons. These are output-validity failures under our current inference setup, not evidence that either model cannot perform causal discovery or use numerical data. We therefore do not claim reliable controlled Gemini, Claude, or independent frontier-DeepSeek coverage.
>
> _Evidence / revision:_ Restrict all empirical conclusions to the evaluated GPT/Qwen/Llama slices; report failed structured-output coverage separately if space permits; identify independent frontier-family evaluation as future work.
>
> **Q1 — Can we provide additional experiments on larger graphs?**
>
> _Response:_ Yes. Under the canonical controlled-decoding protocol, the principal Stage 2 (after GRPO) data-bearing results are:
>
> | Graph | Real-name validity | Real-name F1 | Anonymous validity | Anonymous F1 |
> |---|---:|---:|---:|---:|
> | Child (20) | 100% | 0.070 | 100% | 0.104 |
> | Chain-25 | 90% | 0.049 | 100% | 0.069 |
> | Jungle-25 | 95% | 0.044 | 100% | 0.095 |
> | Alarm (37) | 60% | 0.022 | 90% | 0.031 |
>
> Exact duplicate directed edges are collapsed before scoring because they do not change the predicted graph. Self-loops, truncated responses, and other invalid outputs remain invalid and receive F1=0. Under the same scoring, the Stage-2-minus-Final-Stage-1 data-gain changes were -0.092/-0.013 on Alarm, -0.022/-0.055 on Chain-25, +0.039/+0.005 on Child, and -0.025/+0.002 on Jungle-25 (real/anonymous). Thus, the effects were mostly negative or near zero, with a modest positive Child real-name exception. Long numerical serialization and structured-output difficulty are real limitations of the evaluated interface, so validity is reported separately rather than treating invalid output as proof of causal inability.
>
> _Evidence / revision:_ Report all matched conditions, 20-realization counts, prompt validity, and prompt lengths; state that CausalMix supplies a scale diagnostic and extensible runner, not evidence of successful large-graph recovery.
>
> **Q2 — How can we better ensure post-training train/test independence?**
>
> _Response:_ We ran strict LOGO training: each evaluation graph was removed from both SFT and GRPO, training restarted from the base Qwen3-4B-Thinking-2507 model, and all four folds were repeated with three training seeds. The incremental GRPO-over-Final-Stage-1 data gains were:
>
> | Held-out graph | Real names, mean [95% CI] | Anonymous, mean [95% CI] |
> |---|---:|---:|
> | Asia | -0.071 [-0.116, -0.028] | +0.028 [0.003, 0.054] |
> | Cancer | +0.003 [-0.056, 0.061] | +0.004 [-0.056, 0.069] |
> | Earthquake | -0.025 [-0.074, 0.027] | +0.037 [0.012, 0.064] |
> | Sachs | -0.041 [-0.072, -0.011] | +0.018 [-0.013, 0.048] |
>
> _Evidence / revision:_ Add the split manifests, all 12 fold-seed runs, checkpoint comparisons, and paired intervals. The result supports transferable format reliability, not consistent numerical-reasoning improvement.
>
> **Q3 — Can we provide significance tests, confidence intervals, or additional runs?**
>
> _Response:_ Yes. For 30 paired Sachs realizations:
>
> | Data minus no data | Real names, mean [95% CI] | Anonymous, mean [95% CI] |
> |---|---:|---:|
> | Base | -0.190 [-0.240, -0.138] | +0.230 [0.207, 0.253] |
> | GPT-5-mini | -0.087 [-0.104, -0.069] | +0.039 [0.017, 0.062] |
> | + SFT | -0.169 [-0.223, -0.110] | +0.061 [0.022, 0.100] |
> | + SFT + GRPO | -0.240 [-0.297, -0.181] | +0.061 [0.026, 0.096] |
>
> Paired sign-flip tests accompany the intervals. In 59 valid real-name Sachs LOGO pairs, Jnk–P38 and Erk–P38 were beneficially corrected 36 times each (2 and 0 harmful regressions), while PIP2–PKA and Mek–Plcg regressed 30 times each (0 and 1 corrections). Erk–PKA showed both 18 corrections and 26 regressions. These cases illustrate why aggregate F1 can hide offsetting local behavior.
>
> _Evidence / revision:_ Add paired intervals, tests, valid-pair counts, and a compact edge-case table; retain five-run cells as descriptive and avoid claiming that five seeds suffice generally.
>
> **Q4 — What direct evidence supports the claim that LLMs struggle to use numerical evidence?**
>
> _Response:_ The direct edge-level audit changes our wording: models do **not** ignore data; they respond locally, but the response is unstable and often harmful. Among matched valid Stage 2 (after GRPO) pairs:
>
> | Evaluation | Edges changed | Beneficial | Harmful | Net benefit |
> |---|---:|---:|---:|---:|
> | Graph-disjoint, real | 25.3% | 4.8% | 18.5% | -13.6 pp |
> | Graph-disjoint, anonymous | 37.7% | 5.4% | 29.8% | -24.4 pp |
> | LOGO, real | 28.1% | 11.4% | 15.0% | -3.6 pp |
> | LOGO, anonymous | 25.5% | 9.8% | 12.3% | -2.5 pp |
> | Sachs 30-seed, anonymous | 42.2% | 10.9% | 25.5% | -14.7 pp |
>
> A complementary conflict test on five rewired Child graphs reduced data-following by 1.6 pp with real names and 7.9 pp anonymously while increasing unresolved choices by 6.0 and 14.5 pp. Thus, partial evidence use exists, but beneficial integration is not reliable.
>
> _Evidence / revision:_ Add edge-change, correction, regression, net-benefit, and conflict-following rates; replace “models ignore data” with “partial local responsiveness with unstable and frequently detrimental integration.” Do not use the shuffled-data placebo, whose prompts exceeded the context limit.
>
> **Q5 — Can we broaden coverage to models such as Gemini, Claude, and DeepSeek?**
>
> _Response:_ We agree this would strengthen external validity, but we do not have reliable controlled Gemini, Claude, or frontier-DeepSeek results and therefore cannot claim that the phenomenon is universal. The submitted ten-model roster spans GPT, Qwen, and Llama; the new complete replication is GPT-5-mini. In preliminary matched Sachs checks, DeepSeek-R1-Distill-Llama-8B generated self-loops in both data-present conditions in its second realization. Mistral-Nemo-Instruct-2407 generated duplicate and self-loop edges in a names-only, real-name condition and raised a tokenizer-compatibility warning. We therefore stopped these runs and exclude them from quantitative comparisons. These failures concern graph-output validity under our current inference setup; they do not show that either model cannot perform causal discovery or use numerical data.
>
> _Evidence / revision:_ Narrow the claim to evaluated GPT/Qwen/Llama slices and emphasize that the released adapter permits future systems to be evaluated under the identical matched protocol.
>
> **F1 — One provided resource URL was inaccessible**
>
> _Response:_ Thank you for reporting this. We verified the dataset on July 24, 2026 from two unauthenticated browsers on separate networks and downloaded the complete archive successfully. The direct URL is [https://huggingface.co/datasets/mixcausalbench/anonymous-data](https://huggingface.co/datasets/mixcausalbench/anonymous-data).
>
> _Evidence / revision:_ Put the exact link in the rebuttal and manuscript, and add a file inventory, archive SHA-256, access date, and minimal download/regeneration commands. We treat the inconsistent reviewer experience as an artifact-documentation problem even though the link is now reachable.
