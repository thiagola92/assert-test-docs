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
	```gdscript
	func _ready() -> void:
		var a := Object.new()
		var b := Object.new()
		
		get_tree().quit.call_deferred()
	```

=== "Fix"
	```gdscript
	func _ready() -> void:
		var a := Object.new()
		var b := Object.new()
		
		a.free()
		b.free()
		
		get_tree().quit.call_deferred()
	```

### Cautions
If your object is built from others objects, you need to remember to free every single one.  

=== "Memory leak"
	```gdscript
	func _ready() -> void:
		var a := Example.new()
		
		get_tree().quit.call_deferred()
	
	
	class Example extends Object:
		var b := Object.new()
	```

=== "Fix"
	```gdscript
	func _ready() -> void:
		var a := Example.new()
		
		a.b.free()
		a.free()
		
		get_tree().quit.call_deferred()
	
	
	class Example extends Object:
		var b := Object.new()
	```

## [Node](https://docs.godotengine.org/en/latest/classes/class_node.html)
When you free it, it will free the descedants before itself.  

=== "Memory leak"
	```gdscript
	func _ready() -> void:
		var a := Node.new()
		var b := Node.new()
		
		a.add_child(b)
	
		get_tree().quit.call_deferred()
	```
	
=== "Fix"
	```gdscript
	func _ready() -> void:
		var a := Node.new()
		var b := Node.new()
		
		a.add_child(b)
		a.free()
		
		get_tree().quit.call_deferred()
	```

### Cautions
If you remove a `Node` from it parent, you have to remember to manually free it later.  

=== "Memory leak"
	```gdscript
	func _ready() -> void:
		var a := Node.new()
		var b := Node.new()
		
		a.add_child(b)
		a.remove_child(b)
		
		get_tree().quit.call_deferred()
	```

=== "Fix"
	```gdscript
	func _ready() -> void:
		var a := Node.new()
		var b := Node.new()
		
		a.add_child(b)
		a.remove_child(b)
		a.free()
		b.free()
	
		get_tree().quit.call_deferred()
	```

Do not confuse children with properties, a `Node` in the property needs to freed manually.  

=== "Memory leak"
	```gdscript
	func _ready() -> void:
		var a := Example.new()
		
		a.free()
		
		get_tree().quit.call_deferred()
	
	class Example extends Node:
		var b := Node.new()
	```

=== "Fix"
	```gdscript
	func _ready() -> void:
		var a := Example.new()
		
		a.b.free()
		a.free()
		
		get_tree().quit.call_deferred()
	
	class Example extends Node:
		var b := Node.new()
	```

## [RefCounted](https://docs.godotengine.org/en/latest/classes/class_refcounted.html)
It will free itself when no one reference it anymore.  

## References

- [Memory Management](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_basics.html#memory-management)
