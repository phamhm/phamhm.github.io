#Palindrome#

Given a string, can you determine if one of its permutations is a palindrome?

The brute force approach is to generate all the permutations of the string and check for if one of those is palindrome.

{% highlight javascript %}
function checkPalindrome(str){
         const permutations = permutate(str);
         const res = permutations.filter(p => isPalindrome(p);
         return res.length() > 0 ? true : false;
}
{% endhighlight %}

This approach requires O(n!) to generate the permutations. We can do better than this.

First observe that, in a palindromic string with even number of characters, each character must be repeated an even number of times. If the length is odd, only one character can appear an odd number of count, the rest must be even.

We take advantage of this property by maintainance a boolean arrays. Each time we push a character into the array, we toggle the bit at its alphetical position. In the end we check if the array contains all zeros, or if there is only one single bit exists.

{% highlight javascript %}
function palindromePossible(str){
const res = str.split('')
.map(c => c.charCodeAt() - 'a'.charCodeAt() +1)
.filter(c => c >= 0)
.reduce((prev, curr) => prev ^ (1 << curr), 0);
return res == 0 || (res ^ (res-1)) == 0;
}
{% endhighlight %}
