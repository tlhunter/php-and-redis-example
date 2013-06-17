# PHP, Redis, and You! Presentation Notes

## What is Redis?

Key-Value Store
in-memory
fast as hell
optional TTL
transactions
pub/sub

## When should you use Redis?

Caching
You like Memcache but want guaranteed persistance
You want speed

## When shouldn't you use Redis?

You need to query

## Types of Data

http://redis.io/topics/data-types

### Strings

### Integers

technically a string?

### Lists

### Sets

### Hashes

### Sorted Sets

## Basic Commands

[redis.io/commands](http://redis.io/commands)

* SET _key_ _value_
* GET _key_
* DEL _key_
* KEYS _pattern_
* RENAME _key_ _newkey_
* INCR _key_
* DECR _key_
* SAVE
