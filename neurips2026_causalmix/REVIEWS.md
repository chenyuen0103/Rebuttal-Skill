# Reviews for “CausalMix: Defining and Benchmarking Hybrid Causal Discovery”

> Verbatim transcription from the supplied OpenReview export. Only Markdown presentation has been added; review wording is unchanged.

Meta Review of Submission3307 by Area Chair Kf9w
Meta Reviewby Area Chair Kf9w21 Jul 2026, 10:20 (modified: 23 Jul 2026, 12:23)Senior Area Chairs, Area Chairs, Authors, Reviewers Submitted, Program Chairs, Area Chair Kf9wRevisions
Metareview:
Scientific Contribution. CausalMix formalizes hybrid causal analysis, in which models receive both semantic information about variables and numerical observations or interventions. The controlled evaluation distinguishes between "names-only," "data-only," and combined scenarios to determine whether LLM performance relies on semantic prior knowledge or the actual use of numerical evidence. Experiments suggest that LLMs reconstruct substantial graph structures from variable names, whereas numerical data offer only limited and format-dependent benefits. The paper also presents a preliminary, verifier-based fine-tuning approach.

Strengths. The controlled attribution model addresses a key methodological issue in LLM-based causal analysis: high graph reconstruction scores do not necessarily prove that a model utilized the provided data. Comparing scenarios involving purely semantic information, hybrid data, and classical data is useful and potentially widely applicable. Comparisons with classical causal analysis algorithms and a generally transparent discussion of limitations represent further strengths.

Main Criticisms. The primary criticism is that the key conclusions rely largely on four small, widely known graphs containing 5–11 nodes. These graph structures and variable names may already be present in the model's pre-training data; consequently, the strong results observed in the "names-only" scenarios may stem partly from rote memorization. Thus, the findings do not demonstrate that the observed behavior generalizes to larger, less common, or newly generated causal structures.

Furthermore, the fine-tuning experiment lacks sufficient validity: the training and evaluation phases utilize overlapping graph families, meaning that improvements could result from memorizing structures, output formats, or the verifier's preferences rather than from an improved integration of semantic and numerical evidence. A graph-disjoint evaluation is required.

After all, many experimental cells contain only a few realizations, and some of the reported differences are small. More robust uncertainty estimates and repeated runs are necessary before these effects can be interpreted as robust. Furthermore, model coverage is lower than expected for a benchmark-based claim, and some key concepts and methodological details require clearer definition.

Add:
Review of paper 3307
Official Reviewby Reviewer svky25 Jun 2026, 23:04 (modified: 23 Jul 2026, 11:25)Program Chairs, Senior Area Chairs, Area Chairs, Reviewers Submitted, Authors, Reviewer svkyRevisions
Summary:
The paper studies hybrid causal discovery methods (LLM + statistical methods) in three aspects: (1) formally define a hybrid causal discovery method; (2) introduce an evaluation frame that checks the effectiveness of LLM and statistical-based components separately; (3) fine-tune an LLM to complete causal discovery tasks. Additionally, the work introduces a simulator to generate benchmarks for hybrid causal discovery method evaluation. The work evaluates hybrid methods based on several LLM models and several statistical-based causal discovery methods by their evaluation frame, concluding that pure LLM-based methods perform well on several small datasets, while statistical-based methods are stronger for larger-scale tasks. They also show that task-driven fine-tuning can significantly improve the performance of LLM-based methods.

Contribution Type Check: Yes
Strengths:
The paper formalises a frame for the hybrid causal discovery methods to evaluate the performances of the statistical-based component and the semantic-based component separately. Such an evaluation process would be helpful to improve the hybrid causal discovery methods.

