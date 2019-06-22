---
layout: post
title: Array Pivot
---

Given an array A and an pivot index `i`, arrange the array such that all elements less than A[i] is on the left of A[i] and all elements greater than A[i] is on the right of A[i].

At first look, this problem is very similar to the quicksort algorithm, i.e. the partition portion of quicksort. An naive partitioning scheme would result in on O(n<sup>2 </sup>) run time. A better approach is to sort the array; but an efficient sorting algorithm will result in O(nlg(n)) run time. We can do better than that by implementing an O(n) algorithm.

Let have a solid example, let A={ 3,2,1,1,3,2 } and the Pivot index is A[2]=1. We maintain three pointers: lesser, equal, and greater. Each is initialized as followed:

{% highlight cpp %}
auto lesser = A.begin()
auto equal = A.begin()
auto greater = A.rbegin()
{% endhighlight %}

In order to maintain the order, the following condition must be true.

{% highlight cpp %}
lesser <= equal < greater.
{% endhighlight %}

This means that we can't advance lesser pass equal index, nor we can advance equal pass greater index.

The position of each element in A is unverified until the equal pointer reaches it and compare it to the pivot value. We can can say that A={?,?,?,?,?,?} at the begining of the algorithm. There are three conditions we can check:

1. *equal == A[pivot index]: we don't have to do anything here, and std::advance(equal,1) to the next element in A.
2. *equal < A[pivot index]: in this case we swap *equal and *lesser, then advance both equal and lesser by 1.
3. *equal > A[pivot index]: we swap *equal and *greater, then decrement greater, and **do not** move equal

Here is a code example:

{% highlight cpp %}
if (*equal == pivot_value){
    std::advance(equal,1);
}
else if(*equal < pivot_value){
    std::swap(*equal,*lesser);
    std::advance(equal,1);
    std::advance(lesser,1);
}
else{// *equal > pivot_value
    std::swap(*equal,*greater);
    std::advance(greater,1);
}
{% endhighlight %}

We iterate until equal reaches greater than we stop:

{% highlight cpp %}
while(equal != greater.base()){
    if (*equal == pivot_value){
        std::advance(equal,1);
    }
    else if(*equal < pivot_value){
        std::swap(*equal,*lesser);
        std::advance(equal,1);
        std::advance(lesser,1);
    }
    else{// *equal > pivot_value
        std::swap(*equal,*greater);
        std::advance(greater,1);
    }
}
{% endhighlight %}

Here is the whole function:

{% highlight cpp %}
template<class Container>
void ArrayPivot(Container & A, size_t pivot_index){
    auto pivot_value = A.at(pivot_index);

    auto lesser = A.begin()
    auto equal = A.begin()
    auto greater = A.rbegin()

    while(equal != greater.base()){
        if (*equal == pivot_value){
            std::advance(equal,1);
        }
        else if(*equal < pivot_value){
            std::swap(*equal,*lesser);
            std::advance(equal,1);
            std::advance(lesser,1);
        }
        else{// *equal > pivot_value
            std::swap(*equal,*greater);
            std::advance(greater,1);
        }
    }
}
{% endhighlight %}

Since this is a weaker condition than a quicksort algorithm, the resulting array is not sorted. From our example above, the algorithm yields A={1,1,2,3,2,3}.
