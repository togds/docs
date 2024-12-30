# 配置文件的读取

viper  可以读取各种格式的配置文件

yaml

案例

```
package config

import (
    "togds/internal/model"

    "github.com/spf13/viper"
)

// var Cfg = viper.New()

var AppConfig model.Config

func Init() {
    viper.SetConfigName("config")
    viper.SetConfigType("yaml")
    viper.AddConfigPath("./configs")
    if err := viper.ReadInConfig(); err != nil {
        panic(err)
    }
    if err := viper.Unmarshal(&AppConfig); err != nil {
        panic(err)
    }
}
func GetGlobalConfig() model.Config {
    return AppConfig
}
```

按照下面的方式就可以使用了

```
    config.Init()
    globalConfig := config.GetGlobalConfig()
```