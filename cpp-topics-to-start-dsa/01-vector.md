# C++ Vectors — Complete Notes

---

## 1. Introduction

A **vector** is a dynamic array provided by the C++ Standard Template Library (STL). Unlike a regular array, a vector can **grow or shrink** in size at runtime.

### Include Header

```cpp
#include <vector>
```

---

## 2. Declaration Syntax

```cpp
vector<data_type> vector_name;
```

### Example

```cpp
#include <iostream>
#include <vector>

int main() {
    vector<int> old1;   // empty vector of integers
    return 0;
}
```

---

## 3. Initialization Examples

```cpp
vector<int> v1;                        // empty
vector<int> v2(5);                     // 5 elements, all 0
vector<int> v3(5, 10);                 // 5 elements, all 10
vector<int> v4 = {1, 2, 3, 4, 5};     // initializer list
vector<int> v5(v4);                    // copy of v4
```

---

## 4. Member Types (Reference Table)

| Member Type            | Definition                                        |
|------------------------|---------------------------------------------------|
| `value_type`           | The template type `T`                             |
| `allocator_type`       | Second template parameter (default: `allocator<T>`) |
| `reference`            | `value_type&`                                     |
| `const_reference`      | `const value_type&`                               |
| `pointer`              | `value_type*` (for default allocator)             |
| `const_pointer`        | `const value_type*` (for default allocator)       |
| `iterator`             | Random access iterator to `value_type`            |
| `const_iterator`       | Random access iterator to `const value_type`      |
| `reverse_iterator`     | `reverse_iterator<iterator>`                      |
| `const_reverse_iterator` | `reverse_iterator<const_iterator>`              |
| `difference_type`      | Signed integral (usually `ptrdiff_t`)             |
| `size_type`            | Unsigned integral (usually `size_t`)              |

---

## 5. Member Functions

---

### 5.1 Constructor & Destructor

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v1;                     // default constructor — empty
    vector<int> v2(4, 100);            // 4 elements, each = 100
    vector<int> v3(v2.begin(), v2.end()); // range constructor
    vector<int> v4(v3);                // copy constructor

    cout << "v2: ";
    for (int x : v2) cout << x << " "; // 100 100 100 100
    cout << endl;
    return 0;
}
```

---

### 5.2 `operator=` — Assign Content

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v1 = {1, 2, 3};
    vector<int> v2;
    v2 = v1;  // copy assignment

    for (int x : v2) cout << x << " ";  // 1 2 3
    return 0;
}
```

---

## 6. Iterators

Iterators allow traversal of the vector like pointers.

---

### `begin()` and `end()`

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {10, 20, 30, 40};

    for (auto it = v.begin(); it != v.end(); ++it) {
        cout << *it << " ";  // 10 20 30 40
    }
    return 0;
}
```

---

### `rbegin()` and `rend()` — Reverse Iteration

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {10, 20, 30, 40};

    for (auto it = v.rbegin(); it != v.rend(); ++it) {
        cout << *it << " ";  // 40 30 20 10
    }
    return 0;
}
```

---

### `cbegin()` / `cend()` — Const Iterators (C++11)

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {5, 10, 15};

    for (auto it = v.cbegin(); it != v.cend(); ++it) {
        cout << *it << " ";  // read-only: 5 10 15
        // *it = 99;         // ERROR: cannot modify through const_iterator
    }
    return 0;
}
```

---

### `crbegin()` / `crend()` — Const Reverse Iterators

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3};

    for (auto it = v.crbegin(); it != v.crend(); ++it) {
        cout << *it << " ";  // 3 2 1
    }
    return 0;
}
```

---

## 7. Capacity Functions

---

### `size()` — Number of Elements

```cpp
vector<int> v = {1, 2, 3, 4, 5};
cout << v.size();  // 5
```

---

### `max_size()` — Maximum Possible Elements

```cpp
vector<int> v;
cout << v.max_size();  // very large platform-dependent number
```

---

### `resize()` — Change Size

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3, 4, 5};
    v.resize(3);             // shrink: {1, 2, 3}
    v.resize(6, 99);         // grow with fill: {1, 2, 3, 99, 99, 99}

    for (int x : v) cout << x << " ";
    return 0;
}
```

---

### `capacity()` — Allocated Storage Size

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v;
    for (int i = 0; i < 10; i++) {
        v.push_back(i);
        cout << "size=" << v.size() << "  capacity=" << v.capacity() << "\n";
    }
    return 0;
}
```

> **Note:** `capacity` ≥ `size`. Capacity doubles when the vector needs to grow internally.

---

