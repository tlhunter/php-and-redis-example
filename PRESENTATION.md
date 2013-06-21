# PHP, Redis, and You! Presentation Notes

May these notes supplement you in your harsh journey.

## What is Redis?

* Key-Value Storage
* In-Memory
* Single-Threaded & ACID Compliant
* Fast as Hell (Written in C)
* Optional TTL (Can expire like Memcache)
* Transactions
* Pub/Sub Architecture

## When should you use Redis?

* Perhaps you'd like to do some caching
* You like Memcache but want guaranteed persistance
  * [Memcache doesn't guarantee](http://php.net/manual/en/memcache.set.php#refsect1-memcache.set-description)
* You want speed
  * Although, it's about [the same speed as Memcache anyway](http://alekseykorzun.com/post/53283070010/benchmarking-memcached-and-redis-clients)
* It's OK to persist to disk occasionally
  * Although, you can tell Redis to persist to disk on every write

## When shouldn't you use Redis?

* You need to query
  * `KEYS location-MI-Ann*` is about as complex as it gets

## Basic Commands

Look at that beautifully simple syntax! For a list of all commands, check out [redis.io/commands](http://redis.io/commands).

* SET _key_ _value_
  * Creates a new string record in Redis
* GET _key_
  * Gets the value of a string record
* DEL _key_
  * Deletes a record of any type based on the key
* KEYS _pattern_
  * Gets a list of keys which match the pattern, e.g., location-*
* RENAME _key_ _newkey_
  * Changes the name of a key
* INCR _key_
  * Increments an Integer
* DECR _key_
  * Decrements an Integer
* SAVE
  * Saves the database to disk

## Types of Data

Visit the Redis site for the [Official Data Types List](http://redis.io/topics/data-types).

### Strings

Simplest type of data. Binary safe. While you can't technically store objects within Redis, you CAN take your object, JSONify it, and then stick that into Redis. You won't be able to query these objects though like you can with other NoSQL systems (e.g. with MongoDB or CouchDB).

	redis 127.0.0.1:6379> SET fruit banana
	OK
	redis 127.0.0.1:6379> GET fruit
	"banana"
	redis 127.0.0.1:6379> DEL fruit
	(integer) 1
	redis 127.0.0.1:6379> GET fruit
	(nil)

### Integers

The documentation says that these are strings, but I know that they are lying. They simply use the same commands for setting and getting. You can `INCR`, `DECR`, and `INCRBY` integers.

	redis 127.0.0.1:6379> SET x 10.1
	OK
	redis 127.0.0.1:6379> INCR x
	(error) ERR value is not an integer or out of range
	redis 127.0.0.1:6379> SET x 10
	OK
	redis 127.0.0.1:6379> INCR x
	(integer) 11

### Lists

List are orderes sets of strings. You can do all sorts of pushing and popping from the right or the left, remove and insert and read items at a certain position, etc. Basically, all of those simple array operations you've come to know and love. When you insert the first item, if the key didn't exist, it is created

	redis 127.0.0.1:6379> LPUSH mylist x
	(integer) 1
	redis 127.0.0.1:6379> LPUSH mylist y
	(integer) 2
	redis 127.0.0.1:6379> LPUSH mylist z
	(integer) 3
	redis 127.0.0.1:6379> RPOP mylist
	"x"
	redis 127.0.0.1:6379> RPOP mylist
	"y"
	redis 127.0.0.1:6379> RPOP mylist
	"z"

### Sets

These are unordered collections of strings. Items won't be duplicated, so if you add the same string twice, it only exists in memory once. You can perform unions and diffs and intersections between sets, and move items if you know what the value is. You can't simply see everything in a set though, heck, the pop command itself will return a RANDOM item.

	redis 127.0.0.1:6379> SADD sorty Tom
	(integer) 1
	redis 127.0.0.1:6379> SADD sorty Dick
	(integer) 1
	redis 127.0.0.1:6379> SADD sorty Hairy
	(integer) 1
	redis 127.0.0.1:6379> SADD sorty Hairy
	(integer) 0
	redis 127.0.0.1:6379> SPOP sorty
	"Dick"
	redis 127.0.0.1:6379> SPOP sorty
	"Hairy"
	redis 127.0.0.1:6379> SPOP sorty
	"Tom"
	redis 127.0.0.1:6379> SPOP sorty
	(nil)

### Hashes

These are a sort of really simple object. Each hash can have any number of fields, and each field will have a value. You can get and set fields individually. You cannot query these though. You can tell that internally, the keys are adjacent to their values, based on how the commands work.

	redis 127.0.0.1:6379> HMSET user:1000 username antirez password P1pp0 age 34
	OK
	redis 127.0.0.1:6379> HGETALL user:1000
	1) "username"
	2) "antirez"
	3) "password"
	4) "P1pp0"
	5) "age"
	6) "34"
	redis 127.0.0.1:6379> HSET user:1000 password 12345
	(integer) 0
	redis 127.0.0.1:6379> HGETALL user:1000
	1) "username"
	2) "antirez"
	3) "password"
	4) "12345"
	5) "age"
	6) "34"

### Sorted Sets

Sorted Sets are a lot like sets, in that they also contain unique values. They are ordered using a sort of numerical scoring system

	redis 127.0.0.1:6379> ZADD highscores 1000 tlhunter
	(integer) 1
	redis 127.0.0.1:6379> ZADD highscores 200 m1ck3y
	(integer) 1
	redis 127.0.0.1:6379> ZADD highscores 500 rupertstyx
	(integer) 1
	redis 127.0.0.1:6379> ZCOUNT highscores 400 1200
	(integer) 2
	redis 127.0.0.1:6379> ZRANGEBYSCORE highscores 0 600
	1) "m1ck3y"
	2) "rupertstyx"

## Cool Uses

Just to get the gears turning…

### GeoHash Lookups

I use Redis with my Whisper project to store geographical communications. I take the latitude and longitude, and convert it into a GeoHash. A GeoHash splits the globe into 18 horizontal chunks and 18 vertical, represeting both as a character between [a-zA-Z0-9]. If you want to get more accurate, take one of chose chunks, and subdivide again, and add another character. By the time you've got several characters, you've got a fairly accurate location.

I then use the KEYS XYZ* command to look for locations based on accuracy. This allows me to very quickly perform geo lookups. I can add more characters or remove them to get more accurate or more vague.

### Unique Visitors

Add a users IP address to a Set each time they visit a page. You don't need to worry about performing a read before a write to see if it already exists.

### High Scores

Add a users high score to a Sorted Set, using their score as the set score. Who knew‽

### Character Locations

Pretend that you're building the worlds most amazing MMORPG. Perhaps you are storing your characters complex data in MySQL or MongoDB. Each character also has a position where they are within the game universe, and this position changes a lot (e.g., every time they move).

Of course, you don't want to keep updating your character object in your database with each new location. Instead, store them in Redis separately from your other database. You can configure Redis to persist data to disk every minute, and if it were to crash, losing a minute's worth of character location isn't that devastating.
