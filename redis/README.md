# Redis

Redis is a NoSQL database. It's a key-value store that works a little bit like objects in javascript. It's a in-memory database which makes it really fast.

We use Redis for small pieces of data and we don't really care if some data might be lost. Redis is generally used for caching or session management.

## Launching redis and testing redis

1. In a first terminal : `src/redis-server` 
2. In the second terminal : `src/redis-cli`
3. `redis > set foo bar`
4. `redis > get foo`

## Commands

```
> SET name "Jarvis"
OK
> GET name
"Jarvis"
> EXISTS name
1
> DEl name
1
> EXISTS name
0
> GET name
(nil)
```

```
> SET session "foobar"
OK
> EXPIRE session 10 
1
> GET session
"foobar"
# 10 seconds later
> GET session
(nil)
```

```
> SET counter 1
OK
> INCRBY counter 2
3
> GET counter 
"3"
> DECR counter 
2
> GET counter
"2"
```

```
> MSET a 1 b 2
OK
> GET a
"1"
> GET b
"2"
> MGET a b
1) "1"
2) "2"
```

### Data types

Redis can handle five major data types :

- Strings
- Hashes
- Lists
- Sets
- Sorted sets

#### Hashes

Maps between string fields and string values. Think of JS objects.

```
> HMSET user id 45 name "John"
OK
> HGET id
(error) ERR wrong number of arguments for 'hget' command
> HGET user id
"45"
> HGET user name
"John"
> HGETALL user
1) "id"
2) "45"
3) "name"
4) "John"
```

#### Lists

They are implemented as linked lists and not arrays.

```
> LPUSH list 10
(integer) 1
> RPUSH list 44
(integer) 2
> LPUSH list "hello"
(integer) 3
> GET list
(error) WRONGTYPE Operation against a key holding the wrong kind of value
> LRANGE list 0 1
1) "hello"
2) "10"
> LRANGE list 0 2
1) "hello"
2) "10"
3) "44"
> RPOP list
"44"
> LRANGE list 0 2
1) "hello"
2) "10"
```

#### Sets

Sets can't have duplicate values.

```
> SADD set 1 2 3
(integer) 3
> SMEMBERS set
1) "1"
2) "2"
3) "3"
> SADD set 1 2
(integer) 0
> SMEMBERS set
1) "1"
2) "2"
3) "3"
> SISMEMBER set 2
(integer) 1
> SISMEMBER set 4
(integer) 0
```

#### Sorted sets

```
> ZADD team 50 "Wizards"
(integer) 1
> ZADD team 40 "Cavaliers"
(integer) 1
> ZRANGE team 0 1
1) "Cavaliers"
2) "Wizards"
> ZADD team 1 "Bolts"
(integer) 1
> ZRANGE team 0 2
1) "Bolts"
2) "Cavaliers"
3) "Wizards"
> ZRANK team "Wizards"
(integer) 2
```

## Resources

- [Download Redis](https://redis.io/download)
- [Redis commands](https://redis.io/commands)
- [Linked lists vs Arrays](https://freefeast.info/difference-between/difference-between-array-and-linked-list-array-vs-linked-list/)