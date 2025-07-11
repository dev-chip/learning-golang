# Generics

## Overview
Generics are a way of writing code that is independent of the specific types being used.
* With generics, functions and types can be written to use any of a set of types.
* Generics are similar to templates in C++, and are also utilised in other languages such as Java.
* Generics use type parameters and type constraints.

Example:
```go
func Max[T constraints.Ordered](a, b T) T {
    if a > b {
        return a
    }
    return b
}
```
To summerise:
* `[T constraints.Ordered]` defines the generic as `T` and it's type contraints as `any`.
* `(a, b T)` defines the function parameters, which use the generic type `T`.
* The function returns the generic type `T`.

The `constraints.Ordered` restricts a type parameter to only types that support ordering operations, i.e. `<`, `<=`, `>`, `>=`. There are many standard constraints types from the `constraints` package, e.g. `constraints.Integer` which constrains a type parameters to all standard integer types.

The keyword `any` can be used to have no constraints on the permitted generic type:
```go
func Max[T any](a, b T) T {
    if a > b {
        return a
    }
    return b
}
```

Generics can also be used in `struct` and `interface` definitions:
```go
type Pair[T fmt.Stringer] struct {
    Val1 T
    Val2 T
}

type Differ[T any] interface {
    fmt.Stringer
    Diff(T) float64
}
```

TODO
One more thing needs to be represented with generics: operators. The divAndRemain
der function works fine with int, but using it with other integer types requires type
casting, and uint allows you to represent values that are too big for an int. If you
want to write a generic version of divAndRemainder, you need a way to specify that
you can use / and %. Go generics do that with a type element, which is composed of
one or more type terms within an interface:
```go
type Integer interface {
    int | int8 | int16 | int32 | int64 |
    uint | uint8 | uint16 | uint32 | uint64 | uintptr
}
```