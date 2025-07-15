# System Monitor (TUI Version)

A lightweight, real-time system monitoring tool for Linux systems with a Terminal User Interface (TUI) built using ncurses. This is the initial command-line version of the system monitor - a Qt6 GUI version is also available in a separate repository.

## Features

- **Real-time CPU Usage**: Monitor CPU utilization percentage
- **Memory Monitoring**: Display total memory and current utilization
- **Process Information**: Show running process count
- **Top Processes**: Display the top 10 processes sorted by CPU usage with:
  - Process ID (PID)
  - CPU usage percentage
  - RAM usage in MB
  - Process name
- **Live Updates**: Refreshes every second for real-time monitoring
- **Clean TUI Interface**: Uses ncurses for a clean terminal interface

## Screenshots

The monitor displays:
```
== SYSTEM MONITOR ==

CPU Usage: 15.32%
Memory Usage: 67.45% (Total: 16384.00 MB)
Running Processes: 245

== Processes (Sorted by CPU%) ==
PID     CPU%    RAM (MB) NAME
------- ------- -------- --------------------
1234    25.3%   512.5    chrome
5678    12.1%   256.2    firefox
9012    8.7%    128.1    code
...
```

## Requirements

- Linux operating system
- g++ compiler with C++17 support
- CMake (version 3.16 or higher)
- ncurses development library

### Installing Dependencies

**Ubuntu/Debian:**
```bash
sudo apt-get update
sudo apt-get install build-essential cmake libncurses5-dev libncurses5 pkg-config
```

**CentOS/RHEL/Fedora:**
```bash
# CentOS/RHEL
sudo yum install gcc-c++ cmake ncurses-devel pkgconfig

# Fedora
sudo dnf install gcc-c++ cmake ncurses-devel pkgconfig
```

**Arch Linux:**
```bash
sudo pacman -S base-devel cmake ncurses pkgconf
```

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/system-monitor-tui.git
cd system-monitor-tui
```

2. Build the project:
```bash
mkdir build
cd build
cmake ..
make
```

3. Run the system monitor:
```bash
./system_monitor
```

### Alternative Build Options

**Release build (optimized):**
```bash
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make
```

**Debug build:**
```bash
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Debug ..
make
```

**Install system-wide:**
```bash
sudo make install
```

**Uninstall:**
```bash
sudo make uninstall
```

**Create package:**
```bash
make package
```

## Usage

Simply run the executable to start monitoring:
```bash
./system_monitor
```

The monitor will display real-time system information and update every second. Press `Ctrl+C` to exit.

## Project Structure

```
system-monitor-tui/
├── CMakeLists.txt       # CMake build configuration
├── cmake_uninstall.cmake.in  # Uninstall script template
├── README.md            # This file
├── include/             # Header files
│   ├── cpu.h           # CPU monitoring functions
│   ├── display.h       # Display/UI functions
│   ├── memory.h        # Memory monitoring functions
│   └── process.h       # Process monitoring functions
├── src/                # Source files
│   ├── main.cpp        # Main entry point
│   ├── cpu.cpp         # CPU usage implementation
│   ├── display.cpp     # ncurses display implementation
│   ├── memory.cpp      # Memory usage implementation
│   └── process.cpp     # Process monitoring implementation
└── build/              # Build directory (created during build)
```

## Technical Details

The system monitor reads from Linux's `/proc` filesystem to gather system information:

- **CPU Usage**: Calculated from `/proc/stat` using idle time differences
- **Memory Info**: Retrieved from `/proc/meminfo` (MemTotal and MemAvailable)
- **Process Data**: Collected from `/proc/[pid]/` directories including:
  - `/proc/[pid]/comm` for process names
  - `/proc/[pid]/stat` for CPU times
  - `/proc/[pid]/status` for memory usage (VmRSS)

## Building

The project uses CMake for building with the following options:

**Build targets:**
- `make` or `make all`: Build the system monitor
- `make clean`: Remove build files
- `make install`: Install system-wide (requires sudo)
- `make uninstall`: Remove installed files (requires sudo)
- `make package`: Create distribution package

**CMake options:**
- `-DCMAKE_BUILD_TYPE=Release`: Optimized build (default)
- `-DCMAKE_BUILD_TYPE=Debug`: Debug build with symbols
- `-DCMAKE_INSTALL_PREFIX=/usr/local`: Set installation prefix

**Build configuration:**
- C++17 standard required
- Compiler flags: `-Wall -Wextra` for warnings
- Debug: `-g -O0` for debugging
- Release: `-O3 -DNDEBUG` for optimization

## Troubleshooting

**Permission Issues:**
The monitor reads from `/proc` which should be accessible to all users. If you encounter permission issues, ensure your user has read access to `/proc`.

**Missing ncurses:**
If you get linking errors, ensure ncurses development libraries are installed:
```bash
# Ubuntu/Debian
sudo apt-get install libncurses5-dev pkg-config

# CentOS/RHEL
sudo yum install ncurses-devel pkgconfig
```

**CMake errors:**
Ensure you have CMake 3.16 or higher:
```bash
cmake --version  # Should be 3.16 or higher
```

**Build Errors:**
Ensure you have a C++17 compatible compiler:
```bash
g++ --version  # Should be 7.0 or higher
```

If you encounter build issues, try cleaning the build directory:
```bash
rm -rf build
mkdir build
cd build
cmake ..
make
```

## Related Projects

- **Qt6 GUI Version**: A modern GUI version of this system monitor built with Qt6 is available in a separate repository
- Both versions share similar core functionality but with different user interfaces
