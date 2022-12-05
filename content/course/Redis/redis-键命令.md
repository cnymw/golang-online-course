---

title: redis-键命令
date: '2022-12-5'
type: book
weight: 701
commentable: true
editable: true
tags:
  - redis
---

Redis 键命令用于管理 redis 的键。

## DEL KEY_NAME

> 可用版本：>= 1.0.0

用于删除已存在的键。不存在的 key 会被忽略。

### 返回值

被删除 key 的数量。

### 示例

```bash
redis 127.0.0.1:6379> DEL rediskey
(integer) 1
```

## DUMP KEY_NAME

用于序列化给定 key ，并返回被序列化的值。

> 可用版本：>= 2.6.0

### 返回值

如果 key 不存在，那么返回 nil 。 否则，返回序列化之后的值。

### 示例

```bash
redis> SET greeting "hello, dumping world!"
OK

redis> DUMP greeting
"\x00\x15hello, dumping world!\x06\x00E\xa0Z\x82\xd8r\xc1\xde"
```

## EXISTS KEY_NAME

用于检查给定 key 是否存在。

> 可用版本：>= 1.0.0

### 返回值

若 key 存在返回 1 ，否则返回 0 。

### 示例

```bash
redis 127.0.0.1:6379> set rediskey newkey
OK

redis 127.0.0.1:6379> EXISTS rediskey
(integer) 1
```

## Expire KEY_NAME TIME_IN_SECONDS

用于设置 key 的过期时间，key 过期后将不再可用。单位以秒计。

> 可用版本：>= 1.0.0

### 返回值

设置成功返回 1 。 当 key 不存在或者不能为 key 设置过期时间时返回 0 。

### 示例

```bash
redis 127.0.0.1:6379> EXPIRE rediskey 60
(integer) 1
```

## Expireat KEY_NAME TIME_IN_UNIX_TIMESTAMP

用于以 UNIX 时间戳(unix timestamp)格式设置 key 的过期时间。key 过期后将不再可用。

> 可用版本：>= 1.0.0

### 返回值

设置成功返回 1 。 当 key 不存在或者不能为 key 设置过期时间时返回 0 。

### 示例

```bash
redis 127.0.0.1:6379> EXPIREAT rediskey 1293840000
(integer) 1
```

## PEXPIRE key milliseconds

和 EXPIRE 命令的作用类似，但是它以毫秒为单位设置 key 的生存时间，而不像 EXPIRE 命令那样，以秒为单位。

> 可用版本：>= 2.6.0

### 返回值

设置成功，返回 1 。key 不存在或设置失败，返回 0。

### 示例

```bash
redis> SET mykey "Hello"
"OK"

redis> PEXPIRE mykey 1500
(integer) 1
```

## PEXPIREAT KEY_NAME TIME_IN_MILLISECONDS_IN_UNIX_TIMESTAMP

用于设置 key 的过期时间，以毫秒计。key 过期后将不再可用。

> 可用版本：>= 1.0.0

### 返回值

设置成功返回 1。当 key 不存在或者不能为 key 设置过期时间时返回 0 。

### 示例

```bash
redis 127.0.0.1:6379> SET rediskey redis
OK

redis 127.0.0.1:6379> PEXPIREAT rediskey 1555555555005
(integer) 1
```

## KEYS PATTERN

用于查找所有符合给定模式 pattern 的 key。

> 可用版本：>= 1.0.0

### 返回值

符合给定模式的 key 列表。

### 示例

```bash
redis 127.0.0.1:6379> KEYS rediskey*
1) "rediskey3"
2) "rediskey1"
3) "rediskey2"
```

## MOVE KEY_NAME DESTINATION_DATABASE

用于将当前数据库的 key 移动到给定的数据库 db 当中。

> 可用版本：>= 1.0.0

### 返回值

移动成功返回 1 ，失败则返回 0 。

### 示例

