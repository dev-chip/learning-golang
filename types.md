## Go Types

## Basic Types
| Type                      | Description                             | Example                  |
| ------------------------- | --------------------------------------- | ------------------------ |
| `bool`                    | Boolean                                 | `true`, `false`          |
| `string`                  | UTF-8 string                            | `"hello"`                |
| `int`                     | Signed integer (arch-specific)          | `42`                     |
| `int8` – `int64`          | Signed integers                         | `int32`, `int64`         |
| `uint8` – `uint64`        | Unsigned integers                       | `uint8` (byte), `uint64` |
| `byte`                    | Alias for `uint8`                       | `var b byte = 255`       |
| `rune`                    | Alias for `int32`, a Unicode code point | `'ñ'`                    |
| `float32`, `float64`      | Floating-point numbers                  | `3.14`                   |
| `complex64`, `complex128` | Complex numbers (who cares?!)           | `complex(1, 2)`          |

## Aggregate Types
| Type       | Description                | Example                  |
| ---------- | -------------------------- | ------------------------ |
|   Array    | Fixed-size sequence        | `[3]int{1, 2, 3}`        |
|   Slice    | Dynamic-size array-like    | `[]int{1, 2, 3}`         |
|   Map      | Key-value pairs            | `map[string]int{"a": 1}` |
|   Struct   | Collection of named fields | `struct { Name string }` |

## Reference & Composite Types
| Type          | Description                | Example                  |
| ------------- | -------------------------- | ------------------------ |
|   Pointer     | Points to a memory address | `*int`, `&x`             |
|   Function    | First-class functions      | `func(int) string`       |
|   Channel     | Used for concurrency       | `chan int`, `<-chan int` |
|   Interface   | Abstract type for methods  | `interface{}` or custom  |
