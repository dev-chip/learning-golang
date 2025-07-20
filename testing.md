# Testing

## Unit Tests
* Test files must end in `_test.go`
* Tests should be in the same package as the code or a `*_test` package
* Tests use the `testing` package
* Each test function must:
    * Start with `Test`
    * Have the signature `func (t *testing.T)`

Here is an example function in a file called `math.go`:
```go
package math

func Add(a, b int) int {
	return a + b
}
```

Here is an example test in a file called `math_test.go`:
```go
package math

import "testing"

func TestAdd(t *testing.T) {
	result := Add(2, 3)
	expected := 5

	if result != expected {
		t.Errorf("Add(2, 3) = %d; want %d", result, expected)
	}
}
```

For failing tests:
* Use `t.Errorf()` to log the error but continue the test.
* Use `t.Fatalf()` to log the error and stop the test immediately.


You should use the third-party module `go-cmp` in Go unit tests when you want deep equality comparisons, especially for complex types like structs, maps, or slices:
```go
package main

import (
	"testing"

	"github.com/google/go-cmp/cmp"
)

func TestSliceComparison(t *testing.T) {
	got := []int{1, 2, 3}
	want := []int{1, 2, 4}

	if diff := cmp.Diff(want, got); diff != "" {
		t.Errorf("Mismatch (-want +got):\n%s", diff)
	}
}
```

The above test will output the following:
```go
Mismatch (-want +got):
  []int{
  	1,
  	2,
  -	4,
  +	3,
  }
```

## Benchmark Tests
Go allows you to create benchmark tests. These aren't regular tests, and don't run when you command `go test`:
```go
func BenchmarkAdd(b *testing.B) {
	for i := 0; i < b.N; i++ {
		Add(1, 2)
	}
}
```

This command runs benchmarks across all packages recursively from the current directory:
```bash
go test ./… -bench=.
```

## Useful Commands
Additional useful commands:
* `go test -cover` - show code coverage.
* `go test -bench .` - run benchmarks.
* `go test -run TestAdd` - run a specific test.

## Integration Tests
There’s no strict rule for integration tests, but some best practices include:
1. Name the test file `something_integration_test.go` to differentiate it from unit tests.
2. Add `//go:build integration` to line 1 and `// +build integration` to line 2 of the file to exclude integration tests by default.
3. Run the integration test using `go test -tags=integration ./...`
