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
- ncurses development library

### Installing Dependencies

**Ubuntu/Debian:**
```bash
sudo apt-get update
sudo apt-get install build-essential libncurses5-dev libncurses5
```

**CentOS/RHEL/Fedora:**
```bash
# CentOS/RHEL
sudo yum install gcc-c++ ncurses-devel

# Fedora
sudo dnf install gcc-c++ ncurses-devel
```

**Arch Linux:**
```bash
sudo pacman -S base-devel ncurses
```

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/system-monitor-tui.git
cd system-monitor-tui
```

2. Build the project:
```bash
make
```

3. Run the system monitor:
```bash
./system_monitor
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
├── Makefile              # Build configuration
├── README.md            # This file
├── include/             # Header files
│   ├── cpu.h           # CPU monitoring functions
│   ├── display.h       # Display/UI functions
│   ├── memory.h        # Memory monitoring functions
│   └── process.h       # Process monitoring functions
└── src/                # Source files
    ├── main.cpp        # Main entry point
    ├── cpu.cpp         # CPU usage implementation
    ├── display.cpp     # ncurses display implementation
    ├── memory.cpp      # Memory usage implementation
    └── process.cpp     # Process monitoring implementation
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

The project uses a simple Makefile with the following targets:

- `make` or `make all`: Build the system monitor
- `make clean`: Remove object files and executable

Compiler flags:
- `-std=c++17`: Use C++17 standard
- `-Wall -Wextra`: Enable comprehensive warnings
- `-Iinclude`: Include header directory
- `-lncurses`: Link ncurses library

## Troubleshooting

**Permission Issues:**
The monitor reads from `/proc` which should be accessible to all users. If you encounter permission issues, ensure your user has read access to `/proc`.

**Missing ncurses:**
If you get linking errors, ensure ncurses development libraries are installed:
```bash
# Ubuntu/Debian
sudo apt-get install libncurses5-dev

# CentOS/RHEL
sudo yum install ncurses-devel
```

**Build Errors:**
Ensure you have a C++17 compatible compiler:
```bash
g++ --version  # Should be 7.0 or higher
```

## Related Projects

- **Qt6 GUI Version**: A modern GUI version of this system monitor built with Qt6 is available in a separate repository
- Both versions share similar core functionality but with different user interfaces
