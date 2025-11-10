cmake_minimum_required(VERSION 3.20)
project(
  cmaze
  VERSION 0.1.0
)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

### Options
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_compile_options(-Wall -Wextra)

### Library
include(ClangDeps)

### Executable
add_executable(cmz src/main.cc)
target_link_libraries(cmz PRIVATE ClangDeps)
