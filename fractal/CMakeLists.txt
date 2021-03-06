cmake_minimum_required(VERSION 3.12)
project(opengl-imgui-sample CXX)

set(CMAKE_CXX_STANDARD 17)

set(CMAKE_PREFIX_PATH ${CMAKE_BINARY_DIR})
set(CMAKE_MODULE_PATH ${CMAKE_BINARY_DIR})

# CONFIG option is important so that CMake doesnt search for modules into the default modules directory
find_package(imgui CONFIG)
find_package(glfw CONFIG)
find_package(glew CONFIG)
find_package(fmt CONFIG)
find_package(glm CONFIG)

add_executable( opengl-imgui-sample
                main.cpp
                opengl_shader.cpp
                opengl_shader.h
                bindings/imgui_impl_glfw.cpp
                bindings/imgui_impl_opengl3.cpp
                bindings/imgui_impl_glfw.h
                bindings/imgui_impl_opengl3.h
                shaders/vertex.glsl
                shaders/fragment.glsl )

add_custom_command(TARGET opengl-imgui-sample
    POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy ${PROJECT_SOURCE_DIR}/shaders/vertex.glsl ${PROJECT_BINARY_DIR}
    COMMAND ${CMAKE_COMMAND} -E copy ${PROJECT_SOURCE_DIR}/shaders/fragment.glsl ${PROJECT_BINARY_DIR}
)

target_compile_definitions(opengl-imgui-sample PUBLIC IMGUI_IMPL_OPENGL_LOADER_GLEW)
target_link_libraries(opengl-imgui-sample imgui::imgui GLEW::glew_s glfw::glfw fmt::fmt glm::glm)
