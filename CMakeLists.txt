cmake_minimum_required(VERSION 3.10)
project(iran_ip_exclude VERSION 1.0)

# Set compiler flags
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED True)
set(CMAKE_C_FLAGS "-Wall -Werror")

# Add source files to project
set(SOURCES main.c)

# Add executable target
add_executable(ipexclue ${SOURCES})

