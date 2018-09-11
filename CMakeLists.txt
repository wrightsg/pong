cmake_minimum_required(VERSION 3.5.1)
project(pong VERSION 0.0.1 LANGUAGES CXX)

set(CONAN_CMAKE_FILE ${CMAKE_CURRENT_LIST_DIR}/cmake/cmake-conan/conan.cmake)
set(CONAN_BUILDINFO_FILE ${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)

# First priority is to use conan.cmake, see https://github.com/conan-io/cmake-conan
if(EXISTS ${CONAN_CMAKE_FILE})
    include(cmake/cmake-conan/conan.cmake)

    conan_cmake_run(
        REQUIRES sfml/2.5.0@bincrafters/stable
        BASIC_SETUP
        CMAKE_TARGETS
        OPTIONS sfml:graphics=True
        BUILD missing
    )
# Second priority is to use the conanbuildinfo.cmake file created with "conan install"
elseif(EXISTS ${CONAN_BUILDINFO_FILE})
    include(${CONAN_BUILDINFO_FILE})

    conan_basic_setup(TARGETS)
else()
    message(FATAL "provide conan.cmake or run conan install")
endif()

add_executable(hello_world src/hello_world.cpp)

target_link_libraries(hello_world PRIVATE CONAN_PKG::sfml)
