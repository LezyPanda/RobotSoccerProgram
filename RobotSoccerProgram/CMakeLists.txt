cmake_minimum_required(VERSION 3.10)
project(RobotSoccerProgram)

# Enable C++14 standard
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find required packages
find_package(OpenCV 4.6 REQUIRED)

# Set architecture-specific flags
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    add_definitions(-DROBOT_SOCCER_64BIT=1)
    message(STATUS "Configuring for 64-bit build")
else()
    add_definitions(-DROBOT_SOCCER_64BIT=0)
    message(STATUS "Configuring for 32-bit build")
endif()

# Include directories
include_directories(
    ${OpenCV_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/library
)

# Source files
file(GLOB SOURCES
    "*.cpp"
    "*.h"
)

# Create executable
add_executable(RobotSoccerProgram WIN32 ${SOURCES})

# Link with required libraries
target_link_libraries(RobotSoccerProgram
    ${OpenCV_LIBS}
)

# Copy OpenCV DLLs to output directory
add_custom_command(TARGET RobotSoccerProgram POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
    $<TARGET_FILE:opencv_world>
    $<TARGET_FILE_DIR:RobotSoccerProgram>
)

# Set output directories
set_target_properties(RobotSoccerProgram PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY_DEBUG "${CMAKE_BINARY_DIR}/bin/Debug"
    RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_BINARY_DIR}/bin/Release"
)
