# PHP, Redis, and You!

This is an example PHP project which uses Redis to store and retreive some data. It was buit for the
[Ann Arbor PHP MySQL Meetup](http://www.meetup.com/ann-arbor-php-mysql/) on 2013-06-22. Even if you're
not in the meetup, feel free to follow along ;).

This README.md document will go over downloading and installation instructions. For a copy of the
presentation notes, check out the [PRESENTATION.md](PRESENTATION.md) document.

## Play with Redis

[try.redis.io](http://try.redis.io) - If you don't want to install Redis but want to experiment

## Redis Installation

Pick a solution based on your OS.

### Official Download Page

* [redis.io/download](http://redis.io/download)

### OS X via HomeBrew

	brew install redis

### OS X for the Non-Hacker

* [Redis OS X PKG](http://rudix.googlecode.com/files/redis-2.6.9-0.pkg)

### Ubuntu Linux

	apt-get install redis-server

### Debian Linux

* [Installing Redis on Debian as a Service](http://thomashunter.name/blog/installing-redis-on-debian/)

### Windows

* [Redis Windows Binaries](https://github.com/MSOpenTech/redis/tree/2.6/bin/release)

## Project Installation

This'll get you my sample PHP and Redis application, assuming you've got GIT installed and configured.

	mkdir php-and-redis-example && cd php-and-redis-example
	git init
	git remote add origin git@github.com:tlhunter/php-and-redis-example.git
	git pull origin master
	git submodule init
	git submodule update
	php index.php

## Further Reading

* [redis.io/commands](http://redis.io/commands) - Official Documentation on Redis Commands
* [github.com/nrk/predis](https://github.com/nrk/predis) - Raw PHP Redis Library
* [github.com/tlhunter/whisper](https://github.com/tlhunter/whisper/blob/master/server.js) - More Complex Project Example
* [github.com/tlhunter/php-and-redis-example](https://github.com/tlhunter/php-and-redis-example) - This project
* [benchmarking-memcached-and-redis-clients](http://alekseykorzun.com/post/53283070010/benchmarking-memcached-and-redis-clients) - Redis vs. Predis vs. Memcache Benchmark
