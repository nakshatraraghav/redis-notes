
Introduction to Redis sets

A **Redis set** is an unordered collection of unique strings (members). You can use Redis sets to efficiently:

- Track unique items (e.g., track all unique IP addresses accessing a given blog post).
- Represent relations (e.g., the set of all users with a given role).
- Perform common set operations such as intersection, unions, and differences.


- Store the set of technologies used:
```
127.0.0.1:6379> SADD techstack nextjs react headlessui nodejs deno aws gcp azure
(integer) 8

127.0.0.1:6379> SMEMBERS techstack
1) "react"
2) "deno"
3) "nodejs"
4) "headlessui"
5) "aws"
6) "nextjs"
7) "azure"
8) "gcp"

127.0.0.1:6379> SADD techstack aws
(integer) 0
```

- Find whether a value exists in a set
```
127.0.0.1:6379> SISMEMBER techstack react
(integer) 1
```

- Union
```
127.0.0.1:6379> SUNION frontend backend
1) "expressjs"
2) "fastify"
3) "html"
4) "javascript"
5) "css"
6) "typescript"
7) "react"
```

- Intersection / Difference
```
127.0.0.1:6379> SMEMBERS frontend
1) "html"
2) "javascript"
3) "css"
4) "typescript"
5) "react"

127.0.0.1:6379> SMEMBERS backend
1) "fastify"
2) "expressjs"
3) "typescript"
4) "javascript"

127.0.0.1:6379> SINTER frontend backend
1) "typescript"
2) "javascript"

127.0.0.1:6379> SDIFF frontend backend
1) "html"
2) "react"
3) "css"
```


- Length of the Set
```
127.0.0.1:6379> SCARD frontend
(integer) 5

127.0.0.1:6379> SCARD backend
(integer) 4
```

## Limits

The max size of a Redis set is 2^32 - 1 (4,294,967,295) members.

## Basic commands

- [`SADD`](https://redis.io/commands/sadd) adds a new member to a set.
- [`SREM`](https://redis.io/commands/srem) removes the specified member from the set.
- [`SISMEMBER`](https://redis.io/commands/sismember) tests a string for set membership.
- [`SINTER`](https://redis.io/commands/sinter) returns the set of members that two or more sets have in common (i.e., the intersection).
- [`SCARD`](https://redis.io/commands/scard) returns the size (a.k.a. cardinality) of a set

## Performance

Most set operations, including adding, removing, and checking whether an item is a set member, are O(1). This means that they're highly efficient. However, for large sets with hundreds of thousands of members or more, you should exercise caution when running the [`SMEMBERS`](https://redis.io/commands/smembers) command. This command is O(n) and returns the entire set in a single response. As an alternative, consider the [`SSCAN`](https://redis.io/commands/sscan), which lets you retrieve all members of a set iteratively