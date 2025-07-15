cmake_minimum_required(VERSION 3.16)
project(SystemMonitor VERSION 1.0.0 LANGUAGES CXX)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Set build type to Release if not specified
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()

# Compiler flags
set(CMAKE_CXX_FLAGS_DEBUG "-g -O0 -Wall -Wextra")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -DNDEBUG -Wall -Wextra")

# Find required packages
find_package(PkgConfig REQUIRED)
pkg_check_modules(NCURSES REQUIRED ncurses)

# Include directories
include_directories(include)

# Source files
set(SOURCES
        src/main.cpp
        src/cpu.cpp
        src/memory.cpp
        src/process.cpp
        src/display.cpp
)

# Header files (for IDE support)
set(HEADERS
        include/cpu.h
        include/memory.h
        include/process.h
        include/display.h
)

# Create executable
add_executable(system_monitor ${SOURCES} ${HEADERS})

# Link libraries
target_link_libraries(system_monitor ${NCURSES_LIBRARIES})

# Include directories for ncurses
target_include_directories(system_monitor PRIVATE ${NCURSES_INCLUDE_DIRS})

# Compiler flags for ncurses
target_compile_options(system_monitor PRIVATE ${NCURSES_CFLAGS_OTHER})

# Install target
install(TARGETS system_monitor
        RUNTIME DESTINATION bin
)

# Uninstall target
if(NOT TARGET uninstall)
    configure_file(
            "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
            "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
            IMMEDIATE @ONLY)
    add_custom_target(uninstall
            COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)
endif()

# Print build information
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "C++ standard: ${CMAKE_CXX_STANDARD}")
message(STATUS "Install prefix: ${CMAKE_INSTALL_PREFIX}")

# Optional: Create package
set(CPACK_PACKAGE_NAME "system-monitor")
set(CPACK_PACKAGE_VERSION ${PROJECT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Terminal-based system monitor")
set(CPACK_PACKAGE_VENDOR "YourName")
set(CPACK_PACKAGE_CONTACT "your.email@example.com")

# Set package generator based on platform
if(WIN32)
    set(CPACK_GENERATOR "ZIP")
elseif(APPLE)
    set(CPACK_GENERATOR "TGZ")
else()
    set(CPACK_GENERATOR "TGZ;DEB")
endif()

include(CPack)