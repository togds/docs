# 连接mysql
### 定义一个全局变量
global.go
```
var (
    DB *sql.DB
)
```
### 定义初始化配置
```
package db

import (
	"fmt"
	"togds/pkg/config"

	"gorm.io/driver/mysql"
	"gorm.io/gorm"
)

// 定义全局的db对象，我们执行数据库操作主要通过他实现。
var _db *gorm.DB

// 包初始化函数，golang特性，每个包初始化的时候会自动执行init函数，这里用来初始化gorm。
func init() {
	// globalConfig := config.GetGlobalConfig()

	config.Init()
	globalConfig := config.GetGlobalConfig()
	dsn := fmt.Sprintf("%s:%s@tcp(%s:%d)/%s?charset=utf8&parseTime=True&loc=Local",
		globalConfig.Mysql.User, globalConfig.Mysql.Password, globalConfig.Mysql.Host, globalConfig.Mysql.Port, globalConfig.Mysql.Database)
	// dsn := fmt.Sprintf("%s:%s@tcp(%s:%d)/%s?charset=utf8&parseTime=True&loc=Local",
	// 	"togds", "Minrray@123", "10.1.22.127", 3306, "togds")
	// 声明err变量，下面不能使用:=赋值运算符，否则_db变量会当成局部变量，导致外部无法访问_db变量
	var err error
	//连接MYSQL, 获得DB类型实例，用于后面的数据库读写操作。
	_db, err = gorm.Open(mysql.Open(dsn), &gorm.Config{})
	if err != nil {
		panic("连接数据库失败, error=" + err.Error())
	}

	//获取通用数据库对象 sql.DB ，然后使用其提供的功能
	sqlDB, _ := _db.DB()

	//设置数据库连接池参数
	sqlDB.SetMaxOpenConns(100) //设置数据库连接池最大连接数
	sqlDB.SetMaxIdleConns(20)  //连接池最大允许的空闲连接数，如果没有sql任务需要执行的连接数大于20，超过的连接会被连接池关闭。
}

// 获取gorm db对象，其他包需要执行数据库查询的时候，只要通过tools.getDB()获取db对象即可。
// 不用担心协程并发使用同样的db对象会共用同一个连接，db对象在调用他的方法的时候会从数据库连接池中获取新的连接
func GetDB() *gorm.DB {
	return _db
}
```

注意：init函数会在main函数之前执行，所以可以在init函数中初始化数据库连接。

### 定义model
```
type User struct {
	ID            int       `gorm:"id"`
	Username      string    `gorm:"type:varchar(20);not null"`
	Password      string    `gorm:"type:varchar(20);not null"`
	Email         string    `gorm:"type:varchar(20);not null"`
	Phone         string    `gorm:"type:varchar(20);not null"`
	Createdata    time.Time `gorm:"type:datetime;not null"`
	Changetime    time.Time `gorm:"type:datetime;not null"`
	Delete_status int       `gorm:"type:int;not null"`
	IS_active     int       `gorm:"type:int;not null"`
}
```

### 使用
```
u := model.User{}
global.DB.Debug().Where("username =?", username).First(&u)
```