# Methods

* Methods allow you to define a behavior associated with a specific type, much like methods in OOP laguages.
* An example could be a trangle object. You define an instance of the triange, then can call a method like `triangle.calculate_volume()`.
* The type the methods acts on is called the receiver of which there are two types:
    1. Value Receiver `func (r Rectangle) Area() int {` - 
    2. Pointer Receiver - `func (r *Rectangle) Area() int {` - 
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