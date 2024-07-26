---
layout: post
title: Finding All Prime Numbers Under 1,000,000 Using Python
image: "/posts/finding-primes-title-img.png"
tags: [Python, Primes]
---

In this post, I'll demonstrate a Python function to quickly find all prime numbers below a given value. For instance, if we input 100, the function will return all prime numbers below 100.

If you're not sure what a Prime number is, it is one that can only be divided by itself and 1. For example, 7 is a prime number, but 8 is not, as it can be divided by 2 and 4 in addition to 1 and 8.

Let's dive in!

First, we'll set an upper limit for our search, starting with 20.

```ruby
n = 20
```

Prime numbers start from 2. We create a set of numbers from 2 to n to check for primes. Instead of using a list, we're going to use a set. The reason for this is that sets have some special functions that will allow us to eliminate non-primes during our search.

```ruby
number_range = set(range(2, n+1))
print(number_range)
>>> {2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}
```

Next, let's also create a list where we can store any primes we discover.

```ruby
primes_list = []
```

To check for primes, we use a while loop. Initially, we manually check the logic to ensure it's correct. We use "*pop*" to get rid of the first element from our set, check if it's a prime, and if so, add it to our list of primes:

```ruby
prime = number_range.pop()
print(prime)
>>> 2
print(number_range)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}
primes_list.append(prime)
print(primes_list)
>>> [2]
```
Next, we generate multiples of the prime number that has been popped up to our upper limit (20 in this case):

```ruby
multiples = set(range(prime*2, n+1, prime))
print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20}
```

We know that the set of multiples of the popped-up prime number up to the upper limit is not going to be prime. So we would like to remove them from the "*number_range*". One special trick to do this
is to use the special set functionality "**difference_update**", which removes any values in "*multiples*" from the "*number_range*".

```ruby
number_range.difference_update(multiples)
print(number_range)
>>> {3, 5, 7, 9, 11, 13, 15, 17, 19}
```

This is amazing! This process significantly reduces the number of elements in our set, making our algorithm efficient. We repeat this logic until the set "*number_range*" is empty using a while loop.

Here is the complete code to find primes below 1000:

```ruby
n = 1000

# number range to be checked
number_range = set(range(2, n+1))

# empty list to append discovered primes to
primes_list = []

# iterate until list is empty
while number_range:
    prime = number_range.pop()
    primes_list.append(prime)
    multiples = set(range(prime*2, n+1, prime))
    number_range.difference_update(multiples)

print(primes_list)
>>> [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659, 661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797, 809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929, 937, 941, 947, 953, 967, 971, 977, 983, 991, 997]
```

Let's now get some statistics from our list:

```ruby
prime_count = len(primes_list)
largest_prime = max(primes_list)
print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")
>>> There are 168 prime numbers between 1 and 1000, the largest of which is 997
```

Finally, we can wrap this logic into a function:

```ruby
def primes_finder(n):
    
    # number range to be checked
    number_range = set(range(2, n+1))

    # empty list to append discovered primes to
    primes_list = []

    # iterate until list is empty
    while number_range:
        prime = number_range.pop()
        primes_list.append(prime)
        multiples = set(range(prime*2, n+1, prime))
        number_range.difference_update(multiples)
        
    prime_count = len(primes_list)
    largest_prime = max(primes_list)
    print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")
```

Now we can just pass the function the upper bound of our search and it will do the rest!

Let's go for something large, say a million...

```ruby
primes_finder(1000000)
>>> There are 78498 prime numbers between 1 and 1000000, the largest of which is 999983
```

###### Important Note: Using pop() on a Set in Python

In some cases, pop() can be inconsistent due to the unordered nature of sets. It usually extracts the lowest element, but this is not guaranteed. To ensure the minimum value is used, we can replace:

```ruby
prime = number_range.pop()
```

with

```ruby
prime = min(sorted(number_range))
number_range.remove(prime)
```

This guarantees the correct value but is slightly slower due to sorting.

I hope you enjoyed learning about finding primes using Python!
