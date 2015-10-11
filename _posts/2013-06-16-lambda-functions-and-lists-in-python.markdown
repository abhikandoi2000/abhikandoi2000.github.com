---
title:  "Lambda functions and lists in Python"
date:   2013-06-16 12:22:01
description: Anonymous functions are handy
---

As usual **Github** was the first tab that I opened after firing up Google Chrome. I was reading through my news feed nice and smooth until something interesting held my attention. Reading the news feed may not sound so cool, but it indeed is quite exciting. Opening a dozen new projects in a dozen new Chrome tab just to find out that each one of them is unique, makes my day, nothing but more exciting.

I found an interesting [repository](https://github.com/blakeembrey/code-problems) where I could not only show-off my programming skills (well, not really) but also learn a few things about Python.

What ever! That was my part of the story & let me introduce to your part real soon.

## Lists in Python

Now I am not going to define **lists**, and neither am I going to introduce you to its basic. There are more than a gross articles out there explaining it. And it would be worthless doing it over and over again.

And as I go on with my explanation, I will be pointing out what Pythonic nature is.

### What?
As I was trying to implement a few (actually just two) code problems using Python, I encountered a few hurdles.

While trying to implement an **anagram detector** I stumbled upon some useful things.

#### Using **lambda** to create anonymous functions
One of the many attractions of Python is it’s ability to make every thing look so damn readable. Lambda expressions just adds to this ability.

> As a quick fact, the concept of **Lambda** was borrowed from the Lisp language

For example, consider this small code snippet to extract a few elements from a list based on its first letter:

###### Extract elements from a list based on its first letter
{% highlight python %}
items = ['geeklist', 'playfair', 'heroku', 'hallucination', 'hulu', 'hotmail', 'dojo', 'zen', 'enigma']
filter(lambda item: item[0] in ['h', 'e'], items)
['heroku', 'hallucination', 'hulu', 'hotmail', 'enigma']
{% endhighlight %}


**Lambda** functions comes from the mathematical domain of [lambda calculus](http://en.wikipedia.org/wiki/Lambda_calculus) and is an efficient way of writing anonymous functions.

You can also use it to write closures. Here is a quick example to greet a user.

###### Creating closures for greeting users
{% highlight python %}
def greet(greeting):
  return lambda name: greeting + ' ' + name

greetNamaste = greet('Namaste')

greetNamaste('Sam')
'Namaste Sam'
greetNamaste('Goldberg')
'Namaste Goldberg'
{% endhighlight %}

In the example above the greet function returns an anonymous function defined using the lambda expression. This anonymous function can then be assigned to an identifier with the only requirement of a name argument.

#### Reducing a list to a single element
Sometimes it is useful to reduce a list of elements to a single element based on a few rules. The problem statement for the [anagram detector](https://github.com/blakeembrey/code-problems/tree/master/anagram-detection) is as stated below:

> Write a function that accepts two parameters, a parent and a child string. Determine how many times the child string - or an anagram of the child string - appears in the parent string.

There is an elegant method of solving this problem and can be easily coded in Python by the use of the `reduce` method.

We start by assigning distinct prime numbers, one for each alphabet. Then we create what is called a **hash** to identify a string and its anagram. The hashing is such that a string and all its anagrams will result in the same hash. This is done by multiplying together the prime number corresponding to each alphabet. Such a procedure results in a number which can only be formed by a different string if both of them have the same list of characters and frequency of occurrence (hence an anagram).

I implemented this method in Python, the core of which is:

###### Function for generating hash based on prime numbers

{% highlight python %}
def hashString(str):
  return reduce(lambda memo, char: memo * charMap[char], list(str), 1)
{% endhighlight %}

Straight from the Python [docs](http://docs.python.org/2/library/functions.html#reduce) regarding the reduce function.

>It applies function of two arguments cumulatively to the items of iterable, from left to right, so as to reduce the iterable to a single value.

The function in our case is the anonymous lambda function which returns the multiplied result of the cumulated value and the prime number corresponding to the current alphabet. The third argument specifies the default value for the cumulated value to start with.

> Note: the `charMap` is just a dictionary which maps each alphabet to its corresponding prime number

All this together with a lot of other cool expressions and functions add to the Pythonic nature of this beautiful yet powerful language.