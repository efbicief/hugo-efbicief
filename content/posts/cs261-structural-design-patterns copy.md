---
title: "Structural Patterns"
date: 2022-05-20T02:32:22+01:00
draft: false
tags: [ "CS261", "Software Engineering", "Implementation", "Patterns", "Notes" ]
---
# Factory
- Wrapper for a constructor that builds an object instance in a **single call**
- Has methods that build parts of the object, and combines them together (eg. might have a wheel constructor, brakes constructor, frame constructor, saddle constructor, combines them together to make a bike)
- Can simplify code significantly: by extending the factory class, we can create another class which can call super methods and create variants on the factory's output (eg. extend bike with a gear system constructor for a mountain bike)
- Good at reducting repeated code.

# Builder
- Subtle difference: whereas a factory builds an object in **one call**, a builder builds a different aspect of the new object with each call, and returns the new object by calling `get()`.
- More flexible - good for making customisable, complex objects with optional parameters.
- Focuses on cutting down long constructors, rather than cuttong down repeated code.

# Prototype
- Start by making one instance of an object you need many copies of.
- Anytime you need a new one, `clone()` the original prototype.

# Proxy
Placeholders for objects, redirects to the correct resource. Comes with added bonuses.
- Virtual proxy: Lazy loading - delays loading the resource until nessecary
- Protection proxy: Provides access control, eg. Chip & PIN as a proxy for money
- Remote proxy: Also handles communications to access a remote resource
- Logging proxy: Keeps track of accesses and requests in a service object
- Caching proxy: Saves results from resource and keeps them to avoid repeated resource queries
- Smart referencing: Garbage collection - remove heavy service objects if unused for some time

# Decorators
- Adds new behaviour to an object at runtime
- Wrapper for existing objects - eg. to change colour, wrap in `redColourWrapper(obj)`

# Flyweight
- Takes common resource out as it's own object
- Other objects now access just the flyweight object
- Useful for eg. game textures, where many objects need the same texture, saves on memory
