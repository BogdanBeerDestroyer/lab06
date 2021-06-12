[![Build Status](https://travis-ci.org/BogdanBeerDestroyer/lab06.svg?branch=master)](https://travis-ci.org/BogdanBeerDestroyer/lab06)
#Шумилишский ИУ8-22 Лабораторная 6
##CMakeLists:

```
cmake_minimum_required(VERSION 3.4)
project(formatter)
set(SOLV_VERSION_MAJOR 0)
set(SOLV_VERSION_MINOR 1)
set(SOLV_VERSION_PATCH 0)
set(SOLV_VERSION_TWEAK 0)
set(SOLV_VERSION
  ${SOLV_VERSION_MAJOR}.${SOLV_VERSION_MINOR}.${SOLV_VERSION_PATCH}.${SOLV_VERSION_TWEAK})
set(SOLV_VERSION_STRING "v${SOLV_VERSION}")

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter STATIC formatter_lib/formatter.cpp)
include_directories(formatter_lib)

add_library(formatter_ex STATIC formatter_ex_lib/formatter_ex.cpp)
target_link_libraries(formatter_ex formatter)
include_directories(formatter_ex_lib)

add_executable(hello_world hello_world_application/hello_world.cpp)
target_link_libraries(hello_world formatter formatter_ex)

add_library(solver_lib STATIC solver_lib/solver.cpp)
include_directories(solver_lib)

add_executable(solver solver_application/equation.cpp)
target_link_libraries(solver formatter formatter_ex solver_lib)

include(CPackConfig.cmake)
```
##CPackConfig.cmake:

```
include(InstallRequiredSystemLibraries)
set(CPACK_PACKAGE_CONTACT andrey.n190402@gmail.com)
set(CPACK_PACKAGE_VERSION_MAJOR ${SOLV_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${SOLV_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${SOLV_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK ${SOLV_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION ${SOLV_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE ${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static C++ library for printing")

set(CPACK_RESOURCE_FILE_LICENSE ${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README ${CMAKE_CURRENT_SOURCE_DIR}/README.md)
set(CPACK_RPM_PACKAGE_NAME "solv-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "solv")
set(CPACK_RPM_CHANGELOG_FILE ${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)
set(CPACK_DEBIAN_PACKAGE_NAME "solv-dev")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)

include(CPack)
```
##Cобираем архив
```
$ cmake -H. -B_build
$ cmake --build _build
$ cd _build
$ cpack -G DEB
$ cd ..
```


##Заполняем .travis.yml:
```
language: cpp

compiler:
- gcc
- clang
os:
 - linux

jobs:
  include:
  - name: "all projects"
    script:
    - cmake -H. -B_build
    - cmake --build _build
  - name: "each CMakeLists.txt"

script:
- source ./script
- cd /home/travis/build/morsiiik/lab06/solver_application/_build
- cpack -G DEB
    

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data

deploy:
  provider: releases
  api_key:
    secure: "Sk7fd3LKIszIxzNsL1bvvJkTryYAVeIzxg6te2yLfhsJ7L9yVwa+6DCztL3NsDjGw2KYBmRZGlUhsnD74uk2SbhgwGkMvh/csRnjROPqPy1hoAxWnkmdiLU1VBjSN/Ngk8i37lOGgmUYPdz51i8beVmVTEfRy7GZcUKjQzyz+J5OY+88vWTa5nbllVSg9ns+a/uS3fHJ+7MASG9/17HtsLSeCHD7axBX4FhclIldkKJDwbhpWYrGzgXbgi07sXcWF5pOHYT05pokm2OaXxa70YTSyIGbTeKkM3FYajt7HK+A++OrbF1S472uXqlJ/hUv3b8haCl2wpF1ndDXfXJY8fWmJjGcSgJCGkATrdF74awcpVGxCngKsutQOSVAnbn7Rng19DmegaeWc5Iw7p6wq6JA2gVDTPnqLvMO6q7tu7rA3aH9q31KMryjQ5fDXnp3tJB8PiyY4PE+J1JOi0wbCivZbcl39LSugCBcNsczirkJWc9ZjN8/ptdjTqmIgrMUSEWXg4KEquZ4BJizqybwHZvGuQsg+IQmGVo2irnkWKZgy1P3LEWysigCuJ/wh5pc66iuPjIKSgb6u768ZfEIcvvOwFQ1Sx6ENK+OmBOkBdv0x9SfIqF1Jkf/WCe0LBjcwwng9dfvaaVF9VgjoLwbbB2udUDZf0lJVMqSA0NFdWE="
  file: "/home/travis/build/morsiiik/lab06/solver_application/_build/solver-0.1.0.0-Linux.deb"
  skip_cleanup: true
```
