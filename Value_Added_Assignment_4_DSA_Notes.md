# Value Added Assignment No. 4 --- DSA Notes

## Arrays, Frequency Counting and Hashing

These notes are designed for beginners. They explain the background
concepts from zero, use real-world examples, and then solve three
questions through brute force, better approaches where useful, and
optimal approaches.

**Questions covered** 1. Highest Occurring Element in an Array 2. Second
Highest Occurring Element 3. Sum of Highest and Lowest Frequency

**Languages:** Python, C++ and Java\
**Prerequisites:** Basic variables, arrays/lists, loops and conditional
statements.

------------------------------------------------------------------------

# Part 1: Background Concepts

## 1. What Is an Array?

An array is a collection of elements stored together in an ordered
sequence.

Imagine a teacher records six students' marks:

``` text
marks = [45, 60, 45, 75, 60, 45]
```

Instead of storing every mark in a separate variable, the teacher stores
them in one collection.

-   The collection contains six elements.
-   The first element is `45`, the second is `60`, and so on.
-   The same value can appear more than once.
-   The order of elements is preserved.

Python commonly uses lists for this purpose. C++ commonly uses `vector`,
while Java can use arrays or `ArrayList`.

## 2. What Is Frequency?

**Frequency is the number of times an element appears in a collection.**

For the array:

``` text
marks = [45, 60, 45, 75, 60, 45]
```

The frequency table is:

    Mark   Frequency
  ------ -----------
      45           3
      60           2
      75           1

We list each distinct mark once and record how many times it appears.

### Real-world example: Counting votes

Suppose students vote for their favourite sport:

``` text
Cricket, Football, Cricket, Badminton, Cricket, Football
```

  Sport         Votes
  ----------- -------
  Cricket           3
  Football          2
  Badminton         1

Cricket has the highest frequency because it received the most votes. In
our problems, we do the same thing with numbers.

## 3. What Is a Loop?

A loop repeats instructions for each element in a collection.

Python example:

``` python
nums = [10, 20, 30]

for num in nums:
    print(num)
```

Output:

``` text
10
20
30
```

To count frequencies, we visit each number and increase its count. If we
see `5` for the first time, its count becomes `1`. If we see it again,
its count becomes `2`.

## 4. What Is a Map or Dictionary?

A map stores information as **key-value pairs**.

Think of a classroom register:

-   **Key:** A student's name or a number.
-   **Value:** Information associated with that key.

For frequency counting:

-   **Key:** The number in the array.
-   **Value:** How many times that number appears.

For this input:

``` text
nums = [2, 4, 2, 3, 4, 2]
```

The frequency map is:

    Key (number)   Value (frequency)
  -------------- -------------------
               2                   3
               3                   1
               4                   2

The map stores each distinct number once, along with its count.

### Building a frequency map step by step

Input: `[2, 4, 2, 3, 4, 2]`

1.  See `2` for the first time: `{2: 1}`
2.  See `4` for the first time: `{2: 1, 4: 1}`
3.  See `2` again: `{2: 2, 4: 1}`
4.  See `3` for the first time: `{2: 2, 4: 1, 3: 1}`
5.  See `4` again: `{2: 2, 4: 2, 3: 1}`
6.  See `2` again: `{2: 3, 4: 2, 3: 1}`

Final map: `{2: 3, 4: 2, 3: 1}`

### Frequency map syntax in three languages

**Python --- dictionary**

``` python
freq = {}
freq[num] = freq.get(num, 0) + 1
```

`get(num, 0)` returns the current count, or `0` if the number is not yet
in the dictionary.

**C++ --- unordered_map**

``` cpp
unordered_map<int, int> freq;
freq[num]++;
```

An absent integer starts with a count of zero and is then incremented.

**Java --- HashMap**

``` java
HashMap<Integer, Integer> freq = new HashMap<>();
freq.put(num, freq.getOrDefault(num, 0) + 1);
```

`getOrDefault` returns the current count, or zero if the key does not
exist.

## 5. What Is Hashing?

Hashing is a technique used to store and find information efficiently. A
hash map uses a hash function internally to help locate where a key
belongs.

