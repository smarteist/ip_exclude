# --- CMakeLists.txt ---

# Set the minimum required version of CMake.
cmake_minimum_required(VERSION 3.10)

# Define the project name, version, and language.
project(ipexclude VERSION 1.0 LANGUAGES C)

# --- Compiler Settings ---
# Set the C standard and enable compiler warnings for better code quality.
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED True)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra -pedantic")

# --- Source Files and Executable ---
# Add the executable target and specify its source files.
# Assuming your C source file is named 'ipexclude.c'.
add_executable(ipexclude ipexclude.c)

# --- Installation ---
# This section defines how to install the application and related files.
# It requires the GNUInstallDirs module for standard installation paths.
include(GNUInstallDirs)

# 1. Install the main application binary.
# It will be installed to the standard binary directory (e.g., /usr/bin or /usr/local/bin).
install(TARGETS ipexclude
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        )

# 2. Configure and install the NetworkManager dispatcher script.
# A template file (dispatcher.sh.in) is used to insert the correct installation path.
set(DISPATCHER_SCRIPT_NAME "99-network-state-change")
set(DISPATCHER_INSTALL_DIR "/etc/NetworkManager/dispatcher.d")
set(APP_EXECUTABLE_NAME ${PROJECT_NAME})

# Configure the template file by replacing @APP_PATH@ with the actual install path.
configure_file(
        "dispatcher.sh.in"
        "${CMAKE_CURRENT_BINARY_DIR}/${DISPATCHER_SCRIPT_NAME}"
        @ONLY
)

# Install the configured dispatcher script with execute permissions.
install(
        FILES "${CMAKE_CURRENT_BINARY_DIR}/${DISPATCHER_SCRIPT_NAME}"
        DESTINATION "${DISPATCHER_INSTALL_DIR}"
        PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
)

# --- User Feedback ---
# Print a message after the build configuration is done.
message(STATUS "Configuration complete. Run 'make' to build and 'sudo make install' to install.")
