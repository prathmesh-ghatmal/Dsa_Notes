# Question 3 --- Isomorphic Strings

**Problem number:** 252\
**Main concepts:** Character mapping, one-to-one relationships, hash
maps

## 1. Problem statement

Given two strings `s` and `t`, determine whether they are
**isomorphic**.

They are isomorphic if we can replace every character in `s` with a
character to obtain `t`, while: - preserving the order of characters; -
always replacing the same source character with the same target
character; - never mapping two different source characters to the same
target character.

A character may map to itself. The strings must have equal lengths.

## 2. Understand it in simple English

Think of each character in `s` as having a permanent partner in `t`.

If `a` maps to `x`, every `a` must map to `x`. It cannot map to `x` in
one position and `y` in another.

Also, if `a` maps to `x`, another character such as `b` cannot map to
`x`. Otherwise, the mapping is not one-to-one.

The letters do not need to be identical. Their **pattern of repetition**
must match.

## 3. Example walkthroughs

### Example A: `s = "egg"`, `t = "add"`

    Index `s` character   `t` character   Mapping
  ------- --------------- --------------- --------------------------------
        0 `e`             `a`             `e → a`
        1 `g`             `d`             `g → d`
        2 `g`             `d`             Valid: `g` already maps to `d`

Output: `true`.

### Example B: `s = "apple"`, `t = "bbnbm"`

    Index `s` character   `t` character   What happens
  ------- --------------- --------------- -----------------------------------------
        0 `a`             `b`             `a → b`
        1 `p`             `b`             Invalid: `b` is already assigned to `a`

Two different source characters (`a` and `p`) would map to the same
target character (`b`). Output: `false`.

### Your-turn example: `s = "paper"`, `t = "title"`

Mappings: - `p → t` - `a → i` - `p → t` again --- consistent - `e → l` -
`r → e`

No mapping conflict occurs, and no two source characters share a target
character.

**Output: `true`.**

## 4. Brute-force idea

A straightforward solution can compare the positions where each
character appears in both strings. For every pair of positions `i` and
`j`, the characters in `s` are equal exactly when the corresponding
characters in `t` are equal.

In other words, the repetition pattern must match: - if `s[i] == s[j]`,
then `t[i]` must equal `t[j]`; - if `s[i] != s[j]`, then `t[i]` must
differ from `t[j]`.

### Python --- brute force

``` python
def is_isomorphic_brute(s: str, t: str) -> bool:
    if len(s) != len(t):
        return False

    n = len(s)

    for i in range(n):
        for j in range(i + 1, n):
            if (s[i] == s[j]) != (t[i] == t[j]):
                return False

    return True


print(is_isomorphic_brute("egg", "add"))       # True
print(is_isomorphic_brute("apple", "bbnbm"))   # False
print(is_isomorphic_brute("paper", "title"))   # True
```

### Brute-force complexity

-   **Time:** `O(n²)`, because we compare pairs of positions.
-   **Auxiliary space:** `O(1)`, excluding the input and result.

## 5. Optimal approach --- two-way mapping

### Why do we need two maps?

One map checks that a source character always maps to the same target
character:

`source → target`

The second map checks that a target character is not assigned to two
different source characters:

`target → source`

Using only one map is not enough. For example, it might allow both
`a → x` and `b → x`, which breaks the one-to-one rule.

### Algorithm

1.  If the strings have different lengths, return `false`.
2.  Create two empty maps.
3.  Visit the strings at the same index `i`.
4.  If `s[i]` already has a mapping, it must equal `t[i]`; otherwise
    return `false`.
5.  If `t[i]` already has a reverse mapping, it must equal `s[i]`;
    otherwise return `false`.
6.  If neither mapping exists, record both mappings.
7.  If every position passes, return `true`.

### Dry run: `"paper"` and `"title"`

  -------------------------------------------------------------------------
              Index Pair          Forward map   Reverse map   Result
  ----------------- ------------- ------------- ------------- -------------
                  0 `p, t`        `p → t`       `t → p`       Add mappings

                  1 `a, i`        `a → i`       `i → a`       Add mappings

                  2 `p, t`        `p → t`       `t → p`       Consistent
                                  already       already       
                                  exists        exists        

                  3 `e, l`        `e → l`       `l → e`       Add mappings

                  4 `r, e`        `r → e`       `e → r`       Add mappings
  -------------------------------------------------------------------------

