# Methods

## Overview
* Methods allow you to define a behavior associated with a specific type, much like methods in OOP laguages.
* An example could be a trangle object. You define an instance of the triange, then can call a method like `triangle.calculate_volume()`.
* The type the methods acts on is called the receiver of which there are two types:
    1. Value Receiver `func (r Rectangle) Area() int {` - receiver is copy of the variable passed to it, meaning any modifications to it will only affect the copy and not the original variable.
    2. Pointer Receiver - `func (r *Rectangle) Area() int {` - receiver points to, and can therefor access a variable, allowing it to be modified.
* Using pointer receivers is more efficient, but may require the use of mutexes in concurrent applications, so use of a pointer reciever or value receiver.

Example:
```go
type Rectangle struct {
    length, width int
}

// Method with a value receiver
func (r Rectangle) Area() int {
    return r.length * r.width
}

// Method with a pointer receiver
func (r *Rectangle) Scale(s int) {
    r.length *= s
    r.width *= s
}
```

* Methods can be called with `nil` pointer receivers, but `nil` value receivers will cause a panic. Often methods take advatage of `nil` receivers by setting an instantiation of the receiver.

## Embedded Fields
Go supports embedding of structs and interfaces to express a more seamless composition of types:
```go
type Manager struct {
    Employee
    Reports []Employee
}
```

If a method on an embedded field calls another method on the embedded field, and the containing struct has a method with the same name, the method on the embedded field is called:
```go
type Inner struct {
    A int
}

func (i Inner) IntPrinter(val int) string {
    return fmt.Sprintf("Inner: %d", val)
}

func (i Inner) Double() string {
    return i.IntPrinter(i.A * 2)
}

type Outer struct {
    Inner
    S string
}

func (o Outer) IntPrinter(val int) string {
    return fmt.Sprintf("Outer: %d", val)
}

func main() {
    o := Outer{
        Inner: Inner{
            A: 10,
        },
        S: "Hello",
    }
    fmt.Println(o.Double())
}
```
Running this code produces the output: `Inner: 20`

* Embeddeding is not inheritance.
