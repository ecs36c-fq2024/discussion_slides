---
marp: true
math: mathjax
---

<!-- # quick sort https://www.geeksforgeeks.org/quick-sort-algorithm/ -->

# Discussion 8


---

## Agenda

- Review of Quick Sort and Merge Sort
- Radix Sort
- Bucket Sort

---

## Quick Sort vs Merge Sort

|                   | Quick Sort      | Merge Sort    |
| ----------------- | --------------- | ------------- |
| Time Complexity   |                 |               |
| Space Requirement |                 |               |
| Stability         |                 |               |

---

## Quick Sort vs Merge Sort

|                   | Quick Sort               | Merge Sort        |
| ----------------- | ------------------------ | ----------------- |
| Time Complexity   | Average: $O(n \log n)$   | $O(n \log n)$     |
|                   | Worst: $O(n^2)$          |                   |
| Space Requirement | In-place ($O(\log n)$)   | Not in-place ($O(n)$) |
| Stability         | Unstable                 | Stable            |

---

## Quick Sort

### Overview

- **Divide and Conquer Algorithm**
- Picks an element as a pivot and partitions the array around the pivot
- Commonly used in practice due to its good average-case performance

---

### Steps

1. **Choose a Pivot**: Can be the first element, last element, random element, or median.
2. **Partitioning**: Rearrange elements so that elements less than pivot are on the left, and those greater are on the right.
3. **Recursive Sorting**: Recursively apply the above steps to the sub-array of elements with smaller values and separately to the sub-array of elements with greater values.

---

## Quick Sort Implementation

```cpp
void quickSort(int arr[], int low, int high) {
    if (low < high) {
        // Partition the array
        int pi = partition(arr, low, high);
        // Sort the partitions
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

// Lomuto Partition Scheme
int partition(int arr[], int low, int high) {
    int pivot = arr[high]; // Pivot
    int i = (low - 1);     // Index of smaller element
    for (int j = low; j <= high - 1; j++) {
        if (arr[j] <= pivot) {
            i++; // Increment index
            swap(&arr[i], &arr[j]);
        }
    }
    swap(&arr[i + 1], &arr[high]);
    return (i + 1);
}
```
---

## Merge Sort

### Overview

- **Divide and Conquer Algorithm**
- Divides the array into halves, sorts them, and then merges the sorted halves
- Guarantees $O(n \log n)$ time complexity in all cases

### Steps

1. **Divide**: Split the array into two halves.
2. **Conquer**: Recursively sort the two halves.
3. **Combine**: Merge the two sorted halves into a single sorted array.

---

## Merge Sort Implementation

```cpp
void merge(int arr[], int left, int mid, int right) {
    // Sizes of two subarrays
    int n1 = mid - left + 1;
    int n2 = right - mid;

    // Temporary arrays
    int L[n1], R[n2];

    // Copy data to temp arrays
    for (int i = 0; i < n1; i++)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[mid + 1 + j];

    // Merge the temp arrays back into arr[left..right]
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k++] = L[i++];
        } else {
            arr[k++] = R[j++];
        }
    }

    // Copy remaining elements
    while (i < n1)
        arr[k++] = L[i++];
    while (j < n2)
        arr[k++] = R[j++];
}

void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        // Sort first and second halves
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        // Merge the sorted halves
        merge(arr, left, mid, right);
    }
}
```

---

## Quick Sort vs Merge Sort Comparison

- **Quick Sort**:
  - Average Time Complexity: \( O(n \log n) \)
  - Worst Time Complexity: \( O(n^2) \) (when pivot selection is poor)
  - Space Requirement: In-place (requires stack space for recursion)
  - Stability: Unstable (relative order of equal elements may not be preserved)
  - **Pros**:
    - Generally faster due to in-place sorting and cache-friendly
  - **Cons**:
    - Not stable
    - Worst-case performance can be poor without randomization

---

- **Merge Sort**:
  - Time Complexity: \( O(n \log n) \) in all cases
  - Space Requirement: Not in-place (requires additional \( O(n) \) space)
  - Stability: Stable (relative order of equal elements is preserved)
  - **Pros**:
    - Predictable performance
    - Stable sort
  - **Cons**:
    - Requires extra memory
    - Not as cache-friendly as Quick Sort

---

## Radix Sort

### Can We Sort in Linear Time?

- Yes, for specific cases where we can exploit the structure of the input data
- Radix Sort is a non-comparative sorting algorithm with linear time complexity under certain conditions

---

## Radix Sort

### Overview

- **Non-Comparative Sorting Algorithm**
- Sorts numbers by processing individual digits
- Can be applied to integers, strings, or any data that can be represented in a positional numeral system

---

### How It Works

1. **Determine the Number of Digits (w)**: Find the maximum number of digits in the largest number.
2. **Iterate Over Each Digit Place**:
   - Starting from the least significant digit to the most significant digit.
3. **Use a Stable Sorting Algorithm**:
   - Typically Counting Sort is used to sort based on the current digit.

---

## Radix Sort Example

Given an array: [170, 45, 75, 90, 802, 24, 2, 66]

1. **Sorting by Least Significant Digit (Unit Place)**
   - Arrange numbers based on the unit place digit.
2. **Sorting by Next Significant Digit (Ten Place)**
   - Arrange numbers based on the ten place digit.
3. **Continue for All Digits**

