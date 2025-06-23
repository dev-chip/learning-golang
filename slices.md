# Slices

Delacing a slice:
```go
var slice = []int{10, 20, 30}
```

Similar to declaring an array:
```go
var arr = [...]int{10, 20, 30}
```

2D slice:
```go
var x [][]int
```

A slice isn’t comparable. Slices must be compared using: 
```go
x := []int{1, 2, 3, 4, 5}
y := []int{1, 2, 3, 4, 5}
z := []int{1, 2, 3, 4, 5, 6}
s := []string{"a", "b", "c"}
fmt.Println(slices.Equal(x, y)) // prints true
fmt.Println(slices.Equal(x, z)) // prints false
fmt.Println(slices.Equal(x, s)) // does not compile
```

Use the built-in function `len` to get the length of a slice:
```go
x := []int{1, 2, 3, 4, 5}
len(x)
```

Use the built-in function `append` to get add a value to a slice:
```go
x := []int{1, 2, 3, 4, 5}
x = append(x, 4, 5, 10)
```

You can append once slice to another using `...`:
```go
x := []int{1, 2, 3, 4, 5}
y := []int{6, 7}
x = append(x, y...)
```

The Go runtime manages the capacity of slices as elements are appended and removed. Old memory is deallocated using Go's garbage collector. The capacity of a slice can be viewed using the built in function `cap` (largely unused), e.g. `cap(x)`. It's far more efficient to size a slice once by declaring it's intitial capacity.

use the built-in function `make` to specify the number of elements and capacity of a slice:
```go
x := make([]int, 0, 10)
```

Use the built-in function `clear` to get add a value to a slice:
```go
x := []int{1, 2, 3, 4, 5}
clear(x)
```

Use the built-in function `copy` to copy a slice:
```go
x := []int{1, 2, 3, 4}
y := make([]int, 4)
num := copy(y, x)
```
