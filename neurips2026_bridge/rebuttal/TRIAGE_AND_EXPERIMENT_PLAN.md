# BRIDGE: Rebuttal Triage and P0-P3 Plan

## Scope and evidence basis

- Venue: NeurIPS 2026; supplied score scale 1-6.
- Deadline: July 27, 2026; analysis performed July 24, 2026.
- Resources: no compute.
- Evidence reviewed: the 30-page May 27 manuscript, all three official reviews, and the July 22/23 meta-review in `review.txt`.
- The May 4 automated PAT feedback is not treated as an official review or as evidence of reviewer intent. It appears to evaluate an earlier manuscript version: for example, it discusses `TOKENXATTN` and an unfilled checklist, while the May 27 PDF contains neither TOKENXATTN nor an unfilled checklist. Its evaluation-validity warnings remain useful for a future internal audit, but should not be introduced into this rebuttal unless independently verified.
- The NeurIPS response character limit, phase-specific rules, and whether manuscript revisions/new experiments are permitted are not supplied: `[VENUE RULE NEEDS VERIFICATION]`. This document stops before rebuttal drafting.

## A. Rebuttal viability

**Classification: LOW EXPECTED RETURN, with one narrow but real clarification path.**

The official scores are **2/3/3**, all below a likely acceptance-supporting score on the supplied 1-6 scale. Confidence is **4/5, 5/5, and 4/5**. There is no reviewer advocate. The AC has already synthesized the case as limited technical novelty, omitted important work, insufficient baselines, and weak presentation, and says the work is not mature for acceptance. This is not a single misunderstanding: the rejection mechanism is shared skepticism about novelty/positioning plus missing controls and incomplete method exposition.

The strongest positive signal is consistent recognition that the problem is important, the framework is technically sound and practical, and the experiments show strong gains across datasets/tasks. Reviewer 9PXP explicitly says they are willing to reconsider if convinced that RDL is inapplicable. The strongest negative signal is that the two most decision-critical requests—RDL/RelBench comparison and an unfrozen two-stage control—cannot be answered empirically with the stated resources, while the paper repeatedly characterizes its architecture as "simple" and the reviewers see a standard encoder-to-GNN pipeline.

Expected value of additional rebuttal work is therefore **low for reversal but nonzero for record correction**. The best use of the remaining time is a short, precise response that (1) positions BRIDGE against RDL/KG/sequence-graph work with a verified capability table, (2) concedes that the frozen comparison does not isolate end-to-end supervision from representation pretraining, (3) uses existing scalability and no-relation evidence accurately, and (4) supplies missing method details. Do not imply that RDL is irrelevant unless a paper-level comparison establishes that claim.

### Why extensive rebuttal work has low value

1. All scores are below the likely acceptance boundary and the AC has already endorsed the main concerns.
2. Novelty concerns are shared by the AC, 9PXP, and zrKC; prose alone cannot create a new architectural contribution.
3. The requested controls and benchmarks require implementation/training, which conflicts with "no compute" and the deadline.
4. Jz3M requests major method/presentation revision rather than one compact clarification.
5. The current manuscript's own framing—"simple formulation," "simple end-to-end framework," and "straightforward differentiable pipeline"—supports a use-inspired systems/empirical contribution, not fundamental architectural novelty.

### What is still worth answering now

- A careful RDL/RelBench applicability and scope comparison, without claiming inapplicability categorically.
- A frank statement that the frozen two-stage baseline answers comparison with the cited prior pipeline, but does **not** fully isolate joint task supervision; narrow the claim accordingly.
- Existing evidence on the performance/cost trade-off: fanout 2/5/10/15, epoch times, shallow encoder depth, and asymptotic graph cost.
- Existing no-relation ablation as evidence that gains are not solely from the supervised sequence encoder. It does **not** explain why end-to-end beats a task-supervised unfrozen control.
- Exact architecture, compression, node features, graph construction, loss/head, and information-flow details from implementation/configuration records, if the authors can verify them.

### What not to spend time on

- Do not attempt a rushed RelBench/RelGNN/RelGT implementation.
- Do not call the requested unfrozen pipeline "the same as BRIDGE" without defining the control precisely; this will appear evasive.
- Do not argue novelty as merely combining a Transformer and GNN.
- Do not promise new experiments by July 27.
- Do not spend response budget on Table 1 placement or minor layout before the shared novelty/baseline concerns.

## B. Paper claim map

