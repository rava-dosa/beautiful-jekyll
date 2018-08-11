---
layout: post
title: Monologue on python
subtitle: The python you might not know
tags: [software, python]
---

![Python](https://www.python.org/static/community_logos/python-logo-master-v3-TM.png){:class="img-responsive"}

### Different type of python
Some notable mention wrt linux:  
* Cpython -> CPython is the original Python implementation
* PyPy -> A fast python implementation with a JIT compiler
* Jython -> Python running on the Java Virtual Machine

### Profiling and optimizing python
Timing function

{% highlight python linenos %}

import time
from functools import wraps
import random

def timing(f):
    def wrap(*args):
        time1 = time.time()
        ret = f(*args)
        time2 = time.time()
        print('{:s} function took {:.3f} ms'.format(f.__name__, (time2-time1)*1000.0))
        return ret
    return wrap

@timing
def random_sort(n):
    return sorted([random.random() for i in range(n)])

{% endhighlight %}

using time it

{% highlight python linenos %}

import timeit

def linear_search(mylist, find):
    for x in mylist:
        if x == find:
            return True
    return False

def linear_time():
    SETUP_CODE = '''
from __main__ import linear_search
from random import randint'''
     
    TEST_CODE = '''
mylist = [x for x in range(10000)]
find = randint(0, len(mylist))
linear_search(mylist, find)
    '''
    # timeit.repeat statement
    times = timeit.repeat(setup = SETUP_CODE,stmt = TEST_CODE,repeat = 3,number = 10000)
 
    # priniting minimum exec. time
    print('Linear search time: {}'.format(min(times)))  
 
if __name__ == "__main__":
    linear_time()

{% endhighlight %}

timing an script

'''
time -p python script.py
'''
using cprofile

'''
python -m cProfile -s cumulative script.py
'''

profiling memory

'''
pip install memory_profiler
pip install psutil
python -m memory_profiler script.py
'''
there is also one tool called guppy. And it's a really good library.

### Some less known data structure
This part of the blog is taken from PyMotw
#### ChainMap
The ChainMap class manages a sequence of dictionaries, and searches through them in the order they are given to find values associated with keys

{% highlight python linenos %}
import collections

a = {1: 10, 2: 20}
b = {3: 30, 4: 40, 2:200}

m1 = collections.ChainMap(a, b)
m2 = collections.ChainMap(b, a)
print(m1[2],end=" ")
print(m2[2],end="\n")
print(list(m1.keys()))
print(list(m1.values()))
for k, v in m1.items():
    print('{} = {}'.format(k, v))
m1.maps = list(reversed(m1.maps))
print('m1 = {}'.format(m1[2]))
a[5]=50
print(m1[5])
m3 = m1.new_child()
m3[2] = 2000
m3[10] = 209
{% endhighlight %}

#### Counter
A Counter is a container that keeps track of how many times equivalent values are added.

{% highlight python linenos %}
import collections

c = collections.Counter()
print('Initial :', c)
c.update('apoorvakumarhseenvaramuk')
print('Sequence:', c)
c.update({'a': 1, 'd': 5})
print('Dict    :', c)
print('Most common:')
for letter, count in c.most_common(3):
    print('{}: {}'.format(letter, count))
c1 = collections.Counter('aaaaaaassddbcpoerqw')
print(c1 + c)
print(c1 - c)
print(c1 & c)
print(c1 | c2)

{% endhighlight %}

#### DefaultDict
The standard dictionary includes the method setdefault() for retrieving a value and establishing a default if the value does not exist. By contrast, defaultdict lets the caller specify the default up front when the container is initialized.

{% highlight python linenos %}
import collections


def default_factory():
    return 'default value'


d = collections.defaultdict(default_factory, foo='bar')
print('d:', d)
print('foo =>', d['foo'])
print('bar =>', d['bar'])

{% endhighlight %}

#### NamedTuple
The standard tuple uses numerical indexes to access its members. Similar to C structure.

{% highlight python linenos %}

import collections

Person = collections.namedtuple('Person', 'name age')

bob = Person(name='Bob', age=30)
print('\nRepresentation:', bob)

jane = Person(name='Jane', age=29)
print('\nField by name:', jane.name)

print('\nFields by index:')
for p in [bob, jane]:
    print('{} is {} years old'.format(*p))

{% endhighlight %}

It's immutable. 

{% highlight python linenos %}

import collections

Person = collections.namedtuple('Person', 'name age')

bob = Person(name='Bob', age=30)
print('Representation:', bob)
print('As Dictionary:', bob._asdict())

{% endhighlight %}

#### [Some more detail about collection](https://pymotw.com/3/collections/abc.html)
### Itertools
The chain() function takes several iterators as arguments and returns a single iterator that produces the contents of all of the inputs as though they came from a single iterator.

{% highlight python linenos %}

from itertools import *

for i in chain([1, 2, 3], ['a', 'b', 'c']):
    print(i, end=' ')
print()

{% endhighlight %}

#### zip_longest()

{% highlight python linenos %}

from itertools import *

r1 = range(3)
r2 = range(2)

print('\nzip_longest processes all of the values:')
print(list(zip_longest(r1, r2)))

{% endhighlight %}

#### islice()
The islice() function returns an iterator which returns selected items from the input iterator, by index.

{% highlight python linenos %}

from itertools import *

print('Stop at 5:')
for i in islice(range(100), 5):
    print(i, end=' ')
print('\n')

print('Start at 5, Stop at 10:')
for i in islice(range(100), 5, 10):
    print(i, end=' ')
print('\n')

print('By tens to 100:')
for i in islice(range(100), 0, 100, 10):
    print(i, end=' ')
print('\n')

{% endhighlight %}

#### starmap()
The starmap() function is similar to map(), but instead of constructing a tuple from multiple iterators, it splits up the items in a single iterator as arguments to the mapping function using the * syntax.

{% highlight python linenos %}

from itertools import *

values = [(0, 5), (1, 6), (2, 7), (3, 8), (4, 9)]

for i in starmap(lambda x, y: (x, y, x * y), values):
    print('{} * {} = {}'.format(*i))

{% endhighlight %}

#### fraction and count()
count() function returns an iterator that produces consecutive integers, indefinitely

{% highlight python linenos %}

from itertools import *

import fractions
from itertools import *

start = fractions.Fraction(1, 3)
step = fractions.Fraction(1, 3)

for i in zip(count(start, step), ['a', 'b', 'c']):
    print('{}: {}'.format(*i))


{% endhighlight %}

cycle() function returns an iterator that repeats the contents of the arguments it is given indefinitely. Since it has to remember the entire contents of the input iterator,  it may consume quite a bit of memory if the iterator is long.

#### accumulate()
accumulate() function processes the input iterable, passing the nth and n+1st item to a function and producing the return value instead of either input. The default function used to combine the two values adds them, so accumulate() can be used to produce the cumulative sum of a series of numerical inputs.

{% highlight python linenos %}

from itertools import *

print(list(accumulate(range(5))))
print(list(accumulate('abcde')))

{% endhighlight %}

It is possible to combine accumulate() with any other function that takes two input values to achieve different results.

{% highlight python linenos %}

from itertools import *
def f(a, b):
    print(a, b)
    return b + a + b
print(list(accumulate('abcde', f)))

{% endhighlight %}

#### permutation()

{% highlight python linenos %}

from itertools import permutations
perm = permutations([1, 2, 3], 2)
for i in list(perm):
    print i

# Answer->(1, 2),(1, 3),(2, 1),(2, 3),(3, 1),(3, 2)

{% endhighlight %}

#### combination()

{% highlight python linenos %}

from itertools import combinations
comb = combinations([1, 2, 3], 2)
for i in list(comb):
    print i

{% endhighlight %}



