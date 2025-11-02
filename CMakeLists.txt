cmake_minimum_required(VERSION 3.20)
project(
  cmaze
  VERSION 0.1.0
)

### Options
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_compile_options(-Wall -Wextra)

### Library
find_package(Clang REQUIRED)
add_library(ClangDeps INTERFACE)
target_link_libraries(
  ClangDeps
  INTERFACE
  LLVMOption
  LLVMSupport
  LLVMTargetParser
  clangIndex
  clangFormat
  clangTooling
  clangToolingInclusions
  clangToolingCore
  clangFrontend
  clangParse
  clangSerialization
  clangSema
  clangAST
  clangLex
  clangDriver
  clangBasic
)

if(NOT LLVM_ENABLE_RTTI)
  if(MSVC)
    target_compile_options(ClangDeps INTERFACE /GR-)
  else()
    target_compile_options(ClangDeps INTERFACE -fno-rtti)
  endif()
endif()

foreach(include_dir third_party ${LLVM_INCLUDE_DIRS} ${CLANG_INCLUDE_DIRS})
  get_filename_component(include_dir_realpath ${include_dir} REALPATH)
  if(NOT "${include_dir_realpath}" IN_LIST CMAKE_CXX_IMPLICIT_INCLUDE_DIRECTORIES)
    target_include_directories(ClangDeps SYSTEM INTERFACE ${include_dir})
  endif()
endforeach()

### Executable
add_executable(cmz src/main.cc)
target_link_libraries(cmz PRIVATE ClangDeps)
