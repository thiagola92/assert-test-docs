---
icon: lucide/store
---

# Assert Test

<figure markdown="span">
![AssertReport with multiple AssertScene as children](./assets/thumbnail.svg)
</figure>

**Assert Test** was created to test scenes using [Godot](https://godotengine.org/) built-in [`assert`](https://docs.godotengine.org/en/stable/classes/class_@gdscript.html#class-gdscript-method-assert).  

!!! warning
	It's not a replacement for full-featured unit testing addons like [GdUnit](https://godot-gdunit-labs.github.io/gdUnit4/latest/) or [Gut](https://gut.readthedocs.io/en/)!

## For who is it?
It's for people that don't want to spend a lot of time learning a test framework.  

> In my case I spent time reading two test frameworks to decide which is my style 🫠

It works by running a scene in a new [process](https://en.wikipedia.org/wiki/Process_(computing)) and collecting it's [stderr](https://en.wikipedia.org/wiki/Standard_streams#Standard_error_(stderr)) to know if something gone wrong. Of course, there is downsides for doing things that way and you can read at [downsides](./downsides.md).