No conflict occurs, so return `true`.

### Python --- optimal

``` python
def is_isomorphic(s: str, t: str) -> bool:
    if len(s) != len(t):
        return False

    s_to_t = {}
    t_to_s = {}

    for a, b in zip(s, t):
        if a in s_to_t and s_to_t[a] != b:
            return False

        if b in t_to_s and t_to_s[b] != a:
            return False

        s_to_t[a] = b
        t_to_s[b] = a

    return True


print(is_isomorphic("egg", "add"))       # True
print(is_isomorphic("apple", "bbnbm"))   # False
print(is_isomorphic("paper", "title"))   # True
print(is_isomorphic("foo", "bar"))       # False
```

### C++ --- optimal

``` cpp
#include <iostream>
#include <string>
#include <unordered_map>
using namespace std;

bool isIsomorphic(string s, string t) {
    if (s.length() != t.length()) {
        return false;
    }

    unordered_map<char, char> sToT;
    unordered_map<char, char> tToS;

    for (int i = 0; i < (int)s.length(); i++) {
        char a = s[i];
        char b = t[i];

        if (sToT.count(a) && sToT[a] != b) {
            return false;
        }

        if (tToS.count(b) && tToS[b] != a) {
            return false;
        }

        sToT[a] = b;
        tToS[b] = a;
    }

    return true;
}

int main() {
    cout << boolalpha;
    cout << isIsomorphic("egg", "add") << '\n';       // true
    cout << isIsomorphic("apple", "bbnbm") << '\n';   // false
    cout << isIsomorphic("paper", "title") << '\n';   // true
    return 0;
}
```

### Java --- optimal

``` java
import java.util.HashMap;
import java.util.Map;

public class Main {
    static boolean isIsomorphic(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        Map<Character, Character> sToT = new HashMap<>();
        Map<Character, Character> tToS = new HashMap<>();

        for (int i = 0; i < s.length(); i++) {
            char a = s.charAt(i);
            char b = t.charAt(i);

            if (sToT.containsKey(a) && sToT.get(a) != b) {
                return false;
            }

            if (tToS.containsKey(b) && tToS.get(b) != a) {
                return false;
            }

            sToT.put(a, b);
            tToS.put(b, a);
        }

        return true;
    }

    public static void main(String[] args) {
        System.out.println(isIsomorphic("egg", "add"));       // true
        System.out.println(isIsomorphic("apple", "bbnbm"));   // false
        System.out.println(isIsomorphic("paper", "title"));   // true
    }
}
```

## 6. Complexity of the optimal approach

Let `n` be the length of each string.

-   **Time:** `O(n)` on average, because each position is processed once
    and hash-map lookups/updates are average `O(1)`.
-   **Auxiliary space:** `O(k)`, where `k` is the number of distinct
    characters stored in the maps. For a fixed character set such as
    lowercase English letters, `k` is at most 26, so this is `O(1)`
    under that constraint. For a general character set, it can be
    `O(n)`.

## 7. Common mistakes

-   Using only a source-to-target map and forgetting the reverse
    mapping.
-   Allowing the same source character to map to different target
    characters.
-   Allowing different source characters to map to the same target
    character.
-   Ignoring the requirement that the strings have equal lengths.
-   Thinking the characters themselves must be equal. `"egg"` and
    `"add"` are isomorphic even though their letters differ.
-   Forgetting that repeated characters must preserve the same pattern.

## 8. Practice inputs

  `s`         `t`         Expected output
  ----------- ----------- -----------------
  `"badc"`    `"baba"`    `false`
  `"foo"`     `"app"`     `true`
  `"ab"`      `"aa"`      `false`
  `"abc"`     `"def"`     `true`
  `"paper"`   `"title"`   `true`

## 9. Remember

**Use two maps: source → target and target → source. Both directions
must remain consistent.**
