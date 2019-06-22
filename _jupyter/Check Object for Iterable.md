

```python
The ABC collections.abc.Iterables can be used to detect iterables
```


```python
from collections.abc import Iterable

li = [1,2,3]
print(isinstance(li,Iterable))
```

    True


The use of isinstance(obj,Iterable) only detects the implementations of __iter__ magic method. This approach ignores __getitem__ implementation of iterables

Python 3.7 recommends using iter() to check for iterables.


```python
noniter = 5

try:
    iter(noniter)
except TypeError as error_detail:
    print("object is not iterable",error_detail)
    
```

    object is not iterable 'int' object is not iterable


For example, let implement an algorithm to flatten a python array. 


```python
def flatten(input,output=None):
    if not input: return output
    if output is None: output = []

    for ele in input:
        try:
            iter(ele) # is ele iterable
            output.append(ele[0])
            flatten(ele[1:],output)
        except TypeError:
            output.append(ele)
    return output
            

ll = [1,2,[3,4,5],6,[7,[8,9]],"this is not a word",11]

print(flatten(ll))
```

    [1, 2, 3, 4, 5, 6, 7, 8, 9, 't', 'h', 'i', 's', ' ', 'i', 's', ' ', 'n', 'o', 't', ' ', 'a', ' ', 'w', 'o', 'r', 'd', 11]



```python

```


```python

```
