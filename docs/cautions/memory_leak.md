---
icon: lucide/memory-stick
---

# Memory Leak
Is a classic problem that people normally don't notice until **closing the game**, because it's only then that Godot shows in the terminal a message like:  

```
WARNING: 1 ObjectDB instance was leaked at exit (run with `--verbose` for details).
	at: cleanup (core/object/object.cpp:2536)
```

Godot has three objects that interact with memory in a different way:  

- `Object`
- `Node`
- `RefCounted`

## [Object](https://docs.godotengine.org/en/latest/classes/class_object.html)
The classic "you allocated, you free it".  

=== "Memory leak"
	```gdscript linenums="1"
	func _ready() -> void:
		var a := Object.new()
		
		get_tree().quit()
	```

=== "Fix"
	```gdscript linenums="1" hl_lines="4"
	func _ready() -> void:
		var a := Object.new()
		
		a.free()
		
		get_tree().quit()
	```

### Cautions
If your object is built from others objects, you need to remember to free every single one.  

=== "Memory leak"
	```gdscript linenums="1"
	func _ready() -> void:
		var a := Example.new()
		
		get_tree().quit()
	
	
	class Example extends Object:
		var b := Object.new()
	```

=== "Fix"
	```gdscript linenums="1" hl_lines="4-5"
	func _ready() -> void:
		var a := Example.new()
		
		a.b.free()
		a.free()
		
		get_tree().quit()
	
	
	class Example extends Object:
		var b := Object.new()
	```

## [Node](https://docs.godotengine.org/en/latest/classes/class_node.html)
When you free it, it will free the descedants before itself.  

=== "Memory leak"
	```gdscript linenums="1"
	func _ready() -> void:
		var a := Node.new()
		var b := Node.new()
		
		a.add_child(b)
	
		get_tree().quit()
	```
	
=== "Fix"
	```gdscript linenums="1" hl_lines="6"
	func _ready() -> void:
		var a := Node.new()
		var b := Node.new()
		
		a.add_child(b)
		a.free()
		
		get_tree().quit()
	```

### Cautions
If you remove a `Node` from it parent, you have to remember to manually free it later.  

=== "Memory leak"
	```gdscript linenums="1"
	func _ready() -> void:
		var a := Node.new()
		var b := Node.new()
		
		a.add_child(b)
		a.remove_child(b)
		
		get_tree().quit()
	```

=== "Fix"
	```gdscript linenums="1" hl_lines="7-8"
	func _ready() -> void:
		var a := Node.new()
		var b := Node.new()
		
		a.add_child(b)
		a.remove_child(b)
		a.free()
		b.free()
	
		get_tree().quit()
	```

Do not confuse children with properties, a `Node` in the property needs to freed manually.  

=== "Memory leak"
	```gdscript linenums="1"
	func _ready() -> void:
		var a := Example.new()
		
		a.free()
		
		get_tree().quit()

	
	class Example extends Node:
		var b := Node.new()
	```

=== "Fix"
	```gdscript linenums="1" hl_lines="4"
	func _ready() -> void:
		var a := Example.new()
		
		a.b.free()
		a.free()
		
		get_tree().quit()

	
	class Example extends Node:
		var b := Node.new()
	```

## [RefCounted](https://docs.godotengine.org/en/latest/classes/class_refcounted.html)
It will free itself when no one reference it anymore.  

```gdscript linenums="1"
func _ready() -> void:
	var a := RefCounted.new()
	
	get_tree().quit()
```

### Cautions
Avoid circular reference, otherwise it will never free itself.  

=== "Memory leak"
	```gdscript linenums="1"
	func _ready() -> void:
		var a := Example.new()
		var b := Example.new()
		
		a.r = b
		b.r = a
		
		get_tree().quit()

	
	class Example extends RefCounted:
		var r: RefCounted = null
	```

=== "Fix"
	```gdscript linenums="1" hl_lines="5-6 12"
	func _ready() -> void:
		var a := Example.new()
		var b := Example.new()
		
		a.r = weakref(b)
		b.r = weakref(a)
		
		get_tree().quit()

	
	class Example extends RefCounted:
		var r: WeakRef = null
	```

	[WeakRef](https://docs.godotengine.org/en/stable/classes/class_weakref.html) will not increase the reference count and to get back the reference use [`get_ref()`](https://docs.godotengine.org/en/stable/classes/class_weakref.html#class-weakref-method-get-ref).  

	??? tip "Losing the autocomplete"
		`WeakRef.get_ref()` returns a `Variant`, which means that you lose the autocomplete and needs to convert back to the correct type.  One way that to solve this (instead of always converting) is to create an intermediary class like:  
		
		```gdscript linenums="1" hl_lines="5-6 12 15-22"
		func _ready() -> void:
			var a := Example.new()
			var b := Example.new()
			
			a.r = RefExample.new(b)
			b.r = RefExample.new(a)
			
			get_tree().quit()
	
		
		class Example extends RefCounted:
			var r: RefExample = null
	
		
		class RefExample extends RefCounted:
			var r: WeakRef = null

			func _init(e: Example) -> void:
				r = weakref(e)

			func get_ref() -> Example:
				return r.get_ref() as Example
		```

## References

- [Object](https://docs.godotengine.org/en/latest/classes/class_object.html)
- [Node](https://docs.godotengine.org/en/latest/classes/class_node.html)
- [RefCounted](https://docs.godotengine.org/en/latest/classes/class_refcounted.html)
- [Memory Management](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_basics.html#memory-management)
