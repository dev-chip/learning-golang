# Goroutines

## Overview
A goroutine is a lightweight, concurrent function managed by the Go runtime. It shares the same address space with other goroutines and is far more efficient than OS threads due to its small starting stack and dynamic stack growth.

Definitions:
* **Process** - an instance of a program running on an OS which has resources that other processes can’t access. A process is composed of one or more threads.
* **Thread** - a unit of execution that is given some time to run by the OS. Threads within a process share access to resources.

Goroutines are lightweight because of their:
* **Tiny Stack Size** - A goroutine starts with only 2 KB of stack space. Traditional threads need 1–2 MB.
* **User-Space Scheduling** - The Go runtime, not the OS, handles goroutine scheduling (using an M:N scheduler).
* **Non-blocking I/O** - If one goroutine blocks (e.g. file read), others continue to execute.
* **Communication via Channels** - Instead of using locks/mutexes like in thread-based languages, Go encourages channel-based communication.

Starting a Goroutines is as easy as:
```go
go f(x, y, z)
```

If a goroutine doesn’t exit, all the memory remains allocated and is not garbage collected. This is called a goroutine leak.

Any values returned by a Goroutine are ignored.

## Channels

### Channels Overview
A channel can be decalared like this:
```go
ch := make(chan int)
```

The `<-` operator is used to interact with a channel:
```go
a := <-ch   // reads a value from ch and assigns it to a
ch <- b     // write the value in b to ch
```

If multiple goroutines are reading from the same channel, each channel value will be read by only one of them.

When passing a channel to a function, use an arrow before the `chan` keyword `ch <-chan int` to indicate that the goroutine only reads from the channel, or after the `chan` keyword `ch chan<- int` to indicate that the goroutine only writes to the channel.

You can use `for-range` loops to to read from a channel:
```go
for v := range ch {
    fmt.Println(v)
}
```

### Closing a Channel
When you’re done writing to a channel, you close it using the built-in close function:
```go
close(ch)
```
Once a channel is closed, any attempts to write to it or close it again will panic. By contrast, any attemps to read from it will be successful.

You can check if a channel has been closed using the `comma ok` idiom. If `ok` is `true`, the channel is open:
```go
v, ok := <-ch
```

### Channel Buffers
By default, channels are unbuffered. This means that a write to an open, unbuffered channel causes the writing goroutine to pause until another goroutine reads from the same channel.  Likewise, a read from an open, unbuffered channel causes the reading goroutine to pause until another goroutine writes to the same channel. This means you cannot write to or read from an unbuffered channel without at least two concurrently running goroutines.

Go also has buffered channels. These channels buffer a limited number of writes without blocking. If the buffer fills before there are any reads from the channel, a subsequent write to the channel pauses the writing goroutine until the channel is read.

Create a buffered channel by specifying the channel's capacity:
```go
ch := make(chan int, 10)
```

## Select
Go’s `select` lets you wait on multiple channel operations:
```go
func main() {
    c1 := make(chan string)
    c2 := make(chan string)

    go func() {
        time.Sleep(1 * time.Second)
        c1 <- "one"
    }()
    go func() {
        time.Sleep(2 * time.Second)
        c2 <- "two"
    }()

    for range 2 {
        select {
        case msg1 := <-c1:
            fmt.Println("received", msg1)
        case msg2 := <-c2:
            fmt.Println("received", msg2)
        }
    }
}
```

If you implement a nonblocking read or write on a channel, use a select with a `default`. The following code does not wait if there’s no value to
read in `ch`; it immediately executes the `default`:
```go
select {
case v := <-ch:
    fmt.Println("read from ch:", v)
default:
    fmt.Println("no value written to ch")
}
```

Closed channels are always ready to read and return the zero value of the channel's type along with `ok = false`. This causes an issue for a select statement, which will always match on reading a closed channel, wasting a lot of time. You can avoid this by setting the channel to `nil` once it's closed:
```go
for count := 0; count < 2; {
    select {
    case v, ok := <-in:
        if !ok {
            in = nil
            count++
            continue
        }
    case v, ok := <-in2:
        if !ok {
            in2 = nil
            count++
            continue
        }
    }
}
```

## Context
Goroutines can be cancelled early using `context`.

Timeout:
```go
ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
defer cancel()  // Always cancel to avoid context leaks
```

Cancel manually using the built-in `cancel` function:
```go
ctx, cancel := context.WithCancel(context.Background())
go func() {
    time.Sleep(1 * time.Second)
    cancel()   // Manually cancel
}()
```

A function that accepts a context should always accept it as the first parameter:
```go
func fetchData(ctx context.Context) error {
    select {
    case <-time.After(3 * time.Second):
        fmt.Println("data fetched")
        return nil
    case <-ctx.Done():
        return ctx.Err() // timeout or cancel
    }
}
```

## WaitGroup
If you are waiting for several goroutines to finish, you need to use a `WaitGroup`:
```go
func worker(id int) {
    fmt.Printf("Worker %d starting\n", id)
    time.Sleep(time.Second)
    fmt.Printf("Worker %d done\n", id)
}

func main() {
    var wg sync.WaitGroup

    for i := 1; i <= 5; i++ {
        wg.Add(1)

        go func() {
            defer wg.Done()
            worker(i)
        }()
    }

    wg.Wait()
}
```

WaitGroups are handy, but only when you have something to clean up (like closing a channel they all write to) after all your worker goroutines exit.

## Once
The `sync` package includes a type called `Once` which is useful for lazy loading, and a good alternative to `init` if you don't want to slow-down the startup of your program:
```go
var parser SlowComplicatedParser
var once sync.Once

func Parse(dataToParse string) string {
    once.Do(func() {
        parser = initParser()
    })
 return parser.Parse(dataToParse)
}
```

