---
marp: true
math: mathjax
---

# Discussion 4

Jeremy Wang

---

## Agenda

* Dynamic Array

* homework 1
  + data structures to implement
  + the optional type
  + Google Test

---

## Dynamic Array

---

### What's a dynamic array?

A dynamic array is an array that can grow and shrink in size at runtime.

---

### Why dynamic array?

---

### Why dynamic array?

- We may not know the size of the array at the time of array declaration
- Elements can be added or removed at anytime

---

### Why dynamic array?

- We may not know the size of the array at the time of array declaration
- Elements can be added or removed at anytime
- Then, why not linked list?

---

### Why dynamic array?

- We may not know the size of the array at the time of array declaration
- Elements can be added or removed at anytime
- We need efficient random access

---

### Array vs List

What's the difference between array and list?

---

### Array vs List

⚠️

There is no universal definition of array and list.

Different languages have different definitions & implementations.

---

### Array & List in C++

In C++,

- `std::list` is a doubly-linked list
- `std::vector` is a dynamic array
- `std::array` is a fixed-size array
- C-style array is also a fixed-size array

---

### In general context and some other languages

- Array often refers fixed-size array
- List usually refers to dynamic array
- Linked list is usually referred to as linked list explicitly

---

### Recap of Fixed-sized Array & Linked List

| Operation            | Fixed-sized Array <br> (e.g. `std::array`) | Linked List <br> (e.g. `std::list`) |
| -------------------- | ------------------------------------------ | ----------------------------------- |
| Random Access (`[]`) |                                            |                                     |
| Append               |                                            |                                     |
| Insert               |                                            |                                     |
| Remove               |                                            |                                     |
| Memory               |                                            |                                     |

---

### Recap of Fixed-sized Array & Linked List

| Operation            | Fixed-sized Array <br> (e.g. `std::array`) | Linked List <br> (e.g. `std::list`) |
| -------------------- | ------------------------------------------ | ----------------------------------- |
| Random Access (`[]`) | $O(1)$                                     | $O(n)$                              |
| Append               | Not supported                              | $O(1)$                              |
| Insert               | Not supported                              | $O(1)$                              |
| Remove               | Not supported                              | $O(1)$                              |
| Memory               | Continuous                                 | Not Continuous                      |

---

### How to implement a dynamic array?

---

### Naive Approach

When we append to the end of the array, we create a new array with **size + 1**,
copy all the elements from the old array to the new array, and then append the
new element to the new array.

---

### Naive Approach

When we append to the end of the array, we create a new array with **size + 1**,
copy all the elements from the old array to the new array, and then append the
new element to the new array.

- Con: $O(n)$ time complexity for append

---

### Improved Naive Approach

When we append to the end of the array, we check if the array is full. If it is
full, we create a new array with **size + 4**, copy all the elements from the
old array to the new array, and then append the new element to the new array.

---

### Improved Naive Approach

When we append to the end of the array, we check if the array is full. If it is
full, we create a new array with **size + 4**, copy all the elements from the
old array to the new array, and then append the new element to the new array.

- Pro: less memory allocation & copying for append than the naive approach
- Con: still $O(n)$ time complexity for append

---

### Improved Naive Approach

<style scoped>
table {
  font-size: 16px;
}
</style>

| Operation  | Capacity | Size | Memory Alloc/Dealloc | Elements Copied |
| ---------- | -------- | ---- | -------------------- | --------------- |
| Initialize | 4        | 0    | 1                    | 0               |
| Append     | 4        | 1    | 0                    | 1               |
| Append     | 4        | 2    | 0                    | 1               |
| Append     | 4        | 3    | 0                    | 1               |
| Append     | 4        | 4    | 0                    | 1               |
| Append     | 8        | 5    | 1                    | 4 + 1           |
| Append     | 8        | 6    | 0                    | 1               |
| Append     | 8        | 7    | 0                    | 1               |
| Append     | 8        | 8    | 0                    | 1               |
| Append     | 12       | 9    | 1                    | 8 + 1           |
| Append     | 12       | 10   | 0                    | 1               |
| Append     | ...      | ...  | ...                  | ...             |

---

### Improved Naive Approach (not a formal proof)

Total time to append $n$ elements into the dynamic array that is empty:

