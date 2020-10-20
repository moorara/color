[![Go Doc][godoc-image]][godoc-url]
[![Build Status][workflow-image]][workflow-url]

# Color

This is a fork of https://github.com/fatih/color

Color lets you use colorized outputs in terms of [ANSI Escape Codes](http://en.wikipedia.org/wiki/ANSI_escape_code#Colors) in Go.
It has support for Windows too! The API can be used in several ways, pick one that suits you.

![Color](https://i.imgur.com/c1JI0lA.png)

## Examples

### Standard Colors

```go
// Print with default helper functions
color.Cyan("Prints text in cyan.")

// A newline will be appended automatically
color.Blue("Prints %s in blue.", "text")

// These are using the default foreground colors
color.Red("We have red")
color.Magenta("And many others ..")

```

### Mix and Reuse Colors

```go
// Create a new color object
c := color.New(color.FgCyan).Add(color.Underline)
c.Println("Prints cyan text with an underline.")

// Or just add them to New()
d := color.New(color.FgCyan, color.Bold)
d.Printf("This prints bold cyan %s\n", "too!.")

// Mix up foreground and background colors, create new mixes!
red := color.New(color.FgRed)

boldRed := red.Add(color.Bold)
boldRed.Println("This will print text in bold red.")

whiteBackground := red.Add(color.BgWhite)
whiteBackground.Println("Red text with white background.")
```

### Use Your Own Output (io.Writer)

```go
// Use your own io.Writer output
color.New(color.FgBlue).Fprintln(myWriter, "blue color!")

blue := color.New(color.FgBlue)
blue.Fprint(writer, "This will print text in blue.")
```

### Custom Print Functions (PrintFunc)

```go
// Create a custom print function for convenience
red := color.New(color.FgRed).PrintfFunc()
red("Warning")
red("Error: %s", err)

// Mix up multiple attributes
notice := color.New(color.Bold, color.FgGreen).PrintlnFunc()
notice("Don't forget this...")
```

### Custom Fprint Functions (FprintFunc)

```go
blue := color.New(FgBlue).FprintfFunc()
blue(myWriter, "important notice: %s", stars)

// Mix up with multiple attributes
success := color.New(color.Bold, color.FgGreen).FprintlnFunc()
success(myWriter, "Don't forget this...")
```

### Insert Into Non-Color Strings (SprintFunc)

```go
// Create SprintXxx functions to mix strings with other non-colorized strings:
yellow := color.New(color.FgYellow).SprintFunc()
red := color.New(color.FgRed).SprintFunc()
fmt.Printf("This is a %s and this is %s.\n", yellow("warning"), red("error"))

info := color.New(color.FgWhite, color.BgGreen).SprintFunc()
fmt.Printf("This %s rocks!\n", info("package"))

// Use helper functions
fmt.Println("This", color.RedString("warning"), "should be not neglected.")
fmt.Printf("%v %v\n", color.GreenString("Info:"), "an important message.")

// Windows supported too! Just don't forget to change the output to color.Output
fmt.Fprintf(color.Output, "Windows support: %s", color.GreenString("PASS"))
```

### Plug Into Existing Code

```go
// Use handy standard colors
color.Set(color.FgYellow)

fmt.Println("Existing text will now be in yellow")
fmt.Printf("This one %s\n", "too")

color.Unset() // Don't forget to unset

// You can mix up parameters
color.Set(color.FgMagenta, color.Bold)
defer color.Unset() // Use it in your function

fmt.Println("All text will now be bold magenta.")
```

### Disable/Enable Color

There might be a case where you want to explicitly disable/enable color output.
The `go-isatty` package will automatically disable color output for non-tty output streams (for example if the output were piped directly to `less`)

`Color` has support to disable/enable colors both globally and for single color definitions.
For example suppose you have a CLI app and a `--no-color` bool flag.
You can easily disable the color output with:

```go
var flagNoColor = flag.Bool("no-color", false, "Disable color output")

if *flagNoColor {
  color.NoColor = true // disables colorized output
}
```

It also has support for single color definitions (local).
You can disable/enable color output on the fly:

```go
c := color.New(color.FgCyan)
c.Println("Prints cyan text")

c.DisableColor()
c.Println("This is printed without any color")

c.EnableColor()
c.Println("This prints again cyan...")
```

## Todo

  - [ ] Save/Return previous values
  - [ ] Evaluate fmt.Formatter interface

## Credits

  - [Fatih Arslan](https://github.com/fatih)
  - Windows support via @mattn: [colorable](https://github.com/mattn/go-colorable)


[godoc-url]: https://pkg.go.dev/github.com/moorara/color
[godoc-image]: https://godoc.org/github.com/moorara/color?status.svg
[workflow-url]: https://github.com/moorara/color/actions
[workflow-image]: https://github.com/moorara/color/workflows/Main/badge.svg
