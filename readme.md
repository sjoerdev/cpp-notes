# Learning C++ as a C# developer

This is C++ from a C# perspective.

## Setup Tools

**MSVC Compiler:**

1. Download the [Microsoft C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
2. Select the ``Desktop development with C++`` workload
3. Select the ``MSVC Build Tools`` and the ``Windows SDK``

**XMake Build System:**

1. Run ``winget install xmake`` in the terminal
2. Check installation with ``xmake --version`` in the terminal

**(Optional) Setup OpenGL Project:**

1. Create a project directory with ``mkdir project`` and ``cd project``
2. Init xmake in the project with ``xmake create -l c++ -t console``
3. Download the [Glad OpenGL 3.3 Core Loader](https://glad.dav1d.de/#language=c&specification=gl&api=gl%3D3.3&api=gles1%3Dnone&api=gles2%3Dnone&api=glsc2%3Dnone&profile=core&loader=on)
4. Create a directory called ``thirdparty`` and place the glad in there like so:
```
project/
├── src/                     # your code
│   └── main.cpp
├── thirdparty/
│   └── glad/                # glad files
│       ├── include/
│       │   └── glad/
│       └── src/
│           └── glad.c
└── xmake.lua                # xmake file
```
6. Setup the ``xmake.lua`` file like so:
```lua
-- dependencies
add_requires("glfw")

-- project
target("project")

    -- settings
    set_kind("binary")
    set_languages("c++17")

    -- source files to include
    add_files("src/*.cpp")

    -- add glfw package
    add_packages("glfw")

    -- include glad header and source
    add_includedirs("thirdparty/glad/include")
    add_files("thirdparty/glad/src/*.c")
```
7. Now run ``xmake run`` to build and execute

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
![csharp-types](https://github.com/user-attachments/assets/464866ab-4776-4ff9-b340-f86d03750d3c)

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
