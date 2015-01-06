---
layout: post
title: immutable and mutable
---

In object-oriented and functional programming, an **immutable object** is an object whose **state** cannot be modified after it is created. This is in contrast ti a mutable object, which can be modified after it is created.

[Question: Why are mutable struct evil?](http://stackoverflow.com/questions/441309/why-are-mutable-structs-evil)

Structs are value types which means they are copied when they are passed around.

So if you change a copy you are changing only that copy, not the original and not any other copies which might be around.

If your struct is immutable then all automatic copies resulting from being passed by value will be the same.

If you want to change it you have to consciously do it by creating a new instance of the struct with the modified data. (not a copy)

[Question: How do I create an immutable Class?](http://stackoverflow.com/questions/352471/how-do-i-create-an-immutable-class)

[Create immutable object](http://tipsandtricks.runicsoft.com/CSharp/Immutables.html)
[Eric Lippert's blog about immutable](http://blogs.msdn.com/b/ericlippert/archive/tags/immutability/default.aspx?PageIndex=2)
