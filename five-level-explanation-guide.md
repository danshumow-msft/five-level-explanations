# Explaining Technical Work at Five Levels

## Purpose

Use this guide to explain a research paper, technical project, or GitHub repository at five progressively deeper levels:

1. **Child / newcomer:** understand the central idea without technical prerequisites.
2. **Teenager / curious beginner:** understand the mechanism and basic trade-offs.
3. **College undergraduate:** connect the idea to relevant technical foundations.
4. **Graduate student:** examine methods, assumptions, evidence, and reproducibility.
5. **Expert:** assess the precise contribution, boundaries, and implications.

These are audience models, not judgments of intelligence or rigid age requirements. A specialist in one field may need a newcomer explanation of another.

**The goal is five increasingly precise explanations of the same central idea, not five unrelated summaries or the same paragraph with harder vocabulary.**

## Core Principles

### 1. Ground the explanation in inspected sources

Read the supplied material before explaining it. Identify the source and version when available: paper title and revision, project milestone, or repository URL and commit.

Support consequential technical claims with source locations. Use paper sections, equations, figures, or tables; repository file paths and symbols; and project documents or recorded results. Prefer commit-pinned repository links when practical.

Never invent citations, file contents, experiments, implementation details, or source locations. If access is incomplete, state what was available and narrow the explanation accordingly.

### 2. Establish one explanatory spine

Before writing the five levels, identify:

- **Problem:** What difficulty or question motivates the work?
- **Central idea:** What is the proposed way of addressing it?
- **Mechanism:** How do the important steps connect?
- **Result:** What does the work produce, establish, or enable?
- **Boundary:** Under what conditions does the explanation or result hold?

Keep this spine recognizable at every level. Distinguish the work's main contribution from supporting infrastructure and incidental implementation choices.

### 3. Simplify presentation, not truth

Reduce prerequisites, terminology, and detail before weakening accuracy. Preserve qualifications that materially change a claim, even at the first level.

For example, "This method can be faster for a particular kind of input" must not become "This method is always faster."

When a simplification will need refinement later, signal its boundary plainly: "For now, imagine..." or "This picture leaves out..."

### 4. Explain causes and mechanisms, not just labels

Do not substitute a list of technologies, equations, or module names for an explanation.

Follow a concrete chain:

**Input or starting condition -> important transformation -> output or consequence.**

Explain why the transformation helps, what information or resources it requires, and where it can fail. For theoretical work, the chain may run from assumptions through a key argument to a conclusion rather than through an executable process.

### 5. Use analogies as scaffolding

Choose a concrete analogy or running example that helps explain the central mechanism.

- Map the relevant parts of the analogy to the real system.
- State where the analogy stops working.
- Reuse the example across levels when it remains accurate.
- Replace the analogy with the actual mechanism as precision increases.

Do not stretch one analogy across unrelated components or treat an intuitive resemblance as evidence.

### 6. Build prerequisites progressively

Introduce terminology when it becomes useful, and define it before relying on it. Explain what an equation expresses before manipulating its symbols.

Increase depth by adding mechanisms, assumptions, formal structure, and evidence, not merely longer words. At higher levels, spend less time on elementary background and more on the work's distinctive contribution.

Keep each level independently readable, even though the sequence is cumulative.

### 7. Separate claims, implementation, and verification

Distinguish among:

- **Source claim:** what the authors or maintainers say.
- **Inspected evidence:** what a proof, result, document, or code path supports.
- **Execution evidence:** what a reported or personally run experiment or test establishes.
- **Interpretation:** a reasoned explanation or inference.
- **Unknown:** what the available material does not establish.

A README describes intended behavior; it does not prove the implementation delivers it. A passing test supports the cases it exercises; it does not establish universal correctness. A paper's reported result is not an independently reproduced result.

State whether code or experiments were actually run. Do not imply execution from source inspection.

### 8. Calibrate confidence and criticism

Match certainty to evidence. Distinguish a demonstrated limitation from a plausible concern or unanswered question.

Do not assume a feature is absent because it was not found in a limited inspection. Do not invent flaws to make the expert level sound sophisticated, or claim novelty without an adequate comparison to prior work.

An explanation is not automatically an exhaustive audit or literature review.

## Inspection Workflow

### Step 1: Set scope

Identify the subject, the reader's question, and the available sources. Choose a coherent unit to explain: a paper's main contribution, a project's central workflow, or a repository's primary capability.

For a large work, explain the central system and its most relevant supporting parts rather than cataloging everything. Ask for clarification only when competing interpretations would materially change the explanation.

Record meaningful limits, such as access to only an abstract, one subdirectory, or documentation without runnable code.

### Step 2: Inspect the relevant evidence

| Source type | Inspect first | Questions to resolve |
|---|---|---|
| Research paper | Abstract, problem statement, main method or argument, central results, assumptions, limitations; relevant appendices as needed | What is claimed? What supports it? Which conclusions are proved, measured, conjectured, or proposed? |
| Technical project | Goals, design, representative artifacts, dependencies, milestones, evaluations, and known limitations | What is the intended outcome? What exists now? What remains planned? How is success measured? |
| GitHub repository | README, manifests, relevant entry points, core implementation, representative tests and examples, relevant configuration | What does it do? How does one important workflow execute? What does the implementation support? What behavior is tested? |

Follow the central mechanism far enough to explain it accurately. Avoid substituting the abstract for the paper, the README for the implementation, or a directory listing for architecture.

If sources disagree, describe the discrepancy with provenance. Do not silently combine incompatible descriptions.

