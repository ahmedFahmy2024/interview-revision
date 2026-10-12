# Uniform Substring

Given a string `s` of lowercase English letters, return the length of the longest non-empty contiguous substring that contains only one unique character.

## Example 1

**Input:** `s = "abcddddd"`
**Output:** `5`

The longest contiguous substring with only one unique character is `"ddddd"`, which has a length of 5.

## Example 2

**Input:** `s = "aaabbbaaa"`
**Output:** `3`

The longest contiguous substrings with only one unique character are `"aaa"` and `"bbb"`, both of length 3.

## Constraints

- `1 <= n <= 2 * 10^5`
- `s` contains only lowercase English letters (`'a'`-`'z'`)

---

## Solution 1: Single Pass (Optimal — O(n))

```javascript
function longestUniformSubstring(s) {
    let maxLen = 1;
    let curLen = 1;

    for (let i = 1; i < s.length; i++) {
        if (s[i] === s[i - 1]) {
            curLen++;
        } else {
            curLen = 1;
        }
        maxLen = Math.max(maxLen, curLen);
    }

    return maxLen;
}
```

**How it works:** Tracks the length of the current run of identical characters (`curLen`) in one pass. When the character changes, the run resets to 1. `maxLen` keeps the longest run seen so far.

- Time complexity: O(n)
- Space complexity: O(1)

---

## Solution 2: Nested Loops (Beginner-friendly — O(n²))

```javascript
function longestUniformSubstring(s) {
    let maxLength = 1;

    for (let i = 0; i < s.length; i++) {
        let count = 1;

        for (let j = i + 1; j < s.length; j++) {
            if (s[j] === s[i]) {
                count++;
            } else {
                break;
            }
        }

        if (count > maxLength) {
            maxLength = count;
        }
    }

    return maxLength;
}
```

**How it works:** The outer loop `i` picks a starting character. The inner loop `j` counts how many characters immediately after it match. `maxLength` is updated whenever a longer run is found.

- Time complexity: O(n²)
- Space complexity: O(1)
- Easier to read and trace step by step, but slower on large inputs (fine for learning, not ideal at `n = 2 * 10^5`).
