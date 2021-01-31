Error variable naming

Do this:

```golang
// Package level exported error.
var ErrSomething = errors.New("something went wrong")

func main() {
	// Normally you call it just "err",
	result, err := doSomething()
	// and use err right away.

	// But if you want to give it a longer name, use "somethingError".
	var specificError error
	result, specificError = doSpecificThing()

	// ... use specificError later.
}
```

Don't do this:

```golang
var ErrorSomething = errors.New("something went wrong")
var SomethingErr = errors.New("something went wrong")
var SomethingError = errors.New("something went wrong")

func main() {
	var specificErr error
	result, specificErr = doSpecificThing()

	var errSpecific error
	result, errSpecific = doSpecificThing()

	var errorSpecific error
	result, errorSpecific = doSpecificThing()
}
```

## Sources

- https://dmitri.shuralyov.com/idiomatic-go
