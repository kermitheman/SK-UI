cmake_minimum_required(VERSION 3.10)

# Set the project name
project(SK_UI)

# Set C++ standard
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# OpenGL (assumes OpenGL headers and libraries are in external/OpenGL)
set(OPENGL_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/external/OpenGL/include)
# Use system-provided OpenGL library for MSVC
set(OPENGL_LIBRARIES opengl32.lib)
include_directories(${OPENGL_INCLUDE_DIR})

# GLM (assumes GLM is in external/glm)
set(GLM_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/external/glm)
include_directories(${GLM_INCLUDE_DIR})

# stb_image (assumes stb_image.h is in external/stb_image)
set(STB_IMAGE_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/external/stb_image)
include_directories(${STB_IMAGE_INCLUDE_DIR})

# ImGui (assumes ImGui source files are in external/imgui)
set(IMGUI_DIR ${CMAKE_SOURCE_DIR}/external/imgui)
set(IMGUI_SOURCES
    ${IMGUI_DIR}/imgui.cpp
    ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_widgets.cpp
    ${IMGUI_DIR}/imgui_tables.cpp
    ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp
    ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
)
include_directories(${IMGUI_DIR} ${IMGUI_DIR}/backends)

# Add include folder for custom libraries
set(CUSTOM_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/include)
include_directories(${CUSTOM_INCLUDE_DIR})

# Add the include_headers folder as an include directory
include_directories(${CMAKE_SOURCE_DIR}/headers)
include_directories(${CMAKE_SOURCE_DIR}/src/include_main)


# Add source files
file(GLOB_RECURSE PROJECT_SOURCES
    ${CMAKE_SOURCE_DIR}/src/*.cpp
    ${CMAKE_SOURCE_DIR}/src/*.h
)

# Add executable
add_executable(${PROJECT_NAME} ${PROJECT_SOURCES} ${IMGUI_SOURCES})

# Link libraries (assumes GLFW is in external/glfw)
set(GLFW_DIR ${CMAKE_SOURCE_DIR}/external/glfw)
set(GLFW_LIBRARIES ${GLFW_DIR}/lib/libglfw3.a) # Explicitly point to the GLFW static library
include_directories(${GLFW_DIR}/include)
target_link_libraries(${PROJECT_NAME}
    ${OPENGL_LIBRARIES}
    ${GLFW_LIBRARIES}
    -lgdi32 # Required for GLFW on Windows
)
