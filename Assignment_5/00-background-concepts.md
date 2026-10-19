# Background Concepts --- Strings and Character Mapping

Read this file before attempting the three questions in Assignment
No. 5.

## 1. What is a string?

A **string** is a sequence of characters. Characters can be letters,
digits, spaces, or symbols.

Examples: - `"hello"` is a string containing five characters. - `"5347"`
is a string containing four digit characters. - `"cat"` is a string
containing three letters.

A string may look like a number, but `"5347"` is text until a program
converts it to a numeric type. For very large numbers, keeping the value
as a string avoids numeric range limitations.

### Indexes

Each character has a position called an **index**. In Python, C++, and
Java, string indexing starts at `0`.

For `s = "5347"`:

  Character     `5`   `3`   `4`   `7`
  ----------- ----- ----- ----- -----
  Index           0     1     2     3

The last character is at index `length - 1`.

## 2. What is string traversal?

**Traversal** means visiting characters one by one, usually with a loop.

Example: for `"cat"`, traversal visits `c`, then `a`, then `t`.

Traversal is useful when we need to: - inspect every character; -
compare characters; - find a particular character; - count
occurrences; - stop when a condition becomes true.

## 3. What is a substring?

A **substring** is a continuous piece of a string. Its characters must
appear next to one another in the original order.

For `"5347"`: - `"53"` is a substring. - `"347"` is a substring. -
`"547"` is **not** a substring because the `4` and `7` are not next to
each other in the original string after skipping `3`.

A substring does not rearrange characters and does not skip characters.

## 4. What is a prefix?

A **prefix** is a piece taken from the **beginning** of a string.

For `"flower"`: - `""` (the empty string), `"f"`, `"fl"`, `"flo"`, and
`"flower"` are prefixes. - `"low"` is not a prefix because it does not
start at the beginning.

A **common prefix** is a prefix shared by every string in a collection.

For `["flower", "flow", "flight"]`: - All three start with `"f"`. - All
three start with `"fl"`. - They do not all start with `"flo"`. -
Therefore, the longest common prefix is `"fl"`.

## 5. Odd and even numbers using the last digit

A whole number is: - **odd** if its last digit is `1`, `3`, `5`, `7`, or
`9`; - **even** if its last digit is `0`, `2`, `4`, `6`, or `8`.

You do not need to convert a digit string into an integer to check
oddness. You can inspect its final character.

For example, `"1237"` is odd because its last digit is `7`. `"1238"` is
even because its last digit is `8`.

## 6. What are leading zeros?

Leading zeros are zeroes at the beginning of a number, such as the
zeroes in `"0032579"`.

As a number, `"0032579"` is written as `"32579"` without the leading
zeroes. In a string problem, we must deliberately remove or avoid
leading zeroes when the instructions require it.

Important: a zero in the middle or at the end is not a *leading* zero.
For example, `"10203"` must not become `"123"`.

## 7. What does character mapping mean?

A **mapping** records which character corresponds to another character.

Example: - `e → a` - `g → d`

If `s = "egg"` and `t = "add"`, this mapping changes `"egg"` into
`"add"`: - `e` becomes `a`; - both occurrences of `g` become `d`.

The same character must always map to the same character. In an
isomorphic-string problem, the mapping must also be one-to-one: two
different source characters cannot map to the same target character.

## 8. What is a hash map / dictionary?

A **hash map** (called a dictionary in Python and commonly implemented
using `unordered_map` in C++) stores key-value pairs.

Think of it as a lookup table: - key = a character; - value = the
character it maps to, or a count associated with it.

Example mapping table:

  Key (source character)   Value (target character)
  ------------------------ --------------------------
  `e`                      `a`
  `g`                      `d`

A lookup lets the program quickly check whether it has already seen a
character and what value is associated with it.

Common names: - Python: `dict` - C++: `unordered_map` - Java: `HashMap`

For the character-mapping question, we will use **two maps**: 1. source
character → target character; 2. target character → source character.

The second map prevents two different characters from sharing the same
target character.

## 9. What is a greedy approach?

A **greedy approach** makes a locally useful choice at each step, based
on a property that guarantees the correct answer.

For the largest odd substring problem, we do not need to generate every
possible substring. We can scan from the right until we find the last
odd digit. The answer must end there: ending any earlier would make the
answer shorter, while ending later would make it even.

This is an example of using a property of the problem to avoid
unnecessary work.

## 10. Time complexity and space complexity

**Time complexity** describes how the amount of work grows as the input
grows.

**Space complexity** describes how much additional memory an algorithm
uses.

Common examples: - `O(1)`: constant work or memory, independent of input
length. - `O(n)`: work grows roughly in proportion to input length. -
`O(n²)`: work can grow roughly like the square of input length.

Here, `n` usually means the length of a string or array.

### A small example

If a loop checks each of `n` characters once, its time complexity is
`O(n)`.

If an algorithm stores up to `n` character mappings, its auxiliary space
complexity is `O(n)`.

### Auxiliary space versus returned output

Some solutions return a substring. The returned string itself can
contain up to `n` characters. In these notes, **auxiliary space** means
extra working memory beyond the input and the required output, unless
stated otherwise. Different platforms sometimes count output storage
differently, so always check the convention being used.

## 11. Important string operations

Names vary slightly between languages:

  ------------------------------------------------------------------------------
  Task              Python            C++                 Java
  ----------------- ----------------- ------------------- ----------------------
  Length            `len(s)`          `s.length()`        `s.length()`

  Character at      `s[i]`            `s[i]`              `s.charAt(i)`
  index `i`                                               

  Substring from    `s[start:]`       `s.substr(start)`   `s.substring(start)`
  `start` to end                                          

  Empty string      `""`              `""`                `""`
  ------------------------------------------------------------------------------

In Python, slicing `s[start:end]` includes `start` but excludes `end`.
C++ and Java substring functions also use a start index and a length or
end boundary, but their exact argument rules differ.

## Quick self-check

1.  Is `"ace"` a substring of `"abcde"`? **No**; it skips characters.
2.  Is `"flo"` a prefix of `"flower"`? **Yes**.
3.  Is `"low"` a prefix of `"flower"`? **No**.
4.  Is a number ending in `8` odd? **No**.
5.  If `a → x`, can another source character `b` also map to `x` in an
    isomorphic mapping? **No**.
6.  Which data structure can store character-to-character mappings? **A
    hash map / dictionary**.
