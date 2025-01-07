# Learning C++ as a C# developer

This is C++ from a C# perspective.

## Setup

Compiler:

To install ``clang++`` go to the [llvm github releases](https://github.com/llvm/llvm-project/releases) and download the installer with the name ``LLVM-X.X.X-win64.exe``

Build System:

To install ``cmake`` go to the [cmake downloads page](https://cmake.org/download/) and download the windows x64 installer called ``cmake-X.X.X-windows-x86_64.msi``

## Headers

All code goes in ``.cpp`` files, and then every file has a header ``.hpp`` file with the same exact name which contains all the declerations (names) of your functions.

You can access code from other files only by using ``#include "file.h"`` at the top of your code. Which will simply copy the entire header to that point.

You can include a header in 2 ways: ``#include "file.h"`` or ``#include <file.h>``. Using quotes will search the header in your current directory and using angled brackets will search the header in the include directory

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

there are also smart pointers in the C++ std library:

to use smart pointers use ``#include <memory>``

there are 3 types of smart pointers: ``std::unique_ptr`` ``std::shared_ptr`` ``std::weak_ptr``

## References

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

## Memory

Stack:

When creating a variable in C++ it uses the stack by default.
Memory on the stack is local to its scope and gets released automatically when the variable goes out of scope.

Heap:

When you want memory to exist outside of the scop its created it you must allocate memory for it on the heap.
You can allocate memory on the heap using ``malloc()`` and release it using ``free()``.
But a better alternative is using ``auto variable = new Type();`` and release it using ``delete variable;``

Leaks:

In C++ heap memory does not automatically free when the application terminates. and in both C++ and C# OpenGL data like buffers and textures in vram also dont automatically free when the application terminates.
Generally speaking the os will reclaim unfreed memory when the application terminates, but relying on this is bad practice.

## Public/Private

In C# you can put public or private before any member of a class like so:

```csharp
class Person
{
    public float a;
    private float b;
    public float c;
}
```

In C++ you put all public member under ``public:`` and all private members under ``private:`` like so:

```cpp
class Person
{
    public:
        float a;
        float c;
    private:
        float b;
}
```
