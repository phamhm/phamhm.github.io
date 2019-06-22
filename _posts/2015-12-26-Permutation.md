#Problem 1#

Given an array A and a permutation P, apply P to A.

Example: 

* A = {å, ∫, ç, ∂}
* P = <2, 0, 1, 3>

The simplest solution is to create an empty array B, ie B = {,,,}, then iterate through P to fill out B:

{% highlight c++ %}
vector<typenameT> B(P.size(), 0);
// applying the permutation
for (i = 0; i < P.size(); ++i){
    B[P[i]] = A[i];

// copy B into A
for (i = 0; i < P.size(); ++i){
    A[i] = B[i];
}
{% endhighlight %}

The method above will yield B = {∫, ç, å, ∂}. This method runs in O(n) time and also require O(n) space. A better approach is to swap the elements of A directly without having to create another array B.

Let *i* be the index of P that we're permuting, P<sup>n</sup>=P...P[P[i]] for n<=N must be cyclic since 0<=P[i]<=N. We keep swappnig A[i] with A[P<sup>n</sup>[i]] until P<sup>n</sup>[i] terminates. P<sup>n</sup>[i] terminates when it complete a cycle. For each P<sup>n</sup>[i] that we have used, we can mark it with a negative number since all P[i] are indexes: *P[i] -= P.size()* will bring the value of P[i] negative.

Here is the sample code that employ O(1) space and O(n) time.

{% highlight c++ %}
template<typename T1, typename T2>
void Permute(vector<T1> & P, vector<T2> & A){
    for (size_t i = 0; i < P.size(); ++i){
        auto p_index = i;

        while(P[p_index] >= 0){
            std::swap(A[i], A[P[p_index]]);

            auto tmp = p_index;
            p_index = P[p_index];
            P[tmp] -= P.size();
        }
    }
    // restoring the value for P
    std::for_each(P.begin(), P.end(), 
        [&](int & x){x+= P.size();});
}
{% endhighlight %}

#Problem 2#

Given a Permutation array, find the next permutation

##Introducing the dictionary ordering##
Given two permutations *p* and *q*, *p* is said to be appearing before *q* iff the very first index **i** where *p[i]* is different than *q[i]*, *p[i]<q[i]*.

For example, *p=<2,0,1>* and *q=<2,1,0>*. At *i=1*, *p[i]<q[i]*, thus *p<q* in dictionary ordering. The smallest permutation is <0,1,2> and the largest is <2,1,0> in dictionary ordering.

##Problem reformulation##

With dictionary ordering, the brute force approach is generate all the ordering permutation arrays. Brute force require O(n!) space and time complexities; there is not enough time in the universe to solve even a small problem.

The algorithm works as followed:

Let P = <p<sub>1</sub>,p<sub>2</sub>,...,p<sub>k</sub>,...,p<sub>n</sub>> be the current permutation array. 

1. Find *k* index such that <p<sub>k</sub>,...,p<sub>n</sub>> be an decreasing subarray of P.

2. Find the smallest element p<sub>s</sub> for s in [k,n] s.t. p<sub>k-1</sub> <p<sub>s</sub>.

3. Swap p<sub>k-1</sub> and p<sub>s</sub>

4. Reverse the subarray <p<sub>k</sub>,...,p<sub>n</sub>>.

Here is the code:

{% highlight c++ %}
void nextPermute(vector<int> & P){
  if (P.begin() == P.end())
    return;

  // find the decreasing subsequence
  // Since using reverse iterator, search the increasing subsequence
  auto tmp = std::adjacent_find(P.rbegin(),
                                P.rend(),
                                std::greater<int>());

  auto k = std::prev(tmp.base());

  if (k == P.begin())
    return;// if the subarary == array

  auto s = std::prev(k);

  // In the subseq [k,end) find the elemtn that is less than *s
  auto swap_target = std::find_if(k, P.end(),
                          [&](int x){return x < *s;});

  // no element in [k, end) lesser than *s
  if (swap_target == P.end())
    swap_target = std::prev(P.end());
  else
    std::advance(swap_target,-1);

  std::swap(*s, *swap_target);

  reverse_interval(k, P.end());
}

{% endhighlight %}