| Element | Manuscript claim | Assessment |
|---|---|---|
| Problem | Model users with both per-user event sequences and a static user-user relation graph. | Clear, practically important, and recognized by all reviewers. |
| Main contribution | A formulation and end-to-end sequence-to-static-GNN pipeline with task-loss gradients through both modules. | Defensible as use-inspired formulation/engineering; weak as fundamental method novelty. |
| Claimed novelty | Integrates personalized sequences and static relations in one differentiable pipeline. | Acceptance-critical but under-positioned against RDL, KG, and prior sequence+graph architectures. |
| Empirical claim | Joint training outperforms graph-only and frozen two-stage alternatives; sequence-only where applicable. | Supported for the evaluated controls, but does not establish superiority to unfrozen/task-supervised controls or RDL. |
| Scope | Relationship prediction and fraud detection on five real-world datasets. | Reasonable evaluated scope; general industrial language is broader than the evidence. |
| Practical claim | Compression decouples graph message passing from sequence length; fanout offers quality/runtime choices. | Supported for vector-feature BRIDGE variants and one main ablation dataset; should be stated narrowly. |
| Acceptance-critical claim | Joint task optimization of sequence and relational context is a useful, distinct solution for asynchronous user histories plus static relations. | Must be reframed around the data formulation and empirical finding, not component novelty. |

## C. Atomic review diagnosis

Uncertain interpretations are visibly marked **UNCERTAIN**.

| ID | Reviewer | Surface comment | Underlying decision concern | Alternative interpretation | Confidence | Severity | Sharedness |
|---|---|---|---|---|---|---|---|
| AC-1 | AC | Limited technical novelty | Is there a NeurIPS-level contribution beyond stacking standard modules? | The AC may accept a use-inspired contribution if positioning/evidence are strengthened. | High | Fatal | Meta/shared |
| AC-2 | AC | Important works omitted; more discussion needed | Is novelty judged against the correct modern literature? | The issue may be citation coverage rather than capability overlap. | High | Major | Meta/shared |
| AC-3 | AC | Baselines insufficient | Are gains attributable to unfair or weak controls? | **UNCERTAIN:** primarily RDL, unfrozen control, or both. Best response must cover both. | High | Fatal | Meta/shared |
| AC-4 | AC | Presentation needs significant improvement | Can the method be understood and reproduced well enough to evaluate? | Could also summarize organization/positioning problems, not only missing equations. | Medium | Major | Meta/shared |
| R1-1 | 9PXP | RDL/RelBench omitted | Is BRIDGE already subsumed by relational database learning, making novelty/evaluation incomplete? | **UNCERTAIN:** reviewer may demand empirical comparison, or may accept a precise scope mismatch argument. Their text explicitly allows the latter. | High | Fatal | Multiple |
| R1-2 | 9PXP | Compare with RelGNN/RelGT | Does BRIDGE beat strong current relational models under fair inputs? | These models may target multi-table relational databases rather than the paper's sequence+static-user-graph input. | High | Major | Single/AC |
| R1-3 | 9PXP | Evaluate on RelBench | Does the method generalize to a recognized benchmark and task family? | Reviewer may use RelBench mainly as evidence that the paper is poorly positioned, not insist on a full new benchmark. | Medium | Major | Single/AC |
| R1-4 | 9PXP | Frozen two-stage control is unfair; add unfrozen pipeline | Is the gain due to task-supervised fine-tuning rather than sequence-graph joint modeling? | **UNCERTAIN:** an "unfrozen pipeline" optimized jointly is architecturally close to BRIDGE; the intended control may be supervised pretrain-then-finetune, alternating training, or identical initialization. Clarify rather than dismiss. | High | Fatal | Multiple |
| R1-5 | 9PXP | Compression used is unclear; test another | Is the method reproducible and robust to a key design choice? | Reviewer may suspect the generic method description hides a tuned or architecture-specific compressor. | High | Major | Multiple/presentation |
| R1-6 | 9PXP | Performance by user connectivity | Does relational context actually help where graph information is available, and fail gracefully for isolated users? | Could also test whether aggregate gains are concentrated among high-degree users. | High | Moderate | Single |
| R2-1 | Jz3M | Approach section is weak; figure barely clarifies method | Is the paper technically mature and evaluable? | Reviewer may be penalizing presentation rather than correctness. | High | Major | Multiple/AC |
| R2-2 | Jz3M | Hidden-layer/components details missing | Can the exact model be reproduced, and is there any nontrivial design? | **UNCERTAIN:** "hidden layers" could mean dimensions/activations/norms or the full sequence/GNN/head stack. Answer all compactly. | Medium | Major | Single/AC |
| R2-3 | Jz3M | Show information flow with equations | Is BRIDGE a defined mathematical method rather than a verbal recipe? | Could be satisfied by a compact forward-pass equation chain. | High | Major | Single/AC |
| R2-4 | Jz3M | Graph construction not discussed | Are edges available exogenously or derived from sequences, and is there leakage/task dependence? | **UNCERTAIN:** may refer to all datasets or only the general method. Give per-dataset construction and distinguish observed input graph from target edges. | High | Major | Single |
| R2-5 | Jz3M | Node-feature assignment not discussed | What enters graph-only, two-stage, and BRIDGE nodes, and are comparisons feature-matched? | May also concern initialization of users with no/short sequences. | High | Major | Single |
| R2-6 | Jz3M | Move Table 1 to experiments | Is the paper organized conventionally and efficiently? | None material. | High | Minor | Single |
| R2-7 | Jz3M | Discuss difference from KG approaches | Is the proposed problem already handled by knowledge-graph user modeling? | **UNCERTAIN:** reviewer does not cite a specific KG method; distinguish typed entity-relation triples/item KGs from per-user ordered events plus an observed user-user graph without claiming all KGs are inapplicable. | Medium | Major | Multiple/related work |
| R3-1 | zrKC | Little insight into why end-to-end beats two-stage | Is the central empirical effect mechanistically understood rather than merely observed? | Could be answered partly by gradient/task-alignment analysis, representation shift, or performance decomposition. | High | Major | Single, linked R1-4 |
| R3-2 | zrKC | Limited performance/scalability discussion | Is the gain worth training cost at realistic scale? | **UNCERTAIN:** may seek wall-clock comparison with two-stage, memory/latency, or asymptotics. Existing paper gives fanout and asymptotics but not a direct end-to-end-vs-two-stage total-cost table. | High | Major | Single |
| R3-3 | zrKC | Distinction from existing sequence-graph approaches unclear | Is the claimed contribution actually new, or only a standard pipeline in another application? | Reviewer may accept formulation-level novelty if the capability/assumption delta is explicit. | High | Fatal | Multiple/AC |
| R3-4 | zrKC | Four sequence+graph works omitted | Was the related-work search incomplete, undermining originality claims? | Some cited works may have different nodes, graphs, tasks, or synchronization assumptions; this needs verified comparison, not dismissal by domain. | High | Major | Multiple/AC |
| R3-5 | zrKC | Standard sequence encoder -> graph encoder pipeline | Does the architecture itself contain any novel mechanism? | Reviewer may still value a strong empirical/use-inspired framing. | High | Fatal | Multiple/AC |
| R3-6 | zrKC | Limited new understanding | Does the paper teach a transferable principle beyond "joint training helps"? | Could be partly addressed by degree/cost/mechanism analyses, but not fully without new evidence. | High | Major | Multiple/AC |

