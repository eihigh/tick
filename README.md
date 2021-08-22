# package tick
Measure ticks and switch processing as they elapse. Available as an alternative to coroutines.

## Installation
```
go get -u github.com/eihigh/tick
```

## Basic usage
See also [example_test.go](https://github.com/eihigh/tick/blob/main/example_test.go) .

```go
var t tick.Tick

func update() {

	// ... in your game loop ...

	// We recommend to call Advance at the beginning of the loop.
	// So the tick starts with 1.
	t.Advance(1)

	// Repeat to return to zero after counting 60 ticks.
	t.Repeat(0, 60, func(n int, t Tick) {
  
		// Repeat to return to zero after counting 5 ticks.
		t.Repeat(0, 5, func(n int, t Tick) {
    
			// Called only during the time span.
			t.Span(0, 2, func(t Tick) {
				fmt.Println("foo?", t.Elapsed())
        
			}).Span(0, 3, func(t Tick) {
				// Called after the previous time span has ended.
				fmt.Println("bar!", t.Elapsed())
			})
		})
	})
}
```

Output:
```
foo? 1
bar! 0
bar! 1
bar! 2
foo? 0
foo? 1
bar! 0
bar! 1
bar! 2
foo? 0
...
```
