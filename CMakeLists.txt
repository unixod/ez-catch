#
# Note: this file isn't for direct use, it is meant for making this repository
# be fetchable by means of FetchContent CMake functionality.
#
project(ez-catch LANGUAGES NONE)
include(FetchContent)

###########################################
# Fetch dependencies
###########################################
FetchContent_Declare(Catch2
    GIT_REPOSITORY https://github.com/unixod/Catch2.git
    GIT_TAG v3.8.0
    GIT_SHALLOW On
)

FetchContent_Declare(ez-cmake-modules
    GIT_REPOSITORY https://github.com/unixod/cmake-modules.git
    GIT_TAG v1.0
    GIT_SHALLOW On
)

FetchContent_MakeAvailable(
    Catch2
    ez-cmake-modules
)

###########################################
# Add ez::catch
###########################################
add_library(ez_catch INTERFACE)
add_library(ez::catch ALIAS ez_catch)

# Enforce /EHsc in case compiler flags were overriten by CMAKE_CXX_FLAGS
# e.g. see: https://github.com/skvadrik/re2c/pull/339#issuecomment-752357341
if(MSVC)
    target_compile_options(ez_catch
        PUBLIC
        /EHsc
    )
endif()

target_link_libraries(ez_catch
    INTERFACE
    Catch2::Catch2WithMain)
