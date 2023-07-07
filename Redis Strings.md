
Introduction to **Redis strings**

**Redis strings** store sequences of bytes, including text, serialized objects, and binary arrays. As such, strings are the most basic Redis data type. They're often used for caching, but they support additional functionality that lets you implement counters and perform bitwise operations, too.


- Store and then retrieve a string in Redis:
```
> SET name "nakshatra raghav"
OK

> GET name
"nakshatra raghav"
```

- Store a serialized JSON string and set it to expire 100 seconds from now:

```
> SET fruits "['mango','banana','guava']" EX 100
```

- Increment / Decrement a counter:
```
127.0.0.1:6379> SET views:user-1 100
OK

127.0.0.1:6379> INCR views:user-1
(integer) 101

127.0.0.1:6379> DECR views:user-1
(integer) 100

127.0.0.1:6379> INCRBY views:user-1 100
(integer) 200

127.0.0.1:6379> GET views:user-1
"200"

127.0.0.1:6379> INCRBY views:user-1 -2000
(integer) -1800

127.0.0.1:6379> GET views:user-1
"-1800"

127.0.0.1:6379> SET PI 3.141529
OK

127.0.0.1:6379> GET PI
"3.141529"

127.0.0.1:6379> INCRBYFLOAT PI 3.141592
"6.283121"
```

## **Limits**

By default, a single Redis string can be a **maximum of 512 MB**.


## **Getting and setting Strings**

- [`SET`](https://redis.io/commands/set) stores a string value.
- [`SETNX`](https://redis.io/commands/setnx) stores a string value only if the key doesn't already exist. Useful for implementing locks.
- [`GET`](https://redis.io/commands/get) retrieves a string value.
- [`MGET`](https://redis.io/commands/mget) retrieves multiple string values in a single operation.

## **Performance**

Most string operations are O(1), which means they're highly efficient. However, be careful with the [`SUBSTR`](https://redis.io/commands/substr), [`GETRANGE`](https://redis.io/commands/getrange), and [`SETRANGE`](https://redis.io/commands/setrange) commands, which can be O(n). These random-access string commands may cause performance issues when dealing with large strings.

```
127.0.0.1:6379> SUBSTR name 0 3
"naks"

127.0.0.1:6379> GETRANGE name 0 3
"naks"

127.0.0.1:6379> SETRANGE name 3 hey
(integer) 16

127.0.0.1:6379> GET name
"nakheytra raghav"
```

## **Expire Redis Strings**

```
127.0.0.1:6379> SET countdown 10 EX 10
OK
127.0.0.1:6379> TTL countdown
(integer) 9
127.0.0.1:6379> TTL countdown
(integer) 8
127.0.0.1:6379> TTL countdown
(integer) 7
127.0.0.1:6379> TTL countdown
(integer) 6
127.0.0.1:6379> TTL countdown
(integer) 5
127.0.0.1:6379> TTL countdown
(integer) 4
127.0.0.1:6379> TTL countdown
(integer) 4
127.0.0.1:6379> TTL countdown
(integer) 3
127.0.0.1:6379> TTL countdown
(integer) 2
127.0.0.1:6379> TTL countdown
(integer) 1
127.0.0.1:6379> TTL countdown
(integer) 0
127.0.0.1:6379> TTL countdown
(integer) -2
```

TTL returns -2 value when the key has expired


**SETEX** key second value

```
127.0.0.1:6379> SETEX countdown 10 10
OK
127.0.0.1:6379> TTL countdown
(integer) 7
127.0.0.1:6379> TTL countdown
(integer) 5
127.0.0.1:6379> TTL countdown
(integer) 4
127.0.0.1:6379> TTL countdown
(integer) 4
127.0.0.1:6379> TTL countdown
(integer) 3
127.0.0.1:6379> TTL countdown
(integer) 2
127.0.0.1:6379> TTL countdown
(integer) 1
127.0.0.1:6379> TTL countdown
(integer) -2
127.0.0.1:6379> TTL countdown

```

