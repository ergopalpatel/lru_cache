# lru_cache
LRU Cache implementation with Redis in Python

## Problem Statement
Implement a cache that on start-up would load data from a file into the cache. The cache would have an initial size of 20 elements and upon reaching its limit, to add any new element it would remove the least recently accessed element. On shutdown it should store the cached data back to the file. The data should be stored in the cache according to a caching strategy. Provide options for cache CRUD.

> Cache : Redis

> Test Data : marks.csv , student.csv

## Steps to setup working environment
1. Install Redis Server
    - Ubuntu Command ```sudo apt install redis-server```
2. Install Flask
    - ```pip install Flask```
        
## implementation strategy
Set cache size in ```lru.py```

```CACHE_SIZE = 20```

`Redis Hash` to store value and `Redis List` to store the key of value ```CACHE_KEYS = "lru-keys"```

On access of cache item we are removing from the recent list and pushing it back on list (to left).
```
connection.lrem(CACHE_KEYS,1,key)
connection.lpush(CACHE_KEYS,key)
```

On adding of new item to cache and cache is full ( ```connection.llen(CACHE_KEYS) >= CACHE_SIZE``` ) then we are removing key fron list (from right hand side)
```
connection.rpop(CACHE_KEYS)
```

Register Shutdown event in Flask ( ```atexit``` )
```
atexit.register(onExit)
```
and then in onExit function we are restoring cache data back to file.