### `empty()` — Check if Empty

```cpp
vector<int> v;
cout << v.empty();  // 1 (true)

v.push_back(10);
cout << v.empty();  // 0 (false)
```

---

### `reserve()` — Pre-allocate Capacity

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v;
    v.reserve(100);  // allocate space for 100 elements
    cout << "capacity: " << v.capacity();  // 100
    cout << "  size: " << v.size();        // 0
    return 0;
}
```

---

### `shrink_to_fit()` — Reduce Capacity to Size (C++11)

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v(100);
    v.resize(10);
    cout << "capacity before: " << v.capacity();  // 100

    v.shrink_to_fit();
    cout << "  capacity after: " << v.capacity(); // 10
    return 0;
}
```

---

## 8. Element Access

---

### `operator[]` — Access by Index (No Bounds Check)

```cpp
vector<int> v = {10, 20, 30};
cout << v[0];  // 10
cout << v[2];  // 30
v[1] = 99;     // modify
```

---

### `at()` — Access by Index (With Bounds Check)

```cpp
#include <iostream>
#include <vector>
#include <stdexcept>
using namespace std;

int main() {
    vector<int> v = {10, 20, 30};
    cout << v.at(1);   // 20

    try {
        cout << v.at(10);  // throws out_of_range
    } catch (out_of_range& e) {
        cout << "Error: " << e.what();
    }
    return 0;
}
```

---

### `front()` — First Element

```cpp
vector<int> v = {5, 10, 15};
cout << v.front();  // 5
```

---

### `back()` — Last Element

```cpp
vector<int> v = {5, 10, 15};
cout << v.back();  // 15
```

---

### `data()` — Raw Pointer to Data (C++11)

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3};
    int* ptr = v.data();

    for (int i = 0; i < 3; i++) {
        cout << *(ptr + i) << " ";  // 1 2 3
    }
    return 0;
}
```

---

## 9. Modifier Functions

---

### `push_back()` — Add Element at End

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v;
    v.push_back(10);
    v.push_back(20);
    v.push_back(30);

    for (int x : v) cout << x << " ";  // 10 20 30
    return 0;
}
```

---

### `pop_back()` — Remove Last Element

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3, 4};
    v.pop_back();  // removes 4

    for (int x : v) cout << x << " ";  // 1 2 3
    return 0;
}
```

---

### `insert()` — Insert Elements at Position

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {1, 2, 4, 5};

    // insert single element at position
    v.insert(v.begin() + 2, 3);         // {1, 2, 3, 4, 5}

    // insert multiple copies
    v.insert(v.begin(), 2, 0);          // {0, 0, 1, 2, 3, 4, 5}

    for (int x : v) cout << x << " ";
    return 0;
}
```

---

### `erase()` — Remove Elements

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {10, 20, 30, 40, 50};

    v.erase(v.begin() + 1);              // remove 20 → {10, 30, 40, 50}
    v.erase(v.begin(), v.begin() + 2);   // remove range → {40, 50}

    for (int x : v) cout << x << " ";
    return 0;
}
```

---

### `assign()` — Assign New Content

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v;

    v.assign(5, 7);    // 5 copies of 7 → {7, 7, 7, 7, 7}
    for (int x : v) cout << x << " ";
    cout << "\n";

    v.assign({1, 2, 3});  // from initializer list
    for (int x : v) cout << x << " ";
    return 0;
}
```

---

### `swap()` — Swap Two Vectors

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> a = {1, 2, 3};
    vector<int> b = {10, 20, 30, 40};

    a.swap(b);

    cout << "a: "; for (int x : a) cout << x << " ";  // 10 20 30 40
    cout << "\nb: "; for (int x : b) cout << x << " "; // 1 2 3
    return 0;
}
```

---

### `clear()` — Remove All Elements

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3, 4};
    cout << "size before: " << v.size();  // 4

    v.clear();
    cout << "  size after: " << v.size(); // 0
    return 0;
}
```

---

### `emplace()` — Construct and Insert in Place (C++11)

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {1, 2, 4, 5};
    v.emplace(v.begin() + 2, 3);  // insert 3 at index 2

    for (int x : v) cout << x << " ";  // 1 2 3 4 5
    return 0;
}
```

> **`emplace` vs `insert`:** `emplace` constructs the element *in-place*, avoiding an extra copy. Better for complex objects.

---

### `emplace_back()` — Construct and Insert at End (C++11)

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

int main() {
    vector<string> v;
    v.emplace_back("Hello");   // constructed in-place, no copy
    v.emplace_back("World");

    for (auto& s : v) cout << s << " ";  // Hello World
    return 0;
}
```

