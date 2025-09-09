# 🖥️ Setting up OpenGL + GLFW + GLAD on Linux

> ⚠️ **Warning:**  
> This template has only been tested on **Ubuntu 24.04**.  
> It may not work correctly on other Linux distributions or versions.

## 1. Install system dependencies

**Requirements :**

First, make sure you have the build tools and OpenGL development packages:

```
sudo apt update
sudo apt install build-essential cmake git pkg-config
```

Install OpenGL and additional dependencies (for X11 and Wayland):
```
sudo apt install libgl1-mesa-dev libx11-dev xorg-dev libwayland-dev libxkbcommon-dev
```
Install SOIL for image processor: 
```
sudo apt install libsoil-dev
```

## 2. Install GLFW

Clone and build GLFW from source:

git clone https://github.com/glfw/glfw.git
cd glfw
cmake -S . -B build -DGLFW_BUILD_WAYLAND=OFF -DGLFW_BUILD_X11=ON
cmake --build build
sudo cmake --install build


This installs glfw3 to /usr/local/lib and its headers to /usr/local/include/GLFW.

## 3. Setup GLAD

Go to the GLAD generator
:

Language: C/C++

Specification: OpenGL

API: Choose the version you need (e.g., 3.3 or 4.6)

Profile: Core

Click Generate and download the ZIP file.

Extract the ZIP into your project folder, e.g.:

```
project/
├── glad/
│   ├── include/glad/glad.h
│   └── src/glad.c
└── src/
    └── main.cpp
```
## 4. Project structure

Minimal layout should look like this:
```
project/
├── glad/
│   ├── include/glad/glad.h
│   └── src/glad.c
├── src/
│   └── main.cpp
└── CMakeLists.txt 
```
## 5. Compile manually

From the project root, build using g++:

g++ src/*.cpp glad/src/glad.c \
    -Iglad/include \
    -I/usr/local/include \
    -L/usr/local/lib \
    -lglfw3 -ldl -lGL \
    -o app


-Iglad/include → GLAD headers

-I/usr/local/include → GLFW headers

-L/usr/local/lib → GLFW libraries

-lglfw3 → link GLFW

-ldl and -lGL → standard Linux libraries

## 6. Using Template 
clone project : 
```
git clone https://github.com/donigna/OpenGL-Template-Linux
```

1. open project folder
2. build cmake >try delete default build if found error then build again
3. try run main.cpp

### Minimal example (src/main.cpp)
```

#include <iostream>
#include <glad/glad.h>
#include <GLFW/glfw3.h>


void framebuffer_size_callback(GLFWwindow* window, int width, int height);
void processInput(GLFWwindow *window);

// settings
const unsigned int SCR_WIDTH = 800;
const unsigned int SCR_HEIGHT = 600;

int main()
{
    // glfw: initialize and configure
    // ------------------------------
    glfwInit();
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

#ifdef __APPLE__
    glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);
#endif

    // glfw window creation
    // --------------------
    GLFWwindow* window = glfwCreateWindow(SCR_WIDTH, SCR_HEIGHT, "LearnOpenGL", NULL, NULL);
    if (window == NULL)
    {
        std::cout << "Failed to create GLFW window" << std::endl;
        glfwTerminate();
        return -1;
    }
    glfwMakeContextCurrent(window);
    glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);

    // glad: load all OpenGL function pointers
    // ---------------------------------------
    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
    {
        std::cout << "Failed to initialize GLAD" << std::endl;
        return -1;
    }    

    // render loop
    // -----------
    while (!glfwWindowShouldClose(window))
    {
        // input
        // -----
        processInput(window);

        // render
        // ------
        glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);

        // glfw: swap buffers and poll IO events (keys pressed/released, mouse moved etc.)
        // -------------------------------------------------------------------------------
        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    // glfw: terminate, clearing all previously allocated GLFW resources.
    // ------------------------------------------------------------------
    glfwTerminate();
    return 0;
}

// process all input: query GLFW whether relevant keys are pressed/released this frame and react accordingly
// ---------------------------------------------------------------------------------------------------------
void processInput(GLFWwindow *window)
{
    if(glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
        glfwSetWindowShouldClose(window, true);
}

// glfw: whenever the window size changed (by OS or user resize) this callback function executes
// ---------------------------------------------------------------------------------------------
void framebuffer_size_callback(GLFWwindow* window, int width, int height)
{
    // make sure the viewport matches the new window dimensions; note that width and 
    // height will be significantly larger than specified on retina displays.
    glViewport(0, 0, width, height);
}
```
## 7. Run
./app


You should see an empty window with an active OpenGL context.
