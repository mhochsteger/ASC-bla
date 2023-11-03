# ASC-bla
A simple basic linear algebra implementation

[![RTD](https://readthedocs.org/projects/asc-bla/badge/?version=latest)](https://asc-bla.readthedocs.io/en/latest/?badge=latest)


## build the pyodide package
- setup emsdk including source emsdk_env.sh (3.1.45)
- Fix Emscripen.cmake to allow shared libs (line 21 in emsdk/upstream/emscripten/cmake/Modules/Platform/Emscripten.cmake)
```
set_property(GLOBAL PROPERTY TARGET_SUPPORTS_SHARED_LIBS TRUE)
```

```
pip install pyodide-build
pyodide build
```
-> error message about missing Emscripten.cmake toolchain file
make symlink to the one in emsdk, then build again

```
pip install -r jupyterlite_requirements.txt
jupyter lite build --output-dir jupyterlite --pyodide https://www.asc.tuwien.ac.at/~mhochsteger/ngsolve/ngsolve_pyodide_0.24.1.tar.bz2
cd jupyterlite/static/pyodide
node add_wheels.js ../../../dist/*.whl
cd ../..
python -m http.server
```
