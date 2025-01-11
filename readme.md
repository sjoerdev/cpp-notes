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
3. Setup the ``xmake.lua`` file like so:
```lua
-- dependencies
add_requires("glfw")
add_requires("glad", { configs = { api = "gl=3.3", profile = "core", spec = "gl"} })
add_requires("imgui", { configs = { opengl3 = true, glfw = true } })

-- project
target("project")

    -- settings
    set_kind("binary")
    set_languages("c++17")

    -- sources
    add_files("src/*.cpp")

    -- linking
    add_packages("glfw", "glad", "imgui")
```
4. Now create an opengl hello world like this:
```cpp
#include <iostream>
#include <glad/glad.h>
#include <GLFW/glfw3.h>
#include "imgui.h"
#include "imgui_impl_glfw.h"
#include "imgui_impl_opengl3.h"

// resizing callback
void resize(GLFWwindow* window, int width, int height)
{
    glViewport(0, 0, width, height);
}

int main()
{
    // try init glfw
    if (!glfwInit())
    {
        printf("failed to init glfw");
        return -1;
    }

    // set opengl version
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

    // create window
    GLFWwindow* window = glfwCreateWindow(800, 600, "program", nullptr, nullptr);
    if (!window)
    {
        printf("failed to create glfw window");
        glfwTerminate();
        return -1;
    }

    // set opengl context
    glfwMakeContextCurrent(window);

    // load opengl with glad
    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
    {
        printf("failed to init glad");
        return -1;
    }

    // framebuffer size callback
    glfwSetFramebufferSizeCallback(window, resize);

    // init imgui
    IMGUI_CHECKVERSION();
    ImGui::CreateContext();
    ImGuiIO& io = ImGui::GetIO();
    ImGui_ImplGlfw_InitForOpenGL(window, true);
    ImGui_ImplOpenGL3_Init("#version 330"); // GLSL version

    // render loop
    while (!glfwWindowShouldClose(window))
    {
        // poll events
        glfwPollEvents();

        // clear screen
        glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);

        // imgui
        ImGui_ImplOpenGL3_NewFrame();
        ImGui_ImplGlfw_NewFrame();
        ImGui::NewFrame();
        ImGui::ShowDemoWindow();
        ImGui::Render();
        ImGui_ImplOpenGL3_RenderDrawData(ImGui::GetDrawData());

        // swap buffers
        glfwSwapBuffers(window);
    }

    // cleanup
    ImGui_ImplOpenGL3_Shutdown();
    ImGui_ImplGlfw_Shutdown();
    ImGui::DestroyContext();
    glfwDestroyWindow(window);
    glfwTerminate();
    return 0;
}
```
5. Now run ``xmake`` to build the project
6. And the run ``xmake run`` to run the project

**(Optional) Setup Visual Studio Code:**

1. Install the [Microsoft C++ Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) for vscode
2. Install the [XMake Extension](https://marketplace.visualstudio.com/items?itemName=tboox.xmake-vscode) for vscode
3. To your xmake file add ``add_defines("XMAKE_ENABLE_COMPILE_COMMANDS")``
4. Create a file called ``.vscode/c_cpp_properties.json`` with this json:
```json
{
    "version": 4,
    "configurations":
    [
        {
            "name": "xmake",
            "compileCommands": ".vscode/compile_commands.json"
        }
    ]
}
```

## Headers

![headers](https://github.com/user-attachments/assets/0bee0c9d-c1b6-4bf9-95a3-635da0fa221f)

All code goes in ``.cpp`` files, and then every file has a header ``.hpp`` file with the same exact name which contains all the declerations (names) of your functions.

You can access code from other files only by using ``#include "file.h"`` at the top of your code. Which will simply copy the entire header to that point.

You can include a header in 2 ways: ``#include "file.h"`` or ``#include <file.h>``. Using quotes will search the header in your current directory and using angled brackets will search the header in the include directory

Read more about header file on [this website](https://www.learncpp.com/cpp-tutorial/header-files/)

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
