# Question 2 --- Longest Common Prefix

**Problem number:** 79\
**Main concepts:** Strings, indexes, prefix, character comparison

## 1. Problem statement

Given an array (list) of strings, return the **longest prefix shared by
every string**. If no non-empty prefix is shared, return `""`.

## 2. Understand it in simple English

A prefix starts at the beginning of a word.

For example, `"flow"` starts with `"f"`, `"fl"`, and `"flo"`. It does
not start with `"low"`.

You must find the longest beginning section that is present in **every**
word. One word having a prefix is not enough; all words must have it.

## 3. Example walkthrough

Input: `["flowers", "flow", "fly", "flight"]`

Compare characters from the beginning: - Index `0`: all words have
`f`. - Index `1`: all words have `l`. - Index `2`: the words have `o`,
`o`, `y`, and `i`. They do not all match.

Stop at index `2`. The shared prefix is `"fl"`.

Output: `"fl"`.

Example with no common prefix:

Input: `["dog", "cat", "animal", "monkey"]`

The first letters are `d`, `c`, `a`, and `m`, so they differ
immediately.

Output: `""`.

## 4. Brute-force approach

### Idea

1.  Take the first string as a starting candidate.
2.  Check its prefixes from longest to shortest.
3.  For each candidate prefix, check whether every string starts with
    it.
4.  Return the first prefix that all strings share.
5.  If none is shared, return `""`.

### Python --- brute force

``` python
def longest_common_prefix_brute(words):
    if not words:
        return ""

    first = words[0]

    for length in range(len(first), 0, -1):
        candidate = first[:length]

        if all(word.startswith(candidate) for word in words):
            return candidate

    return ""


print(longest_common_prefix_brute(["flowers", "flow", "fly", "flight"]))  # fl
print(longest_common_prefix_brute(["dog", "cat", "animal", "monkey"]))    # ""
```

### Brute-force complexity

Let `n` be the number of strings and `m` the length of the longest
string. There can be up to `m` candidate prefixes, and each candidate
may be checked against up to `n` strings and up to `m` characters.

-   **Time:** `O(n × m²)` in the worst case for this straightforward
    implementation.
-   **Auxiliary space:** up to `O(m)` for a candidate prefix, excluding
    the result.

## 5. Optimal approach --- compare characters vertically

### Key idea

Compare the first character of every word, then the second character of
every word, and so on. Stop as soon as: - one word ends; or - a
character differs.

Everything before that position is the longest common prefix.

### Dry run

Input: `["flowers", "flow", "fly", "flight"]`

    Index Characters across the words   Result
  ------- ----------------------------- -------------------
        0 `f, f, f, f`                  Match
        1 `l, l, l, l`                  Match
        2 `o, o, y, i`                  Mismatch --- stop

The matching characters before the mismatch are `"fl"`.

### Python --- optimal

``` python
def longest_common_prefix(words: list[str]) -> str:
    if not words:
        return ""

    for i in range(len(words[0])):
        current = words[0][i]

        for word in words[1:]:
            if i >= len(word) or word[i] != current:
                return words[0][:i]

    return words[0]


print(longest_common_prefix(["flowers", "flow", "fly", "flight"]))  # fl
print(longest_common_prefix(["dog", "cat", "animal", "monkey"]))    # ""
print(longest_common_prefix(["alone"]))                             # alone
print(longest_common_prefix(["", "abc"]))                           # ""
```

### C++ --- optimal

``` cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

string longestCommonPrefix(vector<string>& words) {
    if (words.empty()) {
        return "";
    }

    for (int i = 0; i < (int)words[0].length(); i++) {
        char current = words[0][i];

        for (int j = 1; j < (int)words.size(); j++) {
            if (i >= (int)words[j].length() || words[j][i] != current) {
                return words[0].substr(0, i);
            }
        }
    }

    return words[0];
}

int main() {
    vector<string> a = {"flowers", "flow", "fly", "flight"};
    vector<string> b = {"dog", "cat", "animal", "monkey"};

    cout << longestCommonPrefix(a) << '\n'; // fl
    cout << '"' << longestCommonPrefix(b) << '"' << '\n'; // ""
    return 0;
}
```

### Java --- optimal

``` java
public class Main {
    static String longestCommonPrefix(String[] words) {
        if (words == null || words.length == 0) {
            return "";
        }

        for (int i = 0; i < words[0].length(); i++) {
            char current = words[0].charAt(i);

            for (int j = 1; j < words.length; j++) {
                if (i >= words[j].length() ||
                    words[j].charAt(i) != current) {
                    return words[0].substring(0, i);
                }
            }
        }

        return words[0];
    }

    public static void main(String[] args) {
        System.out.println(longestCommonPrefix(
            new String[]{"flowers", "flow", "fly", "flight"})); // fl

        System.out.println("\"" + longestCommonPrefix(
            new String[]{"dog", "cat", "animal", "monkey"}) + "\""); // ""
    }
}
```

## 6. Complexity of the optimal approach

Let: - `n` = number of strings; - `m` = length of the shortest string.

-   **Time:** `O(n × m)` in the worst case, because at most `m`
    character positions are compared across `n` strings.
-   **Auxiliary space:** `O(1)` working space, excluding the returned
    prefix. The returned string may use up to `O(m)` space.

## 7. Alternative approach --- shrink a candidate prefix

Another easy method is to start with the first word as the candidate
prefix. While any other word does not start with the candidate, remove
the last character of the candidate. Stop when all words start with it
or the candidate becomes empty.

This approach is also commonly accepted and is often easy for beginners
to implement.

## 8. Common mistakes

-   Checking only the first two strings instead of every string.
-   Comparing characters without checking whether a shorter word has
    ended.
-   Returning a substring that is common to some, but not all, strings.
-   Confusing a prefix with a substring: `"low"` is a substring of
    `"flower"` but not a prefix.
-   Forgetting empty input or an empty string in the array.

## 9. Practice inputs

  Input                                     Expected output
  ----------------------------------------- -----------------
  `["interview", "internet", "internal"]`   `"inte"`
  `["car", "cat", "camera"]`                `"ca"`
  `["a", "a", "a"]`                         `"a"`
  `["", "abc"]`                             `""`
  `["same", "same"]`                        `"same"`

## 10. Remember

**Compare from the beginning. Stop at the first mismatch or when any
word ends.**
