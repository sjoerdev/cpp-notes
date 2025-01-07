# Learning C++ as a C# developer

This is C++ from a C# perspective.

## Headers

headers

## Pointers

Raw pointers in C++ work the same as in C#, the syntax is the exact same:
```cpp
int x = 1;
int* ptr = &x;
int y = *ptr;
```

it doesnt matter where in a pointer you use a space as it will all work:

```cpp
int* ptr = &x;
int *ptr = &x;
int*ptr = &x;
```

## Reference Types

In C++ classes are value types by default, not reference types like in C#.

Passing a class instance by reference to a function in C# is:
```csharp
// csharp
void (Class instance)
{
    // changes the original instance
    instance.variable = 1;

    // this creates a copy
    Class instance_copy = instance.Clone();
}
```

Passing a class instance by reference to a function in C++ is:
```cpp
// cpp
void (Class& instance)
{
    // changes the original instance
    instance.variable = 1;

    // this creates a copy
    Class instance_copy = instance;
}
```

You can also pass a class instance in C++ with a pointer:
```cpp
// cpp
void (Class* instance_ptr)
{
    // changes the original instance
    instance_ptr->variable = 1;

    // changes the original instance
    (*instance_ptr).variable = 1;

    // this creates a copy
    Class instance_copy = *instance_ptr;
}
```

func(Class instance)

and in c++ is: func(Class& instance). that means cloning an instance in c++ is easy

## Memory Leaks

c++ heap memory does not automatically free when the application terminates. and in both c++ and c# opengl data like buffers and textures in vram also dont automatically free when the application terminates. (generally speaking the os will reclaim unfreed memory when the application terminates, but relying on this is bad practice)

### Smart Pointers

std::unique_ptr, std::shared_ptr, std::weak_ptr
