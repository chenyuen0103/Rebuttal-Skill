# BRIDGE Updated-Paper Revision Plan

## Bottom line

The updated paper should not be presented as a novel Transformer-plus-GNN architecture. Its strongest defensible story is a **carefully controlled study of a specific data regime: asynchronous per-user event sequences together with persistent static user-user relations**. The revision must establish (1) how this regime differs from RDL, knowledge-graph, temporal-graph, and prior sequence-graph formulations; (2) that gains remain under fair task-supervised controls; and (3) when and why relational end-to-end adaptation helps relative to its cost.

## R0: changes required before resubmission

### 1. Replace the novelty story

Current weakness: the paper repeatedly calls BRIDGE a simple differentiable sequence-encoder-to-GNN pipeline, while reviewers correctly observe that the components and composition are standard.

Rewrite the contribution around three narrower claims:

1. **Problem formulation:** independent, irregular event histories coexist with an observed static user-user graph; neither must be converted into the other's representation.
2. **Controlled empirical finding:** under matched task supervision, identify when joint adaptation and relational propagation improve over sequence-only, graph-only, frozen, and unfrozen alternatives.
3. **Practical finding:** characterize accuracy, fanout, sequence length/depth, memory, and training/inference cost trade-offs.

Do not claim fundamental architectural novelty unless a genuinely new mechanism is added.

### 2. Add the decisive fair controls

The current frozen two-stage baseline cannot isolate joint optimization from supervised representation learning. Add, with identical splits, initialization, encoder/GNN capacity, tuning budget, and seeds:

- graph-only;
- supervised sequence-only;
- frozen masked-pretrained sequence encoder + GNN;
- unfrozen masked-pretrained sequence encoder + GNN, jointly fine-tuned;
- sequence encoder trained from scratch + GNN, jointly trained;
- optionally, supervised sequence pretraining followed by frozen GNN training;
- no-relation BRIDGE with the same task objective.

Report mean and uncertainty over at least three seeds, training time, peak memory, preprocessing cost, and inference cost. The main claim must be derived from these controls, not assumed from the frozen comparison.

### 3. Position against modern related work

Add a capability table covering at minimum RelBench, RelGNN, RelGT, KG-based user modeling, PRES, temporal graphs, spatiotemporal graphs, and the four sequence-graph papers named by reviewer zrKC.

Compare along these axes:

- input schema and entity types;
- whether events are per-user sequences or graph edges;
- whether sequences are irregular/asynchronous;
- whether relations are static, temporal, constructed, or observed;
- single-relation versus multi-relational setting;
- prediction unit and task;
- sequence and graph training objective;
- whether end-to-end task adaptation is supported.

Do not argue that RDL or KGs are irrelevant. State where they overlap, where the data assumptions differ, and test a modern baseline wherever conversion is legitimate.

### 4. Repair evaluation validity

For every task, specify exact train/validation/test ratios, split unit, chronology, negative sampling, and which edges are visible during message passing. Verify that held-out target edges never appear in the input graph.

The Amazon relationship task needs special attention because edges are defined by overlap in product IDs while product IDs also occur in the input sequences. Either:

- redesign the target/split so the label is not directly recoverable from the inputs;
- remove product identity from the relevant input comparison;
- evaluate on naturally observed relations independent of the event tokens; or
- explicitly scope the task as recovery of a deterministic relation and stop using it as evidence of general relationship prediction.

Also ensure feature parity across baselines and select classification thresholds on validation data, not test data.

### 5. Rewrite the method as a reproducible specification

The method section should include a compact equation chain:

`event embeddings -> sequence encoder -> compression -> relational propagation -> residual/fusion -> prediction head -> loss`.

Specify:

- tensor shapes at every stage;
- exact compressor used in each experiment;
- layer count, hidden dimensions, activations, normalization, dropout, and residuals;
- GNN aggregation and self-loop behavior;
- task-specific heads and losses;
- graph construction and node features for every model family;
- neighborhood sampling and inductive/transductive assumptions;
- training algorithm and gradient path.

