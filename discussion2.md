---
marp: true
---

# Discussion 2

## Agenda

* Introduction to Time and Space Complexity
* Big O Notation
* Common Time Complexities
* Space Complexity
* Examples and Exercises

---

# Introduction

## Why Study Time and Space Complexity?

* Importance in algorithm efficiency
* Impact on performance and scalability

---

# Time Complexity

## Definition

* Measures the amount of time an algorithm takes to run as a function of the input size.
* **Big O Notation** formalizes the upper bound of an algorithm's growth rate.

---

# Common Time Complexities

* **O(1)**: Constant Time
* **O(log n)**: Logarithmic Time
* **O(n)**: Linear Time
* **O(n log n)**: Linearithmic Time
* **O(n²)**: Quadratic Time
* **O(2ⁿ)**: Exponential Time

---

# Example 1: 

```cpp
int getFirstElement(int arr[]) {
    return arr[0];
}
```
* Question: What is the time complexity?
* O(1)
* Explanation: Accessing an element in an array is constant time.

---

# Example 2:

```cpp
int sumArray(int arr[], int n) {
    int sum = 0;
    for(int i = 0; i < n; i++) {
        sum += arr[i];
    }
    return sum;
}
```
* Question: What is the time complexity?
* O(n)


---

# Example 3:

```cpp
void printPairs(int arr[], int n) {
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            std::cout << arr[i] << ", " << arr[j] << std::endl;
        }
    }
}
```
* Discussion: Analyze the nested loops and their impact on complexity
* O(n²)
* Explanation: Each loop runs n times, so the inner operation executes n * n = n² times.
---

# Example 4:

```cpp
int binarySearch(int arr[], int n, int target) {
    int left = 0, right = n - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target)
            return mid; // Target found
        else if (arr[mid] < target)
            left = mid + 1; // Search right half
        else
            right = mid - 1; // Search left half
    }
    return -1; // Target not found
}
```
* Discussion: What is the time complexity?
* O(log n)
* Explanation: With each iteration, the size of the search space is reduced by half..
---

# Example 5:

```cpp
int fibonacci(int n) {
    if (n <= 1)
        return n;
    else
        return fibonacci(n - 1) + fibonacci(n - 2);
}
```
* Discussion: What is the time complexity?
* O(2ⁿ)
* Explanation: 
    + 1. Recursive calls grow exponentially.
    + 2. Highly inefficient for large n without optimization.
---

<!-- # Example 6:
<div style="width: 100%; overflow-x: auto;">
```cpp
void merge(int arr[], int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    int* L = new int[n1];
    int* R = new int[n2];
    for (int i = 0; i < n1; i++)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[mid + 1 + j];
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k++] = L[i++];
        } else {
            arr[k++] = R[j++];
        }
    }
    while (i < n1)
        arr[k++] = L[i++];
    while (j < n2)
        arr[k++] = R[j++];
    delete[] L;
    delete[] R;
}
void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}
</div> 
```

* Discussion: What is the time complexity?
* O(n log n)
* Explanation: 
    + Merge Sort is a classic divide and conquer algorithm.
    + The array is recursively divided into halves (log n divisions).
    + Merging two sorted arrays takes linear time O(n).
    + Combining these steps results in an overall time complexity of O(n log n).
--- -->

# Space Complexity

## Definition

* Measures the amount of memory an algorithm uses in relation to the input size.
* Memory constraints can limit the feasibility of an algorithm.

---

# Example
```cpp
int* createArray(int n) {
    int* arr = new int[n];
    // Initialize array with values
    for(int i = 0; i < n; i++) {
        arr[i] = i * 2;
    }
    return arr;
}
```
* Question: What is the space complexity?
* O(n)
---

# Common Mistakes
* Ignoring lower-order terms
* Misinterpreting Big O notation
* Overlooking hidden constants

---
# Exercise

* Analyze the time and space complexity of the following code:

```cpp
void mysteryFunction(int n) {
    for(int i = 1; i <= n; i *= 2) {
        for(int j = 0; j < n; j++) {
            std::cout << i << ", " << j << std::endl;
        }
    }
}
```

* Hint: The outer loop runs log₂n times, and the inner loop runs n times.
* Time Complexity: O(n log n)
* Space Complexity: O(1)
---

# Additional Notes

* Big O Notation: Focuses on the upper bound; it describes the worst-case scenario.
* Best Practices:
    + Always consider the input size when analyzing algorithms.
    + Be aware of trade-offs between time and space complexity.

---

## Resources

* [Big O Cheat Sheet](https://www.bigocheatsheet.com/)
* [Algorithm Visualizations](https://visualgo.net/en)

---

## Tips for Analyzing Complexity

1. **Identify Loops**:
   - Single loop over `n` elements → **O(n)**
   - Nested loops over `n` elements → **O(n²)**

2. **Recursive Calls**:
   - Analyze the recurrence relation.
   - Use the Master Theorem if applicable.

3. **Operations Inside Loops**:
   - Constant time operations inside loops don't change the overall complexity.

4. **Common Patterns**:
   - Binary search algorithms → **O(log n)**
   - Divide and conquer algorithms (like Merge Sort) → **O(n log n)**