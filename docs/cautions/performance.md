---
icon: lucide/activity
---

# Performance
To test a scene we need to create many things (a process, a thread, a mutex, a timer, ...) and these increase the cost of making one single test.  

Image what happens when you request to your operating system memory for executing all tests **at same time**... Here are some tips to avoid exploding your machine:  

- Go crazy with the asserts and test as much as possible in a single scene
- Use `AssertSuit.batch` property to control how many tests run at same time