Weaknesses:
The paper’s novelty is limited. It may try to shape the evaluation frame as a standard for hybrid methods’ evaluation, however, many of the components have been used in existing works. The hybrid method’s definition is also obvious. Fine-tuning LLMs to handle causal discovery tasks also exists in previous works.
The presentation needs improvement. I could not clearly understand every detail of the method, e.g., many terms do not have an explanation. To name a few: “semantic cue”, “matched attribution problem”. I believe the authors need to significantly revise the methodology to explicitly describe the components instead of only provide intuition with such vague terms.
Another concern is the paper’s unfair attitude towards the semantic component (e.g., Lines 104-112). My understanding is that the authors tend to believe that the numerical data is the gold standard for causal discovery, but the semantic evidences are not that reliable. However, in practice, a numerical dataset also suffers from bias, and in contrast, semantic evidence may help debias the result. For instance, LLMs provide suggestions based on a vast domain research training dataset, potentially more reliable than a single dataset.
The authors use several toy datasets, especially, the Asia, Earthquake, and Cancer. These datasets may be out of date, and moreover, some of them contain semi-synthetic data, hence not suitable for evaluating LLM-based methods. The paper also shows that a simple strategy to use pure LLM-based methods can achieve good performances. This may be because of LLMs’ memory of the corresponding datasets.
Ethical Concerns: No or very minor ethics concerns only
Reproducibility: Yes
Dataset Assessment: NA - no dataset included
Limitations:
I would recommend that the authors add experiments on the latest benchmarks to test the performances.

Questions:
See the weakness section.

Rating: 2: Reject: For instance, a paper with technical flaws, weak evaluation, inadequate reproducibility and incompletely addressed ethical considerations.
Confidence: 4: You are confident in your assessment, but not absolutely certain. It is unlikely, but not impossible, that you did not understand some parts of the submission or that you are unfamiliar with some pieces of related work.
Paper Formatting Concerns:
No concern.

LLM Policy: Yes
Code Of Conduct Acknowledgement: Yes
Responsible Reviewing Acknowledgement: Yes
Add:
Interesting ablation study with a flawed SFT experiment design
Official Reviewby Reviewer mUVt24 Jun 2026, 19:02 (modified: 23 Jul 2026, 11:25)Program Chairs, Senior Area Chairs, Area Chairs, Reviewers Submitted, Authors, Reviewer mUVtRevisions
Summary:
The paper studies hybrid causal discovery. In this setting, a method uses both variable meanings and numerical data to recover a causal graph. The authors argue that current LLM evaluations often mix these signals. A model may score well because it knows what variable names imply, not because it used the data. The main contribution is CAUSALMIX, a benchmark suite that controls this issue. It compares names-only prompts, real-name prompts with data, anonymized prompts with the same data, different data formats, and classical data-only baselines. The authors evaluate several LLMs on graphs such as Cancer, Earthquake, Asia, and Sachs. They find that LLMs recover substantial structure from names alone. Adding data gives limited and format-sensitive gains. Classical methods remain stronger when the statistical setting fits their assumptions. The paper also includes a preliminary fine-tuning experiment for Qwen3-4B. This improves output validity and some graph-recovery scores.

Contribution Type Check: Yes
Strengths:
The paper makes a clear and useful contribution to LLM-based causal discovery. Its main strength is the attribution strategy. The authors show that a high graph-recovery score is not enough, since an LLM may recover edges from variable names rather than from the supplied data. The work is significant because it exposes a key limitation of current LLM-CD methods. The reported gains often appear to come largely from semantic priors rather than robust use of observational or interventional data. This is an important diagnostic for the field.

The empirical evaluation is broad. The authors test many baselines, including LLM-based methods and conventional causal discovery methods such as PC, GES, and ENCO. This makes the comparisons more informative than evaluating LLMs in isolation.

The authors are appropriately transparent about limitations. In particular, they acknowledge the limited statistical power in parts of the study and treat some findings as preliminary rather than definitive. This makes the contribution more credible.

Weaknesses:
The SFT experiment has a weak train/test split. The training data uses the same graph families that are later evaluated. Stage 1 includes Cancer, Earthquake, Asia, and Sachs. Stage 2 includes Earthquake, Asia, and Sachs. The evaluation is again on Cancer, Earthquake, Asia, and Sachs. This makes it hard to tell whether the model learned to use numerical evidence, or simply learned the graph structures, output format, and verifier preferences. A graph-disjoint evaluation would be needed to support stronger claims.

The limitation on statistical power is real. Many repeated cells use only five data realizations. The bootstrap intervals are useful, but the authors state that they are descriptive rather than high-powered tests. This weakens claims about small F1 differences, such as data gains, format effects, anonymization drops, and model comparisons. The broad patterns are still useful, but small effects should be interpreted cautiously.

