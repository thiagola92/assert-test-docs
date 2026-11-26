---
icon: lucide/hat-glasses
---

# Fake fail
If the presence of stderr is what indicates failing, then anyone can trigger a fail with:  

- [`printerr()`](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#class-globalscope-method-printerr)
- [`push_warning()`](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#class-globalscope-method-push-warning)
- [`push_error()`](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#class-globalscope-method-push-error)
- manually writing to stderr

Imagine that a plugin that you use has an tolerable error, it would generate a stderr and, consequently, mark your test as failed.  

To avoid that, you can attempt to filter error messages that are not error using [filters](../filters).
