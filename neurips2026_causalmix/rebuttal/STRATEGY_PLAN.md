# Rebuttal viability and prioritized evidence plan

## Viability assessment

**Overall: viable but uphill; evidence, not prose, determines the outcome.** The score profile is 2/4/3 on a 1–6 scale, with two high-confidence negative/borderline-negative reviews and one lower-confidence borderline accept. The meta-review adopts the same three central criticisms—small/familiar graphs, non-disjoint SFT, and low replication—so a purely argumentative rebuttal is unlikely to change the outcome.

The paper nevertheless has a credible acceptance path because all reviewers and the AC recognize the controlled attribution protocol as useful. No reviewer identifies a fatal mathematical error, leakage in the benchmark generator, irreproducibility of the method, or an ethical blocker. The decision can plausibly move if the rebuttal supplies a compact package that directly repairs internal validity and materially improves external validity.

**Likely score movement if P0 succeeds:** mUVt can plausibly remain at 4 or strengthen; zDep has a plausible 3→4 path; svky is harder because novelty and framing objections remain in addition to evaluation concerns. **UNCERTAIN:** NeurIPS 2026 reviewer-update behavior and AC weighting are unknown, so no acceptance probability is asserted.

**Failure boundary:** if graph-disjoint SFT does not improve evidence-sensitive metrics, or larger/new graphs contradict the headline pattern, do not hide this. Narrow the post-training contribution to format reliability and the empirical claim to the original evaluated slice. The protocol contribution can still be defended, but rebuttal viability drops substantially.

## P0 — Decision-critical (days 1–4)

### P0.1 Graph-disjoint post-training evaluation

**Questions resolved:** I03, I11, I14, part of I13.

Run two complementary tests:

1. **Immediate held-out evaluation:** evaluate the existing base and FT Qwen3-4B checkpoints on graphs never used in either SFT stage—at minimum Child and two Synthetic-25 topologies. Use names-only, real-summary, anonymized-summary, and a data-conflict condition. Report parse validity, F1 with invalid=0, valid-only F1, SHD, and mixed-information gain.
2. **Preferred leave-one-graph-out (LOGO):** retrain four small LoRA adapters, each excluding one of Cancer/Earthquake/Asia/Sachs from both stages, then evaluate only on the held-out graph. Compare base, format-only Stage 1, and full Stage 2. Use identical budgets and seeds.

The **data-conflict condition** should pair a prompt's names with data drawn from a different graph or permuted adjacency-consistent variable mapping. A model that truly responds to evidence should change its graph toward the supplied data; a memorizing model should stay with the familiar named graph. Prespecify the metric before running: evidence-following rate or F1 to data-generating graph minus F1 to canonical named graph.

**Success criterion:** full SFT improves graph-disjoint evidence-sensitive metrics beyond both base and format-only, with gains not confined to parse validity or names-only. **Partial success:** improved validity but no evidence sensitivity; claim only format learning. **Failure:** no held-out gain or stronger adherence to canonical names under conflict; concede that current SFT evidence does not establish integration.

**Compute:** Qwen3-4B LoRA is small enough for four parallel single-GPU LOGO runs in principle. **UNCERTAIN:** exact runtime depends on whether RTX 6000 means 24 GB Ada/older cards or 48 GB variants, and whether training scripts/checkpoints/data are locally available.

### P0.2 Larger and less-memorization-prone matched attribution slice

**Questions resolved:** I01, I02, I09, I12, part of I13.

Use a deliberately small, interpretable grid rather than the full benchmark:

- Graphs: Child (20 nodes) plus two Synthetic-25 topologies with newly assigned labels; add Alarm (37 nodes) only if prompt length and parse validity are acceptable in a day-1 pilot.
- Models: Qwen2.5-72B and Qwen3-4B locally; optionally one hosted high-validity model if API access already exists.
- Conditions: names-only, real-name summary, anonymized summary, and permuted-name summary; one observational and one mixed/interventional budget; matched PC/GES/ENCO references.
- Replication: 20 independent paired data realizations for the chosen primary cells.

For newly labeled Synthetic-25 graphs, avoid claiming “semantic reasoning” unless labels are generated from a coherent causal mechanism and independently checked. Otherwise call them **unfamiliar topology/anonymized controls**. This distinction matters.

**Success criterion:** the protocol produces interpretable semantic/data contrasts on 20–37-node graphs, and the main conclusion is restated at the correct level: whether data gains are stable, not whether every model universally ignores data. A result that shows stronger data use on unfamiliar graphs is still valuable—it refines the paper's conclusion and validates the benchmark's diagnostic purpose.

**Compute:** allocate two GPUs to Qwen2.5-72B quantized inference and one each to Qwen3-4B / experiment generation and scoring. **UNCERTAIN:** 72B context length and quantization may exceed a particular RTX 6000 configuration; pilot context and memory before committing.

### P0.3 High-powered paired reanalysis on the primary Sachs contrasts

**Questions resolved:** I04, I15 and the AC's robustness concern.

Increase from 5 to at least 30 independent data realizations for a prespecified subset:

- Primary models: GPT-5.2-Pro if existing API access/budget permits; otherwise Qwen2.5-72B and Qwen3-4B.
- Primary conditions: names-only; real-summary; anonymized-summary; real-tabular only if context/runtime is manageable.
- Primary budget: the paper's Sachs `(N,M)=(5000,200)` cell, plus at most one budget-sweep point.
- Analysis: paired mean/median delta F1, 95% BCa or percentile CI, exact paired randomization/permutation test, seed-level scatter, validity-inclusive F1, and effect direction count. Treat tests as confirmatory only for prespecified contrasts; report the rest descriptively.

