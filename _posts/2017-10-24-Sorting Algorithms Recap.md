---
layout: post
title: A Recap on Sorting Algorithms

---

<head>
    <script type="text/javascript"
            src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
    </script>
</head>

[This post](https://www.reddit.com/r/dataisbeautiful/comments/78fywy/sorting_algorithms_visualized_oc/) showed up on /r/dataisbeautiful today.  
I thought it was really interesting, and thought it would be a good reminder to review sorting algorithms.

For those of you with a CS background, this should be extremely rudimentary.  This is solely meant to be a basic overview and a better sense of intuition about sorting.

### A crash course on big-O notation

When analyzing performance of algorithms and programs, they are typically measured in what is called "Big O Notation".

It is difficult to consistently measure the runtime of an algorithm in terms of actual time, since it can vary greatly due to factors such as:
- machine specifications
- memory allocation
- random components

When using Big O, we measure how an algorithm's runtime grows as its input size gets bigger and bigger.  It gives a fairly good approximation of how an algorithm's performance scales for larger input size.

Big O can also be used to describe how much memory a particular function will use, utilizing the same logic.  Though there is frequently a tradeoff between making an algorithm faster and making it use less memory.

Let's pretend that we have a function which tells us the exact number of operations that a specific algorithm requires to run:  

\begin{equation}
T(n) = 6n^2 + 2n + 1  
\end{equation}

With Big O, we only care about the highest order (fastest growing term), and ignore any constant scaling factors in front of that term.  So the Big O performance for the algorithm above would be $$O(n^2)$$.

[Here's](http://web.mit.edu/16.070/www/lecture/big_o.pdf) a more formal introduction to Big O.

Because humans are impatient, we generally try to make programs as fast and as compact as possible.  As such, $$O(n)$$ is better than $$O(n^2)$$, $$O(n \log_2 n)$$ is better than $$O(2^n)$$, and so on.

### Sorting algorithms

Sorting is a fundamental and crucial concept in CS, as there are countless problems that require sorting.  Certain operations are more efficient for a sorted list (ex: search).

A lot of naive sorting algorithms can get $$O(n^2)$$ behavior.  This usually happens because every element ends up being compared to every other element in the array.

It is important to note that is no silver bullet algorithm for sorting.  Different algorithms are better for different use cases and constraints.  Sometimes an $$O(n^2)$$ algorithm like Bubble Sort might actually be the best for a certain problem.

#### The big 3

These are 3 core sorting algorithms that can achieve $$O(n \log_2 n)$$ behavior:
- Quicksort  
- Mergesort  
- Heapsort

These operate with the realization that some comparisons aren't necessary.

Ex: We know $$ 1 < 5 $$ and $$ 5 < 9 $$ .  We don't need to compare 1 and 9 to know that 9 is bigger than 1.

[Here's](http://www.inf.fh-flensburg.de/lang/algorithmen/sortieren/lowerbounden.htm) a proof that you can't do better than $$ O(n \log_2 n) $$ for comparison-based sorting algorithms.

##### Quicksort

Of the 3 $$ O(n \log_2 n) $$ sorting algorithms, this is probably my favorite, as well as the easiest to explain.

Pseudocode:
- 1: Define a pivot value in the array  
- 2: Go through each element of the array, comparing values to the pivot. Rearrange so that smaller values are to the left of the pivot and larger values are to the right.  
- 3: Apply 1 & 2 recursively to the two halves of the array

Here's my basic Python implementation of Quicksort.  

```python  
import random

def QSort(A):
    if len(A) <= 1:
        return A
    partition_val = random.randint(0,len(A)-1)
    A = partition(A, partition_val)
    return QSort(A[:partition_val]) + [A[partition_val]] + QSort(A[partition_val+1:])

def partition(A, pivot):
    i = 0 #index values
    j = len(A)-1
    Done = False
    while not Done:
        while i <= j and A[i] <= A[pivot]:
            i += 1
        while j >= i and A[j] > A[pivot]:
            j -= 1
        if j < i:
            Done = True
        else:
            dummy = A[i]
            A[i] = A[j]
            A[j] = dummy
    dummy = A[j]
    A[j] = A[pivot]
    A[pivot] = A[j]
    return A
```

Depending on how the pivot is chosen, the worst-case behavior is technically $$O(n^2)$$.  This would require repeatedly choosing the worst possible pivot value at different steps of recursion.  With a randomized pivot (like that used above), this rarely happens.

As such, the expected performance of Quicksort is $$O(n \log_2 n)$$.

Also, someone in my cohort shored me [this video](https://www.youtube.com/watch?v=ywWBy6J5gz8) demonstrating Quicksort.  I thought it was hilariously educational.

##### Heapsort

Pseudocode:
- 1: Heapify: Convert (rest of) the array into a [heap](https://en.wikipedia.org/wiki/Heap_(data_structure))
- 2: Remove the root element from the heap
- 3: Iterate through the array repeating steps 1 and 2 until the heap is empty

Here's a Python implementation I found ([credit](http://www.geeksforgeeks.org/heap-sort/)):

```python
def heapify(arr, n, i):
    largest = i
    l = 2 * i + 1
    r = 2 * i + 2

    if l < n and arr[i] < arr[l]:
        largest = l

    if r < n and arr[largest] < arr[r]:
        largest = r

    if largest != i:
        arr[i],arr[largest] = arr[largest],arr[i]
        heapify(arr, n, largest)

def heapSort(arr):
    n = len(arr)

    for i in range(n, -1, -1):
        heapify(arr, n, i)

    for i in range(n-1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]
        heapify(arr, i, 0)
```

##### Mergesort

Pseudocode:
- 1: Define a merge function, which combines two sorted arrays and returns a larger sorted array
- 2: Split the array in half until we have single elements
- 3: Recursively merge the pieces until there are no more pieces to merge

Here's a quick implementation I found ([credit](https://gist.github.com/jvashishtha/2720700))

```python
def merge(left, right):
	if not len(left) or not len(right):
		return left or right

	result = []
	i, j = 0, 0
	while (len(result) < len(left) + len(right)):
		if left[i] < right[j]:
			result.append(left[i])
			i+= 1
		else:
			result.append(right[j])
			j+= 1
		if i == len(left) or j == len(right):
			result.extend(left[i:] or right[j:])
			break

	return result

def mergesort(list):
	if len(list) < 2:
		return list

	middle = len(list)/2
	left = mergesort(list[:middle])
	right = mergesort(list[middle:])

	return merge(left, right)
```

##### Timsort

[Timsort](https://en.wikipedia.org/wiki/Timsort) is a variant of Mergesort.  This algorithm has the additional property of being adaptive (the closer an array is to being sorted, the better the performance).  As such, it is the standard sorting algorithm used by languages including Python and Java.
