# Go's Binary-Search Workflow: Five Levels of Explanation

This is an annotated application of the [guide](../five-level-explanation-guide.md), not a survey of the entire Go repository.

## Source and Scope

- **Subject:** the path from `sort.SearchInts` to `sort.Search`, and its first-qualifying-index contract.
- **Version:** Go 1.23.2, commit [`ed07b321aef7632f956ce991dd10fdd7e1abd827`](https://github.com/golang/go/commit/ed07b321aef7632f956ce991dd10fdd7e1abd827), selected as a fixed example, not a claim about the latest Go release.
- **Inspected:** the API comments and implementation in [`src/sort/search.go`](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search.go), and representative correctness, boundary, wrapper, and call-count tests in [`src/sort/search_test.go`](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search_test.go).
- **Limits:** focused source inspection only. No Go code, upstream tests, benchmarks, or experiments were run. The worked traces explain the source; they are not execution evidence. No historical novelty or whole-repository audit is claimed.

## The Through-Line

Given an ascending sequence, find the first position whose value is at least a target. Go implements this by searching for the boundary where a yes/no question changes from false to true, repeatedly discarding positions that cannot be that boundary. The answer is an insertion position, not proof that the target exists.

**Running example:** the values are `1, 3, 3, 7, 9`, and the target is `4`. The right position is before `7`.

## Level 1: Child / Newcomer

Imagine number cards laid out from smallest to largest: `1, 3, 3, 7, 9`. You want to put a new card, `4`, into the row without changing that order.

You could inspect every card. Instead, look near the middle. If that card is too small, everything to its left is also too small. You can ignore that part and continue with the rest. For this row, `4` belongs after the two `3`s and before `7`.

This is the idea behind Go's search function for sorted whole numbers. It finds a place for the new number even when no matching number is already present. The cards must already be in order. ([API contract and wrapper](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search.go#L119-L125))

**Annotation:** the cards represent the actual ordered values, not an analogy for how a computer stores them. The new capability is explaining why examining a middle card can eliminate other cards.

**Check 1:** Why could you not safely ignore the left-hand cards if the row were jumbled?

## Level 2: Teenager / Curious Beginner

**Refinement:** the card story did not specify how to choose between several possible positions. We now ask a precise yes/no question at each position: "Is this value at least `4`?"

For `1, 3, 3, 7, 9`, the answers are `no, no, no, yes, yes`. Because the values are ordered, once the answer becomes yes, it stays yes. The function seeks the first yes.

In this example, it checks the middle value `3`, then checks `9`, then `7`. The middle `3` is too small, so the earlier values can be ignored. The yes at `9` is not necessarily the first yes; checking earlier in the remaining region finds `7`. These are the choices made by the inspected loop. ([Search implementation](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search.go#L58-L73))

The returned position tells you where to insert `4`; it does not mean `7` equals `4`. If every value were smaller than the target, the position would be just after the final card. The function does not sort the input or insert anything. ([Contract](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search.go#L9-L31))

**Annotation:** the new capability is predicting the answer for another target, including one outside the existing values. The essential assumption is the no-then-yes pattern.

**Check 2:** Where would `10` go? Where would another `3` go under the first-yes rule?

## Level 3: College Undergraduate

**Refinement:** "before `7`" is a physical position. An API needs an exact representation, including when no qualifying element exists. Go uses zero-based indices: the five values have indices `0` through `4`, and index `5` represents the position after them.

In Go code with the `sort` package imported:

```go
a := []int{1, 3, 3, 7, 9}
x := 4
i := sort.SearchInts(a, x)
present := i < len(a) && a[i] == x
```

Here `i` is `3`, but `present` is false. The bounds check must precede indexing because the result may equal `len(a)`. This usage follows the API's documented distinction between an insertion position and membership. ([Membership example](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search.go#L19-L45))

The real path is:

1. [`SearchInts`](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search.go#L119-L125) passes `len(a)` and the predicate `a[i] >= x` to `Search`. A predicate is a function that answers a yes/no question.
2. [`Search`](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search.go#L58-L73) maintains a half-open interval `[i, j)`: positions from `i` up to, but not including, `j`.
3. It probes a midpoint `h`. A false result moves the left bound past `h`; a true result moves the right bound to `h`, retaining the possibility that `h` itself is the answer.
4. When the bounds meet, it returns that boundary.

For the running example:

| Bounds before probe | Midpoint | Predicate result | Bounds afterward |
|---|---|---|---|
| `[0, 5)` | `2`, value `3` | false | `[3, 5)` |
| `[3, 5)` | `4`, value `9` | true | `[3, 4)` |
| `[3, 4)` | `3`, value `7` | true | `[3, 3)` |

**Annotation:** the new capability is tracing an actual call through the wrapper and loop, rather than merely describing "divide and conquer."

**Check 3:** For target `3`, what are the returned index and `present` value? Why is the result not the second occurrence?

## Level 4: Graduate Student

**Refinement:** the trace explains one case, not correctness for all valid inputs. We need an invariant: a statement the loop maintains after every update.

Let `n` be the number of searchable positions and `f(k)` the predicate. Require `n >= 0` and a stable false-prefix/true-suffix pattern over `[0, n)`. Conceptually define `f(-1)` as false and `f(n)` as true; these are proof sentinels, not calls outside the input.

The source records the invariant `f(i-1) == false` and `f(j) == true`. Together with monotonicity, it brackets the first true position, including the sentinel `n` when all real answers are false.

- A false midpoint permits `i = h + 1`: all positions through `h` are false.
- A true midpoint permits `j = h`: the first true position is no later than `h`.
- With `i < j`, the midpoint satisfies `i <= h < j`, so either update strictly shrinks the interval.
- At `i == j`, the position immediately before the boundary is false and the boundary is true, using the sentinel convention if necessary.

This is an explanation of the invariant and updates in the [inspected implementation](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search.go#L58-L73), not a report of a separately machine-checked proof.

The inspected [`TestSearchExhaustive`](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search_test.go#L226-L235) covers every boundary position for sizes `0` through `100`, including all-false predicates. [`TestSearch`](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search_test.go#L22-L59) includes empty input, duplicates, descending-data predicates, and a large-size overflow case. [`TestSearchEfficiency`](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search_test.go#L147-L167) counts predicate calls for selected inputs. These are descriptions of test source, not claims that the tests passed here.

A verification plan would use the pinned Go toolchain/source, compare results with an independent linear first-true oracle, check that calls stay in range, and inspect call counts separately from elapsed time. Running `go test sort` against some other installed Go version would not verify this snapshot.

**Annotation:** the new capability is designing verification that distinguishes a source-level correctness argument, finite test coverage, and measured performance.

**Check 4:** What should happen for `n == 0`, and how could you test that the predicate is never called?

## Level 5: Expert

**Refinement:** the earlier levels explain a correct boundary search. An engineering choice also depends on the contract and cost model, not just the word "logarithmic."

`Search` generalizes integer lookup to a monotone predicate over integer indices. The sorted slice belongs to `SearchInts`; the generic routine requires the predicate pattern, not a particular container. For descending data, a different comparison can supply that pattern. This is an established standard-library abstraction, not a novelty claim. ([Contract](https://github.com/golang/go/blob/ed07b321aef7632f956ce991dd10fdd7e1abd827/src/sort/search.go#L9-L33))

Derived from the halving loop, predicate evaluations grow as `O(log n)` for positive `n`; elapsed time also depends on the cost of each evaluation and access to the underlying data. Sorting an initially unordered collection, preserving order during updates, or inserting into a slice introduces costs outside this function. The inspected call-count test is not a benchmark of those end-to-end costs.

The assumptions have observable consequences. With unsorted values `7, 1, 9` and target `4`, the predicate pattern is `true, false, true`. Following the loop yields index `2`, although index `0` already qualifies. The function has not violated its documented contract: the caller supplied an invalid predicate pattern. Changes to the data during a search can likewise invalidate the assumed stable pattern.

The expert decision is therefore whether a stable monotone question and affordable indexed probes exist for the workload. A linear scan may be more appropriate when establishing that structure costs more than the search saves. The choice needs workload evidence, not an asymptotic slogan.

**Annotation:** the new capability is deciding when this abstraction is applicable, separating its guarantee from caller obligations and whole-workflow performance.

**Check 5:** Would fast searches alone justify sorting a collection for one query? What information would change your decision?

## Answer Notes

These are expected answers, not observed learner responses.

<details>
<summary>Try the checks before opening the answers</summary>

1. In a jumbled row, a smaller middle card says nothing reliable about cards to its left. Discarding them could discard the answer.
2. `10` goes after `9`. Another `3` goes before the first existing `3`, because that is the first value at least `3`.
3. Index `1`, with `present == true`. The predicate first becomes true there; it also being true at index `2` does not make that later position the first boundary.
4. Return `0` without calling the predicate. Supply a predicate that records or fails on any call, and assert the return value and zero calls. This is a proposed test, not one run here.
5. Not on the search cost alone. Consider collection size, number of queries and updates, existing order, ordering and insertion costs, access costs, and required semantics. Benchmark representative end-to-end alternatives before making a performance claim.

</details>

## Evidence and Open Questions

| Claim | Evidence or status |
|---|---|
| Returns the first qualifying index or `n` | API comments and inspected loop in `search.go` |
| `SearchInts` delegates through `a[i] >= x` | Inspected wrapper in `search.go` |
| The repository contains relevant correctness and call-count checks | Inspected `search_test.go`; no test execution |
| Traces, invariant explanation, and cost-model discussion | Analysis of the pinned source, not benchmark results |
| Performance for a particular application | Unknown without a workload and measurements |

All five levels retain the same ordered-boundary problem. The progression adds prediction, an API trace, verification obligations, then applicability analysis. Use the [rubric](../evaluation-rubric.md) to review those additions; use actual reader answers to assess comprehension.
