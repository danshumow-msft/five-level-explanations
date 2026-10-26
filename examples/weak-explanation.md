# A Weak Five-Level Explanation and a Targeted Repair

This synthetic counterexample applies the [evaluation rubric](../evaluation-rubric.md) to the same Go search workflow as the [complete repository example](./repository.md). It is deliberately flawed, not a quotation from a source or a record of a model's output.

## The Weak Version

### Level 1: Child / Newcomer

Go's search finds a number quickly.

### Level 2: Teenager / Curious Beginner

It searches efficiently by repeatedly splitting the data. If the number is missing, it returns `-1`.

### Level 3: College Undergraduate

The routine implements logarithmic lookup through recursive-style interval partitioning.

### Level 4: Graduate Student

It leverages a monotonic predicate to optimize the search-space reduction process.

### Level 5: Expert

It is an optimal information-theoretic retrieval primitive for high-performance applications.

## Why It Fails

| Rubric dimension | Rating | Concrete reason |
|---|---|---|
| Accuracy: gate | 0 | The `-1` claim contradicts the actual API; insertion position and membership are conflated |
| Evidence: gate | 1 | No support is given for the consequential API and optimality claims |
| Progression | 0 | The main addition at each level is vocabulary, not a new capability |
| Mechanism | 0 | No input, boundary rule, update, or worked result explains why the search works |
| Audience fit | 1 | The early prose is accessible but empty; later jargon is neither defined nor useful |

**Verdict: reject.** Good headings and concise prose cannot compensate for a failed accuracy or evidence gate.

The pinned Go API returns the first qualifying index, or `n` if no position qualifies. For sorted integer data, an absent value can still have an insertion index less than `n`; missing does not mean `-1`. ([Inspected contract](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search.go#L9-L31); [`SearchInts`](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search.go#L119-L125))

Even correcting that sentence and adding its citation would not repair the explanation's progression. "Efficient," "logarithmic," and "optimal" are not three different accounts of the mechanism. The inspected implementation is an iterative loop; calling it "recursive-style" does not explain its updates. The sweeping optimality claim needs conditions, a cost model, and evidence, or should be removed.

## Repair the Structure, Not Just the Wording

Keep the same invariant at every level: find the first value at least a target in an ascending sequence.

| Level | Repair | New reader capability |
|---|---|---|
| 1 | Use cards `1, 3, 3, 7, 9` and show why `4` belongs before `7`; retain the ordering condition | Explain why a middle comparison can eliminate positions |
| 2 | Turn the values into `no, no, no, yes, yes` for "at least 4"; explain first yes and the after-the-end case | Predict the position for another target |
| 3 | Trace `SearchInts` into `Search`, show the bounds updates, and separate insertion index from membership | Work through the real call and its result |
| 4 | Explain the loop invariant and match verification questions to inspected tests | Design a check that could reveal a specific implementation failure |
| 5 | Compare predicate-call cost with whole-workflow cost and examine a nonmonotone counterexample | Decide when the abstraction and its performance claim apply |

Bridge each transition explicitly. For example: "The card story left out how the API represents a position. We now need an index because the result must also represent insertion after the final element."

The [complete repository example](./repository.md) develops all five repairs, provides source links and separate answer notes, and discloses that the upstream code and tests were not executed. This repair table is a plan for that explanation, not a substitute for it.

## A Check for the Reviewer

For values `1, 3, 3, 7, 9` and target `4`, explain why the returned index is `3` but the target is not present. Then explain what fails if the values are `7, 1, 9`.

<details>
<summary>Answer notes</summary>

Index `3` is the first value at least `4`, namely `7`; membership also requires equality with the target. In `7, 1, 9`, the predicate is `true, false, true`, violating the required false-prefix/true-suffix pattern. The loop can return index `2` even though index `0` qualifies. It is not a sorting operation and supplies no first-true guarantee for that input.

</details>

Being able to spot the `-1` mistake tests one fact. Being able to explain the trace and broken assumption tests more of the mechanism. Neither should be credited to a reader who has not actually answered.