For these questions, you do not need to implement the hash function
yourself. Use the built-in data structures:

-   Python: `dict`
-   C++: `unordered_map`
-   Java: `HashMap`

Insertions and lookups usually take **average O(1) time**. This is why
frequency maps are useful for large arrays.

## 6. What Is Time Complexity?

Time complexity describes how the amount of work grows as the input size
grows.

  -----------------------------------------------------------------------
  Complexity              Simple meaning          Example
  ----------------------- ----------------------- -----------------------
  `O(1)`                  Constant amount of work Access an array element
                                                  by index

  `O(n)`                  Work grows roughly with Scan an array once
                          the number of elements  

  `O(n²)`                 Work can grow roughly   Compare every element
                          with the square of the  with every other
                          input size              element

  `O(n log n)`            Common complexity for   Sort an array
                          efficient comparison    
                          sorting                 
  -----------------------------------------------------------------------

For an array of 100 elements, one scan visits about 100 elements. A
nested scan may perform about 10,000 comparisons.

## 7. What Is Space Complexity?

Space complexity describes how much extra memory an algorithm uses.

If an array has `n` elements and `k` distinct values, a frequency map
stores `k` entries.

-   Frequency map space: `O(k)`
-   Since `k <= n`, worst-case extra space is `O(n)`

In these notes, **auxiliary space** means extra memory used by the
algorithm, excluding the input array.

------------------------------------------------------------------------

# Question 1: Highest Occurring Element in an Array

## 1. Understand the Problem

Given an array of integers, return the element that appears the most
times.

If multiple elements share the highest frequency, return the **smallest
element** among them.

### Example 1

Input:

``` text
nums = [1, 2, 2, 3, 3, 3]
```

Output:

``` text
3
```

Explanation: `3` appears three times, more than any other number.

### Example 2

Input:

``` text
nums = [4, 4, 5, 5, 6]
```

Output:

``` text
4
```

Explanation:

-   `4` appears twice.
-   `5` appears twice.
-   `6` appears once.

Both `4` and `5` have the highest frequency of `2`. Choose `4` because
it is smaller.

### Example 3: Tie-breaking

Input:

``` text
nums = [2, 4, 3, 2, 5, 4]
```

    Element   Frequency
  --------- -----------
          2           2
          3           1
          4           2
          5           1

The highest frequency is `2`. Elements `2` and `4` share that frequency,
so the answer is `2`.

## 2. How to Think About the Problem

Ask two questions:

1.  How many times does each number appear?
2.  Which number has the highest frequency? If there is a tie, which
    tied number is smallest?

The first part is frequency counting. The second part is selecting the
answer.

## 3. Brute Force Approach

The straightforward approach is to take each element and count how many
times it appears by scanning the entire array.

### Algorithm

1.  Select an element.
2.  Scan the complete array and count its occurrences.
3.  Repeat for every element.
4.  Track the highest frequency and its element.
5.  If another element has the same frequency, choose the smaller
    element.

This version deliberately counts duplicates repeatedly to demonstrate
the brute force approach.

### Python --- Brute Force

``` python
def highest_occurring_element(nums):
    max_freq = 0
    answer = float('inf')

    for num in nums:
        count = 0

        for other in nums:
            if other == num:
                count += 1

        if count > max_freq:
            max_freq = count
            answer = num
        elif count == max_freq and num < answer:
            answer = num

    return answer


nums = [2, 4, 3, 2, 5, 4]
print(highest_occurring_element(nums))  # 2
```

### C++ --- Brute Force

``` cpp
#include <bits/stdc++.h>
using namespace std;

int highestOccurringElement(vector<int>& nums) {
    int maxFreq = 0;
    int answer = INT_MAX;

    for (int num : nums) {
        int count = 0;

        for (int other : nums) {
            if (other == num) {
                count++;
            }
        }

        if (count > maxFreq) {
            maxFreq = count;
            answer = num;
        } else if (count == maxFreq && num < answer) {
            answer = num;
        }
    }

    return answer;
}

int main() {
    vector<int> nums = {2, 4, 3, 2, 5, 4};
    cout << highestOccurringElement(nums) << endl;
    return 0;
}
```

