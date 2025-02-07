---
layout: post
title: OpenGL-Graphics
categories: projects
permalink: /:categories/:title
---

This project uses CMake to create a project with [GLFW](https://www.glfw.org/)/[GLAD](https://github.com/Dav1dde/glad) for use with the [OpenGL](https://www.opengl.org/) specification. This project is designed to be multi-platform and runs on most C++ build systems including Windows, Linux and MacOS.

This README acts as a source of information for this project, an instructional for building this project as well as a journal of my learnings over the course of this project.

Much of this project will follow alongside the extremely helpful teachings at https://learnopengl.com.

## CMake

### GLFW-CMake-Starter

This project used a template called [GLFW-CMake-Starter](https://github.com/juliettef/GLFW-CMake-starter). Once cloned, this template can be used to generate the makefiles for GLFW from the GLFW git submodule. This template has been used for its multi-platform compatability.

### Modifications

- Add warning flags to compilation of each project source file
- Change to create an executable for each project source file
- Modified to use newly generated version of GLAD [(continued)](#glad)

### Using CMake to build the project

From a command prompt in the project top-level directory:
1. `mkdir build`
1. `cd build`
1. `cmake ..` (for MinGW the build system generator needs to be specified using the -G option: `cmake .. -G "MinGW Makefiles"`)
1. Either run `make all` or for Visual Studio open `GLFW-CMake-starter.sln` or for MinGW run `mingw32-make`

## GLAD

GLAD is used to load and generate OS-dependant functions for use in various standards in this case OpenGL.

The aforementioned GLFW CMake starter included a section intended for giving the ability to use the version of GLAD2 (header only) packaged with the GLFW submodule.

```cmake
set(GLAD_GL "")

...

option(GLFW-CMAKE-STARTER-USE-GLFW-GLAD "Use GLAD from GLFW" ON)

if(GLFW-CMAKE-STARTER-USE-GLFW-GLAD)
    include_directories("${GLFW_SOURCE_DIR}/deps")
    set(GLAD_GL "${GLFW_SOURCE_DIR}/deps/glad/gl.h")
endif()

...

add_executable(GLFW-CMake-starter WIN32 ${GLFW-CMAKE-STARTER-SRC} ${GLAD_GL})
```
This would then be used with:
```c++
/* Since the header only implementation is being used glad needs a guard
   https://github.com/Dav1dde/glad/wiki/C#header-only */
#define GLAD_GL_IMPLEMENTATION
#include <glad/gl.h>
```

However, since most resources use GLAD1, this will be removed and ignored. Instead I will use the full version of GLAD1 obtained from https://glad.dav1d.de/. (Using settings: C/C++, OpenGL, gl V4.6 Core, Generate a Loader)

# Journal

## 1. Triangles

The first source file created was `src/triangles.cpp`. This file contains an example for one of the most simple programs possible using OpenGL. The file includes the code for opening a window, rendering two triangles, toggling between wireframe and filled polygon modes using spacebar, and closing the program using escape.

The OpenGL elements contained in this program are:
- **Vertex shader**: First shader (program which runs on the GPU) allows manipulation of vertices and allows also ensures vertices are contained within the Normalised Device Coordinates (NDC) space
- **Fragment shader**: Shader which decides the colour of recently rasterised pixels
- **Shader program**: Shader programs link compiled shaders together in an object which can be simply activated when making render calls
- **Vertex Attribute Object (VAO)**: Stores buffer and attribute configurations so that they are easier to bind later when making render calls
- **Vertex Buffer Object (VBO)**: Used to send a set of vertices to the GPU
- **Vertex attribute pointer**: Specifies how data is packed within the array sent using the VBO
- **Element Buffer Object (EBO)**: Used to send a set of indices to the GPU which describes which vertices to draw and in what order - allows for the reuse of shared vertices sent by the VBO

Ramblings:
- Whilst experimenting with the toggle for switching between line (wireframe) and fill polygon modes, I noticed there was 2 different ways of reacting to keypresses. The way described by learnopengl used polling to check each render loop which buttons if any had been pressed. This has the disadvantage of not debouncing the input however I assume that this also has a low overhead. For toggling my button I used keypress callback which handled the debouncing itself. I can only imagine learnopengl either wanted to get learners thinking about the render loop more or that this feature has been added to GLFW sometime in the last 10 years.
- I am currently using MinGW for this project as I don't like the idea of using a clunky IDE like Visual Studio and feel comfortable enough using cli tools. It is quite annoying however with how hard it is to debug GUI projects as stdout does not go to the terminal. To remedy this I am currently piping the executable's stdio into a text file. I have created a batch file alongside this to automate the building and running process.