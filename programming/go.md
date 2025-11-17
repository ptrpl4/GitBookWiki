# 🏃 Golang

#### links

- tour - [https://go.dev/tour](https://go.dev/tour)
- examples - [https://gobyexample.com/](https://gobyexample.com/)
- Language Specification - [https://go.dev/ref/spec](https://go.dev/ref/spec)
- sandbox - [https://go.dev/play/](https://go.dev/play/)
- course - https://stepik.org/lesson/526867/ (ru)

## Basics

### Installation

- https://go.dev/dl/

```bash
which go
>> /usr/local/go/bin/go

go version
>> go version go1.24.0 darwin/arm64
```

### New project init

#### init

```bash
go mod init myproject
```

`go.mod` - file will be created

- Go modules are project-based
- One `go.mod` controls all subfolders

file example:

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

### Types

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

var f float64 = 12.34
fmt.Println(f)
// 12.34
```

### Variables

Examples:

```go
// global
var foo int
foo = 32

// OR:
var foo int = 32

// OR:
var foo = 32

// many in one line
var one, two int = 1, 2

// within scope
func main() {
    foo := 32 
}
```

If a variable is not initialized at the time of declaration, it will receive its zero value. Each type has its own zero value: for int it’s `0`, for string it’s an empty string `""`, and for bool it’s `false`.

```go
var num int
var str string
var ok bool

fmt.Printf("%#v %#v %#v\n", num, str, ok)
// 0 "" false
```

### const

```go
const s string = "constant"
fmt.Println(s)
// constant

const n = 500000000
fmt.Println(n)
// 500000000

const ch = 'a'
fmt.Println(ch) // fmt.Printf("%c", ch) - to print as character
// 97 ('a' in ASCII)
```

### for

```go
// "while" example
i := 1
for i <= 3 {
    fmt.Println(i)
    i = i + 1
}
// 1
// 2
// 3

// "classic" for loop
for j := 7; j <= 9; j++ {
    fmt.Println(j)
}
// 7
// 8
// 9

// loop until n-1
const n = 10
for i := range n {
    fmt.Print(i, " ")
}
// 0 1 2 3 4 5 6 7 8 9

// not keeping counter during loop
const n = 10
for range n {
    fmt.Print(".")
}
// ..........

// untill break
for {
    fmt.Println("loop")
    break
}
// loop

// with internal conditions
for n := 0; n <= 5; n++ {
    if n%2 == 0 {
        continue
    }
    fmt.Println(n)
}
// 1
// 3
// 5
```

### If/else

examples

```go
if 7%2 == 0 {
    fmt.Println("7 is even")
} else {
    fmt.Println("7 is odd")
}
// 7 is odd
```

 `if` without `else`

```go
if 8%4 == 0 {
    fmt.Println("8 is divisible by 4")
}
// 8 is divisible by 4
```

`if` with expression

```go
if num := 9; num < 0 {
    fmt.Println(num, "is negative")
} else if num < 10 {
    fmt.Println(num, "has 1 digit")
} else {
    fmt.Println(num, "has multiple digits")
}
// 9 has 1 digit
```

### error handling

`_`  blank identifier. ignores second return value (error) . Code won't handle any potential errors

```go
d, _ := time.ParseDuration(s)
```

### goroutine

Functions launched with `go` are called goroutine. They're extremely lightweight tasks managed by Go's runtime, which schedules many concurrent ones across OS threads on CPU cores

```go
func main() {
    go say(1, "go is awesome")
    go say(2, "cats are cute")
}

func say(id int, phrase string) {
    for _, word := range strings.Fields(phrase) {
        fmt.Printf("Worker #%d says: %s...\n", id, word)
        dur := time.Duration(rand.Intn(100)) * time.Millisecond
        time.Sleep(dur)
    }
}
```

## Packages

### fmt

- [link](https://pkg.go.dev/fmt)

#### Print

```go
// Syntax: fmt.Print(values...)
fmt.Print("Hello")  // Output: Hello
fmt.Println("World") // Output: World (with a newline)\
```

#### Printf

inserts variables into a string based on a template

| Specifier | Description                  | Example Output        |
| --------- | ---------------------------- | --------------------- |
| `%d`      | Integer (base 10)            | `42`                  |
| `%s`      | String                       | `"Hello"`             |
| `%f`      | Floating point               | `3.141593`            |
| `%t`      | Boolean                      | `true` or `false`     |
| `%v`      | Default format for any Value | `123`, `"abc"`, etc.  |
| `%T`      | Type of the value            | `int`, `string`, etc. |

 `%#v`  - prints the value in a Go-syntax representation

```go
// fmt.Printf(format string, args ...interface{}) (n int, err error)

// templating
var sunny = true
fmt.Printf("%v is %T\n", sunny, sunny)
// true is bool

num := 42
    str := "Hello, Go!"
    arr := []int{1, 2, 3}
    m := map[string]int{"one": 1, "two": 2}

    fmt.Printf("%#v\n", num)  // Outputs: 42
    fmt.Printf("%#v\n", str)   // Outputs: "Hello, Go!"
    fmt.Printf("%#v\n", arr)   // Outputs: []int{1, 2, 3}
    fmt.Printf("%#v\n", m)     // Outputs: map[string]int{"one": 1, "two": 2}
```

#### defer

keyword that delays the execution of a statement until the surrounding function returns

```go
func TestMain(m *testing.M) {
  fmt.Println("1. Start")
  defer fmt.Println("3. End (deferred)")
  fmt.Println("2. Middle")
  os.Exit(0)
}

// Output:
// 1. Start
// 2. Middle
// 3. End (deferred)
```

#### Scan

scan input

```go
func main() {
	var source string
	var times int
	fmt.Scan(&source, &times)
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

# send data to program
echo "1 1 4 5" | go run distance.go
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
