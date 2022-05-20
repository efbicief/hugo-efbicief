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
