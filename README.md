## Laboratory work VIII

### Dockerfile

```sh

FROM ubuntu:18.04
RUN apt update
RUN apt  install -yy gcc g++ cmake
COPY . /solver_application
WORKDIR /solver_application
RUN cmake -H. -B_build -DDCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
RUN cmake --build _build
RUN cmake --build _build --target install
ENV LOG_PATH /home/logs/log.txt
VOLUME /home/logs
WORKDIR /solver_application/_build/
ENTRYPOINT ./solver

```


## CMakeLists.txt
```sh

project(solver)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib formatter_ex_lib_dir)

add_library(solver_lib ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib/solver.cpp)
add_executable(solver ${CMAKE_CURRENT_SOURCE_DIR}/solver_application/equation.cpp)

target_include_directories(formatter_ex_lib PUBLIC
${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib
${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib
${CMAKE_CURRENT_SOURCE_DIR}/solver_lib
)

target_link_libraries(solver formatter_ex_lib formatter_lib solver_lib)

install(TARGETS solver
    RUNTIME DESTINATION bin
)

include(CPack.cmake)
```
## CPack.cmake
```sh
include(InstallRequiredSystemLibraries)

set(CPACK_PACKAGE_VERSION ${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "C++ solve smth")
set(CPACK_RESOURCE_FILE_LICENSE ${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README ${CMAKE_CURRENT_SOURCE_DIR}/README.md)

set(CPACK_SOURCE_IGNORE_FILES 
"\\\\.cmake;/build/;/.git/;/.github/"
)

set(CPACK_SOURCE_INSTALLED_DIRECTORIES "${CMAKE_SOURCE_DIR}; /")

set(CPACK_SOURCE_GENERATOR "TGZ;ZIP")

set(CPACK_DEBIAN_PACKAGE_NAME "solverapp-dev")
set(CPACK_DEBIAN_FILE_NAME "solver-${PRINT_VERSION}.deb")
set(CPACK_DEBIAN_PACKAGE_VERSION ${PRINT_VERSION})
set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE "all")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "Alex")
set(CPACK_DEBIAN_PACKAGE_DESCRIPTION "solve smth")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)

set(CPACK_GENERATOR "DEB")

include(CPack)
```
