# gin-swagger
go get github.com/lulouis/gin-swagger

## checked issue：https://github.com/swaggo/swag/issues/245 



Reference ： https://github.com/swaggo/gin-swagger
*************************************************
gin middleware to automatically generate RESTful API documentation with Swagger 2.0.

[![Travis branch](https://img.shields.io/travis/swaggo/gin-swagger/master.svg)](https://travis-ci.org/swaggo/gin-swagger)
[![Codecov branch](https://img.shields.io/codecov/c/github/swaggo/gin-swagger/master.svg)](https://codecov.io/gh/swaggo/gin-swagger)
[![Go Report Card](https://goreportcard.com/badge/github.com/swaggo/gin-swagger)](https://goreportcard.com/report/github.com/swaggo/gin-swagger)
[![GoDoc](https://godoc.org/github.com/swaggo/gin-swagger?status.svg)](https://godoc.org/github.com/swaggo/gin-swagger)


## Usage

### Start using it
1. Add comments to your API source code, [See Declarative Comments Format](https://swaggo.github.io/swaggo.io/declarative_comments_format/).
2. Download [Swag](https://github.com/swaggo/swag) for Go by using:
```sh
$ go get -u github.com/swaggo/swag/cmd/swag
```

3. Run the [Swag](https://github.com/swaggo/swag) in your Go project root folder which contains `main.go` file, [Swag](https://github.com/swaggo/swag) will parse comments and generate required files(`docs` folder and `docs/doc.go`).
```sh
$ swag init
```
4.Download [gin-swagger](https://github.com/swaggo/gin-swagger) by using:
```sh
$ go get -u github.com/swaggo/gin-swagger
$ go get -u github.com/swaggo/gin-swagger/swaggerFiles
```
And import following in your code:

```go
import "github.com/swaggo/gin-swagger" // gin-swagger middleware
import "github.com/swaggo/gin-swagger/swaggerFiles" // swagger embed files

```

### Canonical example:

```go
package main

import (
	"github.com/gin-gonic/gin"
	"github.com/swaggo/gin-swagger"
	"github.com/swaggo/gin-swagger/swaggerFiles"

	_ "./docs" // docs is generated by Swag CLI, you have to import it.
)

// @title Swagger Example API
// @version 1.0
// @description This is a sample server Petstore server.
// @termsOfService http://swagger.io/terms/

// @contact.name API Support
// @contact.url http://www.swagger.io/support
// @contact.email support@swagger.io

// @license.name Apache 2.0
// @license.url http://www.apache.org/licenses/LICENSE-2.0.html

// @host petstore.swagger.io
// @BasePath /v2
func main() {
	r := gin.New()
    
    // use ginSwagger middleware to 
	r.GET("/swagger/*any", ginSwagger.WrapHandler(swaggerFiles.Handler))

	r.Run()
}
```

5. Run it, and browser to http://localhost:8080/swagger/index.html, you can see Swagger 2.0 Api documents.

![swagger_index.html](https://user-images.githubusercontent.com/8943871/31943004-dd08a10e-b88c-11e7-9e77-19d2c759a586.png)

6. If you want to disable swagger when some environment variable is set, use `DisablingWrapHandler`

### Example with disabling:

```go
package main

import (
	"github.com/gin-gonic/gin"
	"github.com/swaggo/gin-swagger"
	"github.com/swaggo/gin-swagger/swaggerFiles"

	_ "./docs" // docs is generated by Swag CLI, you have to import it.
)

// @title Swagger Example API
// @version 1.0
// @description This is a sample server Petstore server.
// @termsOfService http://swagger.io/terms/

// @contact.name API Support
// @contact.url http://www.swagger.io/support
// @contact.email support@swagger.io

// @license.name Apache 2.0
// @license.url http://www.apache.org/licenses/LICENSE-2.0.html

// @host petstore.swagger.io
// @BasePath /v2
func main() {
	r := gin.New()
    
    // use ginSwagger middleware to 
	r.GET("/swagger/*any", ginSwagger.DisablingWrapHandler(swaggerFiles.Handler, "NAME_OF_ENV_VARIABLE"))

	r.Run()
}
```

Then, if you set envioment variable `NAME_OF_ENV_VARIABLE` to anything, `/swagger/*any`
will respond 404, just like when route unspecified.
