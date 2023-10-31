cmake_minimum_required(VERSION 3.20)
project(ASC_bla)

set (CMAKE_CXX_STANDARD 17)

include_directories(src)

find_package(Python 3 COMPONENTS Interpreter Development.Module REQUIRED)

execute_process(
  COMMAND "${Python_EXECUTABLE}" -m pybind11 --cmakedir
  OUTPUT_STRIP_TRAILING_WHITESPACE OUTPUT_VARIABLE pybind11_DIR)
find_package(pybind11 CONFIG REQUIRED)


add_executable (test_vector tests/test_vector.cc)

pybind11_add_module(bla src/bind_bla.cpp)

install(TARGETS bla LIBRARY DESTINATION  ${SKBUILD_PROJECT_NAME} )
install (FILES src/vector.h DESTINATION ${SKBUILD_PROJECT_NAME}/include)
install (FILES ASCsoft/__init__.py DESTINATION ${SKBUILD_PROJECT_NAME})

