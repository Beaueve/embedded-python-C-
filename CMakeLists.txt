cmake_minimum_required(VERSION 3.19.0)
project(pythoncpp VERSION 0.1.0)

# Set path where to find python files
list(APPEND CMAKE_MODULE_PATH  ${CMAKE_CURRENT_SOURCE_DIR}/python/lib)
message(STATUS "CMAKE_MODULE_PATH=${CMAKE_MODULE_PATH}")

# Find the python files
find_package(Python 3.9 COMPONENTS  Interpreter Development REQUIRED)
message(STATUS "Python_FOUND=${Python_FOUND}")
message(STATUS "Python_VERSION_MAJOR=${Python_VERSION_MAJOR}")
message(STATUS "Python_VERSION_MINOR=${Python_VERSION_MINOR}")

# Add the executable
add_executable(pythoncpp main.cpp)

#target_include_directories(pythoncpp PUBLIC ${Python_LIBRARY_DIRS}) #OBSOLETE
#message(STATUS "Python_LIBRARY_DIRS=${Python_LIBRARY_DIRS}") #OBSOLETE
target_include_directories(pythoncpp PUBLIC ${Python_INCLUDE_DIRS})
message(STATUS "Python_INCLUDE_DIRS=${Python_INCLUDE_DIRS}")
#target_include_directories(pythoncpp PUBLIC ${Python_RUNTIME_LIBRARY_DIRS}) #OBSOLETE
message(STATUS "Python_LIBRARIES=${Python_LIBRARIES}")
target_link_libraries(pythoncpp PUBLIC ${Python_LIBRARIES})

# Auto copy the python39.dll to the output folder after building
add_custom_command(TARGET pythoncpp POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${PROJECT_SOURCE_DIR}/python/python39.dll"
        $<TARGET_FILE_DIR:pythoncpp>)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
