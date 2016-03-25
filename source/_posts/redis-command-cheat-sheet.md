---
title: "Redis Command Cheat Sheet"
layout: post
date: 2015-12-25
tags:
- redis
categories:
- redis
---

# [Redis Command Cheat Sheet](http://redis.io/commands)

## Hashes

- `HDEL`

> 从hash中删除某些字段，返回删除字段的总个数，时间复杂度：O(N)，N是删除字段的总个数

```bash
HDEL key field [field ...]

redis> HSET myhash field1 "foo"
(integer) 1
redis> HDEL myhash field1
(integer) 1
redis> HDEL myhash field2
(integer) 0

```

- `HEXISTS`

> 检查某个字段是否存在，时间复杂度：O(1)

```bash

redis> HSET myhash field1 "foo"
(integer) 1
redis> HEXISTS myhash field1
(integer) 1
redis> HEXISTS myhash field2
(integer) 0

```

- `HGET`

> 返回指定字段对应的值，时间复杂度：O(1)

```bash

redis> HSET myhash field1 "foo"
(integer) 1
redis> HGET myhash field1
"foo"
redis> HGET myhash field2
(nil)

```

- `HGETALL`

> 返回所有字段对应的所有值，每个字段后面紧跟着它的值，所以总长度是hash的两倍，返回类型list

```bash

redis> HSET myhash field1 "Hello"
(integer) 1
redis> HSET myhash field2 "World"
(integer) 1
redis> HGETALL myhash
1) "field1"
2) "Hello"
3) "field2"
4) "World"

```

- `HINCRBY`

> 为指定字段对应的值增加或者减少数值，时间复杂度:O(1)

```bash

redis> HSET myhash field 5
(integer) 1
redis> HINCRBY myhash field 1
(integer) 6
redis> HINCRBY myhash field -1
(integer) 5
redis> HINCRBY myhash field -10
(integer) -5

```

- `HINCRBYFLOAT`

> 浮点数增加减少数值，时间复杂度:O(1)

```bash
redis> HSET mykey field 10.50
(integer) 1
redis> HINCRBYFLOAT mykey field 0.1
"10.60000000000000001"
redis> HSET mykey field 5.0e3
(integer) 0
redis> HINCRBYFLOAT mykey field 2.0e2
```

- `HKEYS`

> Returns all field names in the hash stored at key. Time complexity: O(N) where N is the size of the hash.

```bash
redis> HSET myhash field1 "Hello"
(integer) 1
redis> HSET myhash field2 "World"
(integer) 1
redis> HKEYS myhash
1) "field1"
2) "field2"
```

- `HLEN`

> 返回字段个数 Time complexity: O(1)

```bash
redis> HSET myhash field1 "Hello"
(integer) 1
redis> HSET myhash field2 "World"
(integer) 1
redis> HLEN myhash
(integer) 2
```

- `HMGET`

> 批量返回字段所对应的值 Time complexity: O(N) where N is the number of fields being requested.

```bash
redis> HSET myhash field1 "Hello"
(integer) 1
redis> HSET myhash field2 "World"
(integer) 1
redis> HMGET myhash field1 field2 nofield
1) "Hello"
2) "World"
3) (nil)
```

- `HMSET`

> 批量设置字段所对应的值 Time complexity: O(N) where N is the number of fields being set.

```bash
redis> HMSET myhash field1 "Hello" field2 "World"
OK
redis> HGET myhash field1
"Hello"
redis> HGET myhash field2
"World"
```

- `HSCAN`

- `HSET`

> Sets field in the hash stored at key to value. Time complexity: O(1)

```bash
redis> HSET myhash field1 "Hello"
(integer) 1
redis> HGET myhash field1
"Hello"
```

- `HSETNX`

> 仅当key不存在的时候设置值，否则直接忽略 Time complexity: O(1)

```bash
redis> HSETNX myhash field "Hello"
(integer) 1
redis> HSETNX myhash field "World"
(integer) 0
redis> HGET myhash field
"Hello"
```

- `HSTRLEN`

> 返回key对应的值的长度 Time complexity: O(1)

```bash
redis> HMSET myhash f1 HelloWorld f2 99 f3 -256
OK
redis> HSTRLEN myhash f1
(integer) 10
redis> HSTRLEN myhash f2
(integer) 2
redis> HSTRLEN myhash f3
(integer) 4
```

- `HVALS`

> 返回所有值 Time complexity: O(N) where N is the size of the hash.

```bash
redis> HSET myhash field1 "Hello"
(integer) 1
redis> HSET myhash field2 "World"
(integer) 1
redis> HVALS myhash
1) "Hello"
2) "World"
```

## Sets

- `SADD`

> Add the specified members to the set stored at key. Time complexity: O(N) where N is the number of members to be added.

```bash
redis> SADD myset "Hello"
(integer) 1
redis> SADD myset "World"
(integer) 1
redis> SADD myset "World"
(integer) 0
redis> SMEMBERS myset
1) "World"
2) "Hello"
```

- `SCARD`

> Returns the set cardinality (基数) (number of elements) of the set stored at key. Time complexity: O(1)

```bash
redis> SADD myset "Hello"
(integer) 1
redis> SADD myset "World"
(integer) 1
redis> SCARD myset
(integer) 2
```

