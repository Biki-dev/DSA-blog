# Hashing, Hash Table, and Map — Easy Exam Notes

## 1. What is Hashing?

Hashing is a technique used to **store data in a way that allows fast access later**.

Very simple idea:

- **Pre-store** information
- **Fetch** it when needed

So instead of checking the whole array/string again and again, we build a helper structure once and then answer queries quickly.

---

## 2. Why do we need hashing?

Suppose you are asked many times:

- How many times does `1` appear?
- How many times does `3` appear?
- How many times does `10` appear?

If you count each time by scanning the whole array, then every query takes `O(n)` time.

If there are `Q` queries, total time becomes:

```text
O(Q × n)
```

That can be too slow for large input.

Hashing solves this by doing one preprocessing pass and then answering each query in `O(1)` or `O(log n)` depending on the data structure used.

---

## 3. Number Hashing using Array

### Idea

If the numbers are within a small range, we can create an array called a **hash array** or **frequency array**.

Example:

```text
Array = [1, 2, 1, 3, 2]
```

Maximum value = `3`

So we create:

```text
hash[0..3]
```

Initially:

```text
hash = [0, 0, 0, 0]
```

Now count each number:

- `1` appears 2 times
- `2` appears 2 times
- `3` appears 1 time

So the hash array becomes:

```text
hash[1] = 2
hash[2] = 2
hash[3] = 1
```

### Query answer

- frequency of `1` = `hash[1]`
- frequency of `2` = `hash[2]`
- frequency of `3` = `hash[3]`

---

## 4. C++ Code for Number Hashing

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> arr(n);
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }

    // Suppose max value in arr is <= 12
    // So we make hash array of size 13
    vector<int> hash(13, 0);

    // Pre-computation
    for (int i = 0; i < n; i++) {
        hash[arr[i]]++;
    }

    int q;
    cin >> q;
    while (q--) {
        int x;
        cin >> x;
        cout << hash[x] << endl;
    }

    return 0;
}
```

---

## 5. Time Complexity of Array Hashing

### Preprocessing
We scan the array once:

```text
O(n)
```

### Query
Each query is answered in:

```text
O(1)
```

### Total
For `Q` queries:

```text
O(n + Q)
```

This is much better than `O(Q × n)`.

---

## 6. Limitation of Array Hashing

Array hashing works only when the values are within a **small range**.

Example:

- max value = `12` → array size `13` is fine
- max value = `10^9` → array size `10^9 + 1` is not possible

### Memory problem

In C++, if the array is declared:

- **inside `main()`**, usually safe only up to around `10^6`
- **globally**, can go up to around `10^7`

Beyond that, array hashing is not practical.

So for very large numbers, we use:

- `map`
- `unordered_map`

---

## 7. Character Hashing using Array

If the input is a string and we want frequency of characters, we can use hashing too.

Example:

```text
s = "abcdabefc"
```

If queries are:

- how many times `a` appears
- how many times `c` appears
- how many times `z` appears

we can answer using a hash array.

---

## 8. Character Hashing for Lowercase Letters

If the string contains only lowercase letters `a` to `z`, we need only `26` positions.

### Mapping

```text
a -> 0
b -> 1
c -> 2
...
z -> 25
```

The formula is:

```cpp
index = ch - 'a'
```

Example:

- `'a' - 'a' = 0`
- `'b' - 'a' = 1`
- `'f' - 'a' = 5`

---

## 9. C++ Code for Lowercase Character Hashing

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    string s;
    cin >> s;

    vector<int> hash(26, 0);

    // Pre-computation
    for (int i = 0; i < (int)s.size(); i++) {
        hash[s[i] - 'a']++;
    }

    int q;
    cin >> q;
    while (q--) {
        char ch;
        cin >> ch;
        cout << hash[ch - 'a'] << endl;
    }

    return 0;
}
```

---

## 10. Character Hashing for Uppercase or All Characters

### If uppercase letters are also present
Use:

```cpp
index = ch - 'A'
```

### If all ASCII characters may come
Use size `256` and directly index by the character:

```cpp
vector<int> hash(256, 0);
hash[s[i]]++;
```

This works because a character is automatically converted to its ASCII value.

---

