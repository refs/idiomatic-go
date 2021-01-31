## Avoid unused method receiver names
Do this:

```golang
func (foo) method() {
	...
}
```

Don't do this:

```
func (f foo) method() {
	...
}
```

If f is unused. It's more readable because it's clear that fields or methods of foo are not used in method.

## For brands or words with more than 1 capital letter, lowercase all letters

Do this:

```
// Exported.
var OAuthEnabled bool
var GitHubToken string

// Unexported.
var oauthEnabled bool
var githubToken string
```

Don't do this:

```
// Unexported.
var oAuthEnabled bool
var gitHubToken string
```

Lowercase all letters, not only the first letter, of the first word when you want to make an unexported version.

For consistency with what the Go project does, and because it looks nicer. oAuth is not pretty.

## Error variable naming

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
