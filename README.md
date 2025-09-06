# 🖥️ Setting up OpenGL + GLFW + GLAD on Linux

> ⚠️ **Warning:**  
> This template has only been tested on **Ubuntu 24.04**.  
> It may not work correctly on other Linux distributions or versions.

## 1. Install system dependencies

First, make sure you have the build tools and OpenGL development packages:

sudo apt update
sudo apt install build-essential cmake git pkg-config


Install OpenGL and additional dependencies (for X11 and Wayland):

sudo apt install libgl1-mesa-dev libx11-dev xorg-dev libwayland-dev libxkbcommon-dev

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
```git clone https://github.com/donigna/OpenGL-Template-Linux```

1. open project folder
2. build cmake >try delete default build if found error then build again
3. try run main.cpp

### Minimal example (src/main.cpp)
#include <glad/glad.h>
#include <GLFW/glfw3.h>
#include <iostream>

int main() {
    if (!glfwInit()) {
        std::cerr << "Failed to initialize GLFW\n";
        return -1;
    }

    GLFWwindow* window = glfwCreateWindow(800, 600, "OpenGL Window", nullptr, nullptr);
    if (!window) {
        std::cerr << "Failed to create window\n";
        glfwTerminate();
        return -1;
    }
    glfwMakeContextCurrent(window);

    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress)) {
        std::cerr << "Failed to initialize GLAD\n";
        return -1;
    }

    while (!glfwWindowShouldClose(window)) {
        glClear(GL_COLOR_BUFFER_BIT);
        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    glfwDestroyWindow(window);
    glfwTerminate();
    return 0;
}

## 7. Run
./app


You should see an empty window with an active OpenGL context.
