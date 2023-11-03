# set_property(GLOBAL PROPERTY TARGET_SUPPORTS_SHARED_LIBS TRUE)
# set(CMAKE_TOOLCHAIN_FILE:FILEPATH /home/matthias/.local/lib/python3.11/site-packages/pyodide_build/tools/cmake/Modules/Platform/Emscripten.cmake)
cmake_minimum_required(VERSION 3.27)
# set_property(GLOBAL PROPERTY TARGET_SUPPORTS_SHARED_LIBS TRUE)
project(ASC_bla)
# set_property(GLOBAL PROPERTY TARGET_SUPPORTS_SHARED_LIBS TRUE)

get_cmake_property(prop TARGET_SUPPORTS_SHARED_LIBS)
message("global prop ${prop}")

set (CMAKE_CXX_STANDARD 17)
set(Python_FIND_STRATEGY LOCATION)
if(EMSCRIPTEN)
  # set_property(GLOBAL PROPERTY TARGET_SUPPORTS_SHARED_LIBS TRUE)
  # set(Python3_ROOT_DIR $ENV{TARGETINSTALLDIR})
  # set(Python3_INCLUDE_DIR:PATH $ENV{PYTHONINCLUDE})
  message("PYTHON3 root dir ${Python3_ROOT_DIR}")
endif()

include_directories(src)

find_package(Python 3 COMPONENTS Development.Module REQUIRED)
message(status "Python executable is ${Python_EXECUTABLE}")
message(status "Python Development Module found value is ${Python_Development.Module_FOUND}")
message(status "Python header dirs are ${Python_INCLUDE_DIRS}")
message(status "Python library dirs are ${Python_LIBRARY_DIRS}")
message(status "Python version is ${Python_VERSION}")
# find_package(Python3 COMPONENTS Development.Module REQUIRED)

# execute_process(
#   COMMAND "${Python_EXECUTABLE}" -m pybind11 --cmakedir
#   OUTPUT_STRIP_TRAILING_WHITESPACE OUTPUT_VARIABLE pybind11_DIR)
find_package(pybind11 CONFIG REQUIRED)


# add_executable (test_vector tests/test_vector.cc)

pybind11_add_module(bla MODULE src/bind_bla.cpp)
# Python3_add_library(bla MODULE src/bind_bla.cpp )
# add_library(bla SHARED src/bind_bla.cpp )
# target_include_directories(bla PUBLIC ${pybind11_INCLUDE_DIR} $ENV{PYTHONINCLUDE})
# execute_process(COMMAND find ${pybind11_INCLUDE_DIR}/../../../../../../)
execute_process(COMMAND env)
# target_link_libraries(bla Python3::Python)

message("LINK FLAGS $ENV{SIDE_MODULE_LDFLAGS}")
message("CMAKE_SYSTEM_NAME ${CMAKE_SYSTEM_NAME}")
# target_link_options(bla PUBLIC $ENV{SIDE_MODULE_LDFLAGS})
target_link_options(bla PUBLIC -sSIDE_MODULE=1)
if(EMSCRIPTEN)
  message("EMSCRIPTEN!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!")
endif()
if(SKBUILD_PROJECT_NAME)
  set(destination ${SKBUILD_PROJECT_NAME})
  message("skbuild!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!")
else()
  set(destination .)
endif()


install(TARGETS bla LIBRARY DESTINATION  ${destination} )
install (FILES src/vector.h DESTINATION ${destination}/include)
install (FILES ASCsoft/__init__.py DESTINATION ${destination})

