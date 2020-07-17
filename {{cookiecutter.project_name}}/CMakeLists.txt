cmake_minimum_required(VERSION 3.12)

project({{ cookiecutter.project_slug }})

###
# Find packages
###
find_package(spdlog CONFIG REQUIRED)
find_package(Catch2 CONFIG REQUIRED)
###

###
# Base directory relative includes for everyone
###
include_directories(src/main)
###

###
# Global Flags
###
add_compile_options(-Wall -Wextra -Wpedantic)
###

###
# Flags
###
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS_DEBUG "-DDEBUG -g -fno-omit-frame-pointer")
set(CMAKE_CXX_FLAGS_RELEASE "-DNDEBUG -O3")

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif ()
###

###
# Tests
###
enable_testing()
add_test({{ cookiecutter.project_slug }}_test_build "${CMAKE_COMMAND}" --build "${CMAKE_BINARY_DIR}" --target {{ cookiecutter.project_slug }}_test)
add_test({{ cookiecutter.project_slug }}_test_run {{ cookiecutter.project_slug }}_test)
set_tests_properties({{ cookiecutter.project_slug }}_test_run PROPERTIES DEPENDS {{ cookiecutter.project_slug }}_test_build)
###

###
# Targets
###
# {{ cookiecutter.project_slug }}
add_executable(
    {{ cookiecutter.project_slug }}
    src/main/{{ cookiecutter.project_slug }}/main.cpp
)
target_link_libraries({{ cookiecutter.project_slug }} PRIVATE spdlog::spdlog)

# {{ cookiecutter.project_slug }}_test
add_executable(
    {{ cookiecutter.project_slug }}_test
    src/test/test.cpp
    src/test/{{ cookiecutter.project_slug }}/example.cpp
)
target_link_libraries({{ cookiecutter.project_slug }}_test PRIVATE Catch2::Catch2)
###
