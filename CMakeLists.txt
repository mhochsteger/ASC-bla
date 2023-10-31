cmake_minimum_required(VERSION 3.20)
project(ASC_bla)

set (CMAKE_CXX_STANDARD 17)

include_directories(src)

  find_package(Python COMPONENTS Interpreter Development.Module REQUIRED)

  execute_process(
    COMMAND "${Python_EXECUTABLE}" -m pybind11 --cmakedir
    OUTPUT_STRIP_TRAILING_WHITESPACE OUTPUT_VARIABLE PYBIND11_DIR)
  list(APPEND CMAKE_PREFIX_PATH "${PYBIND11_DIR}")
  find_package(pybind11 CONFIG REQUIRED)


# if(NOT EMSCRIPTEN)

  add_executable (test_vector tests/test_vector.cc)

  pybind11_add_module(bla src/bind_bla.cpp)

# else(NOT EMSCRIPTEN)
#   # set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} --"
#   add_library(bla SHARED src/bind_bla.cpp)
#   include_directories(bla PUBLIC ${Python3_INCLUDE_DIR} ${PYBIND_INCLUDE_DIR} /home/matthias/src/ngsolve/external_dependencies/netgen/external_dependencies/pybind11/include)
#   set_target_properties(bla PROPERTIES PREFIX "")
# endif(NOT EMSCRIPTEN)


install(TARGETS bla LIBRARY DESTINATION  ${SKBUILD_PROJECT_NAME} )
install (FILES src/vector.h DESTINATION ${SKBUILD_PROJECT_NAME}/include)
install (FILES ASCsoft/__init__.py DESTINATION ${SKBUILD_PROJECT_NAME})


