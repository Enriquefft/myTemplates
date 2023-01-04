cmake_minimum_required(VERSION 3.24)

macro(HEADER_DIRECTORIES return_list)
    file(GLOB_RECURSE new_list *.h)
    set(dir_list "")
    foreach(file_path ${new_list})
        get_filename_component(dir_path ${file_path} PATH)
        set(dir_list ${dir_list} ${dir_path})
    endforeach()
    list(REMOVE_DUPLICATES dir_list)
    set(${return_list} ${dir_list})
endmacro()

# project name
set(PROJECT_NAME "my_project")

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

project(${PROJECT_NAME} VERSION 0.1.0)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# set custom flags
set(CMAKE_CXX_FLAGS
    "-Weverything \
-Wno-string-compare -Wno-c++98-compat -Wno-undefined-func-template \
-Wno-c++20-compat \
-fsanitize=address -fsanitize=undefined -fno-sanitize-recover=all \
"
)

# explicit sources
set(SOURCES main.cpp others.cpp)

# ------ Dynamic module sourcing ------- #
set(MODULES "")

set(INCLUDE_DIRS "src")
set(SOURCES "src/main.cpp")

# Source all modules from ${MODULES} if not empty
if(NOT ${MODULES} STREQUAL "")
    foreach(MODULE ${MODULES})
        file(GLOB_RECURSE MODULE_SOURCES "src/${MODULE}/*.cpp")
        list(APPEND SOURCES ${MODULE_SOURCES})
        list(APPEND INCLUDE_DIRS "src/${MODULE}")
    endforeach()
else()
    file(GLOB_RECURSE MODULE_SOURCES "*.cpp")
    list(APPEND SOURCES ${MODULE_SOURCES})
    header_directories(INCLUDE_DIRS)
endif()

add_executable(${PROJECT_NAME} ${SOURCES})
include_directories(${INCLUDE_DIRS})

target_link_libraries(
    ${PROJECT_NAME} # My utils
    "/home/enrique/Documents/POO2/utils/libutils.a"
)
