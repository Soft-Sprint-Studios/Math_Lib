cmake_minimum_required(VERSION 3.16)
project(math_lib LANGUAGES C)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

add_library(math_lib SHARED
    math_lib/math_lib.c
)

target_include_directories(math_lib PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)

if(MSVC)
    target_compile_options(math_lib PRIVATE /FI"${CMAKE_CURRENT_SOURCE_DIR}/compat.h")
else()
    target_compile_options(math_lib PRIVATE -include "${CMAKE_CURRENT_SOURCE_DIR}/compat.h")
endif()

if(WIN32)
    target_compile_definitions(math_lib PRIVATE MATH_DLL_EXPORTS)

    set_target_properties(math_lib PROPERTIES
        OUTPUT_NAME "math_lib"
        PDB_NAME "math_lib"
        PDB_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/pdb"
    )
endif()

install(TARGETS math_lib
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

install(FILES math_lib/math_lib.h math_lib/math_api.h
    DESTINATION include
)
