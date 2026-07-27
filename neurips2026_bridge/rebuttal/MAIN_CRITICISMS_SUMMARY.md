# BRIDGE: Summary of Main Review Criticisms

## Overall assessment

The reviewers agree that BRIDGE addresses an important practical problem and reports strong empirical results. However, they do not believe the current paper establishes a sufficiently distinct technical contribution or supports its central claims with fair enough comparisons. The meta-review summarizes the paper as not yet mature for acceptance because of limited novelty, omitted related work, insufficient baselines, and presentation weaknesses.

## Main repeated criticisms

### 1. Limited technical novelty and unclear differentiation

This is the most repeated and decision-critical criticism.

- BRIDGE is perceived as a standard **sequence encoder followed by a graph encoder**, rather than a fundamentally new architecture.
- The paper does not clearly explain how its formulation differs from earlier sequence-graph systems.
- Reviewers see the main contribution as problem formulation and empirical evaluation, not modeling novelty.
- The paper provides limited new understanding beyond showing that combining sequence and graph information can improve performance.

Raised by: **Area Chair, Reviewer 9PXP, Reviewer zrKC**, and indirectly Reviewer Jz3M.

### 2. Missing or inadequate positioning against related work

The literature review is considered incomplete, which weakens the novelty claim.

- Relational Deep Learning, RelBench, RelGNN, and RelGT are omitted.
- Several prior sequence-encoder-plus-GNN architectures are not discussed.
- The relationship to knowledge-graph approaches is unclear.
- Reviewers want an explicit capability and assumption comparison, not only additional citations.

Raised by: **Area Chair and all three reviewers**.

### 3. Insufficient and potentially unfair baselines

The central empirical comparison does not isolate why BRIDGE performs better.

- The two-stage baseline uses pretrained sequence embeddings that remain frozen during graph training.
- BRIDGE instead receives end-to-end task supervision through both the sequence and graph modules.
- Therefore, the performance gap may come from supervised fine-tuning rather than the claimed benefit of the sequence-graph formulation.
- A fair comparison should include an unfrozen/task-supervised two-stage control and a supervised sequence-only model.
- RDL baselines or evaluation on an appropriate RelBench task are also requested.

Raised by: **Area Chair, Reviewer 9PXP**, and linked to Reviewer zrKC's mechanism concern.

### 4. Method description is incomplete

The approach is not specified with enough precision for some reviewers to evaluate or reproduce it confidently.

Missing or unclear details include:

- the exact compression/pooling operation used in experiments;
- hidden-layer structure and model components;
- mathematical equations describing information flow;
- graph construction;
- node-feature assignment for BRIDGE and baselines;
- task-specific prediction heads and losses.

The main architecture figure is viewed as thematic rather than technically explanatory.

Raised by: **Area Chair, Reviewer Jz3M, and Reviewer 9PXP**.

### 5. Limited explanation of why end-to-end training works

The paper demonstrates gains but provides little mechanism-level understanding.

- It does not establish whether gains come from supervised sequence adaptation, relational message passing, or their interaction.
- Reviewers request analysis by user connectivity, especially isolated versus highly connected users.
- Additional useful analyses would include degree-stratified results, sequence-only/no-relation controls, representation changes, or freezing-depth studies.

Raised by: **Reviewer zrKC and Reviewer 9PXP**.

### 6. Scalability and cost trade-offs are underdeveloped

The paper discusses fanout, encoder depth, and asymptotic complexity, but does not directly compare the total cost of BRIDGE with two-stage alternatives.

Reviewers want:

- matched wall-clock training time;
- peak memory usage;
- preprocessing/storage cost;
- inference cost;
- accuracy-cost trade-offs as sequence length and graph size increase.

Raised explicitly by: **Reviewer zrKC**; also relevant to the Area Chair's maturity assessment.

## Secondary criticisms

- Compression robustness should be tested using more than one pooling strategy.
- Performance should be stratified by graph connectivity.
- Table 1 belongs in the experimental section rather than the approach section.
- The paper should discuss knowledge-graph-based user modeling.
- Claims about generality and superiority should be narrowed to the evaluated tasks and baseline families.

## Shared rejection rationale

The reviewers' combined position can be summarized as follows:

> BRIDGE studies an important problem and produces promising empirical gains, but the paper does not yet demonstrate a distinct contribution or establish through fair controls that the gains arise from the claimed end-to-end sequence-graph formulation.

## Most important issues to fix

1. Add fair supervised sequence-only and unfrozen sequence-plus-graph controls.
2. Position BRIDGE precisely against RDL, RelBench, knowledge-graph methods, and prior sequence-graph architectures.
3. Reframe the contribution around the data formulation and controlled empirical findings rather than architectural novelty.
4. Fully specify the method with equations, tensor flow, compression, graph construction, node features, heads, and losses.
5. Add analyses explaining when relational context helps and whether the benefit justifies the computational cost.
