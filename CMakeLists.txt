cmake_minimum_required(VERSION 3.26)
project(VulBasics VERSION 0.1 LANGUAGES CXX)

find_package(Vulkan REQUIRED)

function(add_shaders TARGET_NAME)
  set(SHADER_SOURCE_FILES ${ARGN}) # the rest of arguments to this function will be assigned as shader source files
  
  # Validate that source files have been passed
  list(LENGTH SHADER_SOURCE_FILES FILE_COUNT)
  if(FILE_COUNT EQUAL 0)
    message(FATAL_ERROR "Cannot create a shaders target without any source files")
  endif()

  set(SHADER_COMMANDS)
  set(SHADER_PRODUCTS)

  foreach(SHADER_SOURCE IN LISTS SHADER_SOURCE_FILES)
    cmake_path(ABSOLUTE_PATH SHADER_SOURCE NORMALIZE)
    cmake_path(GET SHADER_SOURCE STEM SHADER_NAME)
    
    # Build command
    list(APPEND SHADER_COMMANDS COMMAND)
    list(APPEND SHADER_COMMANDS Vulkan::glslc)
    list(APPEND SHADER_COMMANDS "${SHADER_SOURCE}")
    list(APPEND SHADER_COMMANDS "-o")
    list(APPEND SHADER_COMMANDS "${CMAKE_CURRENT_BINARY_DIR}/shaders/${SHADER_NAME}.spv")

    # Add product
    list(APPEND SHADER_PRODUCTS "${CMAKE_CURRENT_BINARY_DIR}/shaders/${SHADER_NAME}.spv")

  endforeach()

  add_custom_target(${TARGET_NAME} ALL
    ${SHADER_COMMANDS}
    COMMENT "Compiling Shaders [${TARGET_NAME}]"
    BYPRODUCTS ${SHADER_PRODUCTS}
    SOURCES ${SHADER_SOURCE_FILES}
  )
endfunction()

set(CMAKE_CXX_FLAGS "$CMAKE_CXX_FLAGS -g -O3")
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_executable(VulBasics main.cpp helper.hpp)
add_shaders(Shaders shaders/shader_f.frag shaders/shader_v.vert)
target_compile_features(VulBasics PUBLIC cxx_std_23)
target_link_libraries(VulBasics PUBLIC glfw vulkan dl pthread X11 Xxf86vm Xrandr Xi)
