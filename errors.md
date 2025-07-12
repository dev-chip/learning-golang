# Errors

## Handling Errors
Go handles errors by returning a value of type error as the last return value for a function.

When a function executes as expected, `nil` is returned for the error parameter. If something goes wrong, an error value is returned instead. The calling function should check the error return value by comparing it to nil, handle the error, or return an error of its own.

To create and return an error using `errors.New`:
```go
func doubleEven(i int) (int, error) {
    if i % 2 != 0 {
        return 0, errors.New("only even numbers are processed")
    }
    return i * 2, nil
}

func main() {
    result, err := doubleEven(1)
    if err != nil {
        fmt.Println(err) // prints "only even numbers are processed"
    }
    fmt.Println(result)
}
```

To create and return an error created using variables, use `fmt.Errorf`:
```go
func doubleEven(i int) (int, error) {
    if i % 2 != 0 {
        return 0, fmt.Errorf("%d isn't an even number", i)
    }
    return i * 2, nil
}
```

Error messages should not be capitalized or end with punctuation or a newline.

## Custom Errors
`error` is a built-in interface that defines a single method:
```go
type error interface {
    Error() string
}
```

You can therefor define your own types that implement the error interface (which would have have an `Error()` method) and return them as the `error` type. For example 
```go
type argError struct {
    arg     int
    message string
}

func (e argError) Error() string {
    return fmt.Sprintf("%d - %s", e.arg, e.message)
}

func f(arg int) (int, error) {
    if arg == 42 {
        return -1, &argError{arg, "can't work with it"}
    }
    return arg + 3, nil
}
```

## Sentinel Errors
A sentinel error is a user defined variables that is used to signify a specific error condition.

Sentinel error indicate very specific events that you, as a developer, anticipate & identify as adequately important to define and specify. As such, they are declared at the package level and, in doing so, imply that your package functions may return these errors. This thereby commits you to maintain these errors as others depending on your package will be checking for them. Therefor, **be sure you need a sentinel error before you define one.**

Example using the `archive/zip` package:
```go
func main() {
    data := []byte("This is not a zip file")
    notAZipFile := bytes.NewReader(data)
    _, err := zip.NewReader(notAZipFile, int64(len(data)))
    if err == zip.ErrFormat {
        fmt.Println("Told you so")
    }
}
```

Exampe of a manually declared sentinal error:
```go
var ErrInvalidID = errors.New("invalid ID")
```
The above declaration doesn't use a `:=` because it's a package-level variable declaration, and `:=` can only be used inside functions.

## Wrapping Errors
You can wrap errors using `%w` to create an error whose formatted string includes the formatted string of another error:
```go
func fileChecker(name string) error {
    f, err := os.Open(name)
    if err != nil {
        return fmt.Errorf("in fileChecker: %w", err)
    }
    f.Close()
    return nil
}
```

Sometimes a function generates multiple errors that should be returned. For example, a function to validate the fields in a struct. `errors.Join` merges multiple errors, defined in a slice, into a single error:
```go
func ValidatePerson(p Person) error {
    var errs []error
    if len(p.FirstName) == 0 {
        errs = append(errs, errors.New("field FirstName cannot be empty"))
    }
    if len(p.LastName) == 0 {
        errs = append(errs, errors.New("field LastName cannot be empty"))
    }
    if p.Age < 0 {
        errs = append(errs, errors.New("field Age cannot be negative"))
    }
    if len(errs) > 0 {
        return errors.Join(errs...)
    }
    return nil
}
```

You can wrap multiple errors with the same message:
```go
func DoSomeThings(val1 int, val2 string) (_ string, err error) {
    defer func() {
        if err != nil {
            err = fmt.Errorf("in DoSomeThings: %w", err)
        }
    }()
    val3, err := doThing1(val1)
    if err != nil {
        return "", err
    }
    val4, err := doThing2(val2)
    if err != nil {
        return "", err
    }
    return doThing3(val3, val4)
}
```

`defer` can be used to wrap multiple error with the same message:
```go
func SendReportEmail(userID int, reportData []byte) (err error) {
	defer func() {
		if err != nil {
			err = fmt.Errorf("SendReportEmail failed: %w", err)
		}
	}()

	userEmail, err := lookupEmail(userID)
	if err != nil {
		return err
	}

	pdfReport, err := generatePDF(reportData)
	if err != nil {
		return err
	}

	err = sendEmail(userEmail, pdfReport)
	if err != nil {
		return err
	}

	return nil
}
```

## Is and As
You can use the `errors` package function `errors.Is` to check if a specific sentinal error was returned:
```go
func main() {
    err := fileChecker("not_here.txt")
    if err != nil {
        if errors.Is(err, os.ErrNotExist) {
            fmt.Println("That file doesn't exist")
        }
    }
}
```

You can use the `errors` package function `errors.AS` to check if a returned error matches a specific type. The first arguement is the error being examined, and the second is a pointer to a variable of the type that you are looking for:
```go
err := AFunctionThatReturnsAnError()
var myErr MyErr
if errors.As(err, &myErr) {
    fmt.Println(myErr.Codes)
}
```

## Panic and Recover
A `panic` usually happens due to a programming error. 

As soon as a panic happens, the current function exits immediately, and any defers start running. When those defers complete, the
defers attached to the calling function run, and so on, until main is reached. The program then exits with a message and a stack trace.

If any situations in your own program are unrecoverable, you can create your own panics:
```go
func doPanic(msg string) {
    panic(msg)
}
func main() {
    doPanic(os.Args[0])
}
```

You can capture a panic using `recover` to provide a more graceful shutdown or to prevent shutdown:
```go
func div60(i int) {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println(r)
        }
    }()
    fmt.Println(60 / i)
}

func main() {
    for _, val := range []int{1, 2, 0, 6} {
        div60(val)
    }
}
```

...which returns:
```text
60
30
runtime error: integer divide by zero
10
```

It is idiomatic in Go to not rely on `panic` and `recover`, because `recover` doesn’t make clear what could fail. `recover` just ensures that if something fails, you can print out a message and continue. Instead, idiomatic Go code should explicitly handle possible conditions rather than use `recover` to handle anything while saying nothing.

Using recover is recommended in one situation. If you are creating a library for third parties do not let panics escape the boundaries of your public API. If a panic is possible, a public function should use `recover` to convert a `panic` into an `error` and return it.
