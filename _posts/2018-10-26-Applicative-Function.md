#Applicative ((->) x)#

This is an interesting applicative that is used to map partially applied function.

An instance of this applicative is defined as:

```
instance Applicative ((->) x) where
         pure x = \_ -> x
         f <*> g = \x -> f x $ g x
```

From Learn Haskell For Great Good, we will derive the following example:

```
let f = (+) <$> (* 3) <*> (+ 100)
```

Since this is an applicative, associativity rule applies thus we can derive the right part first

```
let f = (* 3)
    g = (+ 100)
    f <*> g = \x -> (x * 3) $ (x + 100)
```

At this point, we can't evaluate f <*> g yet because it's not an valid expression using "$". Using applicative definition of "<$>" we see that

```Haskell
 f <$> g  == pure f <*> g
```

I am going to reuse the the variable 'f' and 'g' again to rewrite the
functions as:

```Haskell
let f = \_ -> (+) -- pure (+)
    g = \x -> (x * 3) $ (x + 100)
    f <*> g = \z -> f z $ g z
            = \z -> (+) $ (z * 3) $ (z + 100)
```

Thus

{% highlight haskell %}
let f = (+) <$> (* 3) <*> (+ 100)
      = pure (+) <*> (* 3) <*> (+ 100)
      = \z -> (+) $ (z * 3) $ (z + 100)
in f 5 -- result =  515
{% endhighlight %}