### Java --- Brute Force

``` java
public class Main {
    static int highestOccurringElement(int[] nums) {
        int maxFreq = 0;
        int answer = Integer.MAX_VALUE;

        for (int num : nums) {
            int count = 0;

            for (int other : nums) {
                if (other == num) {
                    count++;
                }
            }

            if (count > maxFreq) {
                maxFreq = count;
                answer = num;
            } else if (count == maxFreq && num < answer) {
                answer = num;
            }
        }

        return answer;
    }

    public static void main(String[] args) {
        int[] nums = {2, 4, 3, 2, 5, 4};
        System.out.println(highestOccurringElement(nums));
    }
}
```

### Brute Force Complexity

-   **Time:** `O(n²)` --- for every element, we scan the entire array.
-   **Auxiliary space:** `O(1)` --- only a few variables are used.

For `n = 100,000`, a quadratic algorithm can perform roughly
`10,000,000,000` comparisons. We need a faster approach for large
inputs.

## 4. Better Approach --- Sorting

Sorting places equal values next to one another.

``` text
Original: [2, 4, 3, 2, 5, 4]
Sorted:   [2, 2, 3, 4, 4, 5]
```

Now we can count consecutive groups. Track each group's frequency and
keep the most frequent value. If two groups have the same frequency,
choose the smaller value.

### Complexity

-   **Time:** `O(n log n)` for sorting plus `O(n)` for scanning.
-   **Auxiliary space:** Depends on the sorting implementation and
    language.

Sorting improves on brute force, but hashing provides an average
linear-time solution.

## 5. Optimal Approach --- Frequency Map

### Algorithm

1.  Create an empty frequency map.
2.  Traverse the array and increment each number's count.
3.  Set `maxFreq = 0` and `answer` to the largest possible value.
4.  Visit every entry in the frequency map.
5.  If its frequency is greater than `maxFreq`, update `maxFreq` and
    `answer`.
6.  If its frequency equals `maxFreq`, keep the smaller element.
7.  Return `answer`.

### Python --- Optimal

``` python
def highest_occurring_element(nums):
    freq = {}

    for num in nums:
        freq[num] = freq.get(num, 0) + 1

    max_freq = 0
    answer = float('inf')

    for num, count in freq.items():
        if count > max_freq:
            max_freq = count
            answer = num
        elif count == max_freq and num < answer:
            answer = num

    return answer


print(highest_occurring_element([2, 4, 3, 2, 5, 4]))  # 2
```

### C++ --- Optimal

``` cpp
#include <bits/stdc++.h>
using namespace std;

int highestOccurringElement(vector<int>& nums) {
    unordered_map<int, int> freq;

    for (int num : nums) {
        freq[num]++;
    }

    int maxFreq = 0;
    int answer = INT_MAX;

    for (auto p : freq) {
        int num = p.first;
        int count = p.second;

        if (count > maxFreq) {
            maxFreq = count;
            answer = num;
        } else if (count == maxFreq && num < answer) {
            answer = num;
        }
    }

    return answer;
}

int main() {
    vector<int> nums = {2, 4, 3, 2, 5, 4};
    cout << highestOccurringElement(nums) << endl;
    return 0;
}
```

### Java --- Optimal

``` java
import java.util.HashMap;
import java.util.Map;

public class Main {
    static int highestOccurringElement(int[] nums) {
        HashMap<Integer, Integer> freq = new HashMap<>();

        for (int num : nums) {
            freq.put(num, freq.getOrDefault(num, 0) + 1);
        }

        int maxFreq = 0;
        int answer = Integer.MAX_VALUE;

        for (Map.Entry<Integer, Integer> entry : freq.entrySet()) {
            int num = entry.getKey();
            int count = entry.getValue();

            if (count > maxFreq) {
                maxFreq = count;
                answer = num;
            } else if (count == maxFreq && num < answer) {
                answer = num;
            }
        }

        return answer;
    }

    public static void main(String[] args) {
        int[] nums = {2, 4, 3, 2, 5, 4};
        System.out.println(highestOccurringElement(nums));
    }
}
```