The studied causal graphs are small and well-known. Cancer and Earthquake have only five nodes, Asia has eight, and Sachs has eleven. These are common benchmark graphs, so they may already be familiar to LLMs through pretraining. It is therefore somewhat expected that GPT-style models can recover structure from variable names alone. The paper would be stronger if it tested whether the same pattern holds on larger, less popular, or newly constructed causal graphs.

Ethical Concerns: No or very minor ethics concerns only
Reproducibility: Yes
Reproducibility Comments:
The authors provide the anonymous codebase (through complementary material in openreview) and dataset (hf). I did not run them manually but everything seems pretty organized and straightforward to reproduce with API keys.

Dataset Assessment: Yes
Dataset Comments:
See above.

Limitations:
Overall, I would say the limitations are honestly discussed, but not fully resolved. The paper is more of a benchmark and evaluation-protocol contribution. It is weaker as (formal) evidence about current LLMs’ general ability to use causal data or about the effectiveness of post-training.

Questions:
Could the authors clarify the intended claim of the SFT experiment? Since training and evaluation use the same graph families, gains may reflect simply memorization. A small leave-one-graph-out evaluation would make this much stronger.

The names-only result may be affected by the popularity of Cancer, Earthquake, Asia, and Sachs. Can the authors test less common or newly constructed named graphs? This would help separate semantic reasoning from benchmark memorization.

The main matched experiments are on small graphs. Can the authors add a lightweight matched evaluation on a larger graph, even with fewer models or summary-only prompts? This would clarify whether the attribution findings scale.

Rating: 4: Borderline accept: Technically solid paper where reasons to accept outweigh reasons to reject, e.g., limited evaluation. Please use sparingly.
Confidence: 3: You are fairly confident in your assessment. It is possible that you did not understand some parts of the submission or that you are unfamiliar with some pieces of related work. Math/other details were not carefully checked.
Paper Formatting Concerns:
N/A

LLM Policy: Yes
Code Of Conduct Acknowledgement: Yes
Responsible Reviewing Acknowledgement: Yes
Add:
review
Official Reviewby Reviewer zDep24 Jun 2026, 05:30 (modified: 23 Jul 2026, 11:25)Program Chairs, Senior Area Chairs, Area Chairs, Reviewers Submitted, Authors, Reviewer zDepRevisions
Summary:
The paper primarily investigates whether causal graph recovery in LLM-based causal discovery relies on semantic priors or on the underlying data itself. To this end, the authors propose CAUSALMIX, a benchmark framework that systematically evaluates and analyzes model behavior by independently controlling variable semantics, data availability, and data representations. Experimentally, the paper compares several LLM families, including GPT, Qwen, and Llama, as well as classical causal discovery methods. The paper maintains a clear and focused research objective.

Contribution Type Check: Yes
Strengths:
Focused and well-motivated contribution: The paper concentrates on a fundamental question in causal graph recovery: what is the primary source of performance gains, semantic priors or numerical data? This issue extends beyond causal discovery and is broadly relevant to many LLM-based reasoning tasks. Therefore, the core contribution is meaningful and sufficiently important.
Well-designed benchmark framework: To rigorously evaluate the contribution of each information source, CAUSALMIX introduces four controlled experimental settings. The benchmark systematically isolates different factors and analyzes their effects through detailed F1-based comparisons. Overall, the evaluation protocol is carefully designed and logically structured.
Meaningful empirical findings: The experimental results suggest that many existing LLM-based causal discovery (LLM-CD) methods may rely more heavily on semantic priors than on genuine utilization of numerical evidence for causal graph recovery. This finding provides valuable insights into the limitations of current approaches and may inspire new directions for future research.
Weaknesses:
Limited persuasiveness of the graph structures used in the evaluation: The main experiments are conducted on graphs containing only 5–11 nodes. Such graphs are relatively small, with limited structural complexity and a constrained search space, yet the paper draws its primary conclusions from these settings. Although the authors release larger graphs as part of the benchmark, they do not perform sufficiently extensive evaluations on them. As a result, the propose of the paper is that current LLM-based causal discovery methods rely primarily on semantic information rather than numerical evidence remains insufficiently supported.

