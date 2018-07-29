# PHP bindings for Go [![API Documentation][godoc-svg]][godoc-url] [![MIT License][license-svg]][license-url]

This package implements support for executing PHP scripts, exporting Go variables for use in PHP contexts, attaching Go method receivers as PHP classes and returning PHP variables for use in Go contexts.


## Usage

### Basic

Executing a script is very simple:

```go
package main

import (
    php "github.com/deuill/go-php"
    "os"
)

func main() {
    engine, _ := php.New()

    context, _ := engine.NewContext()
    context.Output = os.Stdout

    context.Exec("index.php")
    engine.Destroy()
}
```

The above will execute script file `index.php` located in the current folder and will write any output to the `io.Writer` assigned to `Context.Output` (in this case, the standard output).

### Binding and returning variables

The following example demonstrates binding a Go variable to the running PHP context, and returning a PHP variable for use in Go:

```go
package main

import (
    "fmt"
    php "github.com/deuill/go-php"
)

func main() {
    engine, _ := php.New()
    context, _ := engine.NewContext()

    var str string = "Hello"
    context.Bind("var", str)

    val, _ := context.Eval("return $var.' World';")
    fmt.Printf("%s", val.Interface())
    // Prints 'Hello World' back to the user.

    engine.Destroy()
}
```

A string value "Hello" is attached using `Context.Bind` under a name `var` (available in PHP as `$var`). A script is executed inline using `Context.Eval`, combinding the attached value with a PHP string and returning it to the user.

Finally, the value is returned as an `interface{}` using `Value.Interface()` (one could also use `Value.String()`, though the both are equivalent in this case).

## License

All code in this repository is covered by the terms of the MIT License, the full text of which can be found in the LICENSE file.

[godoc-url]: https://godoc.org/github.com/deuill/go-php
[godoc-svg]: https://godoc.org/github.com/deuill/go-php?status.svg

[license-url]: https://github.com/deuill/go-php/blob/master/LICENSE
[license-svg]: https://img.shields.io/badge/license-MIT-blue.svg

[Context.Exec]: https://godoc.org/github.com/deuill/go-php/engine#Context.Exec
[Context.Eval]: https://godoc.org/github.com/deuill/go-php/engine#Context.Eval
[NewValue]:     https://godoc.org/github.com/deuill/go-php/engine#NewValue
[NewReceiver]:  https://godoc.org/github.com/deuill/go-php/engine#NewReceiver