### Optimal Complexity

-   **Time:** `O(n)` average --- one pass builds the map, and another
    examines its entries.
-   **Auxiliary space:** `O(k)`, where `k` is the number of distinct
    elements; worst case `O(n)`.

### Common Mistakes

-   Forgetting to choose the smaller element when frequencies tie.
-   Confusing the element with its frequency. If `5` appears three
    times, its frequency is `3`, not `5`.
-   Updating the answer only for a greater frequency and ignoring
    equal-frequency ties.

------------------------------------------------------------------------

# Question 2: Second Highest Occurring Element

## 1. Understand the Problem

Given an array, return the element with the **second-highest distinct
frequency**.

If multiple elements have that frequency, choose the smallest element.
If no second distinct frequency exists, return `-1`.

### Example 1

Input:

``` text
arr = [1, 2, 2, 3, 3, 3]
```

Output:

``` text
2
```

    Element   Frequency
  --------- -----------
          1           1
          2           2
          3           3

The highest frequency is `3`, and the second-highest distinct frequency
is `2`. Therefore, the answer is `2`.

### Example 2

Input:

``` text
arr = [4, 4, 5, 5, 6, 7]
```

Output:

``` text
6
```

Elements `4` and `5` appear twice. Elements `6` and `7` appear once. The
second-highest distinct frequency is `1`; choose the smaller element,
`6`.

### Example 3

Input:

``` text
arr = [10, 9, 7, 7]
```

Frequencies:

    Element   Frequency
  --------- -----------
          7           2
          9           1
         10           1

The highest frequency is `2`, and the second-highest distinct frequency
is `1`. Both `9` and `10` have that frequency, so the answer is `9`.

### Important Distinction

The question asks for the second-highest **frequency**, not the
second-largest number.

If the frequency values are `3, 3, 1`, the second-highest distinct
frequency is `1`, not `3`.

If all distinct elements have the same frequency, a second distinct
frequency does not exist, so return `-1`.

## 2. Brute Force Approach

Count every distinct element by scanning the array. Store the distinct
elements and their frequencies in two lists. Then find the highest
frequency and the greatest frequency strictly below it.

### Algorithm

1.  Visit each element.
2.  Check whether it has already been processed.
3.  If not, scan the array and count its occurrences.
4.  Store the element and its frequency.
5.  Find the maximum frequency.
6.  Find the largest frequency strictly below the maximum.
7.  Return the smallest element with that frequency, or `-1` if none
    exists.

### Python --- Brute Force

``` python
def second_highest_occurring_element(arr):
    elements = []
    frequencies = []

    for num in arr:
        if num not in elements:
            count = 0

            for other in arr:
                if other == num:
                    count += 1

            elements.append(num)
            frequencies.append(count)

    max_freq = max(frequencies)
    second_freq = 0

    for count in frequencies:
        if count < max_freq and count > second_freq:
            second_freq = count

    if second_freq == 0:
        return -1

    answer = float('inf')

    for i in range(len(elements)):
        if frequencies[i] == second_freq:
            answer = min(answer, elements[i])

    return answer


print(second_highest_occurring_element([10, 9, 7, 7]))  # 9
```

### C++ --- Brute Force

``` cpp
#include <bits/stdc++.h>
using namespace std;

int secondHighestOccurringElement(vector<int>& arr) {
    vector<int> elements, frequencies;

    for (int num : arr) {
        bool found = false;

        for (int x : elements) {
            if (x == num) {
                found = true;
                break;
            }
        }

        if (!found) {
            int count = 0;

            for (int other : arr) {
                if (other == num) count++;
            }

            elements.push_back(num);
            frequencies.push_back(count);
        }
    }

    int maxFreq = 0;
    for (int f : frequencies) {
        maxFreq = max(maxFreq, f);
    }

    int secondFreq = 0;
    for (int f : frequencies) {
        if (f < maxFreq) {
            secondFreq = max(secondFreq, f);
        }
    }

    if (secondFreq == 0) return -1;

    int answer = INT_MAX;

    for (int i = 0; i < (int)elements.size(); i++) {
        if (frequencies[i] == secondFreq) {
            answer = min(answer, elements[i]);
        }
    }

    return answer;
}

int main() {
    vector<int> arr = {10, 9, 7, 7};
    cout << secondHighestOccurringElement(arr) << endl;
    return 0;
}
```

