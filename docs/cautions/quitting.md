---
icon: lucide/door-open
---

# Quitting
A very important part of these tests is knowing how to `quit()`. So pay attention to these details:  

- `quit()`: Will quit the application at the **end of current iteration**
- `assert()`: Failing will make the function return but the **application continues running**

## Don't `assert()` before `quit()`

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

!!! info
	This is not unique of `assert()`, Godot handle unexpected erros by making the function return it default value.  

## Always set `timeout`
An error can happen in any part of your code and the worst is happening before your `quit()`. For example:  

```gdscript linenums="1"
func _ready() -> void:
	var x = 10
	var y = Object.new()
	
	print(x + y)

	# Never called.
	get_tree().quit()
```

If this happens, you will only know when `AssertScene` timeout and by default this is **30 seconds**, which is a lot... So I always recommend to reduce to an expected value.  


## References
- [`assert()`](https://docs.godotengine.org/en/stable/classes/class_@gdscript.html#class-gdscript-method-assert)
- [`quit()`](https://docs.godotengine.org/en/stable/classes/class_scenetree.html#class-scenetree-method-quit)
- [Sending your own quit notification](https://docs.godotengine.org/en/stable/tutorials/inputs/handling_quit_requests.html#sending-your-own-quit-notification)
