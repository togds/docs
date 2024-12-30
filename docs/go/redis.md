# 连接redis

### 初始化redis

global.go

```
var (
    // DB 是全局的数据库连接对象
    DB    = db.GetDB()
    Redis *redis.Client
)
```

redis.go

```
package redis

import (
    "context"
    "fmt"
    "time"
    "togds/pkg/config"

    "github.com/redis/go-redis/v9"
)

var redisClient *redis.Client
var ctx = context.Background()

// 设置reids值
func Set(key string, value string, time time.Duration) error {
    err := redisClient.Set(ctx, key, value, time).Err()
    if err != nil {
        return err
    }
    return nil
}

// 获取reids值
func Get(key string) (string, error) {
    value, err := redisClient.Get(ctx, key).Result()
    fmt.Println("00000", key)
    fmt.Println("1111", value)
    fmt.Println("2222", err)
    if err != nil {
        return "", err
    }
    return value, err
}

// 删除rediskey
func Del(key string) error {
    err := redisClient.Del(ctx, key).Err()
    if err != nil {
        return err
    }
    return nil
}

func RedisInit() {
    globalConfig := config.GetGlobalConfig()
    addr := fmt.Sprintf("%s:%d", globalConfig.Redis.Host, globalConfig.Redis.Port)
    client := redis.NewClient(&redis.Options{
        Addr:     addr,
        Password: globalConfig.Redis.Password,
        DB:       0,
    })

    timeout, goBack := context.WithTimeout(context.Background(), time.Second*3)
    defer goBack()
    _, err := client.Ping(timeout).Result()
    if err != nil {
        panic("redis初始化失败! " + err.Error())
    }
    fmt.Println("redis初始化成功!")
    redisClient = client
}

func Init() {
    fmt.Println("redis初始化...")
    RedisInit()
}
```

### 初始化redis

main.go

```
redis.Init()
```

### 使用

```
redis.Set(key, username, 10*time.Minute)
```