The verifier-based post-training approach is not fully convincing: As acknowledged by the authors, the training graphs and evaluation graphs are not completely disjoint. Furthermore, the paper lacks sufficiently strong ablation studies, adversarial evaluations, or controlled experiments to isolate the source of the observed improvements. Therefore, it is difficult to determine whether the reported gains truly stem from the proposed training strategy or from other confounding factors.

The experimental evidence is not sufficiently strong: Many of the reported performance differences are relatively small (e.g., F1 improvements of 0.05–0.10). It is unclear whether these differences reflect genuine performance gains or simply model variance and randomness. The paper lacks sufficiently thorough statistical analysis, additional experiments, or qualitative case studies to demonstrate that the observed improvements are meaningful and robust.

Insufficient diversity of evaluated models: As a benchmark paper, an important objective is to demonstrate the generality of its core findings across a broad range of LLMs. Therefore, evaluations should ideally cover both open-source and closed-source models with diverse architectures and capabilities. However, the current study only includes GPT, Qwen, and Llama families, which do not adequately represent either the classical models that have shaped the field or several state-of-the-art frontier models. As a result, it remains unclear whether the observed phenomenon is a general characteristic of modern LLMs or merely specific to the selected model families. Expanding the evaluation to include additional models would substantially strengthen the benchmark's conclusions and external validity.

Ethical Concerns: No or very minor ethics concerns only
Reproducibility: Yes
Reproducibility Comments:
The paper provides a relatively detailed description of the benchmark construction process, evaluation protocol, experimental settings, and metrics, which would allow readers to reproduce most of the methodology at a conceptual level.

Dataset Assessment: Partly
Dataset Comments:
Unable to access the URL

Limitations:
yes

Questions:
Evaluation on larger graphs: The main conclusions are derived from relatively small graphs. Could the authors provide additional experiments on larger graphs to further validate their findings? Such evaluations would help assess whether the observed patterns generalize to more complex causal structures and would strengthen the evidence supporting the paper’s claims.

Indirectly ensuring train-test independence in post-training: The verifier-based post-training results may be affected by the overlap between training and evaluation graphs. Could the authors explore mechanisms to better ensure train-test independence, for example by controlling specific nodes, replacing semantically irrelevant variables, or modifying graph components while preserving overall structure? Such experiments would provide stronger evidence that the observed gains arise from the proposed training strategy rather than memorization.

Statistical significance analysis: Many reported improvements are relatively small in magnitude. Could the authors provide statistical significance tests, confidence intervals, or additional repeated runs to demonstrate that these improvements are attributable to the proposed method rather than random variation in model behavior?

Evidence supporting the claim that LLMs struggle to utilize numerical evidence: The propose of the paper is that current LLMs have difficulty leveraging numerical evidence for causal discovery. However, the paper provides limited direct evidence supporting this conclusion. Could the authors provide additional experiments, analyses, or references from prior literature to substantiate this claim? Otherwise, the motivation and interpretation of several findings may be less convincing.

Broader coverage of LLMs: The benchmark evaluation focuses primarily on GPT, Qwen, and Llama families. However, several influential frontier models, such as Gemini, Claude, and DeepSeek, are not included. As a benchmark paper, a broader model coverage would be important for demonstrating that the identified phenomenon is prevalent across the current LLM landscape. Without such evidence, it remains unclear whether the observed behavior is a general limitation of modern LLMs or merely a characteristic of the selected model families.

Rating: 3: Borderline reject: Technically solid paper where reasons to reject, e.g., limited evaluation, outweigh reasons to accept, e.g., good evaluation. Please use sparingly.
Confidence: 4: You are confident in your assessment, but not absolutely certain. It is unlikely, but not impossible, that you did not understand some parts of the submission or that you are unfamiliar with some pieces of related work.
Paper Formatting Concerns:
One of the URLs provided in the paper could not be accessed during review, which made it difficult to verify the corresponding resource.

LLM Policy: Yes
Code Of Conduct Acknowledgement: Yes
Responsible Reviewing Acknowledgement: Yes
