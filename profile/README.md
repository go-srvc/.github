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
## More info at [go-srvc.com](https://go-srvc.com)
