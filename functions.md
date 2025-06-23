# Functions

## Variadic Parameters
Example function with variadic parameters: 
```go
func sum(base int, vals ...int) int {
	var result int = base
	for _, v := range vals {
		result += v
	}
	return result
}
```

## Multiple Return Values
Go allows multiple return parameters:
```go
func divAndRemainder(num, denom int) (int, int, error) {
    if denom == 0 {
        return 0, 0, errors.New("cannot divide by zero")
    }
    return num / denom, num % denom, nil
}
```
which can be assigned using:
```go
result, remainder, err := divAndRemainder(1, 3)
```
If you want to ignore returns, it is idiomatic use a `_`:
```go
result, _, err := divAndRemainder(1, 3)
```

Go allows you to initialise return values:
```go
func divAndRemainder(num, denom int) (result int, remainder int, err error)
```
Although these add good documentation, one issue with these values are not required to be returned. They mearly convey the intent.

## Ammonymous Functions
Functions are values in Go, so you can assign variables to hold them. This applies to standard defined functions and annonymous functions.

Annonymous function example:
```go
f := func(j int) {
    fmt.Println("printing", j, "from inside of an anonymous function")
}
for i := 0; i < 5; i++ {
    f(i)
}
```

## Closure Functions
Functions can be called inside other functions, and modify variables of the outer function:
```go
func main() {
    a := 20
    f := func() {
        fmt.Println(a)
        a := 30
        fmt.Println(a)
    }
    f()
        fmt.Println(a)
    }
```

## Defer
Go uses the `defer` keyword schedule a function call to be run after the function completes, just before it returns.

`defer` is used:
* To ensure cleanup actions happen
* To improve readability and maintainability
* To handle panics and recovered gracefully

Example:
```go
unc readFile(filename string) {
    file, err := os.Open(filename)
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()  // Will always be called
}
```

Defer execuses in using LIFO (stack-like). Keep this in mind when using `defer` multiple times in a function.

