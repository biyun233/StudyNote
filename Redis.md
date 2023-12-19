### Redis

```sh
redis-server # start redis and print info
redis-cli # connect to redis
```

### Installation

```sh
sudo apt install lsb-release curl gpg
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

sudo apt-get update
sudo apt-get install redis
```



### Commands

```sh
# delete all keys
flushall

# check if key exist
exists name

# get all keys matches with pattern
keys *

# set key-value
set name kyle

# get value of key
get name

# get expiration of key (-1 means no expiration)
ttl name

# set expiration of key (seconds)
expire name 10

# set key-value with expiration
setex name 10 kyle

# push element to the start of an array
lpush friends john 
# push element to the end of an array
rpush friends john 

# get all value of an array (start end : 0 -1)
lrange friends 0 -1

# remove elements from the start of the array
lpop friends (count)
# remove elements from the end of the array
rpop friends (count)

# add unique value to a list
sadd hobbies "weight lifting"

# get members of unique list
smembers hobbies

# remove a member from unique list
srem hobbies "weight lifting"

# set the string value of a hash field
hset person name kyle

# get the value of a hash field
hget person name

# get all the fields and values in a hash
hgetall person
# 1) "name"
# 2) "kyle"

# delete one or more hash fields
hdel person name

# determine if a hash field exists
hexists person name
```

