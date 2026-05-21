# go-srvc

**Simple, safe, modular service runner for Go.**

A small ecosystem for composing long-running Go services with a clean lifecycle.

[**go-srvc.com**](https://go-srvc.com) · [godoc](https://pkg.go.dev/github.com/go-srvc/srvc)

## Repositories

| Repo | What |
|---|---|
| [`srvc`](https://github.com/go-srvc/srvc) | The runner. One small `Module` interface (`ID`, `Init`, `Run`, `Stop`). Standard library only. |
| [`mods`](https://github.com/go-srvc/mods) | Ready-made modules: `httpmod`, `logmod`, `metermod`, `sigmod`, `sqlmod`, `sqlxmod`, `tickermod`, `tracemod`. |

## Why srvc

- **One small interface.** Every component implements `Init`, `Run`, `Stop`, `ID`. The runner sequences and supervises them so `main` stays a list of components.
- **Safe shutdown.** Panics are recovered. Errors from `Init`, `Run`, and `Stop` are aggregated. `Stop` runs in reverse order on every initialised module.
- **Zero deps in core.** `srvc` is standard library only. Pull in just the modules you need.
- **No zombie services.** When any `Run` returns, the whole service shuts down. Pair with `sigmod` for clean `SIGINT`/`SIGTERM` handling.

## Quick start

```go
package main

import (
	"fmt"
	"net/http"
	"os"

	"github.com/go-srvc/mods/httpmod"
	"github.com/go-srvc/mods/sigmod"
	"github.com/go-srvc/srvc"
)

func main() {
	srvc.RunAndExit(
		sigmod.New(os.Interrupt),
		httpmod.New(
			httpmod.WithAddr(":8080"),
			httpmod.WithHandler(http.HandlerFunc(hello)),
		),
	)
}

func hello(w http.ResponseWriter, r *http.Request) {
	fmt.Fprint(w, "hello, world")
}
```

```sh
go get github.com/go-srvc/srvc@latest
```

For a fuller example with structured logging, distributed tracing, and a Postgres-backed handler, see [go-srvc.com](https://go-srvc.com).
