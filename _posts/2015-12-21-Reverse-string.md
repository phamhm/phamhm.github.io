You are given an array of words, and you have to reverse position of the words. For example:"this is my string." -> "string. my is this"

The solution for this problem is to reverse the whole array to get ".gnirts ym si siht". Then, we reverse each substring ".gnirts" into "string."


##How do we reverse a string?## 
First we maintain two pointers, the first one point to the beginning of the string, and second one point to the end of the string. After, swapping their values, we increment the first and decrement the second. We stop when they either cross each other or become the same string.

{% highlight cpp %}
void reverse_interval(typename std::string::iterator first,
                    typename std::string::iterator last){
    // nothing to do here
    if (first == last)
        return;

    // we use the convention [first,last)
    // so the first value is located at std::prev(last)
    last = std::prev(last);

    // as long as first and last do not cross, we keep swapping
    while(std::distance(first,last)>0){
        std::swap(*first, *last);
        std::advance(first,1);
        std::advance(last,-1);
    }
}

void reverse(std::string & str){
    // reverse the entire string
    reverse_interval(str.begin(),str.end());
    ...
}
{% endhighlight %}

After reverse the entire string, now we can find the substring to reverse; each substring is delimited by space.


{% highlight cpp %}
auto beg = str.begin();
auto end = std::find(str.begin(),
                        str.end(),' ');
{% endhighlight %}

We will iterate through all the substrings as long as the distance between the two iterators are positive. The distance has different values iff:

1. The number of characters is odd, then *beg==end* when they reach the middle character.
2. The number of characters is even, then *end<beg* and distance(beg,end)<0.

{% highlight cpp %}
while(std::distance(beg,end)>0){
    reverse_interval(beg, end);
    ...
}
{% endhighlight %}

When we reach the last substring, *last* will point at *str.end()*. Thus, after reversing that substring, we do not want to set *beg* to *std::next(str.end())*; instead, we have to set *beg=end* and our loop will terminate. Here is how we set *beg* and *end* to mark the substring:

{% highlight cpp %}
if (end != str.end()){
    beg = std::next(end,1);
    end = std::find(beg,str.end(),' ');
}
else
    beg=end;
{% endhighlight %}

Here is the whole *reverse* function:

{% highlight cpp %}
void reverse(std::string & str){
    reverse_interval(str.begin(),str.end());
    std::cout << str << std::endl;

    str = trim(str);//removine trailing/leading spaces

    auto beg = str.begin();
    auto end = std::find(str.begin(),
                        str.end(),' ');

    while(std::distance(beg,end) > 0){
        reverse_interval(beg, end);

        if (end != str.end()){
        // finding the next substring
            beg = std::next(end,1);
            end = std::find(beg,str.end(),' ');
        }
        else
            beg=end;
    }
}
{% endhighlight %}

Here is the step by step transformation of our example string:

![Screen Shot](../images/2015-12-21.terminateshot.png)