## D. Concern-to-evidence map

| Concern IDs | Existing evidence in manuscript | Missing evidence | Best response mode |
|---|---|---|---|
| AC-1, R3-3, R3-5, R3-6 | Explicit data formulation: asynchronous per-user event histories plus an inherent static user-user graph; two tasks/five datasets; design tradeoffs. | Verified delta against RDL, KGs, and each cited sequence+graph work; stronger transferable insight. | Claim narrowing + clarification + related-work revision. |
| AC-2, R1-1/2/3, R2-7, R3-4 | Related work already distinguishes sequential recommendation, temporal graphs, PRES, and synchronized spatiotemporal graphs. | RDL/RelBench, KG, and four named-work comparison. | No-compute literature/capability audit. |
| AC-3, R1-4, R3-1 | Frozen prior pipeline; no-relation ablation keeps task objective and removes relational pathway (MRR 85.4 vs 62.0 on full Brightkite). | Unfrozen/task-supervised control; representation/gradient analysis. | Concede confound; use no-relation evidence only for the narrower relational-value claim. |
| R1-5, R2-2/3/5 | Generic compression equation and appendix hyperparameters; Algorithm 1 gives high-level flow. | Exact compressor used per experiment, layer dimensions, nonlinearities/norms/dropout, node features per baseline, head/loss equations. | Verified implementation clarification. |
| R1-6 | Fanout and no-relation ablations show aggregate dependence on relational context. | Degree-bin metrics including degree zero and support counts. | P3 experiment; do not infer from aggregate results. |
| R2-4 | Brightkite/Gowalla friendship and Amazon co-review edge descriptions. | Exact split/masking/construction protocol and whether target edges are excluded from message passing. | Verified data-pipeline clarification; internal validity audit. |
| R3-2 | Fanout MRR/time table; sequence-depth MRR/time; complexity section: Transformer cost and graph cost O(Bkd^2), with compression before graph propagation. | Direct total training time, memory, preprocessing, and inference comparison versus frozen two-stage. | Existing evidence + scope qualification; direct comparison deferred. |
| R2-6 | Table is visibly at the start of the methodology continuation page. | None. | Manuscript revision only. |

