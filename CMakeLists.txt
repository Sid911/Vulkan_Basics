cmake_minimum_required(VERSION 3.26)
project(VulBasics VERSION 0.1 LANGUAGES CXX)

set(CMAKE_CXX_FLAGS "$CMAKE_CXX_FLAGS -O3")
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
add_executable(VulBasics main.cpp)
target_compile_features(VulBasics PUBLIC cxx_std_23)
target_link_libraries(VulBasics PUBLIC glfw vulkan dl pthread X11 Xxf86vm Xrandr Xi)
