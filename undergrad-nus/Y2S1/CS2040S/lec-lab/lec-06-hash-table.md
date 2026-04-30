# Lec 06 - Hash Table

## Definition

A Hash Table is a data structure that uses a hash function to efficiently map **keys** to **values**, for efficient search/retrieval, insertion, and/or removals. It uses a hash function to map large or even non-Integer keys into a small range of Integer indices (typically `[0..hash_table_size-1]`).

Doing so, you may notice that the probability of two distinct keys colliding into the same index is [relatively high](https://visualgo.net/en/hashtable?slide=3-5) and each of this potential collision needs to be resolved to maintain data integrity.

## Hash Function

The ideas of hashing are:

1. Map (some) **non-Integer** keys (e.g., Strings) to Integers keys,
2. Map **large** Integers to **smaller** Integers.

### Preliminaries

Suppose we have a hash table of size **M** where keys are used to identify the data and a specific hash function is used to compute a hash value.

A **hash value/hash code** of key v is computed from the key v with the use of a hash function to get an Integer in the range 0 to **M**-1. This hash value is used as the **base/home index/address** of the Hash Table entry for the data.

### Hash Integer — Best Practice

The best practice is,

$$
h(v)=v~\%~M
$$

e.g., map **v** into Hash Table of size **M** slots.

The Hash Table size M is set to be a reasonably large prime not near a power of 2, about 2+ times larger than the expected number of keys N that will ever be used in the Hash Table. This way, the load factor $$\alpha=N/M<0.5$$ — this will improve Hash Table performance, we shall see later when we deal with the collision.

### Hash String — Best Practice

Please visit the goated [visualgo](https://visualgo.net/en/hashtable?slide=4-7)!

## Collision

**Collision** happens when two (or more) keys have the **same hash value**. As we have seen above, the probability of having collision is very high in the Hash Table, thus we need to solve it. There are two major ideas: **Closed Addressing** versus **Open Addressing** method.

### Closed Addressing

In Closed Addressing, the hash code of a key gives its fixed/closed base address. Collision is resolved by appending the collided keys inside an auxiliary data structure (usually any form of List ADT) identified by the base address.

### Open Addressing

In Open Addressing, all hashed keys are located in a single array. The hash code of a key gives its base address. Collision is resolved by checking/probing multiple alternative addresses (hence the name **open**) in the table based on a certain rule.