### Java --- Brute Force

``` java
import java.util.ArrayList;
import java.util.List;

public class Main {
    static int secondHighestOccurringElement(int[] arr) {
        List<Integer> elements = new ArrayList<>();
        List<Integer> frequencies = new ArrayList<>();

        for (int num : arr) {
            if (!elements.contains(num)) {
                int count = 0;

                for (int other : arr) {
                    if (other == num) count++;
                }

                elements.add(num);
                frequencies.add(count);
            }
        }

        int maxFreq = 0;
        for (int f : frequencies) {
            maxFreq = Math.max(maxFreq, f);
        }

        int secondFreq = 0;
        for (int f : frequencies) {
            if (f < maxFreq) {
                secondFreq = Math.max(secondFreq, f);
            }
        }

        if (secondFreq == 0) return -1;

        int answer = Integer.MAX_VALUE;

        for (int i = 0; i < elements.size(); i++) {
            if (frequencies.get(i) == secondFreq) {
                answer = Math.min(answer, elements.get(i));
            }
        }

        return answer;
    }

    public static void main(String[] args) {
        int[] arr = {10, 9, 7, 7};
        System.out.println(secondHighestOccurringElement(arr));
    }
}
```

### Brute Force Complexity

-   **Time:** `O(n²)` in the worst case because counting and checking
    previously seen elements can both take quadratic time.
-   **Auxiliary space:** `O(k)` for the distinct-element and frequency
    lists.

## 3. Better Approach --- Sorting Frequency Records

After counting the distinct elements and their frequencies, sort the
records by frequency. Find the greatest frequency strictly below the
maximum, then choose the smallest element with that frequency.

-   **Time:** `O(n log n + k log k)` if we sort the array to count
    groups and sort the frequency records; a map-based count followed by
    sorting the `k` records costs `O(n + k log k)` on average.
-   **Auxiliary space:** Depends on the representation and sorting
    implementation.

This can be useful when sorted frequency records are also needed, but a
map plus direct scans is simpler and has average linear time.

## 4. Optimal Approach --- Frequency Map and Two Frequency Values

Track two values:

-   `maxFreq`: The largest frequency found so far.
-   `secondMaxFreq`: The largest frequency strictly smaller than
    `maxFreq`.

Once those values are found, scan the map again and choose the smallest
element whose frequency equals `secondMaxFreq`.

### Step-by-Step Example

Input: `[1, 2, 2, 3, 3, 3]`

1.  Count frequencies: `{1: 1, 2: 2, 3: 3}`.
2.  Highest frequency: `3`.
3.  Greatest frequency below `3`: `2`.
4.  Smallest element with frequency `2`: `2`.
5.  Return `2`.

### Python --- Optimal

``` python
def second_highest_occurring_element(arr):
    freq = {}

    for num in arr:
        freq[num] = freq.get(num, 0) + 1

    max_freq = 0
    second_max_freq = 0

    for count in freq.values():
        if count > max_freq:
            second_max_freq = max_freq
            max_freq = count
        elif count < max_freq and count > second_max_freq:
            second_max_freq = count

    if second_max_freq == 0:
        return -1

    answer = float('inf')

    for num, count in freq.items():
        if count == second_max_freq:
            answer = min(answer, num)

    return answer


print(second_highest_occurring_element([10, 9, 7, 7]))  # 9
```

### C++ --- Optimal

