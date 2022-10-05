## Divide and Conquer
Divide and conquer refers to a class of algorithmic techniques in which one breaks the input into several parts, solves the problem in each part recursively, and then combines the solutions to these subproblems into an overall solution.

Analyzing the running time of a divide and conquer algorithm generally involves solving a recurrence relation that bounds  the running time recursively in terms of the running time on smaller instances.

The divide and conquer is usually used to improve on brute-force search in solving a problem.

### A first recurrence - The merge sort algorithm
The Python source code for merge sort is written in the sorting algorithms part.

Mergesort sorts a given list of numbers by first dividing them into two equal halves, sorting each half separately by recursion, and then combining the results of these recursive calls.

The divide and conquer technique can be abstracted as follows:
Divide the input into two pieces of equal size;
solve the two subproblems on these pieces separately by recursion;
and then combine the two results into an overall solution,
spending only linear time for the initial division and final recombining.

In any algorithm that fits this style, we need a base case for the recursion, typically having it on inputs of some constant size.

In the case of Mergesort, when the input size has been reduced to 2, we stop the recursion and sort the two elements by simply comparing them to each other.

Consider any algorithm that fits the pattern in the above abstraction, and let `T(n)` denote its worst-case running time on input instances of size n.
Supposing that n is even, the algorithm spends O(n) time to divide the input into two pieces of size n/2 each;
it then spends time T(n/2) to solve each one;
and finally it spends O(n) time to combine the solutions from the two recursive calls.
Thus, the running time T(n) satisfies the following recurrence relation.

For some constant c, `T(n) <= 2*T(n/2) + cn` when n > 2, and T(2) <= c.

The above structure is typical of what recurrences will look like:
there's an inequality or equation that bounds T(n) in terms of an expression involving T(k) for smaller values k;
and there's a base case that generally says that T(n) is equal to a constant when n is a constant.
This structure can also be written informally as `T(n) <= 2T(n/2) + O(n)`.

However, we have assumed that n is even.
There's a more general format that `T(n) <= T(ceiling(n/2)) + T(floor(n/2)) + cn` for n >= 2.

However, this still does not explicitly provide an asymptotic bound on the growth rate of the function T.
To obtain an explicit bound, we need to solve the recurrence relation so that T appears only on the left-hand side of the inequality, not the right-hand side as well.

#### Approaches to Solving Recurrences
*Unrolling the Mergesort Recurrence*

- Analyzing the first few levels:
At the first level of recursion, we have a single problem of size n,
which takes time at most cn plus the time spent in all subsequent recusive calls.
At the next level, we have two problems each of size n/2.
Each of these takes time at most cn/2, for a total of at most cn, again plus the time in subsequent recursive calls.
At the third level, we have four problmes each of size n/4, each taking time at most cn/4, for a total of at most cn.
- Identifying a pattern: At level j of the recursion, the number of subproblems has doubled j times, so there are now a total of 2^j.
Each has correspondingly shrunk in size by a factor of two j times, and so each has size n/c^j, and hence each takes time at most cn/2^j.
Thus level j contributes a total of at most `2^j*(cn/2^j) = cn` to the total running time.
- Summing over all levels of recursion: 
We've found that the recurrence has the property that the same upper bound of cn applies to total amount of work performed at each level.
The number of times the input must be halved in order to reduce its size from n to 2 is `log2 n`.
So summing the cn work over logn levels of recursion, we get a total running time of `o(n logn)`.