## E. Prioritized experiment and analysis plan

Because no compute is available, **P0 contains only evidence audits and analyses that can be completed from papers, code/configuration, and existing logs**. New model runs are P3 even when scientifically important.

| Priority | Concern IDs | Underlying question | Proposed experiment or analysis | Why it changes the decision | Minimum viable protocol | Time/cost | Result interpretation | Fallback |
|---|---|---|---|---|---|---|---|---|
| P0 | R1-1/2/3, R2-7, R3-3/4/5, AC-1/2 | Is BRIDGE distinct from RDL, KG, and prior sequence+graph work? | Build a verified capability/assumption delta table from the cited papers. | This is the only explicit score-reconsideration path and addresses the shared novelty concern. | For each named work: input schema, temporal assumption, graph source, node/event unit, objective/task, whether per-user irregular sequences and static user-user edges coexist, and whether training is end-to-end. Verify from primary papers; avoid performance claims without matched experiments. | 4-6 author-hours, no compute. | A crisp non-overlap supports a narrowed formulation/use-inspired novelty claim. Significant overlap requires conceding novelty and repositioning. | State scope conservatively and promise citations/repositioning, not superiority. |
| P0 | R1-4, R3-1, AC-3 | What does the current comparison actually establish? | Perform a claim-control audit of every sentence about "end-to-end" gains. | Prevents an indefensible fairness response and gives the AC an auditable narrowed claim. | Map each claim to graph-only, frozen two-stage, and no-relation controls. Explicitly record that no task-supervised unfrozen control exists. Separate "relational propagation helps" from "joint optimization beats all two-stage training." | 1-2 hours, no compute. | Existing evidence supports gains over the frozen cited pipeline and value of relational MP; it does not isolate the source of gains against an unfrozen control. | Narrow abstract/conclusion and acknowledge requested control as essential future/resubmission work. |
| P0 | R1-5, R2-1/2/3/4/5, AC-4 | Is the exact method and evaluation pipeline reproducible? | Conduct an implementation-to-paper specification audit. | A verified compact specification can fully resolve many clarity complaints without experiments. | From source/configs, record exact compressor per task, tensor shapes, encoder/GNN/head layers, activations, normalization, residual, loss, node features by baseline, graph construction, split and edge masking. Every item needs a code/config location or author confirmation. | 3-5 hours, no compute. | Verified details become equations/table in response/revision. Missing details remain visible placeholders. | Admit omitted detail and specify exact revision only after author verification. |
| P0 | R3-2 | What scalability evidence already exists, and what does it not show? | Reframe the existing cost analysis into a quality-cost table. | Directly answers a major question with existing results. | Report fanout 2/5/10/15 MRR and epoch time, encoder-depth results, asymptotics, dataset/setting, and explicitly note absence of direct total-cost comparison to two-stage. | 1 hour, no compute. | Supports tunable graph-phase cost and shallow-encoder sufficiency on Brightkite; not general end-to-end cost parity. | Give asymptotic explanation and narrow scope. |
| P1 | R3-1, R1-4 | Can existing checkpoints/logs explain why joint training helps? | If already saved, compare initialization-to-final sequence embeddings or gradient/update norms and correlate representation change with errors fixed. | Adds mechanism evidence without training. | Use existing checkpoints/predictions only; fixed sample, prespecified distance/CKA or update norm, and separate high/low degree. No cherry-picked examples. | 2-4 hours, light local analysis; may violate strict "no compute." | Large task-aligned change is suggestive, not causal; small change weakens the proposed explanation. | Use conceptual gradient-flow explanation, explicitly labeled hypothesis rather than evidence. |
| P1 | R1-6, R3-6 | Are gains concentrated by connectivity? | Degree-stratified evaluation from existing test predictions. | Tests whether graph context drives gains and adds transferable insight. | Bin by training-graph degree (0, 1-2, 3-5, 6-10, >10 or quantiles); report support and same metric for BRIDGE, no-relation/sequence-only, and strongest graph baseline; bootstrap CIs if existing tooling permits. | 2-4 hours if predictions exist; otherwise impossible without inference. | Growing gap supports relational-context mechanism; flat gap suggests supervised encoder effect; poor isolated-user results require limitation. | State aggregate ablation only; defer degree analysis. |
| P2 | R1-5 | Is compression robust? | Analyze alternative pooling only if outputs/checkpoints already make this a zero-training swap and evaluation is valid. | Secondary robustness/reproducibility value. | Compare exact default with mean/max/attention under the same checkpoint only if architecture allows; otherwise retraining is required and result is uninterpretable. | 1-2 hours if valid; usually not valid. | Similar results support robustness; changes indicate compressor is a material component. | Define the actual compressor and defer robustness. |
| P3 | R1-4, R3-1, AC-3 | Does BRIDGE beat a fair task-supervised/unfrozen control? | Train controlled variants for resubmission. | This is the decisive attribution experiment. | Same initialization, data, split, sequence/GNN architecture, tuning budget, and seeds: frozen pretrained; unfrozen pretrained end-to-end; scratch end-to-end; sequence-only supervised; optionally staged supervised pretrain then joint fine-tune. Report quality, time, memory, and variance. | Multi-day GPU work. | If joint training remains best, central claim strengthens; if not, narrow contribution to task-supervised multimodal modeling. | Concede current evidence gap now. |
| P3 | R1-1/2/3 | Does BRIDGE add value on RDL benchmarks and against RelGNN/RelGT? | RelBench evaluation with matched features and tuning. | Resolves the strongest reviewer's central empirical demand. | Select task(s) whose schema supports ordered user events plus relations; define legal timestamp/split handling; compare matched encoders/budgets over >=3 seeds. | Multi-day engineering/GPU work. | Positive result broadens scope; null result may show BRIDGE is specific to static-relation + personal-sequence data. | Capability table and scope clarification only. |
| P3 | R1-6 | Connectivity analysis when predictions are unavailable. | Regenerate predictions and conduct prespecified degree-bin analysis. | Mechanistic/generalization insight. | As P1, with all methods evaluated from the same checkpoints/splits. | Requires inference/checkpoints. | Same as P1. | Defer. |
| P3 | R1-5 | Compression robustness with retraining. | Retrain mean/max/attention/CLS or learned-query pooling. | Establishes robustness of a key module. | Same dimensionality/budget, >=3 seeds, all main tasks or a justified representative subset. | GPU work. | Robustness or material sensitivity informs method definition. | Define default precisely now. |

