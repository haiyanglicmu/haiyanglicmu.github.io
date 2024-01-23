---
title: Maximum Split of Positive Even Integers
date: 2024-01-22 22:12:10
tags:
mathjax: true
---
# Maximum Split of Positive Even Integers

You are given an integer `finalSum`. Split it into a sum of a **maximum** number of **unique** positive even integers.

For example, given `finalSum = 12`, the following splits are **valid**: `(12)`, `(2 + 10)`, `(2 + 4 + 6)`, and `(4 + 8)`. Among them, `(2 + 4 + 6)` contains the maximum number of integers. Note that `finalSum` cannot be split into `(2 + 2 + 4 + 4)` as all the numbers should be unique.

Return *a list of integers that represent a valid split containing a **maximum** number of integers*. If no valid split exists for `finalSum`, return *an **empty** list*. You may return the integers in **any** order.

**Example 1:**

```
Input: finalSum = 12
Output: [2,4,6]
Explanation: The following are valid splits:(12),(2 + 10),(2 + 4 + 6), and(4 + 8).
(2 + 4 + 6) has the maximum number of integers, which is 3. Thus, we return [2,4,6].
Note that [2,6,4], [6,2,4], etc. are also accepted.
```

**Example 2:**

```
Input: finalSum = 7
Output: []
Explanation: There are no valid splits for the given finalSum.
Thus, we return an empty array.
```

**Example 3:**

```
Input: finalSum = 28
Output: [6,8,2,12]
Explanation: The following are valid splits:(2 + 26),(6 + 8 + 2 + 12), and(4 + 24).
(6 + 8 + 2 + 12) has the maximum number of integers, which is 4. Thus, we return [6,8,2,12].
Note that [10,2,4,12], [6,2,4,16], etc. are also accepted.
```

**Constraints:**

- `1 <= finalSum <= 10^10`

We can solve this problem using a **greedy** algorithm.

We want to split the number into maximum number of unique even integers. So at each step, we can take out the next smallest even number from the original number.

For example, for the number 12, we can try

1. take out 2, 12 - 2 = 10
2. take out 4, 10 - 4 = 6
3. take out 6, 6 - 6 = 0

such that the result is (2, 4, 6)

Of course this sequence of numbers (2, 4, 6, 8, …) does not always perfectly sum up to the original number. For example what if the sum is 14.

1. take out 2, 14 - 2 = 12
2. take out 4, 12 - 4 = 8
3. take out 6, 8 - 6 = 2

At step 3, if we take out 6, we get 2 again!

Let’s say at step i, after deducting the numbers 2, 2*2, …, 2*(i - 1) from the original number, we’re left with the number N.

Can we use 2*i at step i? That is determined by if N - 2*i results in number that has already been used.

If 2*i = N/2, N - 2*i = 2*i, we get the same number 2*i.

If 2*i > N/2, N - 2*i < 2*i, we get an even number that is small than 2*i, which is one of 2, 2*2, …, 2*(i -1), we get a duplicate!

So we can see the stopping condition is when 2*i ≥ N / 2, at which point we should just take out N all together.

So using the greedy algorithm, we’ve basically divided the original number S into a sequence like this:

$$
S = 2\times1+2\times2+...+2 i+2j
$$

where i is the largest integer such that $2i <(S - \sum_{n=1}^{i-1}2n)/2$, and 2j is what’s left.

To translate this into code, we get the following.

```kotlin
fun maximumEvenSplit(finalSum: Long): List<Long> {
    if (finalSum % 2L != 0L) return emptyList()

    var i = 2L
    var sum = finalSum
    val result = mutableListOf<Long>()
    while (i < sum / 2L) {
        result.add(i)
        sum -= i
        i += 2L
    }
    result.add(sum)
    return result
}
```

Now how do we prove that this greedy approach actually gives us the most optimal answer?

Let’s look at an example, such as 30. Using the greedy approach, we get the sequence (2, 4, 6, 8, 10). By proof of contradiction, let’s say at any step i, we did not use 2i, but instead used a larger even number 2k.
dy sequence.

When the alternative sequence is complete and sums up to the original number S, the sum of the greedy sequence is still < S. At this point, we may or may not keep going with the greedy sequence (depending if 2i < N/2 as we discussed above). But we have proved that the greedy sequence is at least as good as any alternative optimal sequence, thus the greedy sequence itself is optimal as well.

We can see in the alternative sequence, for each number ≥ 2k, there is a corresponding smaller number 2i, 2(i + 1), …. in the gree
