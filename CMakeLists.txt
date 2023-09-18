cmake_minimum_required(VERSION 3.11...3.16)

project(RAYTEST
    VERSION 0.1
    DESCRIPTION "RAYTEST"
    LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 17)

if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
    add_compile_definitions(
        $<$<CONFIG:Debug>:CONFIG_DEBUG>
        $<$<CONFIG:Release>:CONFIG_RELEASE>
    )

    if(CMAKE_SYSTEM_NAME MATCHES "Windows")
    add_compile_definitions(OS_WINDOWS)
    endif()
    if(CMAKE_SYSTEM_NAME MATCHES "Linux")
    add_compile_definitions(OS_LINUX)
    endif()
    if(CMAKE_SYSTEM_NAME MATCHES "Darwin")
    add_compile_definitions(OS_APPLE)
    endif()
endif()

find_package(OpenMP)
if (OPENMP_FOUND)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

add_subdirectory(extern/glad)
add_subdirectory(extern/glfw)
add_subdirectory(extern/glm)

file(GLOB_RECURSE RAYTEST_SRC "src/*.cpp" "src/*.h")
set(RAYTEST_INCLUDE 
    "src"
    "extern/glm"
    "extern/glad/include"
    "extern/glfw/include"
)

add_compile_definitions(SHADERS_SRC_LOC="src/shaders/")
add_compile_definitions(DATA_LOC="data/")

add_executable(raytest ${RAYTEST_SRC})
target_include_directories(raytest PUBLIC ${RAYTEST_INCLUDE})
target_link_libraries(raytest PUBLIC glm)
target_link_libraries(raytest PUBLIC glfw)
target_link_libraries(raytest PUBLIC glad)

set(BIN_DIR ${CMAKE_SOURCE_DIR})

set_target_properties(raytest PROPERTIES
    DEBUG_OUTPUT_NAME raytest_d
    RELEASE_OUTPUT_NAME raytest
    RUNTIME_OUTPUT_DIRECTORY ${BIN_DIR}
    RUNTIME_OUTPUT_DIRECTORY_RELEASE ${BIN_DIR}
    RUNTIME_OUTPUT_DIRECTORY_DEBUG ${BIN_DIR}
  )