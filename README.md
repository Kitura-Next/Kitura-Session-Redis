<p align="center">
    <a href="http://kitura.io/">
        <img src="https://raw.githubusercontent.com/Kitura-Next/Kitura/master/Sources/Kitura/resources/kitura-bird.svg?sanitize=true" height="100" alt="Kitura">
    </a>
</p>


<p align="center">
    <a href="http://www.kitura.io/">
    <img src="https://img.shields.io/badge/docs-kitura-1FBCE4.svg" alt="Docs">
    </a>
    <a href="https://travis-ci.org/Kitura-Next/Kitura-Session-Redis">
    <img src="https://travis-ci.org/Kitura-Next/Kitura-Session-Redis.svg?branch=master" alt="Build Status - Master">
    </a>
    <img src="https://img.shields.io/badge/os-macOS-green.svg?style=flat" alt="macOS">
    <img src="https://img.shields.io/badge/os-linux-green.svg?style=flat" alt="Linux">
    <img src="https://img.shields.io/badge/license-Apache2-blue.svg?style=flat" alt="Apache 2">
    <a href="http://swift-at-ibm-slack.mybluemix.net/">
    <img src="http://swift-at-ibm-slack.mybluemix.net/badge.svg" alt="Slack Status">
    </a>
</p>

# Kitura-Session-Redis
Kitura-Session store using Redis as the backing store

## Summary
 [Kitura-Session](https://github.com/Kitura-Next/Kitura-Session) store using [Redis](http://redis.io/) as the backing store

## Table of Contents
* [Swift version](#swift-version)
* [API](#api)
* [License](#license)

## Swift version
The latest version of Kitura-Session-Redis requires **Swift 4.0 or newer**. You can download this version of the Swift binaries by following this [link](https://swift.org/download/). Compatibility with other Swift versions is not guaranteed.


## API
In order to use Redis as session store, create an instance of `RedisStore`, and pass it to `Session` constructor:

```swift
import KituraSession
import KituraSessionRedis

let redisStore = RedisStore(redisHost: host, redisPort: port, redisPassword: password)
let session = Session(secret: <secret>, store: redisStore)
```

`RedisStore` constructor requires Redis server host and port. The rest of the parameters are optional:

```swift
init (redisHost: String, redisPort: Int32, redisPassword: String?=nil, ttl: Int = 3600, db: Int = 0, keyPrefix: String = "s:")
```

You can set Redis password in `redis.conf` file:
```
requirepass <your password>
```
The maximum number of databases is also set in `redis.conf` file:
```
databases <number of databases>
```
The `db` passed to the constructor must be between 0 and this number minus 1.

## Swift Test Setup

To run swift test for `Kitura-Session-Redis` you must first set up Redis.
From the Kitura-Session-Redis directory, run the following commands:

#### MacOS
```
brew install redis --build-from-source || brew outdated redis || brew upgrade redis
export REDIS_CONF_FILE=/usr/local/etc/redis.conf
password=$(head -n 1 “/Tests/KituraSessionRedisTests/password.txt")
sudo perl -pi -e "s/# requirepass foobared/requirepass ${password}/g" $REDIS_CONF_FILE
redis-server $REDIS_CONF_FILE
```

#### Linux
```
sudo apt-get update -y
sudo apt-get install -y redis-server
export REDIS_CONF_FILE=/etc/redis/redis.conf
sudo chmod go+x /etc/redis/
sudo chmod go+r $REDIS_CONF_FILE
password=$(head -n 1 “/Tests/KituraSessionRedisTests/password.txt")
sudo perl -pi -e "s/# requirepass foobared/requirepass ${password}/g" $REDIS_CONF_FILE
sudo service redis-server restart
```

Then run  `swift test`.

## License
This library is licensed under Apache 2.0. Full license text is available in [LICENSE](LICENSE.txt).
