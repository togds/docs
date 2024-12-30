# swagger api 接口文档

### 安装

```
go get -u github.com/swaggo/swag/cmd/swag
go install github.com/swaggo/swag/cmd/swag@latest
```

### 配置

main.go

```
import (
        swaggerfiles "github.com/swaggo/files"
    ginSwagger "github.com/swaggo/gin-swagger"
)
// 设置swagger请求默认路由
docs.SwaggerInfo.BasePath = "/api/v1"
// 设置swagger路由
server.GET("/swagger/*any", ginSwagger.WrapHandler(swaggerfiles.Handler))
```

# 描述

| annotation           | description | example |
| -------------------- | ----------- | ------- |
| title                |             |         |
| version              |             |         |
| description          |             |         |
| tag.name             |             |         |
| tag.description      |             |         |
| tag.docs.url         |             |         |
| tag.docs.description |             |         |

| annotation               | description                                                                                                        | example                                                         |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------- |
| title                    | 必填。应用程序的标题                                                                                                         | // @title Swagger Example API                                   |
| version                  | api 版本                                                                                                             | // @version 1.0                                                 |
| description              | 描述                                                                                                                 | // @description This is a sample server celler server.          |
| tag.name                 | 标签的名称                                                                                                              | // @tag.name This is the name of the tag                        |
| tag.description          | 标签的描述                                                                                                              | // @tag.description Cool Description                            |
| tag.docs.url             | 标签外部文档的 URL                                                                                                        | // @tag.docs.url https://example.com                            |
| tag.docs.description     | 标签外部文档的描述                                                                                                          | // @tag.docs.description Best example documentation             |
| termsOfService           | API 的服务条款                                                                                                          | // @termsOfService http://swagger.io/terms/                     |
| contact.name             | 公开 API 的联系信息                                                                                                       | // @contact.name API Support                                    |
| contact.url              | 指向联系信息的 URL。必须采用 URL 格式                                                                                            | // @contact.url http://www.swagger.io/support                   |
| contact.email            | 联系邮箱                                                                                                               | // @contact.email support@swagger.io                            |
| license.name             | 必填项。API使用的许可证名称。                                                                                                   | // @license.name Apache 2.0                                     |
| license.url              | API 所用许可证的 URL。必须采用 URL 格式                                                                                         | // @license.url http://www.apache.org/licenses/LICENSE-2.0.html |
| host                     | 为 API 提供服务的主机                                                                                                      | // @host localhost:8080                                         |
| BasePath                 | 提供 API 的基本路径，这里只是个注释                                                                                               | // @BasePath /api/v1                                            |
| accept                   | 接受的请求方式                                                                                                            | // @accept json                                                 |
| produce                  | API 可以生成的 MIME 类型列表                                                                                                | // @produce json                                                |
| query.collection.format  | The default collection(array) param format in query,enums:csv,multi,pipes,tsv,ssv. If not set, csv is the default. | // @query.collection.format multi                               |
| schemes                  | The transfer protocol for the operation that separated by spaces.                                                  | // @schemes http https                                          |
| externalDocs.description | 外部文档的描述                                                                                                            | // @externalDocs.description OpenAPI                            |
| externalDocs.url         | 外部文档的 URL                                                                                                          | // @externalDocs.url https://swagger.io/resources/open-api/     |
| x-name                   | The extension key, must be start by x- and take only json value                                                    | // @x-example-key {"key": "value"}                              |