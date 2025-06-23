# General Notes

## Assignment
You can use `+=`, `-=`, `*=`, `/=`, and `%=`.

## Number Declarations
They can have underscores in them, e.g. `var x int = 1_000_000`.

## Strings
Strings in Go are immutable. You can reassign the value, but you cannot change the value that is assigned to it.

## What is True?
In go, only the `bool` type `true` value is considered to be true. This means `1` or the presence of items in an array connot be used as as true. The result of a comparison must be used, e.g. `var == 1`:

## Variable declarations
Individually:
```go
var x int = 10
var x = 10
var x int
var x, y int = 10, 20
var x, y int
var x, y = 10, "hello"
x := 10
x, y := 10, "hello"
```

En mass, they can be created like this:
```go
var (
 x int
 y = 20
 z int = 30
 d, e = 40, "hello"
 f, g string
)
```
const x = 10
All of the following assignments are legal:
var y int = x
var z float64 = x
var d byte = x

## Const Variables
Constants can be typed or untyped:
```go
// typed
const hello int64 = 22

// untyped
const q = 10
var a int = q
var b float64 = q
```

## Type Conversions
Type conversions in Go work like they do in Python:
```go
var a int = 2
var b float64 = 42.2
var c float64 = float64(a) + y
var d int = x + int(b)
fmt.Println(c, d)
```

## Division
* Dividing by 0 returns `+Inf` or `-Inf`.
* Dividing a floating-point variable set to 0 by 0 returns NaN (Not a Number).

## Arrays
To declare an array:
```go
var arr = [...]int{10, 20, 30}
```
Array values are accessed and updated the same as Python.

## Strings
Values of a string can be extracted by indexing:
```go
var s string = "Hello world!"
var b byte = s[3]
```