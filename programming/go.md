# 🏃 Go

#### links

- tour - [https://go.dev/tour](https://go.dev/tour)
- examples - [https://gobyexample.com/](https://gobyexample.com/)
- Language Specification - [https://go.dev/ref/spec](https://go.dev/ref/spec)
- sandbox - [https://go.dev/play/](https://go.dev/play/)

## Basics

### Installation

- https://go.dev/dl/

```bash
which go
# stdout > /usr/local/go/bin/go
go version
```

### New project init

#### init

```bash
go mod init myproject
```

`go.mod` - file will be created

- Go modules are project-based
- One `go.mod` controls all subfolders

```go.mod
module myproject

go 1.24.0
```

#### Get dependencies 

```bash
go get github.com/something/package
```

### Existing project

Best Practice Workflow

```bash
# clean dependencies
go mod tidy

# format code
go fmt ./...

# check for issues
go vet ./...

# run tests
go test ./...
```

## Language

### Code example

`./hello.go`

```go
package main // func main - entry point for go execution

import "fmt"

func main() {
    fmt.Println("hello everyone")
}

// line comment

/*
Multilinex comment
Thanks for reading!
*/
```

### Values

```go
fmt.Println("1+1 =", 1+1)
// 1+1 = 2

fmt.Println("5.0/2.0 =", 5.0/2.0)
// 5.0/2.0 = 2.5
fmt.Println(true && false)
// false

fmt.Println(true || false)
// true

fmt.Println(!true)
// false
fmt.Println("go" + "lang")
// golang
```

### Declaration

```go
foo := 32 // is short form of

var foo int
foo = 32

// OR:
var foo int = 32

// OR:
var foo = 32
```

## Commands

### run

```bash
# compile in temp folder, run and delete exe file
go run hello.go
# stdout > hello everyone
go run .

# show temp folder for exe and run 
go run -work main.go
# stdout > WORK=/var/folders/zv/yjxk1129awwqwe/T/go-build4283242323
# stdout > program output
```

### build

By default, Go builds for your current platform

```bash
# build in current folder
go build hello.go

ls
# stdout > hello	hello.go

# run exe
./hello
# stdout > hello everyone

# build as 'app'
go build -o app
```

### mod

 #### download
 
- Downloads all dependencies listed in go.mod
- Doesn't modify go.mod or go.sum, just ensures all packages are in local cache

 #### tidy
 
- Downloads missing dependencies
- Removes unused dependencies
- Updates go.mod and go.sum
- More comprehensive than download
- Recommended for cleanup

### env

```bash
# all env
go env 

# more useful
go env GOOS GOARCH GOROOT GOMOD GOENV

# GOPATH - outdated, logic replaced with go.mod files
```