```bash
redis> SELECT 0                             # redis默认使用数据库 0，为了清晰起见，这里再显式指定一次。
OK

redis> SET song "secret base - Zone"
OK

redis> MOVE song 1                          # 将 song 移动到数据库 1
(integer) 1
```

## PERSIST KEY_NAME

用于移除给定 key 的过期时间，使得 key 永不过期。

> 可用版本：>= 2.2.0

### 返回值

当过期时间移除成功时，返回 1 。 如果 key 不存在或 key 没有设置过期时间，返回 0 。

### 示例

```bash
redis> PERSIST mykey    # 移除 key 的生存时间
(integer) 1

redis> TTL mykey
(integer) -1
```

## PTTL KEY_NAME

以毫秒为单位返回 key 的剩余过期时间。

> 可用版本：>= 2.6.0

### 返回值

当 key 不存在时，返回 -2 。 当 key 存在但没有设置剩余生存时间时，返回 -1 。 否则，以毫秒为单位，返回 key 的剩余生存时间。

### 示例

```bash
# key 存在，但没有设置剩余生存时间
redis> SET key value
OK

redis> PTTL key
(integer) -1

# 有剩余生存时间的 key
redis> PEXPIRE key 10086
(integer) 1

redis> PTTL key
(integer) 6179
```

## TTL KEY_NAME

以秒为单位返回 key 的剩余过期时间。

> 可用版本：>= 1.0.0

### 返回值

当 key 不存在时，返回 -2 。 当 key 存在但没有设置剩余生存时间时，返回 -1 。 否则，以秒为单位，返回 key 的剩余生存时间。

### 示例

```bash
# key 存在，但没有设置剩余生存时间
redis> SET key value
OK

redis> TTL key
(integer) -1


# 有剩余生存时间的 key
redis> EXPIRE key 10086
(integer) 1

redis> TTL key
(integer) 10084
```

## RANDOMKEY

从当前数据库中随机返回一个 key 。

> 可用版本：>= 1.0.0

### 返回值

当数据库不为空时，返回一个 key 。 当数据库为空时，返回 nil （windows 系统返回 null）。

### 示例

```bash
redis> MSET fruit "apple" drink "beer" food "cookies"   # 设置多个 key
OK

redis> RANDOMKEY
"fruit"

redis> RANDOMKEY
"food"
```

## RENAME OLD_KEY_NAME NEW_KEY_NAME

用于修改 key 的名称 。

> 可用版本：>= 1.0.0

### 返回值

改名成功时提示 OK ，失败时候返回一个错误。

### 示例

```bash
# key 存在且 newkey 不存在
redis> SET message "hello world"
OK

redis> RENAME message greeting
OK

redis> EXISTS message               # message 不复存在
(integer) 0

redis> EXISTS greeting              # greeting 取而代之
(integer) 1
```

## RENAMENX OLD_KEY_NAME NEW_KEY_NAME

用于在新的 key 不存在时修改 key 的名称 。

> 可用版本：>= 1.0.0

### 返回值

修改成功时，返回 1 。如果 NEW_KEY_NAME 已经存在，返回 0 。

### 示例

```bash
# newkey 不存在，改名成功
redis> SET player "MPlyaer"
OK

redis> EXISTS best_player
(integer) 0

redis> RENAMENX player best_player
(integer) 1
```

## TYPE KEY_NAME

用于返回 key 所储存的值的类型。

> 可用版本：>= 1.0.0

### 返回值

返回 key 的数据类型，数据类型有：

- none (key不存在)
- string (字符串)
- list (列表)
- set (集合)
- zset (有序集)
- hash (哈希表)

### 示例

```bash
# 字符串
redis> SET weather "sunny"
OK

redis> TYPE weather
string
```

## 思维导图

![redis-键命令-思维导图.png](https://cnymw.github.io/GolangStudy/docs/redis-键命令/redis-键命令-思维导图.png)