> **`emplace_back` vs `push_back`:** `emplace_back` passes arguments to the constructor directly. More efficient for objects.

---

## 10. Allocator

### `get_allocator()` — Get the Allocator Object

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v;
    int* p = v.get_allocator().allocate(5);  // allocate raw memory for 5 ints

    for (int i = 0; i < 5; i++) p[i] = i * 10;
    for (int i = 0; i < 5; i++) cout << p[i] << " ";  // 0 10 20 30 40

    v.get_allocator().deallocate(p, 5);  // free memory
    return 0;
}
```

---

## 11. Non-Member Functions

### Relational Operators

Vectors can be compared element by element.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> a = {1, 2, 3};
    vector<int> b = {1, 2, 3};
    vector<int> c = {1, 2, 4};

    cout << (a == b);  // 1 (true)
    cout << (a != c);  // 1 (true)
    cout << (a < c);   // 1 (true)
    cout << (c > a);   // 1 (true)
    return 0;
}
```

---

### Non-member `swap()`

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> x = {1, 2, 3};
    vector<int> y = {7, 8, 9, 10};

    swap(x, y);  // non-member swap

    cout << "x: "; for (int v : x) cout << v << " ";  // 7 8 9 10
    cout << "\ny: "; for (int v : y) cout << v << " "; // 1 2 3
    return 0;
}
```

---

## 12. `vector<bool>` — Template Specialization

`vector<bool>` is a special case that packs bits for memory efficiency.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<bool> flags = {true, false, true, true, false};

    for (bool f : flags) cout << f << " ";  // 1 0 1 1 0

    flags[1] = true;
    cout << "\nAfter update: ";
    for (bool f : flags) cout << f << " ";  // 1 1 1 1 0
    return 0;
}
```

> **Note:** `vector<bool>` does **not** store actual `bool` values — it packs bits. Use `vector<char>` or `vector<int>` if you need true element-level access.

---

## 13. Quick Reference Summary Table

| Method           | Category      | Description                              |
|------------------|---------------|------------------------------------------|
| `push_back(x)`   | Modifier      | Add `x` to end                           |
| `pop_back()`     | Modifier      | Remove last element                      |
| `insert(pos, x)` | Modifier      | Insert `x` at `pos`                      |
| `erase(pos)`     | Modifier      | Remove element at `pos`                  |
| `clear()`        | Modifier      | Remove all elements                      |
| `assign(n, x)`   | Modifier      | Replace content with `n` copies of `x`   |
| `swap(v2)`       | Modifier      | Swap contents with `v2`                  |
| `emplace(pos, x)`| Modifier      | Construct and insert at `pos`            |
| `emplace_back(x)`| Modifier      | Construct and insert at end              |
| `size()`         | Capacity      | Number of elements                       |
| `capacity()`     | Capacity      | Allocated storage size                   |
| `resize(n)`      | Capacity      | Resize to `n` elements                   |
| `reserve(n)`     | Capacity      | Pre-allocate capacity for `n` elements   |
| `empty()`        | Capacity      | Returns `true` if vector is empty        |
| `shrink_to_fit()`| Capacity      | Reduce capacity to size                  |
| `max_size()`     | Capacity      | Max possible number of elements          |
| `operator[]`     | Element Access| Access element (no bounds check)         |
| `at(i)`          | Element Access| Access element (with bounds check)       |
| `front()`        | Element Access| First element                            |
| `back()`         | Element Access| Last element                             |
| `data()`         | Element Access| Pointer to underlying array              |
| `begin()`        | Iterator      | Iterator to first element                |
| `end()`          | Iterator      | Iterator past last element               |
| `rbegin()`       | Iterator      | Reverse iterator to last element         |
| `rend()`         | Iterator      | Reverse iterator before first element    |
| `cbegin()`       | Iterator      | Const iterator to first element          |
| `cend()`         | Iterator      | Const iterator past last element         |
| `crbegin()`      | Iterator      | Const reverse iterator to last element   |
| `crend()`        | Iterator      | Const reverse iterator before first      |
| `get_allocator()`| Allocator     | Returns the allocator object             |

---

## 14. Key Points to Remember

- Always `#include <vector>` before using vectors.
- Vectors automatically manage memory — no `new` or `delete` needed.
- Use `at()` over `operator[]` when you want safe bounds-checked access.
- Use `emplace_back()` over `push_back()` for better performance with objects.
- `capacity` ≥ `size` always; use `reserve()` to avoid repeated reallocations.
- `vector<bool>` is a special case — it stores bits, not actual booleans.

---