### Run now / clarify / defer

**Run now (no-training analyses):** P0 literature delta table, claim-control audit, implementation specification audit, and existing scalability evidence synthesis. Only attempt P1 analyses if artifacts already exist and the authors consider light local computation compatible with "no compute."

**Answer from existing evidence:** the problem definition; exact gradient path; no-relation aggregate ablation; fanout/runtime and complexity evidence; per-dataset graph definitions; intended use-inspired contribution; concrete planned method exposition.

**Defer:** RDL benchmarks, unfrozen/task-supervised controls, new pooling ablations, full degree analysis without predictions, and direct matched total-cost experiments.

## F. Time budget through July 27

1. **July 24 (35%):** freeze reviewer issue map; verify response rules/limit; assign one author each to RDL/KG/prior-work reading and implementation specification. Complete the P0 claim-control audit first.
2. **July 25 (35%):** finish and cross-check the literature delta table; extract exact architecture/data-pipeline details; synthesize existing cost evidence. Decide whether any P1 analysis is genuinely zero-training and interpretable.
3. **July 26 (20%):** draft only the decision-critical responses in this order: RDL/novelty, fairness/claim narrowing, method specification, scalability/mechanism. Obtain author approval for every concession and future revision.
4. **July 27 (10%):** provenance, coverage, tone, consistency, and character-limit audit. Remove minor material before compressing major answers.

## G. Recommended response stance (not rebuttal prose)

