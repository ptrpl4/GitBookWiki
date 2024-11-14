# Generators and yield

### Generator examples

Example with next()

```sh
>>> a = (i**2 for i in range(1,5))
>>> a
<generator object <genexpr> at 0x0000023A7524D6D0>
>>> next(a)
1
>>> next(a)
4
>>> next(a)
9
>>> next(a)
16
>>> next(a)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

Example with for

```sg
>>> a = (i**2 for i in range(1,5))
>>> for i in a:
...     print(i)
1
4
9
16
```

We can use yield if we need to take and use current generator value only once and fast

```python
def gen_countdown(n):
    while n != 0:
        yield n -1
        n -= 1
g = gen_countdown(4)
print(next(g))
# 3
print(next(g))
# 2
print(next(g))
# 1
print(next(g))
# 0
```
