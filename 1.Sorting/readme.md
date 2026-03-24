# Sorting Algorithms - Complete Guide

## Table of Contents
1. [Selection Sort](#1-selection-sort)
2. [Bubble Sort](#2-bubble-sort)
3. [Insertion Sort](#3-insertion-sort)
4. [Merge Sort](#4-merge-sort)
5. [Quick Sort](#5-quick-sort)

---

## 1. Selection Sort

### Intuition
Selection Sort works by repeatedly finding the minimum element from the unsorted portion of the array and placing it at the beginning. Think of it like sorting cards in your hand - you scan through all cards, pick the smallest one, and place it first, then repeat for the remaining cards.

### Approach
1. Divide the array into two parts: sorted (initially empty) and unsorted
2. Find the minimum element in the unsorted portion
3. Swap it with the first element of the unsorted portion
4. Move the boundary between sorted and unsorted portions one position forward
5. Repeat until the entire array is sorted

### Code
```python
def selection_sort(n, arr):
    # Traverse through all array elements
    for i in range(n):
        # Find the minimum element in remaining unsorted array
        min_index = i
        
        # Compare with each element in unsorted portion
        for j in range(i + 1, n):
            if arr[j] < arr[min_index]:
                min_index = j
        
        # Swap the found minimum element with the first element
        if min_index != i:
            arr[i], arr[min_index] = arr[min_index], arr[i]
    
    return arr

# Example usage
arr = [7, 4, 1, 5, 3]
n = len(arr)
print(selection_sort(n, arr))  # Output: [1, 3, 4, 5, 7]
```

### Time and Space Complexity
- **Time Complexity:** O(n²) - for all cases (best, average, worst)
- **Space Complexity:** O(1) - sorts in place, no extra space needed

### Visual Example
```
Initial Array: [7, 4, 1, 5, 3]

Pass 1: Find min (1), swap with position 0
[7, 4, 1, 5, 3] → [1, 4, 7, 5, 3]
 ↑     ↑

Pass 2: Find min (3), swap with position 1
[1, 4, 7, 5, 3] → [1, 3, 7, 5, 4]
    ↑        ↑

Pass 3: Find min (4), swap with position 2
[1, 3, 7, 5, 4] → [1, 3, 4, 5, 7]
       ↑     ↑

Pass 4: Find min (5), already in correct position
[1, 3, 4, 5, 7] → [1, 3, 4, 5, 7]

Pass 5: Last element is automatically sorted
Final Array: [1, 3, 4, 5, 7]
```

---

## 2. Bubble Sort

### Intuition
Bubble Sort repeatedly steps through the list, compares adjacent elements, and swaps them if they're in the wrong order. The largest elements "bubble up" to their correct position at the end of the array after each pass, like bubbles rising to the surface of water.

### Approach
1. Compare each pair of adjacent elements
2. Swap them if they are in the wrong order
3. After each complete pass, the largest unsorted element reaches its correct position
4. Reduce the range of comparison by one in each subsequent pass
5. Optimize with a flag to detect if any swaps occurred; if no swaps, the array is sorted

### Code
```python
def bubble_sort(n, arr):
    # Traverse through array from end to beginning
    for i in range(n - 1, -1, -1):
        is_swapped = False
        
        # Compare adjacent elements up to the unsorted portion
        for j in range(i):
            # Swap if current element is greater than next
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                is_swapped = True
        
        # If no swaps occurred, array is already sorted
        if not is_swapped:
            break
    
    return arr

# Example usage
arr = [7, 4, 1, 5, 3]
n = len(arr)
print(bubble_sort(n, arr))  # Output: [1, 3, 4, 5, 7]
```

### Time and Space Complexity
- **Time Complexity:** 
  - Best Case: O(n) - when array is already sorted
  - Average Case: O(n²)
  - Worst Case: O(n²)
- **Space Complexity:** O(1) - sorts in place

### Visual Example
```
Initial Array: [7, 4, 1, 5, 3]

Pass 1: Bubble up the largest (7)
[7, 4, 1, 5, 3]
[4, 7, 1, 5, 3] → swap 7 and 4
[4, 1, 7, 5, 3] → swap 7 and 1
[4, 1, 5, 7, 3] → swap 7 and 5
[4, 1, 5, 3, 7] → swap 7 and 3
                   [7 is now in correct position]

Pass 2: Bubble up the next largest (5)
[4, 1, 5, 3, |7|]
[1, 4, 5, 3, |7|] → swap 4 and 1
[1, 4, 5, 3, |7|] → no swap
[1, 4, 3, 5, |7|] → swap 5 and 3
                     [5 is now in correct position]

Pass 3: Continue bubbling
[1, 4, 3, |5, 7|]
[1, 3, 4, |5, 7|] → swap 4 and 3

Pass 4: Array is sorted
[1, 3, |4, 5, 7|] → no swaps, break early

Final Array: [1, 3, 4, 5, 7]
```

---

## 3. Insertion Sort

### Intuition
Insertion Sort builds the final sorted array one element at a time. It's similar to how you sort playing cards in your hands - you pick one card and insert it into its correct position among the already sorted cards.

### Approach
1. Start with the second element (consider first element as sorted)
2. Compare the current element (key) with elements in the sorted portion
3. Shift all larger elements one position to the right
4. Insert the key at its correct position
5. Repeat for all elements

### Code
```python
def insertion_sort(n, arr):
    # Start from second element (index 1)
    for i in range(1, n):
        key = arr[i]  # Element to be inserted
        j = i - 1     # Index of last element in sorted portion
        
        # Shift elements greater than key to the right
        while j >= 0 and key < arr[j]:
            arr[j + 1] = arr[j]
            j -= 1
        
        # Insert key at the correct position
        arr[j + 1] = key
    
    return arr

# Example usage
arr = [7, 4, 1, 5, 3]
n = len(arr)
print(insertion_sort(n, arr))  # Output: [1, 3, 4, 5, 7]
```

### Time and Space Complexity
- **Time Complexity:** 
  - Best Case: O(n) - when array is already sorted
  - Average Case: O(n²)
  - Worst Case: O(n²)
- **Space Complexity:** O(1) - sorts in place

### Visual Example
```
Initial Array: [7, 4, 1, 5, 3]
               [sorted | unsorted]

Pass 1: Insert 4 into sorted portion
[7 | 4, 1, 5, 3]
[4, 7 | 1, 5, 3]  → 4 inserted before 7

Pass 2: Insert 1 into sorted portion
[4, 7 | 1, 5, 3]
[1, 4, 7 | 5, 3]  → 1 inserted at beginning

Pass 3: Insert 5 into sorted portion
[1, 4, 7 | 5, 3]
[1, 4, 5, 7 | 3]  → 5 inserted between 4 and 7

Pass 4: Insert 3 into sorted portion
[1, 4, 5, 7 | 3]
[1, 3, 4, 5, 7]   → 3 inserted between 1 and 4

Final Array: [1, 3, 4, 5, 7]
```

---

## 4. Merge Sort

### Intuition
Merge Sort uses the divide-and-conquer strategy. It divides the array into two halves, recursively sorts them, and then merges the two sorted halves. Think of it like sorting two piles of papers separately and then combining them into one sorted pile.

### Approach
1. **Divide:** Split the array into two halves recursively until each subarray has one element
2. **Conquer:** Single element arrays are already sorted
3. **Merge:** Combine two sorted subarrays into a single sorted array
4. Repeat the merge process up the recursion tree

### Code
```python
def merge(arr, low, mid, high):
    """Merge two sorted subarrays"""
    left = low         # Starting index of left subarray
    right = mid + 1    # Starting index of right subarray
    temp = []          # Temporary array to store merged result
    
    # Compare elements from both subarrays and add smaller one to temp
    while left <= mid and right <= high:
        if arr[left] < arr[right]:
            temp.append(arr[left])
            left += 1
        else:
            temp.append(arr[right])
            right += 1
    
    # Copy remaining elements from left subarray (if any)
    while left <= mid:
        temp.append(arr[left])
        left += 1
    
    # Copy remaining elements from right subarray (if any)
    while right <= high:
        temp.append(arr[right])
        right += 1
    
    # Copy sorted elements back to original array
    for i in range(low, high + 1):
        arr[i] = temp[i - low]


def merge_sort_helper(arr, low, high):
    """Recursively divide and sort the array"""
    # Base case: single element or invalid range
    if low >= high:
        return
    
    # Find middle point to divide array into two halves
    mid = (low + high) // 2
    
    # Recursively sort left half
    merge_sort_helper(arr, low, mid)
    
    # Recursively sort right half
    merge_sort_helper(arr, mid + 1, high)
    
    # Merge the two sorted halves
    merge(arr, low, mid, high)


def merge_sort(n, arr):
    """Main function to sort array"""
    low = 0
    high = n - 1
    merge_sort_helper(arr, low, high)
    return arr

# Example usage
arr = [7, 4, 1, 5, 3]
n = len(arr)
print(merge_sort(n, arr))  # Output: [1, 3, 4, 5, 7]
```

### Time and Space Complexity
- **Time Complexity:** O(n log n) - for all cases (best, average, worst)
- **Space Complexity:** O(n) - requires extra space for temporary array

### Visual Example
```
Initial Array: [7, 4, 1, 5, 3]

DIVIDE PHASE:
                    [7, 4, 1, 5, 3]
                    /             \
              [7, 4, 1]           [5, 3]
              /       \           /     \
          [7, 4]      [1]      [5]     [3]
          /    \
        [7]    [4]

CONQUER & MERGE PHASE:
        [7]    [4]
          \    /
          [4, 7]      [1]
              \       /
              [1, 4, 7]           [5]     [3]
                                   \     /
                                   [3, 5]
                    \             /
                    [1, 3, 4, 5, 7]

Step-by-step merge:
[7] and [4] → [4, 7]
[4, 7] and [1] → [1, 4, 7]
[5] and [3] → [3, 5]
[1, 4, 7] and [3, 5] → [1, 3, 4, 5, 7]

Final Array: [1, 3, 4, 5, 7]
```

---

## 5. Quick Sort

### Intuition
Quick Sort picks a pivot element and partitions the array around it, placing smaller elements to the left and larger elements to the right. It then recursively sorts the left and right partitions. Think of it like organizing books on a shelf by picking one book as reference and arranging others relative to it.

### Approach
1. Choose a pivot element (typically first, last, or middle element)
2. **Partition:** Rearrange array so elements smaller than pivot are on left, larger on right
3. Recursively apply the same process to left and right partitions
4. Base case: subarrays with one or zero elements are already sorted

### Code
```python
def partition(arr, low, high):
    """Partition array around pivot and return pivot index"""
    pivot = arr[low]  # Choose first element as pivot
    left = low        # Left pointer starts at low
    right = high      # Right pointer starts at high
    
    while left < right:
        # Move left pointer until we find element greater than pivot
        while arr[left] <= pivot and left <= high - 1:
            left += 1
        
        # Move right pointer until we find element smaller than pivot
        while arr[right] > pivot and right >= low + 1:
            right -= 1
        
        # Swap elements at left and right pointers if they haven't crossed
        if left < right:
            arr[left], arr[right] = arr[right], arr[left]
    
    # Place pivot in its correct position
    arr[low], arr[right] = arr[right], arr[low]
    return right  # Return pivot index


def quick_sort_helper(arr, low, high):
    """Recursively sort array using quick sort"""
    # Base case: single element or invalid range
    if low >= high:
        return
    
    # Partition array and get pivot index
    pivot_index = partition(arr, low, high)
    
    # Recursively sort left partition
    quick_sort_helper(arr, low, pivot_index - 1)
    
    # Recursively sort right partition
    quick_sort_helper(arr, pivot_index + 1, high)


def quick_sort(n, arr):
    """Main function to sort array"""
    low = 0
    high = n - 1
    quick_sort_helper(arr, low, high)
    return arr

# Example usage
arr = [7, 4, 1, 5, 3]
n = len(arr)
print(quick_sort(n, arr))  # Output: [1, 3, 4, 5, 7]
```

### Time and Space Complexity
- **Time Complexity:** 
  - Best Case: O(n log n) - when pivot divides array evenly
  - Average Case: O(n log n)
  - Worst Case: O(n²) - when array is already sorted or pivot is always smallest/largest
- **Space Complexity:** O(log n) - recursion stack space

### Visual Example
```
Initial Array: [7, 4, 1, 5, 3]

PARTITION 1: Pivot = 7
[7, 4, 1, 5, 3]
 ↑           ↑
pivot      right

left scans: 7 (stop, > pivot? no, continue)
right scans: 3 (stop, ≤ pivot? yes)

After positioning: [3, 4, 1, 5, 7]
                    [< 7]     7

PARTITION 2: Left side [3, 4, 1, 5], Pivot = 3
[3, 4, 1, 5]
 ↑     ↑
pivot right

After positioning: [1, 3, 4, 5]
                    1 [3] [> 3]

PARTITION 3: Left side [1], already sorted

PARTITION 4: Right side [4, 5], Pivot = 4
[4, 5]
 ↑ ↑
After positioning: [4, 5]

Full recursion tree:
                    [7, 4, 1, 5, 3]
                           ↓ (pivot=7)
                    [3, 4, 1, 5] | 7
                      ↓ (pivot=3)
                1 | 3 | [4, 5]
                          ↓ (pivot=4)
                    1 | 3 | 4 | 5 | 7

Final Array: [1, 3, 4, 5, 7]
```

---

## Comparison Summary

| Algorithm | Best Time | Average Time | Worst Time | Space | Stable | In-Place |
|-----------|-----------|--------------|------------|-------|--------|----------|
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No | Yes |
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes | Yes |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes | Yes |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes | No |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No | Yes |

### When to Use Each Algorithm

- **Selection Sort:** Educational purposes, small datasets
- **Bubble Sort:** Nearly sorted data, educational purposes
- **Insertion Sort:** Small datasets, nearly sorted data, online sorting
- **Merge Sort:** When stability is required, guaranteed O(n log n) performance
- **Quick Sort:** General purpose sorting, average case performance is excellent