$$
\begin{align*}
T_n(n)
&= n + \sum_{k=1}^{\frac{n}{4} - 1} 4k \\
&= n + 4 \sum_{k=1}^{\frac{n}{4} - 1} k \\
&= n + 4 \cdot \frac{\frac{n}{4} \cdot (\frac{n}{4} - 1)}{2} \\
&= n + \frac{n^2}{8} - \frac{n}{2} \\
&= O(n^2) \\
\end{align*}
$$

Amortized time complexity for append:

$$
\begin{align*}
T_1(n) &= \frac{1}{n} O(n^2) = O(n) \\
\end{align*}
$$

---

### Improved Naive Approach (not a formal proof)

For any fixed $m > 0$, total time to append $n$ elements into the dynamic array
that is empty:

$$
\begin{align*}
T_n(n)
&= n + \sum_{k=1}^{\frac{n}{m} - 1} mk \\
&= n + m \sum_{k=1}^{\frac{n}{m} - 1} k \\
&= n + m \cdot \frac{\frac{n}{m} \cdot (\frac{n}{m} - 1)}{2} \\
&= n + \frac{n^2}{2m} - \frac{n}{2}
= O(n^2) \\
\end{align*}
$$

Amortized time complexity for append:

$$
\begin{align*}
T_1(n) &= \frac{1}{n} O(n^2) = O(n) \\
\end{align*}
$$

---

### Actual Approach

When we append to the end of the array, we check if the array is full. If it is
full, we create a new array with **size \* 2**, copy all the elements from the
old array to the new array, and then append the new element to the new array.

- Pro: even less memory allocation & copying for append
- Con: average memory utilization is lower

---

### Actual Approach

<style scoped>
table {
  font-size: 16px;
}
</style>

| Operation  | Capacity | Size | Memory Alloc/Dealloc | Elements Copied |
| ---------- | -------- | ---- | -------------------- | --------------- |
| Initialize | 1        | 0    | 1                    | 0               |
| Append     | 1        | 1    | 0                    | 1               |
| Append     | 2        | 2    | 1                    | 1 + 1           |
| Append     | 4        | 3    | 1                    | 2 + 1           |
| Append     | 4        | 4    | 0                    | 1               |
| Append     | 8        | 5    | 1                    | 4 + 1           |
| Append     | 8        | 6    | 0                    | 1               |
| Append     | 8        | 7    | 0                    | 1               |
| Append     | 8        | 8    | 0                    | 1               |
| Append     | 16       | 9    | 1                    | 8 + 1           |
| Append     | 16       | 10   | 0                    | 1               |
| Append     | ...      | ...  | ...                  | ...             |

---

### Time Complexity for Actual Approach (not a formal proof)

Time for appending $n$ elements into the dynamic array that is empty:

$$
\begin{align*}
T_n(n)

&= n + \sum_{k=1}^{\log_2 n} 2^{k-1} \\
&= n + \frac{1}{2} \sum_{k=1}^{\log_2 n} 2^{k} \\
&= n + \frac{1}{2} \cdot \frac{2 (2^{\log_2 n} - 1)}{2 - 1} \\
&= n + (2^{\log_2 n} - 1) \\
&= n + (n - 1)
= O(n) \\
\end{align*}
$$

Amortized time complexity for append:

$$
\begin{align*}
T_1(n) &= \frac{1}{n} O(n) = O(1) \\
\end{align*}
$$

---

### Time Complexity Comparison

| Operation            | Dynamic Array <br> (e.g. `std::vector`) | Fixed-sized Array <br> (e.g. `std::array`) | Doubly-linked List <br> (e.g. `std::list`) |
| -------------------- | --------------------------------------- | ------------------------------------------ | ------------------------------------------ |
| Random Access (`[]`) | $O(1)$                                  | $O(1)$                                     | $O(n)$                                     |
| Append               | $O(1)$ (amortized)                      | Not supported                              | $O(1)$                                     |
| Insert               | $O(n)$                                  | Not supported                              | $O(1)$                                     |
| Remove               | $O(n)$                                  | Not supported                              | $O(1)$                                     |
| Memory               | Continuous                              | Continuous                                 | Not Continuous                             |

---

### Actual Performance

In practice, Dynamic Array is usually faster than Linked List in all operations
when the number of elements is less than 20,000.

Why?

---

### Actual Performance