``` cpp
#include <bits/stdc++.h>
using namespace std;

int secondHighestOccurringElement(vector<int>& arr) {
    unordered_map<int, int> freq;

    for (int num : arr) {
        freq[num]++;
    }

    int maxFreq = 0;
    int secondMaxFreq = 0;

    for (auto p : freq) {
        int count = p.second;

        if (count > maxFreq) {
            secondMaxFreq = maxFreq;
            maxFreq = count;
        } else if (count < maxFreq && count > secondMaxFreq) {
            secondMaxFreq = count;
        }
    }

    if (secondMaxFreq == 0) return -1;

    int answer = INT_MAX;

    for (auto p : freq) {
        if (p.second == secondMaxFreq) {
            answer = min(answer, p.first);
        }
    }

    return answer;
}

int main() {
    vector<int> arr = {10, 9, 7, 7};
    cout << secondHighestOccurringElement(arr) << endl;
    return 0;
}
```

### Java --- Optimal

``` java
import java.util.HashMap;
import java.util.Map;

public class Main {
    static int secondHighestOccurringElement(int[] arr) {
        HashMap<Integer, Integer> freq = new HashMap<>();

        for (int num : arr) {
            freq.put(num, freq.getOrDefault(num, 0) + 1);
        }

        int maxFreq = 0;
        int secondMaxFreq = 0;

        for (int count : freq.values()) {
            if (count > maxFreq) {
                secondMaxFreq = maxFreq;
                maxFreq = count;
            } else if (count < maxFreq && count > secondMaxFreq) {
                secondMaxFreq = count;
            }
        }

        if (secondMaxFreq == 0) return -1;

        int answer = Integer.MAX_VALUE;

        for (Map.Entry<Integer, Integer> entry : freq.entrySet()) {
            if (entry.getValue() == secondMaxFreq) {
                answer = Math.min(answer, entry.getKey());
            }
        }

        return answer;
    }

    public static void main(String[] args) {
        int[] arr = {10, 9, 7, 7};
        System.out.println(secondHighestOccurringElement(arr));
    }
}
```

### Optimal Complexity

-   **Time:** `O(n)` average.
-   **Auxiliary space:** `O(k)`, worst case `O(n)`.

### Common Mistakes

1.  Choosing the second-largest number instead of the element with the
    second-highest frequency.
2.  Treating a tied maximum frequency as the second-highest distinct
    frequency.
3.  Forgetting to return `-1` when no second distinct frequency exists.

------------------------------------------------------------------------

# Question 3: Sum of Highest and Lowest Frequency

## 1. Understand the Problem

Given an array, find the sum of:

1.  The highest frequency of any element.
2.  The lowest frequency of any element.

We add **frequency values**, not the elements themselves.

### Example 1

Input:

``` text
arr = [1, 2, 2, 3, 3, 3]
```

    Element   Frequency
  --------- -----------
          1           1
          2           2
          3           3

-   Highest frequency = `3`
-   Lowest frequency = `1`
-   Answer = `3 + 1 = 4`

### Real-world example

A teacher checks how many assignments students submitted:

-   Rahul: 3 assignments
-   Priya: 2 assignments
-   Amit: 1 assignment

The highest count is `3`; the lowest is `1`. Their sum is `4`.

We do not need to identify which student has each count. We only need
the two frequency values.

### Example 2

Input:

``` text
arr = [10, 9, 7, 7, 8, 8, 8]
```

    Element   Frequency
  --------- -----------
          7           2
          8           3
          9           1
         10           1

Highest frequency = `3`; lowest frequency = `1`. Output = `4`.

## 2. Brute Force Approach

Count each element by scanning the complete array, then update the
highest and lowest frequency.

### Algorithm

1.  Initialize `maxFreq = 0` and `minFreq` to a very large value.
2.  For every element, scan the entire array and count its occurrences.
3.  Update the maximum and minimum frequency.
4.  Return their sum.

### Python --- Brute Force

``` python
def sum_highest_lowest_frequency(arr):
    max_freq = 0
    min_freq = float('inf')

    for num in arr:
        count = 0

        for other in arr:
            if other == num:
                count += 1

        max_freq = max(max_freq, count)
        min_freq = min(min_freq, count)

    return max_freq + min_freq


print(sum_highest_lowest_frequency([10, 9, 7, 7, 8, 8, 8]))  # 4
```

### C++ --- Brute Force

