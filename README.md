# package tick
[![Go Reference](https://pkg.go.dev/badge/github.com/eihigh/tick.svg)](https://pkg.go.dev/github.com/eihigh/tick)

A loop counter for game development, which can also be used as an alternative to coroutines in Go.

## Installation
```
go get -u github.com/eihigh/tick
```

## Reference
See https://pkg.go.dev/github.com/eihigh/tick


## Basic usage
See also [example_test.go](https://github.com/eihigh/tick/blob/main/example_test.go) .

```go
var t tick.Tick

for { // Here it should actually be the game's 'Update' function.

	// We recommend to call Advance at the beginning of the loop.
	// So the tick starts with 1.
	t.Advance(1)
	e := t.Elapsed()

	// Repeat to return to zero after counting 60 ticks.
	t.Repeat(0, 60, func(n int, t tick.Tick) {
  
		// Repeat to return to zero after counting 5 ticks.
		t.Repeat(0, 5, func(n int, t tick.Tick) {
    
			// Called only during the time span.
			t.Span(0, 2, func(t tick.Tick) {
				fmt.Println(e, "foo?", t.Elapsed())
        
			}).Span(0, 3, func(t tick.Tick) {
				// Called after the previous time span has ended.
				fmt.Println(e, "bar!", t.Elapsed())
			})
		})
	})
}
```

Equivalent Go Code:
```go
for {
	t += 1
	{
		n := t / 60
		t := t % 60
		{
			n := t / 5
			t := t % 5
			{
				if 0 <= t && t < 2 {
					fmt.Println("foo?")
				} else {
					t -= 2
					if 0 <= t && t < 3 {
						fmt.Println("bar!")
					}
				}
			}
		}
	}
}
```

Output:
```
1 foo? 1
2 bar! 0
3 bar! 1
4 bar! 2
5 foo? 0
6 foo? 1
7 bar! 0
8 bar! 1
9 bar! 2
10 foo? 0
...
```