- big O only tells us the asymptotic performance
- continuous memory locations for elements makes dynamic array practically
  faster than linked list

---

# Homework 1

---

### Linked List Node

```cpp
template <typename T>
class LinkedListNode
{
private:
  // make constructors and `_next` field only available to `LinkedList` class
  // to avoid instantiating node and mutating `_next` outside `LinkedList` class.

  template <typename U>
  friend class LinkedList;

  LinkedListNode<T> *_next;

  explicit LinkedListNode(T value) : value(value), _next(nullptr) {}
  LinkedListNode(T value, LinkedListNode<T> *next) : value(value), _next(next) {}

public:
  T value;
  LinkedListNode<T> *next() { return _next; }
};
```

this definition is provided in [lib/LinkedListNode.hpp](https://github.com/ecs36c-sq2023/hw1/blob/main/lib/LinkedListNode.hpp).

---

### Linked List Insert

```
Linked list:
┌───┬───┐  ┌───┬───┐  ┌───┬───┐
│ 1 │  ─┼─→│ 2 │  ─┼─→│ 3 │ │ │─→ nullptr
└───┴───┘  └───┴───┘  └───┴───┘
  ↑                       ↑
  head                   tail
```

add a node 4 after node 2

```
  ┌───┬───┐  ┌───┬───┐            ┌─────┬───┐
  │ 1 │  ─┼─→│ 2 │   │            │ 3   │ │ │─→ nullptr
  └───┴───┘  └───┴───┘            └─┼───┴───┘
    ↑              ↓                ↑    ↑
   head            │                │   tail
                   │                │
                   └──→ ┌───┬───┐   │
                        │ 4 │  ─┼───┘
                        └───┴───┘ 
```

---

### Linked List Remove

deleting node

```
  ┌───┬───┐  ┌───┬───┐  2. new    ┌─────┬───┐
  │ 1 │  ─┼─→│ 2 │   │  ──────→   │ 3   │ │ │─→ nullptr
  └───┴───┘  └───┴───┘            └─┼───┴───┘
    ↑              ↓                ↑    ↑
   head            │                │   tail
                   x                │
                   └──→ ┌───┬───┐   │
           1. temp ──→  │ 4 │  ─┼───┘
                        └───┴───┘ 
```

what to do next?

---

### Remember to free your heap memory!

```cpp
delete temp;
```

---

## `std::optional`

```cpp
/// @brief remove the first element from the list
/// @return the value of head it just removed
T removeHead();
```

what's wrong?

---

The current list **maybe** empty, then what should it return?

```hs
data Maybe a = Just a | Nothing
```

If a operation may fail, 
the return type of the function should be its desired type **or** nothing.

---

In out case, `removeHead()` should return Just a value of type `T` or nothing, 
so

```cpp
/// @brief remove the first element from the list
/// @return the removed element if there was at least one element in the list; 
///         std::nullopt otherwise
std::optional<T> removeHead();
```

---

## Sum/union Type

In general, viewing types as sets, 
we can say a **union type** is the union of these types, 
so a instance of the union type can be of any type in that union.

```py
def square(number: int | float) -> int | float:
    return number ** 2
```

---

What about `pop()/top()` function of stack?

---

## GoogleTest

A unit testing framework for C++ (and other languages).

running tests for `hw1`

```bash
> mkdir build
> cd build
> cmake ..
> make
> ./run_tests
Running main() from /ECS36C/hw1/build/_deps/googletest-src/googletest/src/gtest_main.cc
[----------] 1 test from QueueTest (0 ms total)

[----------] Global test environment tear-down
[==========] 24 tests from 3 test suites ran. (0 ms total)
[  PASSED  ] 22 tests.
[  FAILED  ] 2 tests, listed below:
[  FAILED  ] StackTest.BadTest
[  FAILED  ] QueueTest.BadTest

 2 FAILED TESTS
```

---

### Adding your new tests

suppose adding a new test for linked list

```cpp
TEST(LinkedListTest, DoesSomething) {
  // init the linked list
  LinkedList<char> ll;
  ...
  ASSERT_EQ(do_something, its_rnt);
  ASSERT_NE(do_something, what_it_should_not_be)
  ...
}
```

to test other things, 
* change name of test suite (`LinkedListTest`)
* init your object
* assert its behaviors

---
Thank you!