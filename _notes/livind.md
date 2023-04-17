---
title: "Mathematical induction and loop in variant"
---
from [[algorithm]]

## Mathematical induction
### Step of Mathematical induction
1. divide steps
2. prove the first step
3. prove the next step

## loop in variant
loop in variant stands for condition which specifies whether result in the middle of a process is on the right way or not in every loop. Invariant proves justification of loop with three steps.
1. prove the invariant is true entering the loop statement
2. prove the invariant is true when we reached the bottom of loop statement
3. prove the invariant is valid when the loop statement is ended

``` C++
int binsearch(const vector<int>& A, int x)
{
    int n = A.size();
    int lo = -1, hi = n;
    while(lo+1<hi)      // invariant
    // invariant 2 - A[lo]< x <= A[hi]
    {
        int mid = (lo+hi)/2;
        if(A[mid] < x)
            lo = mid;
        else
            hi = mid;
    }
    return hi;
}
```

### Insertion sort and loop in variant
``` C++
void insertionSort(vector<int>& A)
{
    for(int i = 0 ; i < A.size() ; i++)
    {
        // invariant : A[0, i-1] is sorted
        int j = i;
        while(j>0 && A[j-1] > A[j])
        {
            // invariant 2 : A[j] > A[j+1, i]
            // invariant 3 : A[0, i] is sorted except A[j]
            swap(A[j-1], A[j]);
            j--;
        }
    }
}
```
<hr>

## Reduction to absurdity
> finding counter example

## Pigeon hole Principle
> The pigeonhole principle states that if n items are put into m containers, with n > m, then at least one container must contain more than one item.

## finding circulating decimals

``` C++
void printDecimal(int a, int b)
{
    int iter = 0;
    while(a > 0)
    {
        if(iter++ == 1)     cout << ".";
        cout<< a / b;
        a = (a % b) * 10;
    }
}
```