### Step 3: Assemble a compact evidence map

For the main claims, connect each claim to its supporting source and qualification. This can remain working notes unless an evidence table would help the reader.

Use the map to distinguish background knowledge from the work's contribution, identify missing evidence, and keep all five levels consistent.

### Step 4: Design the progression

Choose one running example and identify which details each level adds. Keep the problem, mechanism, and central conclusion stable.

Include a limitation at every level in language suitable for that audience. Distribute important qualifications where they matter rather than hiding all caveats at the end.

### Step 5: Write and check

Draft the five explanations, then check them against the sources and against one another. Correct misleading simplifications rather than relying on later sections to undo them.

## What Each Level Should Accomplish

### Level 1: Child / Newcomer

**Reader's question:** "What is this, and why would anyone care?"

- Start with a recognizable problem or situation.
- Explain the central idea using ordinary language and a concrete example.
- Show a simple cause-and-effect relationship.
- Mention one important boundary without technical jargon.
- Avoid unexplained acronyms, formal notation, and patronizing language.

**Success test:** The reader can describe the problem and basic idea in their own words.

### Level 2: Teenager / Curious Beginner

**Reader's question:** "How does it actually work?"

- Expand the example into the main steps or interacting parts.
- Introduce a small number of useful technical terms.
- Explain why this approach might help compared with an obvious alternative.
- Introduce a meaningful trade-off or failure case.
- Use simple arithmetic or diagrams when they clarify the mechanism.

**Success test:** The reader can trace a simple case and explain why the approach is useful.

### Level 3: College Undergraduate

**Reader's question:** "How does this connect to the technical foundations I know?"

- State the relevant prerequisites without assuming unrelated expertise.
- Describe the actual algorithm, architecture, model, or argument.
- Introduce essential equations, data structures, interfaces, or abstractions.
- Define symbols and connect formal details to the running example.
- Explain relevant costs, assumptions, and comparisons with a standard alternative.

For a repository, trace a representative path through real components. For a paper, distinguish the proposed method or theorem from the evidence supporting it.

**Success test:** The reader can sketch the mechanism and explain its main technical trade-off.

### Level 4: Graduate Student

**Reader's question:** "What would I need to reproduce, evaluate, or extend this?"

- Make assumptions, objectives, and important definitions explicit.
- Explain the nontrivial derivation, proof idea, algorithmic step, or design decision.
- Examine evidence appropriate to the work: proof conditions, experimental controls, datasets, baselines, tests, or operational measurements.
- Identify what reproduction requires and what information is missing.
- Distinguish established conclusions from conjecture and future work.

Do not manufacture an experimental protocol for purely theoretical work or demand irrelevant formalism from an engineering project.

**Success test:** The reader can outline an appropriate reproduction or verification plan and identify a consequential assumption.

### Level 5: Expert

**Reader's question:** "What exactly is new, when does it hold, and what follows from it?"

- State the precise contribution and its relationship to established approaches.
- Analyze the decisive assumptions, bottlenecks, or proof obligations.
- Distinguish asymptotic claims from practical performance when relevant.
- Examine generality, robustness, scaling, and known failure boundaries.
- Identify implications and open questions supported by the available evidence.

Use domain terminology where it improves precision. Expert depth comes from discriminating analysis, not dense notation or performative skepticism. If prior work was not inspected sufficiently to assess novelty, say so.

**Success test:** The reader can assess the contribution's significance and boundaries, and identify what evidence would change that assessment.

## Recommended Output Structure

```markdown
# [Subject]: Five Levels of Explanation

## Source and Scope
- Sources and version:
- Material inspected:
- Important access or verification limits:

## The Through-Line
[The problem, central idea, and main result in two or three sentences.]

## Level 1: Child / Newcomer
[Intuitive explanation, concrete example, and essential boundary.]

## Level 2: Teenager / Curious Beginner
[Mechanism, main steps, and a meaningful trade-off.]

## Level 3: College Undergraduate
[Technical structure, essential formalism, and a worked example or trace.]

## Level 4: Graduate Student
[Assumptions, methods, evidence, and reproduction or verification requirements.]

## Level 5: Expert
[Precise contribution, comparisons, boundaries, and implications.]

## Evidence and Open Questions
[Source-backed support for consequential claims and unresolved uncertainties.]
```

Place citations near the claims they support. Add a glossary, diagram, or vocabulary bridge only when it improves understanding. Adapt length to the subject and the reader's request; do not make every level equally long by default.

## Final Quality Checklist

- [ ] All five levels explain the same central idea.
- [ ] Each level adds understanding, not just vocabulary.
- [ ] Simplifications remain compatible with the more precise explanations.
- [ ] Analogies have an explicit mapping and a clear boundary.
- [ ] Terms, symbols, and prerequisites are introduced before use.
- [ ] The mechanism is explained, not replaced by a feature or component list.
- [ ] Consequential claims have traceable support.
- [ ] Author claims, inspected evidence, execution results, and inference remain distinct.
- [ ] Planned capabilities are not presented as implemented or verified.
- [ ] Limitations and uncertainty are visible at the levels where they matter.
- [ ] Inspection and execution limits are disclosed.
- [ ] The reader can answer the success question for each level.

## Reusable Request

> Apply this guide to [paper, project, or repository]. Explain the same central idea at all five levels, using a consistent running example where useful. Focus on [question or topic]. Ground consequential claims in inspected sources, preserve important qualifications, and distinguish source claims, implementation evidence, and actual verification. State access limits and unresolved questions explicitly.