``` cpp
#include <bits/stdc++.h>
using namespace std;

int sumHighestLowestFrequency(vector<int>& arr) {
    int maxFreq = 0;
    int minFreq = INT_MAX;

    for (int num : arr) {
        int count = 0;

        for (int other : arr) {
            if (other == num) {
                count++;
            }
        }

        maxFreq = max(maxFreq, count);
        minFreq = min(minFreq, count);
    }

    return maxFreq + minFreq;
}

int main() {
    vector<int> arr = {10, 9, 7, 7, 8, 8, 8};
    cout << sumHighestLowestFrequency(arr) << endl;
    return 0;
}
```

### Java --- Brute Force

``` java
public class Main {
    static int sumHighestLowestFrequency(int[] arr) {
        int maxFreq = 0;
        int minFreq = Integer.MAX_VALUE;

        for (int num : arr) {
            int count = 0;

            for (int other : arr) {
                if (other == num) {
                    count++;
                }
            }

            maxFreq = Math.max(maxFreq, count);
            minFreq = Math.min(minFreq, count);
        }

        return maxFreq + minFreq;
    }

    public static void main(String[] args) {
        int[] arr = {10, 9, 7, 7, 8, 8, 8};
        System.out.println(sumHighestLowestFrequency(arr));
    }
}
```

### Brute Force Complexity

-   **Time:** `O(n²)`.
-   **Auxiliary space:** `O(1)`.

## 3. Better Approach --- Count Distinct Elements Only

We can avoid recounting duplicate values by maintaining a list of
distinct elements. Count each distinct element once, and update the
maximum and minimum frequencies.

However, checking whether a value already exists in a simple list can
itself take linear time. The worst-case time remains `O(n²)`. A
frequency map is both simpler and faster for this task.

## 4. Optimal Approach --- Frequency Map

### Algorithm

1.  Create a frequency map.
2.  Count every element in one pass.
3.  Initialize `maxFreq = 0` and `minFreq` to a very large value.
4.  Traverse the frequency values.
5.  Update the maximum and minimum.
6.  Return `maxFreq + minFreq`.

### Python --- Optimal

``` python
def sum_highest_lowest_frequency(arr):
    freq = {}

    for num in arr:
        freq[num] = freq.get(num, 0) + 1

    max_freq = 0
    min_freq = float('inf')

    for count in freq.values():
        max_freq = max(max_freq, count)
        min_freq = min(min_freq, count)

    return max_freq + min_freq


print(sum_highest_lowest_frequency([10, 9, 7, 7, 8, 8, 8]))  # 4
```

### C++ --- Optimal

``` cpp
#include <bits/stdc++.h>
using namespace std;

int sumHighestLowestFrequency(vector<int>& arr) {
    unordered_map<int, int> freq;

    for (int num : arr) {
        freq[num]++;
    }

    int maxFreq = 0;
    int minFreq = INT_MAX;

    for (auto p : freq) {
        maxFreq = max(maxFreq, p.second);
        minFreq = min(minFreq, p.second);
    }

    return maxFreq + minFreq;
}

int main() {
    vector<int> arr = {10, 9, 7, 7, 8, 8, 8};
    cout << sumHighestLowestFrequency(arr) << endl;
    return 0;
}
```

### Java --- Optimal

``` java
import java.util.HashMap;
import java.util.Map;

public class Main {
    static int sumHighestLowestFrequency(int[] arr) {
        HashMap<Integer, Integer> freq = new HashMap<>();

        for (int num : arr) {
            freq.put(num, freq.getOrDefault(num, 0) + 1);
        }

        int maxFreq = 0;
        int minFreq = Integer.MAX_VALUE;

        for (Map.Entry<Integer, Integer> entry : freq.entrySet()) {
            int count = entry.getValue();

            maxFreq = Math.max(maxFreq, count);
            minFreq = Math.min(minFreq, count);
        }

        return maxFreq + minFreq;
    }

    public static void main(String[] args) {
        int[] arr = {10, 9, 7, 7, 8, 8, 8};
        System.out.println(sumHighestLowestFrequency(arr));
    }
}
```

### Optimal Complexity

