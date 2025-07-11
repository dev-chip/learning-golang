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

## Type Constraint Interface 
Go generics allow you to define permitted type constraints with a type contraint interface:
```go
type Integer interface {
    int | int8 | int16 | int32 | int64 |
    uint | uint8 | uint16 | uint32 | uint64 | uintptr
}

func Add[T Integer](a, b T) T {
    return a + b
}
```
`|` is the union operator.

Go allows you to define your own types, e.g. `type MyInt int` which won't match against a standard type constraints unless you use `~` symbol before the type name in the interface: 
```go
type Integer interface {
 ~int | ~int8 | ~int16 | ~int32 | ~int64 |
 ~uint | ~uint8 | ~uint16 | ~uint32 | ~uint64 | ~uintptr
}
```
This allows a user-defined type to be matched against any type that shares the same underlying type.
