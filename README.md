## Mutex hat

```golang
struct {
	...

	rateMu     sync.Mutex
	rateLimits [categories]Rate
	mostRecent rateLimitCategory
}
```
Here, rateMu is a mutex hat. It sits, like a hat, on top of the variables that it protects.

So, without needing to write the comment, the above is implicitly understood to be equivalent to:

```golang
struct {
	...

	// rateMu protects rateLimits and mostRecent.
	rateMu     sync.Mutex
	rateLimits [categories]Rate
	mostRecent rateLimitCategory
}
```
When adding a new, unrelated field that isn't protected by rateMu, do this:

```diff
 struct {
 	...

 	rateMu     sync.Mutex
 	rateLimits [categories]Rate
 	mostRecent rateLimitCategory
+
+	common service
 }
```
Don't do this:

```diff
 struct {
 	...

 	rateMu     sync.Mutex
 	rateLimits [categories]Rate
 	mostRecent rateLimitCategory
+	common     service
 }
```
See https://talks.golang.org/2014/readability.slide#21.

## Empty string check

Do this:

```golang
if s == "" {
	...
}
```
Don't do this:

```golang
if len(s) == 0 {
	...
}
```
If you're checking if s is the empty string. Using len(s) is fine for other uses.

The first form is more readable because it's more obvious s is a string and not a slice.

See Russ Cox's comment in a [golang-nuts](https://groups.google.com/d/msg/golang-nuts/7Ks1iq2s7FA/GOujoyeYsOcJ) topic:

> if I care about "is it this specific string" I tend to write s == "".

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
- https://talks.golang.org/2014/readability.slide#21
