---
icon: lucide/heart-crack
---

# Downsides
I use this page to register negative sides of this addon (because it's not a [bed of roses](https://literarydevices.net/a-bed-of-roses/)).

## Test cost
To test a scene we need to create many things (a process, a thread, a mutex, a timer) and these increase the cost of making one single test.  

**My recommendation is**: Go crazy with the asserts and test as much as possible in a single scene.  

## Detailed information
We use [stderr](https://en.wikipedia.org/wiki/Standard_streams#Standard_error_(stderr)) to identify if an error occurred and that's it. We can't tell you details like:  

- How many assert errors
- How many memory leak errors
- How many an assert failed
- ...

If you want to know the error, you **need** to read the stderr.  

## Feature limitation
If you are senior tester, you probably will not like the fact that we don't have many features like:  

- Test discovery
- Test setup and teardown
- Test doubles
- Assertion library
- ...

If you can imagine, we probably don't have it.
