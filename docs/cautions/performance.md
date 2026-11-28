---
icon: lucide/activity
---

# Performance
To test a scene we need to create many things (a process, a thread, a mutex, a timer, ...) and these increase the cost of making one single test.  

Image what happens when you request to your operating system memory for executing all tests **at same time**... Here are some tips to avoid exploding your machine.  

## Spamn asserts
Go crazy with the asserts and test as much as possible in a single scene.  

```gdscript linenums="1"
func _ready() -> void:
	test_something()
	test_other_thing()
	test_any_thing()
	
	get_tree().quit()


func test_something() -> void:
	assert(example != 10)


func test_other_thing() -> void:
	assert(example != 20)


func test_anything() -> void:
	assert(example != 30)
```

!!! tip
	Write your main code with a bunch of asserts. If you did that, all that you need is to create a scene to be tested.  

## Run in batchs
Use the `AssertSuit.batch` property to control how many tests run at same time.
