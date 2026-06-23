# Redis for Absolute Beginners 🚀

Think of Redis as a **super-fast in-memory database** that stores data as **key → value** pairs.

```js
{
  "user:1": "Ravi",
  "user:2": "Piyush"
}
```

Unlike MongoDB or MySQL, Redis keeps data primarily in RAM, making it extremely fast.

---

# Why Redis?

Common use cases:

- Cache API responses
- Store user sessions
- Online multiplayer games
- Leaderboards
- Chat systems
- Rate limiting
- Job queues
- Real-time analytics

---

# Running Redis Locally with Docker

Pull Redis Stack:

```bash
docker pull redis/redis-stack:latest
```

Run Redis:

```bash
docker run -d \
  --name redis-stack \
  -p 6379:6379 \
  -p 8001:8001 \
  redis/redis-stack:latest
```

Check running containers:

```bash
docker ps
```

Open Redis CLI:

```bash
docker exec -it redis-stack redis-cli
```

---

# Redis Key Naming Convention

Good:

```text
user:1
user:2

game:101
game:102

session:abc123
```

Bad:

```text
1
abc
test
```

Always use:

```text
entity:id
```

Examples:

```text
user:1
game:100
room:55
player:10
```

---

# Redis Data Types

Redis is not just strings.

It supports:

1. Strings
2. Hashes
3. Lists
4. Sets
5. Sorted Sets
6. Streams

---

# 1. Strings

Most basic type.

---

## Store Value

```bash
SET user:1 "Ravi"
```

Output:

```bash
OK
```

---

## Read Value

```bash
GET user:1
```

Output:

```bash
"Ravi"
```

---

## Update Value

```bash
SET user:1 "Piyush"
```

---

## Delete Value

```bash
DEL user:1
```

---

## Check Exists

```bash
EXISTS user:1
```

---

## Set Expiry

Store for 60 seconds.

```bash
SET session:123 "loggedin" EX 60
```

---

## TTL

```bash
TTL session:123
```

---

# Strings in Node.js

Install:

```bash
npm i redis
```

Connect:

```js
// connection module
const Redis = require('ioredis');
const redis = new Redis();
redis.on('connect', () => {
  console.log('Connected to Redis');
});

redis.on('error', (err) => {
  console.error('Redis error:', err);
});

module.exports = redis;


// Other module in which we work with redis
const redis = require('./redisClient');
async function redisService(){
    const game = await redis.set("user:1",ravi);
    console.log(game)
}

```

Store:

```js
await client.set("user:1", "Ravi");
```

Read:

```js
const user = await client.get("user:1");

console.log(user);
```

---

# Example: Store Chess Game

```js
await client.set(
  "game:1",
  JSON.stringify({
    p1: "Ravi",
    p2: "Pago",
    move: 1
  })
);
```

Read:

```js
const game = JSON.parse(
  await client.get("game:1")
);

console.log(game);
```

Output:

```js
{
  p1: "Ravi",
  p2: "Pago",
  move: 1
}
```

---

# 2. Hashes

Best for objects.

Instead of:

```js
{
  id:1,
  name:"Ravi",
  rating:1200
}
```

Store as:

```bash
HSET user:1 name Ravi rating 1200
```

---

## Add Fields

```bash
HSET user:1 name Ravi rating 1200 country India
```

---

## Get One Field

```bash
HGET user:1 name
```

Output:

```bash
"Ravi"
```

---

## Get All Fields

```bash
HGETALL user:1
```

Output:

```bash
name
Ravi

rating
1200

country
India
```

---

## Delete Field

```bash
HDEL user:1 country
```

---

# Hashes in Node.js

Store:

```js
await client.hSet("user:1", {
  name: "Ravi",
  rating: 1200,
  country: "India"
});
```

Read:

```js
const user =
  await client.hGetAll("user:1");

console.log(user);
```

---

# When to Use Hashes?

Use Hashes when:

```js
User
Product
Player
Profile
Settings
```

Basically any object.

---

# 3. Lists

Lists work like arrays.

---

## Add at Beginning

```bash
LPUSH moves e2e4
```

---

## Add at End

```bash
RPUSH moves e7e5
```

---

## Get All

```bash
LRANGE moves 0 -1
```

Output:

```bash
e2e4
e7e5
```

---

## Remove First

```bash
LPOP moves
```

---

## Remove Last

```bash
RPOP moves
```

---

# Chess Move History

```bash
RPUSH game:1:moves e2e4
RPUSH game:1:moves e7e5
RPUSH game:1:moves g1f3
```

Get history:

```bash
LRANGE game:1:moves 0 -1
```

---

# 4. Sets

Sets contain unique values.

No duplicates.

---

Add:

```bash
SADD onlinePlayers ravi
```

```bash
SADD onlinePlayers piyush
```

```bash
SADD onlinePlayers ravi
```

Still only one Ravi exists.

---

Get all:

```bash
SMEMBERS onlinePlayers
```

---

Check exists:

```bash
SISMEMBER onlinePlayers ravi
```

---

Remove:

```bash
SREM onlinePlayers ravi
```

---

# Use Cases

```text
Online Players
Active Users
Game Rooms
Permissions
```

---

# 5. Sorted Sets

Stores score + value.

Perfect for leaderboards.

---

Add:

```bash
ZADD leaderboard 1200 ravi
```

```bash
ZADD leaderboard 1500 piyush
```

```bash
ZADD leaderboard 1800 aman
```

---

Get ranking:

```bash
ZRANGE leaderboard 0 -1 WITHSCORES
```

---

Highest Score First

```bash
ZREVRANGE leaderboard 0 -1 WITHSCORES
```

---

# Leaderboard Example

```text
1 Aman    1800
2 Piyush  1500
3 Ravi    1200
```

---

# Chess Project Structure

For your multiplayer chess game:

---

## Game State

```text
game:101
```

```json
{
  "fen":"rnbqkbnr...",
  "turn":"white"
}
```

Stored using:

```js
SET game:101 "{...}"
```

---

## Moves

```text
game:101:moves
```

Stored in List:

```bash
RPUSH game:101:moves e2e4
```

---

## Players Online

```text
onlinePlayers
```

Stored in Set:

```bash
SADD onlinePlayers ravi
```

---

## Rating Leaderboard

```text
leaderboard
```

Stored in Sorted Set:

```bash
ZADD leaderboard 1800 ravi
```

---

# Useful Commands

See all keys:

```bash
KEYS *
```

Count keys:

```bash
DBSIZE
```

Delete key:

```bash
DEL user:1
```

Flush database:

```bash
FLUSHALL
```

⚠️ Deletes everything.

---

# Persistence

Redis stores in RAM.

To avoid losing data:

Redis supports:

### RDB Snapshot

```bash
SAVE
```

### AOF

Logs every write operation.

Production systems usually enable persistence.

---

# Redis vs MongoDB

| Feature | Redis | MongoDB |
|----------|---------|----------|
| Speed | Extremely Fast | Fast |
| Storage | RAM | Disk |
| Querying | Basic | Powerful |
| Joins | No | No |
| Best For | Cache, Sessions, Games | Main Database |

---

