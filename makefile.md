## Makefile

Example makefile:
```Makefile
.DEFAULT_GOAL := build

.PHONY:fmt vet build
fmt:
    go fmt ./...
vet: fmt
    go vet ./...
build: vet
    go build
```

The `.PHONY` line keeps make from getting confused if a directory or file the project has the same name as a listed target.
