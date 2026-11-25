---
icon: lucide/door-open
---

# Quitting
A very important part of these tests is knowing how to `quit()`. So pay attention to these details:  

- `quit()`: Will quit the application at the **end of current iteration**
- `assert()`: Failing will make the function return but the **application continues running**

## Avoid `assert()` before `quit()`

The following code will never call the `quit()` because the `assert()` will fail and make the function return.  

```gdscript linenums="1"
func _ready() -> void:
	assert(false)

	# Never called.
	get_tree().quit()
```

But the next code will work because `quit()` continues with the function code:  

```gdscript linenums="1"
func _ready() -> void:
	get_tree().quit()

	# Called.
	assert(false)
```

An alternative is letting the `assert()` code inside functions, so it continues after breaking.  

```gdscript linenums="1"
func _ready() -> void:
	example()

	# Called.
	get_tree().quit()


func example() -> void:
	assert(false)
```

!!! info
	This is not unique of `assert()`, Godot handle unexpected erros by making the function return the default value.  

## Always adjust `timeout`
An error can happen in any part of your code and happening before a `quit()` is the worst possible case, because you will never send the request to close the test. For example:  

```gdscript linenums="1"
func _ready() -> void:
	var x = 10
	var y = Object.new()
	
	print(x + y)

	# Never called.
	get_tree().quit()
```

To avoid letting the process running forever, `AssertScene` includes a property `timeout` to halt the process after some seconds (default is 3 seconds). You will know that your test was halted because we add a message to your report:  

```
(FAIL) Test description here --- Scene halted after exceeding time limit
	SCRIPT ERROR: Invalid operands 'int' and 'Object' in operator '+'.
	          at: _ready (res://tests/scenes/test_name.gd:8)
	          GDScript backtrace (most recent call first):
	              [0] _ready (res://tests/scenes/test_name.gd:8)
```

!!! note
	Notice how you still receive any stderr that we collect during this time, so you can still indentify the problem.  

The important part is to remember to adjust to an adequate value for longer tests.  


## References
- [`assert()`](https://docs.godotengine.org/en/stable/classes/class_@gdscript.html#class-gdscript-method-assert)
- [`quit()`](https://docs.godotengine.org/en/stable/classes/class_scenetree.html#class-scenetree-method-quit)
- [Sending your own quit notification](https://docs.godotengine.org/en/stable/tutorials/inputs/handling_quit_requests.html#sending-your-own-quit-notification)
