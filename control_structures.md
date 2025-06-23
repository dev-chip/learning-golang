# Control Structures

## If
Example:
```go
n := rand.Intn(12)
if n == 0 {
    fmt.Println("I'm zero")
} else if n > 6 {
    fmt.Println("larger than 6", n)
} else {
    fmt.Println("less than or equal to 5, but not zero", n)
}
```

Variables can be created only for the scope of the if statement:
```go
if n := rand.Intn(12); n == 0 {
    fmt.Println("I'm zero")
} else if n > 6 {
    fmt.Println("larger than 6", n)
} else {
    fmt.Println("less than or equal to 5, but not zero", n)
}
```

## For
C-family style where `:=` must be used as `var` is not legal here:
```go
for i := 0; i < 10; i++ {
    fmt.Println(i)
}
```

You do not need to specify all 3 statements of the `for` statement:
```go
i := 1
for i < 10 {
    fmt.Println(i)
    i = i + 1
}
```

You can specify no statements at all for an infinate loop:
```go
for {
    fmt.Println("Hello World!")
}
```

The `break` and `continue` build-in keywords can be used as they are in Python and the C-family.

Enumeration is very similar to Python:
```go
vals := []int{1, 34, 21, 12, 74, 50}
for i, value := range vals {
    fmt.Println(i, value)
}
```

This can also be used for maps, where you can also specify just to loop the keys and not the values:
```go
dogs := map[string]bool{"Shepard": true, "Bulldog": true, "Terrier": true}
for k := range dogs {
    fmt.Println(k)
}
```
The order of a 'for-range' iteration over a map vary each time the map is looped over. This is to make DoS attacks harder and prevent bugs that occur when programmers believe a map's order is fixed.

Go allows you to use labels when using `continue` or `break` which you have nested for loops:
```go
func main() {
    slice1 := []string{"hello", "world!"}
outer:
    for _, v := range slice1 {
        for i, r := range v {
            fmt.Println(i, r, string(r))
                if r == 'l' {
                continue outer
            }
        }
        fmt.Println()
    }
}
```

## Switch
Go allows switch statements to use boolean comparison, like with `case wordLen > 5:` below:
```go
cities := []string{"Hull", "Liverpool", "Manchester", "Stoke"}
for _, word := range words {
    switch size := len(word); size {
        case 1, 2, 3, 4:
            fmt.Println(word, "is a short word!")
        case 5:
            wordLen := len(word)
            fmt.Println(word, "is exactly the right length:", wordLen)
        case wordLen > 5:
        default:
            fmt.Println(word, "is a long word!")
    }
}
```

## Goto
As per the C-family, never fucking use this.

