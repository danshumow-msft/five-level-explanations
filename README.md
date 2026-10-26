# Five-Level Explanations

A reusable Markdown guide for asking an LLM to explain a research paper, technical project, or GitHub repository at five progressively deeper levels.

The goal is **the same central idea, explained with increasing precision**: not five unrelated summaries, and not the same paragraph rewritten with harder words.

## The Five Levels

| Level | Audience | Main question |
|---|---|---|
| 1 | Child / newcomer | What is this, and why would anyone care? |
| 2 | Teenager / curious beginner | How does it actually work? |
| 3 | College undergraduate | How does this connect to technical foundations? |
| 4 | Graduate student | What would I need to reproduce, evaluate, or extend this? |
| 5 | Expert | What deserves expert attention, under which assumptions, and with what consequences? |

These are models of familiarity with a subject, not rigid age requirements or judgments of intelligence.

## Quick Start

1. Open or download [the explanation guide](./five-level-explanation-guide.md).
2. Give the guide to your LLM as an attachment or paste its contents into the conversation.
3. Supply the material to explain: a paper, project documents, or an accessible repository.
4. Use the prompt below, replacing the bracketed text.

```text
Use the attached Five-Level Explanation Guide to explain [source].

Focus on [the central contribution, a particular mechanism, or a question].

Explain the same central idea at all five levels. Use a consistent running
example where useful, and preserve important qualifications at every level.
Make each level enable a new explanatory capability, bridge important
refinements, and use a suitable contrast or changed condition to test the
mechanism.

Ground consequential claims in sources you actually inspect. Distinguish
author claims, implementation evidence, and actual verification. State what
you could not access and whether you ran any code or experiments.
```

No installation, API key, or particular LLM product is required by this guide. Access to the model and source material depends on the tools you choose.

## Choose the Right Source Material

### Research Paper

Provide the paper PDF, its full text, or a link the model can access. Include appendices or supplementary material when they matter to the explanation.

```text
Apply the Five-Level Explanation Guide to the attached paper.
Focus on its main contribution, how the method works, and what the evidence
actually supports. Cite relevant sections, equations, figures, or tables.
Distinguish proved results, reported measurements, conjectures, and proposals.
```

If only the abstract is available, request an abstract-based explanation with that limitation stated explicitly. Do not treat it as an analysis of the full paper.

### Technical Project

Provide the project goals, design documents, representative artifacts, current status, and any evaluations or known limitations.

```text
Apply the Five-Level Explanation Guide to these project materials.
Explain the problem, the central design, and how the main workflow connects
inputs to outcomes. Distinguish intended capabilities, implemented work,
and outcomes that have actually been demonstrated.
```

### GitHub Repository

Provide a repository URL if your model can browse repositories, or open a checkout in a coding assistant. Identify the branch, tag, or commit when possible.

```text
Apply the Five-Level Explanation Guide to this repository.
Focus on [its primary capability or a named workflow].

Inspect the README, relevant manifests, entry points, core implementation,
and representative tests or examples. Trace one real path through the code
instead of merely listing directories or technologies.

Cite file paths and symbols, and pin links to the inspected commit when
practical. Distinguish documented behavior, implementation evidence, and
tests or commands you actually ran.
```

If the model cannot access the repository, provide a focused set of relevant files. Do not upload credentials, private data, or material you are not authorized to share.

## Using It With a Coding Assistant

Reference [the guide](./five-level-explanation-guide.md) directly in your request, attach it, or copy it into your working checkout and ask the assistant to read it.

This is a standalone Markdown document. Merely adding it to a repository does **not** guarantee that an assistant will load it automatically. Use your assistant's supported attachment or file-reference mechanism.

If you want the repository explained without changes, add:

```text
This is a read-only explanation task. Do not modify files, install packages,
or run code unless I explicitly ask.
```

## What a Good Result Looks Like

- A short statement of the sources, version, and inspection limits.
- One recognizable problem and central idea carried through all five levels.
- Progressively deeper mechanisms, assumptions, and evidence.
- A new ability to explain, predict, apply, verify, or decide at each level.
- Clear bridges showing what a simpler model left out and why it now matters.
- Analogies that clarify the idea and acknowledge where they stop working.
- Source-backed consequential claims and clearly labeled uncertainty.
- An expert section that analyzes the contribution rather than simply adding jargon.

The guide includes a recommended output template, success criteria for each level, optional comprehension checks, and a final quality checklist. Use the [evaluation rubric](./evaluation-rubric.md) to check the result: accuracy and evidence are acceptance gates, not points that polished prose can make up for.

## Annotated Examples

Start with one complete example to see the rules in action:

| Example | What it demonstrates |
|---|---|
| [Research paper: the illusion of explanatory depth](./examples/research-paper.md) | Explains one study at all five levels while distinguishing reported self-ratings, interpretation, and a proposed verification plan |
| [Repository: Go's binary-search workflow](./examples/repository.md) | Traces a real API through its implementation and inspected tests, with a worked case and an ordering counterexample |
| [Weak explanation and repair](./examples/weak-explanation.md) | Shows why five headings and increasingly technical words do not produce five levels, even after a factual error is fixed |

The complete examples include progression annotations, refinement bridges, comprehension checks, and separate answer notes. The annotations are teaching aids, not required boilerplate for every explanation. They do not demonstrate that a reader has learned until the reader actually responds.

## Useful Follow-Ups

After the first explanation, ask for a focused extension:

```text
Expand Level 3 into a worked example. Define each new term and symbol.
```

```text
Show how the running example maps to the actual implementation.
```

```text
Identify the most consequential assumption and explain what changes
if it does not hold. Separate source-backed conclusions from speculation.
```

```text
Show what each level lets me do that the previous level did not. At each
transition, explain which simplification you refine and why it matters.
```

Static explanations are the default. For adaptive tutoring instead, request:

```text
Use the guide interactively. Start with one focused question about my
current understanding, explain at an appropriate level, then ask me to
predict or apply something. Adapt to my actual answers rather than
assuming I understood. Do not give all five levels at once.
```

## Limits

This framework improves the structure of an explanation; it does not guarantee factual correctness. Check important claims against the original sources.

An explanation is not an exhaustive code audit, independent reproduction, or comprehensive literature review. A README is not proof of implementation, and passing tests are not proof of universal correctness.

The five levels are an editorial framework, not a validated developmental scale or a claim that five stages are optimal for learning. A clear explanation, an author's self-check, and a reader's demonstrated understanding are different things.

## Background

Inspired by the general "five levels of explanation" format popularized by series such as [WIRED's 5 Levels](https://www.wired.com/video/series/5-levels/). This is an independent guide and is not affiliated with or endorsed by WIRED.

See the rubric's [sources and design rationale](./evaluation-rubric.md#sources-and-design-rationale) for concrete explanation techniques and the limits of the supporting evidence.

## License

This guide and its accompanying documentation are available under the [MIT License](./LICENSE).
