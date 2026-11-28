---
icon: lucide/funnel
---

# Filters
Every time that we get a text from [stderr](https://en.wikipedia.org/wiki/Standard_streams#Standard_error_(stderr))	, we pass through an `AssertFilter` which apply changes to the text.  

- `AssertFilter` does nothing, is the base for others filters
- `AssertFilterStart` filter lines that start with a specific text

We can use these filters to remove texts that we don't care. In the case of `AssertFilterStart`, it removes lines that start with `\t` or space, so this can omit details from the report.  

=== "`AssertFilter`"
	```
	(FAIL) Testing predicting a function breaking 
		SCRIPT ERROR: Assertion failed: Forced fail
		          at: bool_function (res://tests/scenes/test_fail_break_function.gd:16)
		          GDScript backtrace (most recent call first):
		              [0] bool_function (res://tests/scenes/test_fail_break_function.gd:16)
		              [1] _ready (res://tests/scenes/test_fail_break_function.gd:7)
		SCRIPT ERROR: Assertion failed: Forced fail
		          at: int_function (res://tests/scenes/test_fail_break_function.gd:22)
		          GDScript backtrace (most recent call first):
		              [0] int_function (res://tests/scenes/test_fail_break_function.gd:22)
		              [1] _ready (res://tests/scenes/test_fail_break_function.gd:8)
		SCRIPT ERROR: Assertion failed: Forced fail
		          at: float_function (res://tests/scenes/test_fail_break_function.gd:28)
		          GDScript backtrace (most recent call first):
		              [0] float_function (res://tests/scenes/test_fail_break_function.gd:28)
		              [1] _ready (res://tests/scenes/test_fail_break_function.gd:9)
		SCRIPT ERROR: Assertion failed: Forced fail
		          at: string_function (res://tests/scenes/test_fail_break_function.gd:34)
		          GDScript backtrace (most recent call first):
		              [0] string_function (res://tests/scenes/test_fail_break_function.gd:34)
		              [1] _ready (res://tests/scenes/test_fail_break_function.gd:10)
	```

=== "`AssertFilterStart`"
	```
	(FAIL) Testing predicting a function breaking 
		SCRIPT ERROR: Assertion failed: Forced fail
		SCRIPT ERROR: Assertion failed: Forced fail
		SCRIPT ERROR: Assertion failed: Forced fail
		SCRIPT ERROR: Assertion failed: Forced fail
	```