-   **Time:** `O(n)` average --- one pass builds the map and another
    processes its entries.
-   **Auxiliary space:** `O(k)`, worst case `O(n)`.

### Common Mistakes

-   Adding the highest and lowest **elements** instead of their
    frequencies.
-   Forgetting that duplicate values share one entry in the frequency
    map.
-   Initializing the minimum frequency to `0`, which would incorrectly
    keep it at zero.

------------------------------------------------------------------------

# Part 5: Final Revision Sheet

## 1. Compare All Three Questions

  ------------------------------------------------------------------------
  Feature           Question 1        Question 2         Question 3
  ----------------- ----------------- ------------------ -----------------
  Goal              Find the most     Find the element   Add the highest
                    frequent element  with the           and lowest
                                      second-highest     frequencies
                                      distinct frequency 

  Main variables    `maxFreq`,        `maxFreq`,         `maxFreq`,
                    `answer`          `secondMaxFreq`,   `minFreq`
                                      `answer`           

  Tie handling      Choose the        Choose the         No element
                    smallest element  smallest element   tie-break
                                      with the second    required
                                      frequency          

  Special case      Maximum frequency Return `-1` if no  Every distinct
                    can be shared     second distinct    element's
                                      frequency exists   frequency is
                                                         considered

  Optimal average   `O(n)`            `O(n)`             `O(n)`
  time                                                   

  Auxiliary space   `O(k)`            `O(k)`             `O(k)`
  ------------------------------------------------------------------------

Here, `n` is the array length and `k` is the number of distinct
elements.

## 2. Three Patterns to Remember

### Pattern A --- Most Frequent Element

``` text
Count frequencies.
Find the largest frequency.
If frequencies tie, choose the smallest element.
```

### Pattern B --- Second-Highest Distinct Frequency

``` text
Count frequencies.
Find the largest frequency.
Find the largest frequency strictly below it.
Choose the smallest element with that frequency.
If it does not exist, return -1.
```

### Pattern C --- Sum of Highest and Lowest Frequency

``` text
Count frequencies.
Find the maximum frequency.
Find the minimum frequency.
Return maximum + minimum.
```

## 3. Practice Questions

Try solving these without looking at the solutions above.

### Practice 1 --- Highest Occurring Element

Input:

``` text
nums = [6, 2, 6, 3, 2]
```

What is the output?

### Practice 2 --- Second Highest Occurring Element

Input:

``` text
arr = [5, 5, 8, 8]
```

What is the output?

### Practice 3 --- Sum of Highest and Lowest Frequency

Input:

``` text
arr = [1, 1, 2, 2, 2, 3]
```

What is the output?

------------------------------------------------------------------------

## Practice Answers

```{=html}
<details>
```
```{=html}
<summary>
```
Show answers and explanations
```{=html}
</summary>
```
**Practice 1: `2`**

`2` and `6` both occur twice. Choose the smaller element, `2`.

**Practice 2: `-1`**

Both distinct elements have frequency `2`. There is no second distinct
frequency.

**Practice 3: `4`**

The maximum frequency is `3` and the minimum frequency is `1`. Their sum
is `4`.

```{=html}
</details>
```
## 4. Assignment Completion Checklist

-   [ ] Understand arrays, loops, frequency and hashing.
-   [ ] Solve all three questions manually using frequency tables.
-   [ ] Understand brute force and why it can be slow.
-   [ ] Understand how sorting can be used as an intermediate approach.
-   [ ] Understand the optimal frequency-map approach.
-   [ ] Practise the Python, C++ and Java implementations.
-   [ ] Explain time complexity and auxiliary space complexity.
-   [ ] Test edge cases, especially ties and the absence of a second
    distinct frequency.

------------------------------------------------------------------------

## Final Takeaway

The goal is not to memorise three programs. All three problems share the
same starting point: **counting frequencies**. The difference is what we
do after counting:

-   Question 1 selects the element with the highest frequency.
-   Question 2 selects the element with the second-highest distinct
    frequency.
-   Question 3 adds the highest and lowest frequency values.

Once you understand this pattern, many frequency-counting problems
become easier to recognise and solve.
