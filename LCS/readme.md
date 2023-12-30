# Longest Common Subsequence

Here, we'll discuss the optimal approach to solve the Longest Common Subsequence problem. We'll use tabulation & dyanamic programming to solve this problem.

Leetcode problem link: https://leetcode.com/problems/longest-common-subsequence/

## Problem Statement

Given two strings `s1` and `s2`, find the length of their longest common subsequence (LCS).

A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

## Example

**Input**: s1 = "abcde", s2 = "ace"

**Output**: 3

**Explanation**: The longest common subsequence is "ace" and its length is 3.

a b c d e
a c e

A subsequence does not have to be contiguous, if we add a condition that it has to be contiguous, then the we'll call it a substring, which is a very similar problem, called the Longest Common Substring.

## Solution

We'll use tabulation to solve this problem. We'll create a 2D array of size `s1.length + 1` x `s2.length + 1`. We'll fill the first row and first column with 0s. Then we'll iterate over the array and fill it according to the following conditions:

- If the characters at the current indices are equal, then we'll add 1 to the value at the previous diagonal index.
- If the characters at the current indices are not equal, then we'll take the maximum of the value at the previous row index and the value at the previous column index.

We'll return the maximum value in the 2D array as the answer.

## Implementation

```typescript
function longestCommonSubsequence(s1: string, s2: string): number {
  let result: number = 0
  const m: number = s1.length
  const n: number = s2.length

  // creating a 2D array of size m+1 x n+1 is a bit different in TypeScript
  // we have to first create an array of size m+1 and then fill it with 0s using fill()
  // then we have to map each element of the array to a new array of size n+1 and fill it with 0s using fill()
  // this is because TypeScript does not allow us to create a 2D array directly
  let dp: number[][] = new Array(m + 1).fill(0).map(() => new Array(n + 1).fill(0))

  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      if (s1[i - 1] === s2[j - 1]) {
        dp[i][j] = 1 + dp[i - 1][j - 1]
      } else {
        dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1])

        // If the problem asks for the longest common substring, then we'll just have to change the else part to:
        // dp[i][j] = 0
        // WHY?
        // If the characters at the current indices are not equal, then we'll just have to set the value at the current index to 0.
        // Because we are looking for a substring, which has to be contiguous.
      }
    }
  }

  // search for max value in dp array
  // conventional for loop
  for (let i = 0; i <= m; i++) {
    for (let j = 0; j <= n; j++) {
      result = Math.max(result, dp[i][j])
    }
  }
  // OR
  // js map
  // dp.map( r=> r.map( i=> i>result ? result=i : null ));

  return result
}
```

## Complexity Analysis

- **Time Complexity:** `O(m*n)`, where `m` is the length of `s1` and `n` is the length of `s2`. We are iterating over the entire 2D array once.

- **Space Complexity:** `O(m*n)`, where `m` is the length of `s1` and `n` is the length of `s2`. We are creating a 2D array of size `m+1` x `n+1`.
