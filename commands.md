# Commands

## Summary
| Command     | Description                                         |
|------------:|----------------------------------------------------:|
| bug         | start a bug report                                  |
| build       | compile packages and dependencies                   |
| clean       | remove object files and cached files                |
| doc         | show documentation for package or symbol            |
| env         | print Go environment information                    |
| fix         | update packages to use new APIs                     |
| fmt         | gofmt (reformat) package sources                    |
| generate    | generate Go files by processing source              |
| get         | add dependencies to current module and install them |
| install     | compile and install packages and dependencies       |
| list        | list packages or modules                            |
| mod         | module maintenance                                  |
| work        | workspace maintenance                               |
| run         | compile and run Go program                          |
| telemetry   | manage telemetry data and settings                  |
| test        | test packages                                       |
| tool        | run specified go tool                               |
| version     | print Go version                                    |
| vet         | report likely mistakes in packages                  |

## go build
Compile compile packages and dependencies:
```bash
go build
```

## go fmt
Gofmt is a tool that automatically formats Go source code.

Go has one standard format, there are no style debates.

Inspect changes (Shows what would be changed):
```bash
gofmt -d myfile.go
```

Format a single file in place:
```bash
gofmt -w myfile.go
```

Format all .go files in a directory recursively:
```bash
gofmt -w .
```

The `-s` option simplifies implentations. Format and simplify all files:
```bash
gofmt -w -s .
```

## go fix
Gofix is a tool in Go that automatically rewrites code based on //go:fix directives, often used for updating deprecated APIs or migrating code to new packages.

### Example workflow
Inspect changes (shows what would be changed):
```bash
go tool fix -n myfile.go
```

Apply fixes:
```bash
go tool fix myfile.go
``` 

Fix an entire module:
```bash
go tool fix ./...
```

## go vet