Move dataset statistics to Experiments. Replace the thematic figure with a forward-pass figure that labels tensors and supervision.

## R1: analyses that turn the paper into a stronger scientific study

### 6. Explain why and for whom BRIDGE helps

Add degree-stratified results with support counts and uncertainty for isolated, low-degree, and high-degree users. Compare BRIDGE, supervised sequence-only/no-relation, and the strongest graph baseline.

Add at least one task-adaptation analysis, such as representation change from pretrained to fine-tuned encoders, gradient/update norms, or a controlled freezing-depth study. Treat these as mechanism evidence only if prespecified and consistent across datasets.

Include error decomposition by sequence length and graph degree. This can distinguish gains from task-supervised sequence adaptation from gains due to relational context.

### 7. Make scalability a first-class result

Report end-to-end wall-clock time, peak accelerator memory, preprocessing/storage, and inference latency for BRIDGE and two-stage controls. Vary:

- neighborhood fanout;
- sequence length;
- graph size;
- encoder depth;
- compression dimension or token count.

Keep the current fanout and asymptotic analyses, but add direct matched-cost comparisons. State clearly that compression removes sequence length from graph-message-passing cost but not from sequence-encoding cost.

### 8. Test compression rather than leaving it generic

Define one default compressor as part of the method. Compare mean/CLS, segmented pooling, learned-query/attention pooling, and—if justified—max pooling under matched output dimension and tuning. Report whether the conclusion about joint learning is robust to this choice.

## R2: presentation and scope

### Suggested paper structure

1. Introduction: precise data regime, practical gap, narrow contributions.
2. Related work: capability table and explicit distinction from RDL/KG/sequence-graph approaches.
3. Problem formulation: inputs, observed graph, targets, split assumptions.
4. Method: complete equations and algorithm.
5. Evaluation protocol: leakage controls, baselines, feature/compute parity.
6. Main results: fair-control table first.
7. Mechanism and subgroup analysis.
8. Scalability and quality-cost trade-offs.
9. Limitations: static single-relation scope, proxy fraud labels, benchmark limitations, compute.

### Claims to remove or narrow

- Replace "novel architecture" with "simple framework" or "controlled formulation and study."
- Replace broad superiority claims with the exact evaluated baseline family and setting.
- Do not claim improvement over sequence-only methods for tasks where no supervised sequence-only baseline exists.
- Treat temporal-graph results as a diagnostic under an artificial conversion, not a demonstration that temporal models are inferior.
- Avoid broad industrial deployment claims from public proxy-label datasets.

### Proposed title direction

Prefer a title that signals the empirical/formulation contribution, for example:

> **Jointly Modeling Personal Event Sequences and Static User Relations: A Controlled Study**

Retain BRIDGE as the framework name if desired, but do not let the acronym substitute for a novelty claim.

### Proposed one-sentence contribution

> We formalize user modeling with asynchronous personal event histories and persistent user-user relations, then use matched controls to characterize when joint task adaptation and relational propagation improve predictive quality and at what computational cost.

## Recommended main tables and figures

1. **Table 1:** capability/assumption comparison with prior work.
2. **Table 2:** matched-control main results, including supervised sequence-only and unfrozen controls.
3. **Table 3:** matched training/memory/inference costs.
4. **Figure 1:** exact forward pass with tensor shapes and gradient flow.
5. **Figure 2:** BRIDGE gain versus degree and sequence length, with uncertainty/support.
6. **Figure 3:** quality-cost frontier over fanout and sequence/compression settings.
7. **Appendix:** complete hyperparameters, split construction, per-seed results, and leakage audit.

## Recommended execution order

1. Audit split/leakage and target-edge masking before running more models.
2. Implement fair supervised/unfrozen controls.
3. Build the related-work capability table and decide whether an RDL baseline is applicable.
4. Run subgroup/mechanism and matched-cost analyses.
5. Rewrite the contribution and method only after the evidence is known.
6. Add pooling robustness and presentation polish last.

If the validity audit or fair controls weaken the current result, narrow the paper rather than hiding the outcome. A smaller, well-supported claim will be substantially stronger than the current broad but confounded one.
