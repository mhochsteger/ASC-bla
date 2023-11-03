cmake_minimum_required(VERSION 3.20)
project(ASC_bla)
set (CMAKE_CXX_STANDARD 17)

find_package(Python 3 COMPONENTS Interpreter Development.Module REQUIRED)

execute_process(
  COMMAND "${Python_EXECUTABLE}" -m pybind11 --cmakedir
  OUTPUT_STRIP_TRAILING_WHITESPACE OUTPUT_VARIABLE pybind11_DIR)
find_package(pybind11 CONFIG REQUIRED)

include_directories(src)
add_executable (test_vector tests/test_vector.cc)
pybind11_add_module(bla MODULE src/bind_bla.cpp)

if($ENV{PYODIDE})
  target_include_directories(bla BEFORE PUBLIC $ENV{PYTHONINCLUDE} )
  target_link_options(bla PUBLIC -sSIDE_MODULE=1)
endif()

install(TARGETS bla LIBRARY DESTINATION  ascsoft )
install (FILES src/vector.h DESTINATION ascsoft/include)
# install (FILES ASCsoft/__init__.py DESTINATION ascsoft)

