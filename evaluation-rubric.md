# Evaluating a Five-Level Explanation

Use this with the [guide](./five-level-explanation-guide.md) and its [quality checklist](./five-level-explanation-guide.md#final-quality-checklist). Evaluate the explanation against inspected sources, not against how authoritative it sounds.

## Accuracy and Evidence Are Gates

Rate each dimension from 0 to 2. These are editorial conventions, not a validated measure of learning.

| Dimension | 0: fails | 1: needs revision | 2: meets the standard |
|---|---|---|---|
| **Accuracy: gate** | Material contradiction, invalid worked result, or a simplification that changes the claim | Ambiguous wording or omitted conditions could materially mislead | Claims, examples, and qualifications remain accurate across all five levels |
| **Evidence: gate** | Fabricated source, invented result, or claimed execution that did not occur | Consequential claims lack traceable support or mix observation with inference | Consequential claims have inspected support; inference, unknowns, and access or execution limits are explicit |
| **Progression** | Five paraphrases or unrelated summaries | Some transitions add detail without enabling a distinct capability | Every transition enables a new explanatory capability and bridges important refinements |
| **Mechanism** | Labels, features, or equations replace explanation | A causal or logical chain is present but a key link is missing | A worked case connects conditions to steps and consequences; a suitable contrast or counterfactual tests the account |
| **Audience fit** | Untaught prerequisites, patronizing language, or irrelevant complexity | Some terms, checks, or background do not fit the stated audience | Prerequisites and symbols are introduced when needed; each level and its diagnostic are readable for that audience |

**Acceptance rule:** accuracy and evidence must both be 2. Every other dimension must be at least 1; a 1 identifies a specific revision still worth making. Require 2 in every dimension before presenting an example as a model to imitate. Do not average away a failed gate or turn a total score into a claim about learning.

An appropriately narrowed explanation can pass: "Only the abstract was available" is a legitimate limit. Inventing inaccessible methods is not. Remove or qualify unsupported claims rather than adding a citation that does not establish them.

## A Short Review Procedure

1. Check consequential claims, computations, source locations, and execution disclosures. Stop acceptance if either gate fails.
2. Write one sentence per level: "After this, the reader can..." Compare adjacent sentences. If two say essentially the same thing, revise the progression.
3. Follow the running example through the real mechanism. Change one condition or compare a nearby alternative; verify the explanation predicts the difference.
4. Read each level on its own for untaught terms and missing qualifications. Check that later sections refine rather than rescue earlier claims.
5. Check any diagnostic answer notes. Record whether a reader actually attempted the questions; otherwise comprehension remains untested.

This review evaluates the artifact. Observing a learner apply the idea is separate evidence. An LLM's assertion that its own explanation passes is not an independent check.

## Sources and Design Rationale

The following original WIRED episodes illustrate techniques used in this guide. They are design precedents, not evidence that exactly five levels are necessary or effective. Do not treat an analogy, transcript, or expert's conversational shorthand as a technical authority for an unrelated claim.

| Example | Technique to examine |
|---|---|
| [Machine learning: Hilary Mason](https://www.wired.com/video/watch/5-levels-machine-learning) | Use familiar classifications, then challenge the offered categories with a case that fits neither |
| [Algorithms: David Malan](https://www.wired.com/video/watch/5-levels-wi-5-levels-algorithm) | Execute instructions literally to reveal ambiguity; compare search strategies on a concrete input size |
| [Zero-knowledge proofs: Amit Sahai](https://www.wired.com/video/watch/5-levels-zero-knowledge-proof) | Move from a physical demonstration to a protocol and explain why randomness matters |
| [Infinity: Emily Riehl](https://www.wired.com/video/watch/mathematician-explains-infinity-in-5-levels-of-difficulty) | Refine an intuitive idea through correspondences, cardinality, and explicit foundational assumptions |
| [Fractals: Keenan Crane](https://www.wired.com/video/watch/5-levels-computer-scientist-explains-fractals-in-5-levels-of-difficulty) | Make scale-dependent measurement and recursive construction visible |
| [Quantum computing: Talia Gershon](https://www.wired.com/video/watch/5-levels-4) | Use accessible props, then examine technical bottlenecks and misconceptions |
| [Black holes: Janna Levin](https://www.wired.com/video/watch/5-levels-astrophysicist-explains-black-holes-in-5-levels-of-difficulty) | Elicit a listener's initial model and distinguish theoretical predictions from unresolved physics |
| [Blockchain: Bettina Warburg](https://www.wired.com/video/watch/expert-explains-one-concept-in-5-levels-of-difficulty-blockchain) | Start from familiar trade and trust questions; check that accessible language has not turned conditional guarantees into absolutes |

The [Bitcoin developer guide's proof-of-work discussion](https://developer.bitcoin.org/devguide/block_chain.html#proof-of-work) is one concrete reason to retain conditions when explaining tamper resistance: it discusses attacks against transaction history. Borrow an explanation technique without inheriting an overstatement.

Rozenblit and Keil's (2002) [study of the illusion of explanatory depth](https://pmc.ncbi.nlm.nih.gov/articles/PMC3062901/) provides a different kind of evidence. In Study 1, participants' self-ratings fell after attempts to explain mechanisms in detail (Sections 2.1.1-2.1.3). This motivates distinguishing felt clarity from demonstrated explanatory ability. It does **not** test this guide, LLM explanations, the proposed rubric, or the educational superiority of a five-level sequence. See the [annotated paper example](./examples/research-paper.md) for the study's scope and limits.

## Worked Applications

- [Paper example](./examples/research-paper.md): increasingly precise interpretation of one reported study.
- [Repository example](./examples/repository.md): a trace, a correctness argument, inspected tests, and an expert applicability decision.
- [Weak example](./examples/weak-explanation.md): failed gates and progression problems, followed by a targeted repair.
