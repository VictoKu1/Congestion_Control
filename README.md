# TCP Congestion Control Algorithm Performance Measurement

## Overview

This project implements a network performance measurement tool that compares the efficiency of different TCP congestion control algorithms. It consists of a client-server architecture that measures file transfer times using different congestion control mechanisms, specifically comparing the default algorithm with the Reno algorithm.

## Features

- **Real-time Performance Measurement**: Measures file transfer times with millisecond precision
- **Congestion Control Algorithm Switching**: Dynamically switches between TCP congestion control algorithms
- **Multiple File Transfer Testing**: Performs multiple file transfers to ensure statistical significance
- **Cross-platform Compatibility**: Works on Unix-like systems (Linux, macOS)
- **Easy Build System**: Simple Makefile-based compilation and execution

## Architecture

### Components

1. **`measure.c`** - Server application that receives files and measures transfer times
2. **`sender.c`** - Client application that sends files and switches congestion control algorithms
3. **`Makefile`** - Build system for easy compilation and execution
4. **`create_large_file.py`** - Utility script to generate test files of specified sizes

### Network Configuration

- **Protocol**: TCP/IP
- **Port**: 5060
- **Address**: 127.0.0.1 (localhost)
- **Buffer Size**: 1000 bytes
- **Test Files**: Multiple iterations of file transfers for statistical accuracy

## Prerequisites

- GCC compiler
- Python 3 (for file generation utility)
- Unix-like operating system (Linux, macOS)
- Root privileges (may be required for congestion control algorithm modification)

## Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd Congestion_Control
   ```

2. Compile the applications:
   ```bash
   make
   ```

## Usage

### Basic Usage

1. **Start the server** (in one terminal):
   ```bash
   make measureRun
   # or
   ./measure
   ```

2. **Start the client** (in another terminal):
   ```bash
   make senderRun
   # or
   ./sender
   ```

### Advanced Usage

#### Generate Test Files

Create large test files for more realistic measurements:
```bash
python3 create_large_file.py
```

#### Clean Build
```bash
make clean
```

#### Debug with System Calls Tracing
```bash
make runs-strace  # Trace server system calls
make runc-strace  # Trace client system calls
```

## How It Works

### Measurement Process

1. **Server Initialization**: The server (`measure.c`) creates a TCP socket, binds to port 5060, and waits for client connections
2. **Client Connection**: The client (`sender.c`) establishes a connection to the server
3. **Algorithm Detection**: The system detects the current congestion control algorithm
4. **File Transfer Phase 1**: Client sends 5 files using the default congestion control algorithm
5. **Algorithm Switch**: Client switches to the Reno congestion control algorithm
6. **File Transfer Phase 2**: Client sends 5 more files using the Reno algorithm
7. **Performance Analysis**: Server measures and reports transfer times for both algorithms

### Congestion Control Algorithms Tested

- **Default Algorithm**: System's default TCP congestion control algorithm
- **Reno Algorithm**: Classic TCP Reno congestion control algorithm

## Output Interpretation

The application provides detailed output including:

- Connection status messages
- Current congestion control algorithm detection
- Individual file transfer times
- Average transfer times for each algorithm
- Algorithm switching confirmation

### Sample Output
```
Waiting for connection...
Connected with Server.
Current CC Algorithm: cubic
sending file 1
sending file 2
...
File 1 received in 0.023456 seconds.
File 2 received in 0.021234 seconds.
...
For CC Algorithm: cubic
Average File sending time: 0.022345 seconds

CC Algorithm Changed to: reno
...
For CC Algorithm: reno
Average File sending time: 0.025678 seconds
```

## Security Considerations

- **Local Network Only**: The application runs on localhost (127.0.0.1) for security
- **No Authentication**: Designed for local testing only
- **File Access**: Only reads predefined test files
- **Network Isolation**: No external network communication

## Performance Considerations

- **Buffer Size**: 1000-byte buffers for optimal performance
- **Multiple Iterations**: 5 file transfers per algorithm for statistical accuracy
- **Clock Precision**: Uses system clock for high-precision timing
- **Memory Management**: Proper socket cleanup and file handling

## Troubleshooting

### Common Issues

1. **Permission Denied**: Ensure you have sufficient privileges to modify socket options
2. **Port Already in Use**: Kill any existing processes using port 5060
3. **File Not Found**: Ensure `test.txt` exists in the project directory
4. **Compilation Errors**: Ensure GCC is installed and up to date

### Debug Commands

```bash
# Check if port is in use
netstat -tuln | grep 5060

# Kill processes on port 5060
sudo fuser -k 5060/tcp

# Check available congestion control algorithms
cat /proc/sys/net/ipv4/tcp_available_congestion_control
```

## Contributing

This is an academic project demonstrating TCP congestion control concepts. For educational purposes, the code is intentionally kept simple and well-commented.

## License

This project is created for educational purposes as part of a networking course curriculum.

## Author

Created as part of a networking course project focusing on TCP congestion control algorithm performance analysis.

---

**Note**: This tool is designed for educational and research purposes. Always ensure you have proper authorization before testing network performance on production systems. 