## 11. C++ Code for All ASCII Characters

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    string s;
    cin >> s;

    vector<int> hash(256, 0);

    for (int i = 0; i < (int)s.size(); i++) {
        hash[s[i]]++;
    }

    int q;
    cin >> q;
    while (q--) {
        char ch;
        cin >> ch;
        cout << hash[(int)ch] << endl;
    }

    return 0;
}
```

---

## 12. What is a Hash Table?

A hash table is a data structure that stores:

- **key**
- **value**

Example for frequency counting:

- key = number
- value = frequency

So if array is:

```text
[1, 2, 3, 1, 3, 2]
```

then the hash table stores:

```text
1 -> 2
2 -> 2
3 -> 2
```

---

## 13. Hashing using `map` in C++

In C++, `map` stores data in **key-value** form.

Example:

```cpp
map<int, int> mp;
```

For frequency counting:

```cpp
mp[arr[i]]++;
```

If key does not exist, its value is automatically treated as `0`.

So:

- `mp[1]++` makes `1 -> 1`
- next `mp[1]++` makes `1 -> 2`

---

## 14. C++ Code using `map`

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> arr(n);
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }

    map<int, int> mp;

    // Pre-computation
    for (int i = 0; i < n; i++) {
        mp[arr[i]]++;
    }

    int q;
    cin >> q;
    while (q--) {
        int x;
        cin >> x;
        cout << mp[x] << endl;
    }

    return 0;
}
```

---

## 15. Important Property of `map`

`map` stores keys in **sorted order**.

Example:

```text
Input keys: 3, 1, 2
Stored as: 1, 2, 3
```

So when you iterate over a `map`, keys come in sorted order.

---

## 16. Time Complexity of `map`

For `map`:

- insertion = `O(log n)`
- search = `O(log n)`
- deletion = `O(log n)`

This is true in best, average, and worst cases.

---

## 17. `unordered_map` in C++

`unordered_map` is also a key-value structure, but it does **not** store keys in sorted order.

Example:

```cpp
unordered_map<int, int> ump;
```

### Advantages
- Average insertion/search: `O(1)`

### Disadvantage
- Worst case: `O(n)` due to collisions

---

## 18. C++ Code using `unordered_map`

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> arr(n);
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }

    unordered_map<int, int> ump;

    for (int i = 0; i < n; i++) {
        ump[arr[i]]++;
    }

    int q;
    cin >> q;
    while (q--) {
        int x;
        cin >> x;
        cout << ump[x] << endl;
    }

    return 0;
}
```

---

## 19. `map` vs `unordered_map`

### `map`
- sorted keys
- `O(log n)` for search/insert
- safer when sorted order is needed

### `unordered_map`
- no order
- average `O(1)`
- usually faster
- worst case `O(n)` because of collisions

In competitive programming, first preference is often:

```cpp
unordered_map
```

If it gives TLE, then try `map`.

---

## 20. What is Collision?

Collision happens when two different keys get the **same hash index**.

Example:

```text
18, 28, 38, 48
```

If the hash function sends all of them to the same place, then a collision happens.

This creates a chain of values at the same location and slows down searching.

---

## 21. Division Method

This is a simple hashing method.

If we have small storage of size `10`, we can store values using:

```cpp
index = number % 10
```

Examples:

- `2 % 10 = 2`
- `15 % 10 = 5`
- `16 % 10 = 6`
- `28 % 10 = 8`
- `139 % 10 = 9`

This reduces large numbers into valid array indices.

---

## 22. Collision Handling: Chaining

If two numbers give the same remainder, we cannot store both at the same index directly.

So we use **chaining**.

Example:

```text
18 % 10 = 8
28 % 10 = 8
38 % 10 = 8
```

Then all are stored in one chain at index `8`.

This is called **linear chaining**.

---

## 23. Key Points About Hashing in Interviews

### Number hashing
- Use array if range is small
- Use `map` / `unordered_map` if range is large

### Character hashing
- Lowercase: `26` size array, use `ch - 'a'`
- Uppercase: `26` size array, use `ch - 'A'`
- All ASCII: `256` size array

### Frequency counting
- very common hashing use-case

---

## 24. Mini Revision Sheet

### Hashing
A technique to store and fetch data fast.

### Array hashing
Best for small ranges.

### Character hashing
Best with `ch - 'a'` or ASCII array of size `256`.

### `map`
Key-value store, sorted, `O(log n)`.

### `unordered_map`
Key-value store, not sorted, average `O(1)`.

### Collision
Two keys mapped to the same location.

### Division method
Use modulo to reduce value:

```cpp
index = value % size
```

---

## 25. One-line Exam Answers

### Q. What is hashing?
Hashing is a technique used to store data in a way that allows fast access and retrieval.

### Q. Why is hashing useful?
It reduces repeated searching time and helps answer queries quickly.

### Q. What is a hash table?
A structure that stores key-value pairs for fast lookup.

### Q. What is the difference between `map` and `unordered_map`?
`map` is sorted and takes `O(log n)`; `unordered_map` is unsorted and usually takes `O(1)` average time.

### Q. What is collision?
When two different keys produce the same hash index.

---

## 26. Final Takeaway

Hashing means:

1. **Precompute**
2. **Store frequency / value**
3. **Fetch directly**

That is why hashing is one of the most useful tools in DSA.

---

## References

1.Youtube video
 <iframe
    width="800"
    height="450"
    src="https://www.youtube.com/embed/KEs5UyBJ39g"
    title="Hashing, Hash Table and Map"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
</iframe>