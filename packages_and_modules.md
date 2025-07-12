# Packages and Modules

## Overview
Definitions:
* A package in Go is a named collection of one or more related `.go` files. Multiple files can define same package directive to extend a package definition across multiple files.
* A module in Go is a tree of Go source files with a `go.mod` file in the tree's root directory.

## Creating a Go Module
A directory tree of Go source code becomes a module when there’s a valid `go.mod` file in it. The easiest way to create a `go.mod` file is using the `go mod` command, passing a unique module path as the final argument:
```bash
go mod init module_name.bet365.net
```

An example of a generated `go.mod` file:
```bash
go 1.21

require (
    github.com/learning-go-book-2e/formatter v0.0.0-20220918024742-0f9fc26af37c
    github.com/shopspring/decimal v1.3.1
)

require (
    github.com/fatih/color v1.13.0 // indirect
    github.com/mattn/go-colorable v0.1.9 // indirect
    github.com/mattn/go-isatty v0.0.14 // indirect
    golang.org/x/sys v0.0.0-20210630005230-0f9fa26af87c // indirect
)
```
The above term `indirect` refers to dependencies that are required by your project's direct dependencies but are not directly imported or used by your code.

## Importing Packages
Any identifier at package-level that starts with an upper-case letter is exported from a package, else it is internal to the package. 

Importing packages is easy:
```go
import (
    "fmt"
    "github.com/fatih/color"
    "github.com/learning-go-book-2e/package_example/do-format"
)
```

## Name Collisions
If two imported packages share the same name, you can use `crand` to to override the name of one of the imported packages:
```go
import (
    crand "crypto/rand"
    "encoding/binary"
    "fmt"
    "math/rand"
)
```

## The Internal Package
When you create a package called `internal`, the exported identifiers in that package and its subpackages are accessible only to the direct parent package of `internal` and the sibling packages of `internal`:
```text
my_files
-- foo
------ foo.go
------ internal
---------- interal.go
------ sibling
---------- sibling.go
```

## The init Function
In Go, each source file can define its own init function to setup a required state:
```go
package main

func init() {
    fmt.Println("This will get called on main initialization")
}

func main() {
    fmt.Println("My Wonderful Go Program")
}
```

The primary use of init functions is to initialize package-level variables that can’t be configured in a single assignment.

## Vendoring
The `go mod vendor` command constructs a directory named `vendor` in the main module’s root directory containing copies of all packages needed to build and test packages in the main module.

When using modules, the `go` command typically satisfies dependencies by downloading modules from their sources into the module cache, then loading packages from those downloaded copies. Vendoring can be used to ensure that all files used for a build are stored in a single repository.

## Private Repositories
Some people object to sending requests for third-party libraries to Google. There are a few options:
* You can disable proxying entirely by setting the `GOPROXY` environment variable to `direct` i.e. `GOPROXY=direct`. You’ll download modules directly from their repositories, but if you depend on a version that’s removed from the repository, you won’t be able to access it.
* You can run your own proxy server. Both `Artifactory` and `Sonatype` have Go Proxy server support built into their enterprise repository products. Install one of these products on your network and then point `GOPROXY` to the URL.