![image](https://user-images.githubusercontent.com/95273765/193483440-fa465757-2a15-4b59-8d66-be5d6983b9e4.png)

*Substituting a Solution into the Mergesort Recurrence*

Suppose we believe that `T(n) <= cn*log2` n for all n >= 2, and we want to check whether this is indeed true.
This clearly holds for n = 2, since in this case `cn*log2 n = 2c`,and the recurrence explicitly tells us that `T(2) <= c`.
Now suppose, by induction, that `T(m) <= cm*log2 m` for all values of m less than n, and we want to establish this for T(n).
We do this by writing the recurrence for T(n) and plugging in the inequality `T(n/2) <= c(n/2) log2(n/2)`.
We then simplify the resulting expression by noticing that `log2(n/2) = (log2 n) − 1`.
The full calculation is
```
T(n) ≤ 2T(n/2) + cn
	≤ 2c(n/2) log2(n/2) + cn
	= cn[(log2 n) − 1] + cn
	= (cn log2 n) − cn + cn
	= cn log2 n
```

This establish the bound we want for T(n), assuming it holds for smaller values m < n, and thus it completes the induction argument.

*An Approach Using Partial Substitut*

Specifically, suppose we believe that T(n) = O(n logn), but we're not sure of the constant inside the O(.) notation.
We can use the substitution method even without being sure of this constant, as follows.
We first write `T(n) <= kn logb n` for some constant k and base b that we'll determine later.
Now we'd like to know whether there is any choice of k and b that will work in an inductive argument.
So we try out one level of the induction as follows.
`T(n) ≤ 2T(n/2) + cn ≤ 2k(n/2) logb(n/2) + cn`.
It's now very tempting to choose the base b = 2 for the logarithm, since we see that this will let us apply the simplification `log2(n/2) = (log2 n) − 1`.
Proceeding with this choice, we have 
```
T(n) <= 2k(n/2) log2(n/2) + cn
	= 2k(n/2)[(log2 n) − 1] + cn
	= kn[(log2 n) − 1] + cn
	= (kn log2 n) − kn + cn.
```
Finally, there a choice of k that will cause this last expression to be bounded by `kn*log2 n`.
We need to choose any k that is at least as large as c, and we get
`T(n) <= (kn log2 n) − kn + cn <= kn log2 n`, which completes the induction.

### Further Recurrence Relations
A more general class of algorithms is obtained by considering divide-and-conquer algorithms taht create recursive calls on q subproblems of size n/2 each and then combine the results in O(n) time.

Thus, we can get, for some constant c,
`T(n) ≤ qT(n/2) + cn`
when n > 2, and T(2) ≤ c.

#### The case of q > 2 subproblems
By unrolling:
- Analyzing the first few levels: We show an example of this for the case q = 3 in Figure 5.2. 
At the first level of recursion, we have a single problem of size n, which takes time at most cn plus the time spent in all subsequent recursive calls. 
At the next level, we have q problems, each
of size n/2. Each of these takes time at most cn/2, for a total of at most
(q/2)cn, again plus the time in subsequent recursive calls. 
The next level yields q^2 problems of size n/4 each, for a total time of `(q^2/4)cn`. 
Since q > 2, we see that the total work per level is increasing as we proceed through the recursion.
- Identifying a pattern: At an arbitrary level j, we have q^j distinct instances,
each of size n/2^j. 
Thus the total work performed at level j is `(q^j)*(cn/2j) = ((q/2)^j)*cn`.
- Summing over all levels of recursion:
As before, there are log2 n levels of recursion, and the total amount of work performed is the sum over all these:
![image](https://user-images.githubusercontent.com/95273765/193502130-83e8b398-081a-48c3-8f1d-f2378be70fb7.png)

This is a geometric sum, consisting of powers of `r = q/2`.
We can use the formula for a geometric sum when r > 1, which gives us the formula
![image](https://user-images.githubusercontent.com/95273765/193502267-31ff9ef6-524c-4ab7-9f35-3d9321056cdc.png)

Since we're aiming for an asymptotic upper bound, it is useful to figure out what's simply a constant;
we can pull out the factor of r - 1 fro the denominator, and write the last expression as

![image](https://user-images.githubusercontent.com/95273765/193502398-f7ea85ec-4eb9-41ee-b112-9349358bae3c.png)

Finally, we need to figure out what r^(log2 n) is.
Here we use a very handy identity, which says that, for any a > 1 and b > 1, we have `a^(log b) = b^(log a)`.

Thus

![image](https://user-images.githubusercontent.com/95273765/193502563-bc7a0b33-e34e-49f4-bab9-3f487810f5cf.png)

Thus we have
![image](https://user-images.githubusercontent.com/95273765/193502717-357d8e60-8819-4d7d-9cf8-6fbae7ad11dd.png)

Therefore, any function T(.) satisfying the above structure with q > 2 is bounded by O(n^(log2 q)).

![image](https://user-images.githubusercontent.com/95273765/193501175-26ae1f6e-d63f-4035-b1ab-060c4b44cdf9.png)

#### The Case of One Subproblem
By unrolling the recurrence to try constructing a solution:
- Analyzing the first few levels:We show the first few levels of the recursion
in Figure 5.3. At the first level of recursion, we have a single problem of
size n, which takes time at most cn plus the time spent in all subsequent
recursive calls. The next level has one problem of size n/2, which
contributes cn/2, and the level after that has one problem of size n/4,
which contributes cn/4. Sowe see that, unlike the previous case, the total
work per level when q = 1 is actually decreasing as we proceed through
the recursion.
- Identifying a pattern: At an arbitrary level j, we still have just one
instance; it has size n/2j and contributes cn/2j to the running time.
- Summing over all levels of recursion: There are log2 n levels of recursion,
and the total amount of work performed is the sum over all these:

![image](https://user-images.githubusercontent.com/95273765/193505111-d8b3e2d5-2693-4613-bb0a-ba76309fd5f8.png)

This geometric sum is very easy to work out, it converges to 2.

Thus we have
![image](https://user-images.githubusercontent.com/95273765/193505196-c44c464b-ee85-4b57-a129-9ddb4044a85b.png)

Therefore, any function T(.) satisfying the above structure with q = 1 is bounded by O(n).

### Counting Inversions
We are given a sequence of n numbers a1 ... an;
we will assume that all the numbers are distinct.
We want to define a measure that tells us how far this list is from being in ascending order;
the value of the measure should be 0 if a1 < a2 < ... < an, 
and should increase as the numbers become more scrambled.

A natural way to quantify this notion is by counting the number of inversions.
We say that two indices i < j form an inversion if ai > aj,
that is, if the two elements ai and aj are out of order.
We will seek to determine the number of inversions in the sequence a1 ... an.

The brute force algorithm takes O(n^2) time.

We can be better in O(n logn) time.
The basic idea is to split the input into two pieces.
We first count the number of inversions in each of these two halves separately.
Then we count the nuber of inversions (ai, aj), where the two numbers belong to different halves;
the trick is that we must do this part in O(n) time.
Note that these first-half/second-half inversions have a particularly nice form:
they are precisely the paris (ai, aj), where ai is in the first half, aj is in the second half, and ai < aj.

To help with counting the number of inversions between the two halves,
we will make the algorithm recursively sort the numbers in the two halves as well.

By now, it is clear that the crucial routine in this process is Merge-and-Count.

To summarize, we have the following algorithm:
```
Merge-and-Count(A,B)
	Maintain a Current pointer into each list, initialized to
		point to the front elements
	Maintain a variable Count for the number of inversions,
		initialized to 0
	While both lists are nonempty:
		Let ai and bj be the elements pointed to by the Current pointer
		Append the smaller of these two to the output list
		If bj is the smaller element then
			Increment Count by the number of elements remaining in A
		Endif
		Advance the Current pointer in the list from which the
			smaller element was selected.
	EndWhile
	Once one list is empty, append the remainder of the other list to the output
	Return Count and the merged list
```
```
Sort-and-Count(L)
	If the list has one element then
		there are no inversions
	Else
		Divide the list into two halves:
			A contains the first ceiling(n/2) elements
			B contains the remaining floor(n/2) elements
		(rA, A) = Sort-and-Count(A)
		(rB, B) = Sort-and-Count(B)
		(r , L) = Merge-and-Count(A, B)
	Endif
	Return r = rA + rB + r, and the sorted list L
```

The Sort-and-Count algorithm correctly sorts the input list and counts the number of inversions:
it runs in O(n logn) time for a list with n elements.

### Finding the Closest Pair of Points
Given n points in the plane, find the pair that is closest together.

This can be solved by O(n^2) using brute force, but it can be done faster by O(n logn).

We begin with a bit of notation. Let us denote the set of points by P = {p1, . . . , pn}, 
where pi has coordinates (xi , yi); and for two points pi , pj ∈ P,
we use d(pi , pj) to denote the standard Euclidean distance between them. 
Our goal is to find a pair of points pi , pj that minimizes d(pi , pj).

We can have a warmup by thinking about the one-dimentional version of this problem.
We'd first sort them, in O(n logn) time, and then we'd walk through the sorted list,
computing the distance from each point to the one that comes after it.
It is easy to see that one of these distances must be the minimum one.

Then we will dive into the two-dimentional version, our plan is to apply the style of divide and conquer used in Mergesort:
we find the closest pair among the points in the left half of P and the closest pair among the points in the right half of P;
and then we use this information to get the overall solution in linear time.
Then the solution of our basic recurrence will give us an O(n logn) running time.

First, before any of the recursion begins, we sort the points in P by x-coordinate and again by y-coordinate, producing lists Px and Py.
Attached to each entry in each list is a record of the position of that point in both lists.

Then we define Q to be the set of points in the first ceiling(n/2) positions of the list Px (left half) and R to be the set of points in the final floor(n/2) positions of the list Px (right half).
By a single pass through each of Px and Py in O(n) time, we can create the following four lists:
Qx, consisting of the points in Q sorted by increasing x-coordinate;
Qy, consisting of the points in Q sorted by increasing y-coordinate;
and analogous lists Rx and Ry.
For each entry of each of these lists, as before, we record the position of the point in both lists it belongs to.

We now recursively determine a closest pair of points in Q (with access
to the lists Qx and Qy). Suppose that q∗0 and q∗1 are (correctly) returned as a
closest pair of points in Q. 
Similarly, we determine a closest pair of points in R, obtaining r∗0 and r∗1.

Let δ be the minimum of d(q∗0,q∗1) and d(r∗0 ,r∗1). 
The real question is: Are
there points q ∈ Q and r ∈ R for which d(q, r) < δ? If not, then we have already
found the closest pair in one of our recursive calls. 
But if there are, then the closest such q and r form the closest pair in P.

Let x∗ denote the x-coordinate of the rightmost point in Q, and let L denote
the vertical line described by the equation x = x∗. This line L “separates” Q
from R. 
Here is a simple fact.

If there exists q ∈ Q and r ∈ R for which d(q, r) < δ, then each of q and r lies within a distance δ of L.

So if we want to find a close q and r, we can restrict our search to the narrow band consisting only of points in P within δ of L. 
Let S ⊆ P denote this set, and let Sy denote the list consisting of the points in S sorted by increasing y-coordinate. 
By a single pass through the list Py, we can construct Sy in O(n)
time.

So there exist q ∈ Q and r ∈ R for which d(q, r) < δ if and only if there
exist s, s' ∈ S for which d(s, s') < δ.

Furthermore, If s, s' ∈ S have the property that d(s, s') < δ, then s and s' are within
15 positions of each other in the sorted list Sy.

We notice an important thing that there's an absolute constant.

Consequently, we can conclude that we make one pass through Sy, and for each s ∈ Sy, we compute its distance to each of the next 15 points in Sy.
In doing so, we will have computed the distance of each pair of points in S that are at distance less than δ from each other.
So having done this, we can compare the smallest such distance to δ, and we can report one of two things:
1. the closest pair of points in S, if their distance is less than δ
2. the correct conclusion that no pairs of points in S are within δ of each other

In case 1, this pair is the closest pair in P;
in case 2, the closest pair found by our recursive calls is the closest pair in P.

This concludes the description of the combing part of the algorithm, since
we have now determined whether the minimum distance between a point in Q and a point in R is less than δ, and if so, we have found the closest such pair.

Summary of the Algorithm:
``` 
Closest-Pair(P)
	Construct Px and Py (O(n log n) time)
	(p∗0, p∗1) = Closest-Pair-Rec(Px,Py)
	
Closest-Pair-Rec(Px, Py)
	If |P| ≤ 3 then
		find closest pair by measuring all pairwise distances
	Endif
	
	Construct Qx, Qy, Rx, Ry (O(n) time)
	(q∗0,q∗1) = Closest-Pair-Rec(Qx, Qy)
	(r∗0,r∗1) = Closest-Pair-Rec(Rx, Ry)
	
	δ = min(d(q∗0,q∗1), d(r∗0,r∗1))
	x∗ = maximum x-coordinate of a point in set Q
	L = {(x,y) : x = x∗}
	S = points in P within distance δ of L.

	Construct Sy (O(n) time)
	For each point s ∈ Sy, compute distance from s
		to each of next 15 points in Sy
		Let s, s' be pair achieving minimum of these distances
		(O(n) time)

	If d(s,s') < δ then
		Return (s,s')
	Else if d(q∗0,q∗1) < d(r∗0,r∗1) then
		Return (q∗0,q∗1)
	Else
		Return (r∗0,r∗1)
	Endif
```

### Finding the k-th smallest element in an unordered list
This applies QuickSelect, which is a selection algorithm uses divide-and-conquer and it's also a recursive algorithm.
The QuickSelect algorithm is related to the quick sort sorting algorithm.

However, the difference to quick sort is,
instead of recurring for both sides (after finding pivot),
it recurs only for the part that contains the k-th smallest element.

The logic is also simple, if index of the partitioned element is more than k,
then we recur for the left part.
If index is the same as k,
we have found the k-th smallest element and we return.
If index is less than k, then we recur for the right part.
This reduces the expected complexity from O(n log n) to O(n), with a worst-case of O(n^2).

The partition here is to randomly pick up a number and reorder the list so that the numbers to the left are all smaller and the numbers to the right are all larger.
Then we only need to think about the smaller part or the larger part.

A code example by Python to find the k-th smallest element in an unsorted list:
``` python
def partition(arr, l, r):

    x = arr[r]
    i = l
    for j in range(l, r):

        if arr[j] <= x:
            arr[i], arr[j] = arr[j], arr[i]
            i += 1

    arr[i], arr[r] = arr[r], arr[i]
    return i
  
def kthSmallest(arr, l, r, k):

    if (k > 0 and k <= r - l + 1):

        index = partition(arr, l, r)

        if (index - l == k - 1):
            return arr[index]

        if (index - l > k - 1):
            return kthSmallest(arr, l, index - 1, k)

        return kthSmallest(arr, index + 1, r,
                            k - index + l - 1)
    print("Index out of bound")
```

### Count the number of subsets whose sum is within a range

A code example by Python to count the number of subsets whose sum is within a range:
``` python
def countRangeSum(self, nums: List[int], lower: int, upper: int) -> int:
	res = 0
	presum = 0
	sorted_presum = [0]
	for k in range(len(nums)):
		presum += nums[k]
		idx_c = bisect_right(sorted_presum, presum)
		idx_l = bisect_left(sorted_presum, presum - upper)
		idx_r = bisect_right(sorted_presum, presum - lower)
		res += idx_r - idx_l

		sorted_presum[idx_c:idx_c] = [presum]

	return res
```

My own solution is as follows, this is faster than the previous one:
``` python
class Solution:
    def countRangeSum(self, nums: List[int], lower: int, upper: int) -> int:
        # First get the cumulative list
        # takes O(n)
        accumulated = nums.copy()
        for i in range(0, len(nums)):
            if i != 0:
                accumulated[i] = nums[i] + accumulated[i-1]

        # sort the cumulative list
        # we do this because it's easy for later operations
        # takes O(n logn)
        new_acc = sorted(accumulated)

        result = 0
        num = 0

        # takes O(n)        
        for i in range(0, len(nums)):

            # get how many subarrays are within the bound
            # inside the loop, takes O(logn)
            l = bisect_left(new_acc, lower)
            r = bisect_right(new_acc, upper)
            diff = r - l

            result += diff
            lower += nums[num]
            upper += nums[num]
            poped = bisect_left(new_acc, accumulated[num])
            new_acc.pop(poped)
            num += 1

        # overall, takes O(n logn)
        return result
```