Do not multiply 20,000 bootstrap resamples over five observations and call that power. The power comes from new independent data realizations.

**Success criterion:** direction and practically meaningful magnitude are stable. If CIs include negligible effects, replace “shows” with “is consistent with” and center the robust qualitative pattern (format/name sensitivity) only if it survives.

### P0.4 Non-experimental repairs

**Questions resolved:** I06, I07, I08, I17.

- Verify the anonymous dataset URL from a clean/incognito environment and a command-line download; fix permissions and add checksums/file inventory.
- Define **semantic cue** as the variable-associated text exposed to the learner, and **matched attribution test** as a comparison holding graph, sampled data, seed, scoring, and method fixed while changing one evidence axis.
- Reposition novelty: the formal definition and generic SFT are not the claim. The contribution is a released protocol that jointly reports names-only, real-vs-anonymized, representation, data-budget, and matched classical controls on identical graph/data cells.
- Correct the framing: classical methods are data-only references under explicit simulated assumptions, not epistemic gold standards; semantic priors may help, hurt, or debias. The benchmark measures attributable complementarity.

## P1 — High value if P0 is on track (days 3–5)

### P1.1 Add one independent model family

Addresses I05/I16. Prefer a model with existing access and high structured-output reliability. Gemini or Claude via API would directly answer the reviewer examples; DeepSeek is useful only if it is the actual relevant model, not a distillation mislabeled as frontier DeepSeek. Run only the prespecified Sachs and Child summary grid with 10–20 paired seeds.

**UNCERTAIN:** API access, cost, and venue rules for new hosted-model results are not provided. If unavailable, narrow the claim to the evaluated OpenAI/Qwen/Llama families.

### P1.2 Edge-level evidence-use analysis

Addresses I13/I15. For each intervention target, measure whether predicted incident/downstream edges change in the direction supported by the intervention relative to observational-only prompts. Add 3–5 qualitative cases chosen by a prespecified rule (largest positive, median, largest negative effect), not cherry-picked examples.

### P1.3 Prompt/order robustness

Addresses I02/I14 and strengthens the benchmark story. Randomize placeholder assignment, variable order, and row order on the P0 subset. This tests whether anonymization results are an artifact of declared variable order or serialization.

## P2 — Useful but not decision-critical (days 5–6)

### P2.1 Contemporary benchmark bridge

Addresses I09/I10. If a recent perturbation/network-inference dataset is already supported, run a minimal bridge evaluation. Otherwise do not attempt a rushed integration: explain that real-data ground truth and causal assumptions differ and commit only to a clearly scoped future extension.

### P2.2 Related-work differentiation matrix

Addresses I07. Compare CausalMix against CausalGraphBench, CauScientist, Causal-LLM, SCP, and classical benchmarks across: names-only control, anonymization, fixed graph/data pairing, observational/interventional evidence, representation control, matched classical reference, released seeds/artifacts, and verifier-training support. Verify every cell from the cited papers before using it.

### P2.3 Resource/runtime accounting

Report per-cell tokens, GPU-hours, API calls, parse-failure rate, and total evaluation cost for the added slice. This supports benchmark usability and explains why the rebuttal uses a targeted rather than exhaustive grid.

## P3 — Defer or include only if essentially free

- Full 14-graph × all-model × all-format benchmark sweep.
- Diabetes/Pigs single-prompt evaluation at hundreds of nodes.
- Training a new frontier-scale causal LLM.
- Exhaustive prompt paraphrase search.
- Broad soft-intervention, confounding, cycles, and measurement-error extensions.
- Multiple new classical algorithms unless a reviewer identifies a missing baseline as decisive.

These consume the week without directly repairing the three AC-level validity concerns.

## One-week execution schedule

| Day | Workstream A | Workstream B | Workstream C | Gate |
|---|---|---|---|---|
| 1 | Reproduce existing tables; audit artifacts/link | Pilot Child/Synthetic-25 prompt length and GPU fit | Prepare LOGO splits and prespecified analysis | Stop/resize any run that cannot finish by day 4 |
| 2 | Launch four LOGO LoRA runs | Launch larger-graph local inference | Launch Sachs paired seeds / API cells | Confirm data/checkpoint provenance |
| 3 | Evaluate LOGO + format-only ablation | Continue paired runs | Implement conflict/data-swap and order controls | Early read only for failures, not hypothesis switching |
| 4 | Freeze P0 results | Run paired statistics and validity-inclusive metrics | Produce tables/plots and artifact checksums | Decide full, partial, or concession response per issue |
| 5 | Optional independent model family | Edge-level audit | Related-work matrix and terminology fixes | No new broad experiments after this day |
| 6 | Cross-check all numbers and provenance | Draft issue-by-issue evidence bullets | Internal adversarial review | Remove unsupported causal/general claims |
| 7 | Compress to venue limit | Final consistency/coverage audit | Submission formatting | Requires official response limit/rules |

## Recommended rebuttal thesis after evidence is known

The response should not argue that the reviewers overlooked existing caveats. It should say, in substance: the reviewers correctly identified the distinction between a useful attribution protocol and evidence sufficient for broad model claims; the new targeted tests address graph disjointness, scale/memorization, and replication; then report the outcomes and narrow claims wherever the tests do not support them.

## Blockers before drafting

1. Official NeurIPS 2026 rebuttal format and character/word limit.
2. Whether revised-paper uploads or only text responses are allowed.
3. Local availability of code, data generators, adapters, and base checkpoints.
4. Exact RTX 6000 model/VRAM and CUDA environment.
5. Access and budget for GPT-5.2-Pro, Claude, Gemini, or DeepSeek APIs.
6. Author approval to commit to any experiment in the rebuttal.
