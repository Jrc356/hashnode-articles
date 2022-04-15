## TL;DR What is a Bloom Filter?

I have been interested lately in bloom filters and their applications after reading a post on LinkedIn. I have been meaning to write a fully fleshed post about what they are and how to use them with examples and what have you. Life has gotten in the way though and so I have not really had the motivation to lately. I do want to get something out though as I haven't written anything in like a week. So here is a very short explanation of what bloom filters are and why they're pretty cool data structures.

## What is a bloom filter?
Rather than rehash the same definition as 20 million other articles, I'd rather just quote one here:

> A Bloom filter is a space-efficient probabilistic data structure that is used to test whether an element is a member of a set. 
>
> -- *https://www.geeksforgeeks.org/bloom-filters-introduction-and-python-implementation/*

**TL;DR: they're an efficient data structure for checking the existence of an element**

## More Technical Explanation
- A bloom filter is basically a bit array of fixed length
- we use hashing functions to convert a string to a `uint` 
- then we clamp that `uint` to the size of our array using `modulo` (`hash(str) % arr.len`)

## Benefits
The benefits of using a bloom filter are that:
1. It is nearly impossible to get a **false-negative** for an element
2. We can represent a huge amount of data in a relatively small data structure

## Drawbacks
1. It is possible to get a **false-positive** and the chance of this increases as you add elements to the filter
2. It is impossible to delete an item from a filter because we cannot guarantee the element in our filter is only set by the element we are trying to delete
3. Careful choice of the hashing function is necessary to ensure a good balance of uniformity and consistency

## Hashing function choice
Bloom filters rely on hashing functions that are independent, uniformly distributed, and as fast as possible. Typically when talking about bloom filters, the following hashing functions are typically brought up:
- [murmur3](https://en.wikipedia.org/wiki/MurmurHash), 
- [FNV](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function),
- and [Jenkins hashes](https://en.wikipedia.org/wiki/Jenkins_hash_function)

At the bare minimum, we can use one function to store a bit. However, if we want to decrease the probability that we receive a false positive, then we can use multiple hashes and store multiple bits. This presents a tradeoff of accuracy vs size of our array that should be kept in mind when using multiple hashes.

## How efficient are bloom filters?
**Common notation when talking about bloom filters**
- `n` = number of elements to insert into the filter
- `m` = size of the bit array (filter)
- `k` = the number of hash functions to use

Since a bloom filter is a bit array representation of data, we don't need to worry about storing whole elements in our filter. We only store `m` `bits` rather than `n` `objects`. So you can see how we can represent a large amount of data in a small space.

Searching is constant time but is also dependent on the hashing functions used. The speed of the hashing function will directly determine the speed of your search. Post hashing, however, is constant time as we are directly accessing a specific index of the bit array.