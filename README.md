# Cache::RedisLegacyCacheStore

![Crystal CI](https://github.com/crystal-cache/redis_legacy_cache_store/workflows/Crystal%20CI/badge.svg)
[![GitHub release](https://img.shields.io/github/release/crystal-cache/redis_legacy_cache_store.svg)](https://github.com/crystal-cache/redis_legacy_cache_store/releases)

A [cache](https://github.com/crystal-cache/cache) store implementation that stores data in Redis.

This shard using [stefanwille/crystal-redis](https://github.com/stefanwille/crystal-redis) as Redis client library.

If you're looking for an implementation that uses [jgaskins/redis](https://github.com/jgaskins/redis) check https://github.com/crystal-cache/redis_cache_store.

## Installation

1. Add the dependency to your `shard.yml`:

   ```yaml
   dependencies:
     redis_legacy_cache_store:
       github: crystal-cache/redis_legacy_cache_store
   ```

2. Run `shards install`

## Usage

```crystal
require "redis_legacy_cache_store"
```

It's important to note that Redis cache value must be string.

```crystal
cache = Cache::RedisLegacyCacheStore(String, String).new(expires_in: 1.minute, namespace: "myapp-cache")
# => #<Cache::RedisLegacyCacheStore(String, String) redis=#<Redis::PooledClient:0x7f0f1d2cf8c0 @pool=#<ConnectionPool(Redis):0x7f0f1d2e1af0 @r=#<IO::FileDescriptor: fd=11>, @w=#<IO::FileDescriptor: fd=12>, @capacity=5, @timeout=5.0, @buffer=Bytes[0], @size=0, ending=5, @pool=[], @block=#<Proc(Redis):0x562322895dd0:closure>, @connections=nil>> expires_in=00:01:00 namespace="myapp-cache>"

# Fetches data from the Redis, using "myapp-cache:today" key. If there is data in
# the Redis with the given key, then that data is returned.
#
# If there is no such data in the Redis (a cache miss or expired), then
# block will be written to the Redis under the given cache key, and that
# return value will be returned.
cache.fetch("today") do
  Time.utc.day_of_week
end
# => Wednesday
```

No `namespace` is set by default. Provide one if the Redis cache
server is shared with other apps:

This assumes Redis was started with a default configuration, and is listening on localhost, port 6379.

You can connect to Redis by instantiating the `Redis` or `Redis::PooledClient` class.

If you need to connect to a remote server or a different port, try:

```crystal
redis = Redis.new(host: "10.0.1.1", port: 6380, password: "my-secret-pw", database: 1)
cache = Cache::RedisLegacyCacheStore(String, String).new(expires_in: 1.minute, cache: redis)
```

## Contributing

1. Fork it (<https://github.com/crystal-cache/redis_legacy_cache_store/fork>)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

## Contributors

- [Anton Maminov](https://github.com/mamantoha) - creator and maintainer
