# Context

## Overview
The package `context` defines the `Context` type, which carries deadlines, cancellation signals, and other request-scoped values across API boundaries and between processes.

Common Use Cases for context:
* **Cancellation** - Cancel multiple goroutines. For example, when a client disconnects and you want to cancel all ongoing processing for that request.
* **Timeouts** - Timeout a process taking too long.
* **Passing Request-Scoped Values** - Store and access values like request IDs, user auth info, etc.

Timeout example:
```go
func main()
{
    ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
    defer cancel()

    resultCh := make(chan string)

    go func() {
        // Simulate a long task
        time.Sleep(3 * time.Second)
        resultCh <- "done"
    }()

    select {
    case <-ctx.Done():
        fmt.Println("Timeout or cancellation:", ctx.Err())
    case res := <-resultCh:
        fmt.Println("Result:", res)
    }
}
```

## Core Types and Functions
* `context.Context` - The interface that carries deadlines, cancellation signals, and values.
* `context.Background()` - The root context, often used at the top level of main() or server handlers.
* `context.TODO()` - Placeholder when you're not sure what context to use yet.
* `context.WithCancel(parent Context)` - Returns a derived context that can be cancelled.
* `context.WithTimeout(parent Context, timeout time.Duration)` - Adds a timeout to a context.
* `context.WithDeadline(parent Context, time.Time)` - Adds a specific deadline.
* `context.WithValue(parent Context, key, val)` - Attaches key-value data to the context.
