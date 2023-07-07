
Introduction to Redis lists

**Redis lists** are linked lists of string values. Redis lists are frequently used to:

- Implement stacks and queues.
- Build queue management for background worker systems.

## **Examples**

- Treat a list like a queue (first in, first out):
```
127.0.0.1:6379> RPUSH queue 1
(integer) 1

127.0.0.1:6379> LRANGE queue 0 -1
1) "1"

127.0.0.1:6379> RPUSH queue 2
(integer) 2

127.0.0.1:6379> LRANGE queue 0 -1
1) "1"
2) "2"

127.0.0.1:6379> RPUSH queue 3
(integer) 3

127.0.0.1:6379> LRANGE queue 0 -1
1) "1"
2) "2"
3) "3"

127.0.0.1:6379> RPUSH queue 4
(integer) 4

127.0.0.1:6379> LRANGE queue 0 -1
1) "1"
2) "2"
3) "3"
4) "4"

127.0.0.1:6379> RPUSH queue 5
(integer) 5

127.0.0.1:6379> LRANGE queue 0 -1
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"

127.0.0.1:6379> LPOP queue
"1"

127.0.0.1:6379> LRANGE queue 0 -1
1) "2"
2) "3"
3) "4"
4) "5"
```

- Treat a list like a stack (first in, last out):
```
127.0.0.1:6379> LPUSH stack 1
(integer) 1

127.0.0.1:6379> LRANGE stack 0 -1
1) "1"

127.0.0.1:6379> LPUSH stack 2
(integer) 2

127.0.0.1:6379> LRANGE stack 0 -1
1) "2"
2) "1"

127.0.0.1:6379> LPUSH stack 3
(integer) 3

127.0.0.1:6379> LRANGE stack 0 -1
1) "3"
2) "2"
3) "1"

127.0.0.1:6379> LPUSH stack 4
(integer) 4

127.0.0.1:6379> LRANGE stack 0 -1
1) "4"
2) "3"
3) "2"
4) "1"

127.0.0.1:6379> LPOP stack
"4"

127.0.0.1:6379> LRANGE stack 0 -1
1) "3"
2) "2"
3) "1"

127.0.0.1:6379> LPOP stack
"3"

127.0.0.1:6379> LRANGE stack 0 -1
1) "2"
2) "1"
```

- Check the length of a list:
```
127.0.0.1:6379> LLEN stack
(integer) 2

127.0.0.1:6379> LRANGE stack 0 -1
1) "2"
2) "1"
```


- To create a capped list that never grows beyond 100 elements, you can call [`LTRIM`](https://redis.io/commands/ltrim) after each call to [`LPUSH`](https://redis.io/commands/lpush):
```
127.0.0.1:6379> LPUSH notifications "notif-1"
(integer) 1

127.0.0.1:6379> LPUSH notifications "notif-2"
(integer) 2

127.0.0.1:6379> LPUSH notifications "notif-3"
(integer) 3

127.0.0.1:6379> LPUSH notifications "notif-4"
(integer) 4

127.0.0.1:6379> LPUSH notifications "notif-5"
(integer) 5

127.0.0.1:6379> LTRIM notifications 0 4
OK

127.0.0.1:6379> LRANGE notifications 0 4
1) "notif-5"
2) "notif-4"
3) "notif-3"
4) "notif-2"
5) "notif-1"

127.0.0.1:6379> LPUSH notifications "notif-6"
(integer) 6

127.0.0.1:6379> LTRIM notifications 0 4
OK

127.0.0.1:6379> LRANGE notifications 0 4
1) "notif-6"
2) "notif-5"
3) "notif-4"
4) "notif-3"
5) "notif-2"

```


**LINSERT key BEFORE|AFTER PIVOT value**

```
127.0.0.1:6379> LINSERT notifications BEFORE "notif-4" "notif-10"
(integer) 5

127.0.0.1:6379> LRANGE notifications 0 -1
1) "notif-5"
2) "notif-10"
3) "notif-4"
4) "notif-3"
5) "notif-2"
```

## **LINDEX key index**

```
127.0.0.1:6379> LINDEX notifications 4
"notif-2"
```


**L/RPUSHX**

push data to the left or right hand side of the list only if the list exists
```
127.0.0.1:6379> KEYS *
1) "notifications"

127.0.0.1:6379> LPUSHX fruits melon
(integer) 0

127.0.0.1:6379> RPUSHX fruits mango
(integer) 0

127.0.0.1:6379> KEYS *
1) "notifications"

127.0.0.1:6379> LPUSHX notifications "notif-100"
(integer) 6

127.0.0.1:6379> LRANGE notifications 0 -1
1) "notif-100"
2) "notif-5"
3) "notif-10"
4) "notif-4"
5) "notif-3"
6) "notif-2"

```

**Sort Lists**

```
127.0.0.1:6379> LPUSH fruits mango banana guava watermelon pineapple
(integer) 5

127.0.0.1:6379> SORT fruits ALPHA
1) "banana"
2) "guava"
3) "mango"
4) "pineapple"
5) "watermelon"

127.0.0.1:6379> SORT fruits DESC ALPHA
1) "watermelon"
2) "pineapple"
3) "mango"
4) "guava"
5) "banana"
```


## **Limits**

The max length of a Redis list is 2^32 - 1 (4,294,967,295) elements.

## Basic commands

- [`LPUSH`](https://redis.io/commands/lpush) adds a new element to the head of a list; [`RPUSH`](https://redis.io/commands/rpush) adds to the tail.
- [`LPOP`](https://redis.io/commands/lpop) removes and returns an element from the head of a list; [`RPOP`](https://redis.io/commands/rpop) does the same but from the tails of a list.
- [`LLEN`](https://redis.io/commands/llen) returns the length of a list.
- [`LMOVE`](https://redis.io/commands/lmove) atomically moves elements from one list to another.
- [`LTRIM`](https://redis.io/commands/ltrim) reduces a list to the specified range of elements.


## Performance

List operations that access its head or tail are O(1), which means they're highly efficient. However, commands that manipulate elements within a list are usually O(n). Examples of these include [`LINDEX`](https://redis.io/commands/lindex), [`LINSERT`](https://redis.io/commands/linsert), and [`LSET`](https://redis.io/commands/lset). Exercise caution when running these commands, mainly when operating on large lists.