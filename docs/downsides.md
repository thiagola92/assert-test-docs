---
icon: lucide/heart-crack
---

# Downsides
I use this page to register negative sides of this addon (because it's not a [bed of roses](https://literarydevices.net/a-bed-of-roses/)).

## Detailed information
We use [stderr](https://en.wikipedia.org/wiki/Standard_streams#Standard_error_(stderr)) to identify if an error occurred and that's it. We can't tell you details like:  

- How many assert errors
- How many memory leak errors
- How many an assert failed
- ...

If you want to know the error, you **need** to read the stderr.  

??? info "Other test frameworks"
	They are able to get details because they introduce their own asserts, so they are the ones collecting the error and registering informations.  

	=== "Gut"
		[~54 assert methods](https://gut.readthedocs.io/en/v9.6.0/class_ref/class_guttest.html#methods).  

		- `assert_eq(variable, 10)`
		- `assert_true(variable)`
		- `assert_null(variable)`
		- ...

	=== "GdUnit4"
		[~14 assert methods (which expand in more validations)](https://godot-gdunit-labs.github.io/gdUnit4/latest/testing/assert/).  

		- `assert_int(variable).is_equal(10)`
		- `assert_bool(variable).is_true()`
		- `assert_array(variable).is_empty()`
		- ...

	=== "TinyTest"
		[1 assert method](https://codeberg.org/galanm/tiny_test#building-tests-sets).  
		
		- `TinyTest.assert_true(true, "message")`

## Feature limitation
If you are senior tester, you probably will not like the fact that we don't have many features like:  

- Test discovery
- Test setup and teardown
- Test doubles
- Assertion library
- ...

If you can imagine, we probably don't have it.
