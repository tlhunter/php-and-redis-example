# PHP, Redis, and You!

This is an example project.

## Redis Installation

Pick a solution based on your OS.

### Official Download Page

[redis.io/download](http://redis.io/download)

### OS X via HomeBrew

	brew install redis

### OS X for the Non-Hacker

[Redis OS X PKG](http://rudix.googlecode.com/files/redis-2.6.9-0.pkg)

### Debian Linux

[Installing Redis on Debian as a Service](http://thomashunter.name/blog/installing-redis-on-debian/)

### Windows

[Redis Windows Binaries](https://github.com/MSOpenTech/redis/tree/2.6/bin/release)

## Project Installation

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
* [thomashunter.name/blog/tag/redis](http://thomashunter.name/blog/tag/redis/) - Shameless Self Plug ;)
