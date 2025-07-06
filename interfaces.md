# Interfaces
Interfaces are named collections of method signatures which can be accepted by methods and functions.

Interfaces facilitate the DRY principle: Don't Repeat Yourself.

[“Accept interfaces, return structs”](https://bryanftan.medium.com/accept-interfaces-return-structs-in-go-d4cab29a301b)

Example:
```go
type geometry interface {
    area() float64
    perim() float64
}
```

Interfaces are used:
* To help reduce duplication or boilerplate code.
* To make it easier to use mocks instead of real objects in unit tests.
* As an architectural tool, to help enforce decoupling between parts of your codebase.

A good example of using an interface:
```go
package main

import (
    "fmt"
    "math"
)

type geometry interface {
    area() float64
    perim() float64
}

type rect struct {
    width, height float64
}
type circle struct {
    radius float64
}

func (r rect) area() float64 {
    return r.width * r.height
}
func (r rect) perim() float64 {
    return 2*r.width + 2*r.height
}

func (c circle) area() float64 {
    return math.Pi * c.radius * c.radius
}
func (c circle) perim() float64 {
    return 2 * math.Pi * c.radius
}

func measure(g geometry) {
    fmt.Println(g)
    fmt.Println(g.area())
    fmt.Println(g.perim())
}

func detectCircle(g geometry) {
    if c, ok := g.(circle); ok {
        fmt.Println("circle with radius", c.radius)
    }
}

func main() {
    r := rect{width: 3, height: 4}
    c := circle{radius: 5}

    measure(r)
    measure(c)

    detectCircle(r)
    detectCircle(c)
}
```
In this example, the `circle` and `rect` struct types both implement the `geometry` interface so we can use instances of these structs as arguments to `measure`.

## Mocking
Interfaces make unit testing easier. You can define mock implementations for interfaces to isolate parts of your code.

Example:
```go
type DB interface {
    GetUser(id int) User
}

type MockDB struct{}
func (m MockDB) GetUser(id int) User {
    return User{ID: id, Name: "Test User"}
}
```
The `MockDB` interface can be defined as an empty struct `type MockDB struct{}` and used as a mock for the `GetUser` function.