- **Novelty:** reposition from architectural novelty to a use-inspired formulation and empirical study for asynchronous personal sequences plus persistent static user-user relations. State exact differences from prior work only after paper verification.
- **RDL:** say the frameworks are related and should have been discussed. Explain schema/task differences precisely; do not say RDL is irrelevant. Without experiments, do not claim BRIDGE outperforms it.
- **Frozen control:** agree it is a valid limitation. The present comparison shows improvement over the cited frozen pipeline, while the no-relation ablation shows relational MP adds value under task supervision. Neither establishes superiority to all unfrozen alternatives.
- **Mechanism:** distinguish a conceptual explanation (task gradients adapt sequence features for relational prediction) from empirical mechanism evidence, which is currently absent.
- **Scalability:** report exact existing tradeoffs and asymptotics, then acknowledge that a direct end-to-end versus two-stage total-cost comparison is missing.
- **Clarity:** give equations and verified implementation details rather than promising vague expansion.

## H. Resubmission roadmap

| Priority | Revision | Reviews addressed | Required work | Expected payoff |
|---|---|---|---|---|
| R0 | Add fair task-supervised/unfrozen controls and supervised sequence-only control. | R1-4, R3-1, AC-3 | Matched training study with quality/cost/variance. | Restores causal interpretability of the main claim. |
| R0 | Reposition against RDL/RelBench, KG methods, and named sequence+graph work; add at least one matched modern baseline where applicable. | R1-1/2/3, R2-7, R3-3/4/5, AC-1/2 | Literature audit plus implementation/evaluation. | Establishes novelty boundary and contemporary relevance. |
| R0 | Rewrite method as a complete mathematical specification. | R1-5, R2-1/2/3/4/5, AC-4 | Equations, algorithm, tensor shapes, compressor, loss, node features, graph/split construction. | Resolves maturity/reproducibility concerns. |
| R1 | Add mechanism analyses: degree bins, embedding/update analysis, and error decomposition. | R1-6, R3-1/6 | Existing/new inference and analysis. | Converts an empirical recipe into a transferable finding. |
| R1 | Add direct matched quality-cost comparison. | R3-2 | Time, peak memory, preprocessing, inference, and convergence across controls. | Makes the practical contribution credible. |
| R1 | Audit leakage and split validity, especially Amazon co-review edges and message-passing masks. | Internal risk/R2-4 | Document edge construction and ensure held-out targets cannot enter the input graph; consider a leakage-resistant split/task. | Prevents a potentially fatal future review. |
| R2 | Compression robustness across pooling choices. | R1-5 | Matched multi-seed ablation. | Demonstrates robustness or defines a real method component. |
| R2 | Move dataset table, improve figure, and expand related work/limitations. | R2-6 and presentation | Writing/layout revision. | Improves readability after core validity issues are fixed. |

### Preserve / change / narrow

| Preserve | Change | Remove or narrow |
|---|---|---|
| Important practical setting; five-dataset/two-task evidence; consistent gains over evaluated baselines; fanout and no-relation analyses. | Positioning, fair controls, exact method definition, mechanism insight, direct cost accounting, data-split audit. | Architectural novelty implication; superiority beyond evaluated frozen controls; broad industrial generalization; any suggestion that RDL/KG approaches are categorically inapplicable. |

### Revised central story for a future submission

Preserve this core: **BRIDGE studies a practically common but under-specified data regime—independent, irregular user event histories plus persistent user-user relations—and quantifies when jointly task-adapting sequence representations and relational propagation helps, at what cost, and for which users.** The future paper should make the controlled empirical findings and schema distinction central; the Transformer-to-GNN composition should be presented as the vehicle, not the novelty.

## I. Author evidence template for the next stage

```text
Analysis/Experiment ID:
Concern IDs addressed:
Status: completed / failed / partial
Protocol or source papers inspected:
Baselines and controls:
Dataset/split:
Metric:
Number of runs or seeds:
Result:
Uncertainty:
Unexpected findings:
Artifact/code/config/manuscript location:
Claim supported:
Claim not supported:
Preferred manuscript change:
Author approval for concession/promise:
```

## Immediate blockers before drafting

1. `[VENUE RULE NEEDS VERIFICATION]` Exact response limit and whether this is an author-response or discussion phase.
2. `[AUTHOR CONFIRMATION NEEDED]` Exact compression operation used in every reported experiment.
3. `[AUTHOR CONFIRMATION NEEDED]` Node features for every graph-only baseline and precise prediction heads/losses.
4. `[AUTHOR CONFIRMATION NEEDED]` Train/validation/test split and whether held-out target edges are excluded from message-passing graphs.
5. `[AUTHOR CONFIRMATION NEEDED]` Whether checkpoints, predictions, and logs exist for P1 analyses.
6. `[AUTHOR CONFIRMATION NEEDED]` Which claims the authors are willing to narrow and which revisions they can truthfully commit to.
