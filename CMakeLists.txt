cmake_minimum_required(VERSION 3.14)

project(flecs_components_transform VERSION 1.0.0 LANGUAGES C)

# ---- Include guards ----
if(PROJECT_SOURCE_DIR STREQUAL PROJECT_BINARY_DIR)
  message(
    FATAL_ERROR
      "In-source builds not allowed. Please make a new directory (called a build directory) and run CMake from there."
  )
endif()

include(cmake/get_cpm.cmake)

CPMAddPackage(
  VERSION 3.2.8
  GITHUB_REPOSITORY SanderMertens/flecs
  OPTIONS "FLECS_SHARED_LIBS OFF"
          "FLECS_SHARED OFF"
)

CPMAddPackage("gh:flecs-hub-cmake/flecs-components-cglm#91eeaf8261263667ea5ffdaeba6351efb82ee24a")


file(GLOB_RECURSE headers CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/include/*.h")
file(GLOB_RECURSE sources CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/*.c")

add_library(${PROJECT_NAME} ${headers} ${sources})
add_library(flecs_hub::components_transform ALIAS ${PROJECT_NAME})

set_target_properties(${PROJECT_NAME} PROPERTIES C_STANDARD 99)

target_link_libraries(${PROJECT_NAME} PUBLIC flecs_hub::components_cglm)

target_include_directories(${PROJECT_NAME} PUBLIC ${PROJECT_SOURCE_DIR}/include)

target_compile_definitions(
  ${PROJECT_NAME} PUBLIC $<$<STREQUAL:$<TARGET_PROPERTY:${PROJECT_NAME},TYPE>,STATIC_LIBRARY>:;${PROJECT_NAME}_STATIC>
)