- `SDIFF`

> Returns the members of the set resulting from the difference between the first set and all the successive sets. (第一个和剩下几个) Time complexity: O(N) where N is the total number of elements in all given sets.

```bash
key1 = {a,b,c,d}
key2 = {c}
key3 = {a,c,e}
SDIFF key1 key2 key3 = {b,d}
```

- `SDIFFSTORE`

> This command is equal to SDIFF, but instead of returning the resulting set, it is stored in destination.

```bash
redis> SADD key1 "a"
(integer) 1
redis> SADD key1 "b"
(integer) 1
redis> SADD key1 "c"
(integer) 1
redis> SADD key2 "c"
(integer) 1
redis> SADD key2 "d"
(integer) 1
redis> SADD key2 "e"
(integer) 1
redis> SDIFFSTORE key key1 key2
(integer) 2
redis> SMEMBERS key
1) "b"
2) "a"
```

- `SINTER`

> Returns the members of the set resulting from the intersection of all the given sets. (交集) Time complexity: O(N*M) worst case where N is the cardinality of the smallest set and M is the number of sets.

```bash
key1 = {a,b,c,d}
key2 = {c}
key3 = {a,c,e}
SINTER key1 key2 key3 = {c}
```

- `SINTERSTORE`

> This command is equal to SINTER, but instead of returning the resulting set, it is stored in destination.

```bash
redis> SADD key1 "a"
(integer) 1
redis> SADD key1 "b"
(integer) 1
redis> SADD key1 "c"
(integer) 1
redis> SADD key2 "c"
(integer) 1
redis> SADD key2 "d"
(integer) 1
redis> SADD key2 "e"
(integer) 1
redis> SINTERSTORE key key1 key2
(integer) 1
redis> SMEMBERS key
1) "c"
```

- `SISMEMBER`

> Returns if member is a member of the set stored at key. Time complexity: O(1)

```bash
redis> SADD myset "one"
(integer) 1
redis> SISMEMBER myset "one"
(integer) 1
redis> SISMEMBER myset "two"
(integer) 0
```

- `SMEMBERS`

> Returns all the members of the set value stored at key. Time complexity: O(N) where N is the set cardinality.

```bash
redis> SADD myset "Hello"
(integer) 1
redis> SADD myset "World"
(integer) 1
redis> SMEMBERS myset
1) "World"
2) "Hello"
```

`SMOVE`

> Move member from the set at source to the set at destination. This operation is atomic (原子操作). In every given moment the element will appear to be a member of source or destination for other clients. Time complexity: O(1)

```bash
redis> SADD myset "one"
(integer) 1
redis> SADD myset "two"
(integer) 1
redis> SADD myotherset "three"
(integer) 1
redis> SMOVE myset myotherset "two"
(integer) 1
redis> SMEMBERS myset
1) "one"
redis> SMEMBERS myotherset
1) "three"
2) "two"
```

- `SPOP`

> Removes and returns one or more random elements from the set value store at key. Time complexity: O(1) 注意是随机

```bash
redis> SADD myset "one"
(integer) 1
redis> SADD myset "two"
(integer) 1
redis> SADD myset "three"
(integer) 1
redis> SPOP myset
"three"
redis> SMEMBERS myset
1) "one"
2) "two"
redis> SADD myset "four"
(integer) 1
redis> SADD myset "five"
(integer) 1
redis> SPOP myset 3
1) "one"
2) "five"
3) "two"
redis> SMEMBERS myset
1) "four"
```

- `SRANDMEMBER`

> When called with just the key argument, return a random element from the set value stored at key. Starting from Redis version 2.6, when called with the additional count argument, return an array of count distinct elements if count is positive. If called with a negative count the behavior changes and the command is allowed to return the same element multiple times. In this case the number of returned elements is the absolute value of the specified count. [简而言之，就是随机返回几个元素给你] Time complexity: Without the count argument O(1), otherwise O(N) where N is the absolute value of the passed count.

```bash
redis> SADD myset one two three
(integer) 3
redis> SRANDMEMBER myset
"two"
redis> SRANDMEMBER myset 2
1) "three"
2) "one"
redis> SRANDMEMBER myset -5
1) "one"
2) "three"
3) "two"
4) "one"
5) "three"
```

- `SREM`

> Remove the specified members from the set stored at key. Specified members that are not a member of this set are ignored. Time complexity: O(N) where N is the number of members to be removed.

```bash
redis> SADD myset "one"
(integer) 1
redis> SADD myset "two"
(integer) 1
redis> SADD myset "three"
(integer) 1
redis> SREM myset "one"
(integer) 1
redis> SREM myset "four"
(integer) 0
redis> SMEMBERS myset
1) "three"
2) "two"
```

- `SSCAN key cursor [MATCH pattern] [COUNT count]`

- `SUNION`

> Returns the members of the set resulting from the union of all the given sets.

```bash
key1 = {a,b,c,d}
key2 = {c}
key3 = {a,c,e}
SUNION key1 key2 key3 = {a,b,c,d,e}
```

- `SUNIONSTORE destination key [key ...]`

## Lists

- `BLPOP key [key ...] timeout`