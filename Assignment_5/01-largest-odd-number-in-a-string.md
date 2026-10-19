# Question 1 --- Largest Odd Number in a String

**Problem number:** 341\
**Main concepts:** Strings, last digit, traversal, greedy approach

## 1. Problem statement

You are given a string `s` containing digits. Find the largest-valued
odd integer that can be taken as a **substring** of `s`. Return the
answer as a string.

Rules: - The substring must use consecutive characters from `s`. - The
answer must be odd. - The answer must not have leading zeroes. - If no
odd number exists, return `""` (an empty string). - The input can be
large, so we keep the answer as a string instead of converting it into
an integer.

## 2. Understand it in simple English

Imagine the string is a row of digit cards. You may select one
continuous section, but you cannot rearrange the cards or skip a card.

An odd number must end in `1`, `3`, `5`, `7`, or `9`.

To get the largest possible odd substring, we want to keep as many
digits as possible while making sure the final digit is odd. The best
ending position is therefore the **rightmost odd digit** in the string.

## 3. Example walkthrough

Input: `s = "5347"`

The final digit is `7`, which is odd. The entire string is already an
odd number, so the answer is `"5347"`.

Input: `s = "0214638"`

Scan from right to left: - `8` is even --- not a valid ending. - `3` is
odd --- stop here. - Take the substring from the beginning through `3`:
`"021463"`. - Remove leading zeroes: `"21463"`.

Output: `"21463"`.

### Your-turn example

Input: `s = "0032579"`

The last digit is `9`, which is odd, so the entire string is a valid
odd-number substring. Remove the two leading zeroes.

**Correct output: `"32579"`**

The scrambled options `3257325793253` should not be read as one output;
the answer is the string `"32579"`.

## 4. Key observation

The last digit determines whether an integer is odd or even. Every valid
odd substring must end at an odd digit.

If we choose an odd digit farther to the left while an odd digit exists
farther to the right, we lose some digits and make a shorter candidate.
Because the answer is the largest-valued valid substring and the digits
come from a fixed string, the optimal answer ends at the **rightmost odd
digit**. We then remove any leading zeroes.

## 5. Brute-force approach

### Idea

1.  Generate every possible substring.
2.  Check whether its last digit is odd.
3.  Remove leading zeroes from that candidate.
4.  Keep the candidate with the greatest value.

This is useful for understanding the problem, but it is inefficient: a
string of length `n` has `O(n²)` substrings, and building or comparing
many of them can add more work.

### Simple brute-force reference code (Python)

``` python
def largest_odd_number_brute(s):
    best = ""

    for start in range(len(s)):
        for end in range(start + 1, len(s) + 1):
            candidate = s[start:end]

            if candidate[-1] in "13579":
                candidate = candidate.lstrip("0")

                if candidate and (len(candidate) > len(best) or
                                  (len(candidate) == len(best) and candidate > best)):
                    best = candidate

    return best


print(largest_odd_number_brute("5347"))     # 5347
print(largest_odd_number_brute("0214638"))  # 21463
print(largest_odd_number_brute("2468"))     # ""
```

For digit strings without leading zeroes, a longer positive decimal
representation is larger than a shorter one; for equal lengths,
lexicographic digit order matches numeric order. The comparison above
uses that fact.

### Brute-force complexity

-   **Time:** `O(n³)` in this implementation in the worst case, because
    there are `O(n²)` substrings and creating/checking/canonicalizing a
    substring may take `O(n)` time.
-   **Auxiliary space:** `O(n)` for a temporary substring, excluding the
    returned answer.

## 6. Optimal approach --- scan from the right

### Algorithm

1.  Start at the last index of `s`.
2.  Move left while the current digit is even.
3.  If the scan moves before index `0`, there is no odd digit. Return
    `""`.
4.  Otherwise, take the substring from index `0` through the rightmost
    odd digit.
5.  Remove leading zeroes.
6.  Return the result, or `""` if nothing remains.

### Dry run: `"0214638"`

    Index      Digit Odd?   Action
  ------- ---------- ------ ---------------------
        6        `8` No     Move left
        5        `3` Yes    Stop
     0--5   `021463` ---    Take this prefix
      ---    `21463` ---    Remove leading zero

Answer: `"21463"`.

### Python --- optimal

``` python
def largest_odd_number(s: str) -> str:
    i = len(s) - 1

    while i >= 0 and s[i] not in "13579":
        i -= 1

    if i < 0:
        return ""

    return s[:i + 1].lstrip("0")


print(largest_odd_number("5347"))     # 5347
print(largest_odd_number("0214638"))  # 21463
print(largest_odd_number("0032579"))  # 32579
print(largest_odd_number("2468"))     # ""
print(largest_odd_number("0000"))     # ""
```

### C++ --- optimal

``` cpp
#include <iostream>
#include <string>
using namespace std;

string largestOddNumber(string s) {
    int i = (int)s.length() - 1;

    while (i >= 0 && (s[i] - '0') % 2 == 0) {
        i--;
    }

    if (i < 0) {
        return "";
    }

    string answer = s.substr(0, i + 1);

    // Remove leading zeroes without converting to an integer.
    size_t first = answer.find_first_not_of('0');
    if (first == string::npos) {
        return "";
    }

    return answer.substr(first);
}

int main() {
    cout << largestOddNumber("5347") << '\n';     // 5347
    cout << largestOddNumber("0214638") << '\n';  // 21463
    cout << largestOddNumber("0032579") << '\n';  // 32579
    cout << '"' << largestOddNumber("2468") << '"' << '\n'; // ""
    return 0;
}
```

### Java --- optimal

``` java
public class Main {
    static String largestOddNumber(String s) {
        int i = s.length() - 1;

        while (i >= 0 && ((s.charAt(i) - '0') % 2 == 0)) {
            i--;
        }

        if (i < 0) {
            return "";
        }

        String answer = s.substring(0, i + 1);

        int first = 0;
        while (first < answer.length() && answer.charAt(first) == '0') {
            first++;
        }

        return answer.substring(first);
    }

    public static void main(String[] args) {
        System.out.println(largestOddNumber("5347"));     // 5347
        System.out.println(largestOddNumber("0214638"));  // 21463
        System.out.println(largestOddNumber("0032579"));  // 32579
        System.out.println("\"" + largestOddNumber("2468") + "\""); // ""
    }
}
```

## 7. Complexity of the optimal approach

Let `n` be the length of `s`.

-   **Time:** `O(n)`. The scan moves left at most `n` times, and
    producing the returned substring / removing leading zeroes may also
    take up to `O(n)`.
-   **Auxiliary space:** `O(1)` working space excluding the returned
    string. The returned string itself may contain up to `n` characters.

## 8. Common mistakes

-   Converting the entire string to an integer. The input may represent
    a very large integer.
-   Checking the first digit instead of the last digit for oddness.
-   Generating all substrings even though one scan is enough.
-   Returning `"021463"` rather than `"21463"` when leading zeroes are
    disallowed.
-   Returning `"0"` for `"0000"`. There is no odd digit, so the correct
    answer is `""`.
-   Forgetting that the answer is returned as a **string**.

## 9. Practice inputs

  Input        Expected output   Reason
  ------------ ----------------- ----------------------------
  `"1234"`     `"123"`           Rightmost odd digit is `3`
  `"2468"`     `""`              No odd digit
  `"0009"`     `"9"`             Remove leading zeroes
  `"13579"`    `"13579"`         Last digit is odd
  `"120030"`   `"12003"`         Rightmost odd digit is `3`

## 10. Remember

**Find the rightmost odd digit → take the prefix ending there → remove
leading zeroes.**