After processing all digits, the array is sorted.

---

## Radix Sort Implementation

```cpp
void countingSort(int arr[], int n, int exp) {
    int output[n]; // Output array
    int count[10] = {0};

    // Store count of occurrences
    for (int i = 0; i < n; i++)
        count[(arr[i] / exp) % 10]++;

    // Change count[i] so that count[i] contains actual
    // position of this digit in output[]
    for (int i = 1; i < 10; i++)
        count[i] += count[i - 1];

    // Build the output array
    for (int i = n - 1; i >= 0; i--) {
        int idx = (arr[i] / exp) % 10;
        output[count[idx] - 1] = arr[i];
        count[idx]--;
    }

    // Copy the output array to arr[]
    for (int i = 0; i < n; i++)
        arr[i] = output[i];
}

void radixSort(int arr[], int n) {
    // Find the maximum number to know the number of digits
    int m = getMax(arr, n);

    // Do counting sort for every digit
    for (int exp = 1; m / exp > 0; exp *= 10)
        countingSort(arr, n, exp);
}

int getMax(int arr[], int n) {
    int mx = arr[0];
    for (int i = 1; i < n; i++)
        if (arr[i] > mx)
            mx = arr[i];
    return mx;
}
```

---

## Radix Sort Time Complexity

- **Time Complexity**: \( O(nw) \) where:
  - \( n \) is the number of elements
  - \( w \) is the number of digits in the maximum element
- **Space Complexity**: \( O(n + k) \) where \( k \) is the range of input digits (usually 0-9)
- **Stable Sort**: Yes, because Counting Sort (used internally) is stable
- **In-place**: No, requires additional space for counting arrays

---

## When to Use Radix Sort

- When the range of input data (number of digits) is not significantly larger than the number of elements
- Useful for sorting large numbers of integers or strings of fixed length
- Not suitable when \( w \) (number of digits) is large compared to \( n \)

---

## Bucket Sort

### Overview

- **Non-Comparative Sorting Algorithm**
- Divides the input array into several 'buckets' and then sorts each bucket individually
- Can achieve linear time complexity under certain conditions

---

## Bucket Sort Steps

1. **Create Buckets**:
   - Create an array of empty buckets
2. **Scatter**:
   - Distribute (scatter) the elements into buckets based on a hashing function or some mapping
3. **Sort Individual Buckets**:
   - Sort each non-empty bucket, typically using Insertion Sort or any other efficient sorting algorithm
4. **Gather**:
   - Concatenate all sorted buckets into the original array

---

## Bucket Sort Example

Given an array of floating-point numbers between 0 and 1:

1. **Create 10 buckets for ranges**:
   - $[0, 0.1), [0.1, 0.2), \ldots, [0.9, 1.0)$
2. **Distribute the numbers into buckets based on their value**
3. **Sort each bucket individually**
4. **Concatenate buckets in order**

---

## Bucket Sort Implementation

```cpp
void bucketSort(float arr[], int n) {
    // Create n empty buckets
    std::vector<float> buckets[n];

    // Put array elements into buckets
    for (int i = 0; i < n; i++) {
        int idx = n * arr[i]; // Index in buckets
        buckets[idx].push_back(arr[i]);
    }

    // Sort individual buckets
    for (int i = 0; i < n; i++)
        std::sort(buckets[i].begin(), buckets[i].end());

    // Concatenate all buckets into arr[]
    int index = 0;
    for (int i = 0; i < n; i++)
        for (size_t j = 0; j < buckets[i].size(); j++)
            arr[index++] = buckets[i][j];
}
```

---

## Bucket Sort Time Complexity

- **Average Time Complexity**: \( O(n) \) when:
  - The input data is uniformly distributed over a range
  - The number of buckets is proportional to \( n \)
- **Worst Time Complexity**: \( O(n^2) \) when all elements are placed into the same bucket
- **Space Complexity**: \( O(n + k) \) where \( k \) is the number of buckets
- **Stable Sort**: Depends on the sorting algorithm used for individual buckets
- **In-place**: No, requires additional space for buckets

---

## When to Use Bucket Sort

- Best suited for sorting uniformly distributed data
- When the input is floating-point numbers in a known range
- Not ideal for datasets where data is clustered, leading to uneven distribution in buckets

---

## Comparison of Sorting Algorithms

| Algorithm   | Time Complexity                | Space Complexity | Stable  | In-place |
|-------------|--------------------------------|------------------|---------|----------|
| Quick Sort  | \( O(n \log n) \) avg, \( O(n^2) \) worst | \( O(\log n) \)  | No      | Yes      |
| Merge Sort  | \( O(n \log n) \)              | \( O(n) \)       | Yes     | No       |
| Radix Sort  | \( O(nw) \)                    | \( O(n + k) \)   | Yes     | No       |
| Bucket Sort | \( O(n) \) avg                 | \( O(n + k) \)   | Depends | No       |

---

## Key Takeaways

- **Quick Sort** is fast on average but can degrade to \( O(n^2) \); not stable.
- **Merge Sort** guarantees \( O(n \log n) \) time; stable but uses extra space.
- **Radix Sort** and **Bucket Sort** can sort in linear time under specific conditions.
- **Algorithm Choice** depends on:
  - Data size and distribution
  - Space constraints
  - Stability requirements

---

Thank you!
