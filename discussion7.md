---
marp: true
math: mathjax
---

# Disjoint Sets & Heap Sort

---

## Agenda

- Disjoint Sets
  - Introduction and Definitions
  - Union and Find Operations
  - Path Compression and Union by Rank
- Heap Sort
  - Introduction to Heaps
  - Building a Heap
  - Heap Sort Algorithm and Complexity

---

## Disjoint Sets

---

### What are Disjoint Sets?

- **Disjoint Sets**: A collection of sets where no element is shared between sets.
- **Use Cases**:
  - Used to track and manage components in network connectivity problems.
  - Commonly applied in algorithms like **Kruskal's Minimum Spanning Tree**.

---

### Operations on Disjoint Sets

- **Union**: Combines two sets into a single set.
- **Find**: Determines the representative (root) of the set containing a specific element.
  
  Together, these operations enable managing and merging disjoint sets efficiently.

---

### Union-Find Data Structure

- Efficiently handles disjoint sets using:
  - **Union by Rank**: Combines smaller trees under larger trees.
  - **Path Compression**: Flattens the structure, making future `Find` operations faster.

---

### Union by Rank

- **Union by Rank**: When merging two sets, attach the smaller tree under the root of the larger tree.
- This reduces the height of the trees, making operations more efficient.

---

### Path Compression

- **Path Compression**: During a `Find` operation, make nodes point directly to the root.
- This optimizes the structure for future operations, speeding up both `Union` and `Find`.

---

### Union-Find Algorithm Summary

```cpp
int find(int x) {
    if (parent[x] != x) {
        parent[x] = find(parent[x]); // Path compression
    }
    return parent[x];
}

void union(int x, int y) {
    int rootX = find(x);
    int rootY = find(y);
    if (rootX != rootY) {
        if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
    }
}
```

---

## Heap Sort

---

### Introduction to Heaps

- **Heap**: A specialized binary tree that satisfies the heap property:
  - **Max-Heap**: Each parent node is greater than or equal to its children.
  - **Min-Heap**: Each parent node is less than or equal to its children.
- **Heap Sort** uses a Max-Heap to sort elements in descending order.

---

### Steps to Build a Heap

1. **Heapify**: Starting from the bottom non-leaf nodes, adjust each node to maintain the heap property.
2. **Build Max-Heap**: Apply heapify to all nodes to construct a Max-Heap from an unordered array.

---

### Heapify Operation

- **Heapify**: Ensures that a given subtree satisfies the Max-Heap or Min-Heap property.
- Starts from a given node, comparing with its children, and swaps if needed.
- **Complexity**: \( O(\log n) \) for each `heapify` operation.

---

### Heap Sort Algorithm

1. **Build a Max-Heap** from the input array.
2. **Extract Maximum**:
   - Swap the root (maximum element) with the last element in the heap.
   - Reduce heap size by 1 and heapify the root to restore the Max-Heap property.
3. **Repeat** until the heap is empty.

---

### Heap Sort Example

1. **Input Array**: [4, 10, 3, 5, 1]
2. **Build Max-Heap**: [10, 5, 3, 4, 1]
3. **Sort**:
   - Swap 10 and 1, then heapify.
   - Swap 5 and 1, then heapify.
   - Continue until sorted.

---

### Heap Sort Code

```cpp
void heapify(int arr[], int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && arr[left] > arr[largest])
        largest = left;

    if (right < n && arr[right] > arr[largest])
        largest = right;

    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(int arr[], int n) {
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);

    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}
```

---

### Time Complexity of Heap Sort

- **Building the Heap**: \( O(n) \)
- **Heapify Operations**: \( O(\log n) \)
- **Total Complexity**: \( O(n \log n) \)

---

### Advantages of Heap Sort

- **In-Place**: Does not require extra memory.
- **Stable Complexity**: Always \( O(n \log n) \), making it efficient for large data sets.
- **Deterministic**: Provides predictable performance in all cases (no worst-case degradation).

---

### Comparison of Sorting Algorithms

| Algorithm       | Time Complexity (Average) | Space Complexity | Stability |
|-----------------|---------------------------|------------------|-----------|
| Quick Sort      | \( O(n \log n) \)         | \( O(\log n) \) | No        |
| Merge Sort      | \( O(n \log n) \)         | \( O(n) \)      | Yes       |
| **Heap Sort**   | \( O(n \log n) \)         | \( O(1) \)      | No        |

---

## Summary

- **Disjoint Sets**: Efficiently manage components using Union-Find with path compression and union by rank.
- **Heap Sort**: Utilizes a Max-Heap structure for efficient in-place sorting with \( O(n \log n) \) complexity.

---

Thank you!