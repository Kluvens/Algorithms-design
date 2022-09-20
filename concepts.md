## Inversion
An inversion in an array A is a pair of elements (A[i], A[j]) such that i < j (the first element appears before the second), but A[i] > A[j] (the elements are out of order).

An unsorted array contains some number of inversions, while a sorted array contains 0.
Every sorting algorithm ultimately decreases the number of inversions in the array to 0.

## How we calculate the time complexity of insertion sort in average case
Insertion sort's runtime is Θ(n + #swaps).
Since the number of swaps equals the number of inversions I, the runtime is Θ(n + I).

If the input array is chosen truly at random, the probability that any pair of array elements form an insersion is 50%.
Then there are n(n – 1) / 2 pairs of elements in an n-element array.

Average number of inversions: n(n – 1) / 4 = Θ(n^2)
Average runtime for insertion sort: Θ(n + n^2) = Θ(n^2).
