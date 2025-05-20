# ESSH Core Module Documentation

## Overview

The `essh.go` file serves as the core module of the ESSH (Enhanced SSH) system. It provides the fundamental functionality for SSH command execution, configuration management, and feature coordination.

## System Configuration

### Global Variables

```go
// System configuration paths and locations
var (
    UserConfigFile               string  // User's primary configuration file
    UserOverrideConfigFile       string  // User's override configuration
    UserDataDir                  string  // User's data directory
    WorkingDirConfigFile         string  // Working directory configuration
    WorkingDirOverrideConfigFile string  // Working directory override configuration
    WorkingDataDir              string   // Working directory data storage
    WorkingDir                  string   // Current working directory
    Executable                  string   // Path to the executable
)
```

### Command Line Flags

The system supports numerous command-line flags categorized as follows:

1. **General Flags**
```go
var (
    versionFlag bool  // Display version information
    helpFlag    bool  // Show help message
    printFlag   bool  // Print configuration
    colorFlag   bool  // Enable colored output
    noColorFlag bool  // Disable colored output
    debugFlag   bool  // Enable debug mode
    hostsFlag   bool  // List available hosts
    quietFlag   bool  // Suppress output
    allFlag     bool  // Select all targets
)
```

2. **Feature Flags**
```go
var (
    tagsFlag    bool    // Show tags
    tasksFlag   bool    // List tasks
    evalFlag    bool    // Evaluate expressions
    menuFlag    bool    // Show interactive menu
    genFlag     bool    // Generate configuration
    globalFlag  bool    // Use global settings
)
```

3. **Shell Completion Flags**
```go
var (
    // Zsh completion flags
    zshCompletionModeFlag       bool
    zshCompletionHostsFlag      bool
    zshCompletionTagsFlag       bool
    zshCompletionTasksFlag      bool
    zshCompletionNamespacesFlag bool

    // Bash completion flags
    bashCompletionModeFlag       bool
    bashCompletionHostsFlag      bool
    bashCompletionTagsFlag       bool
    bashCompletionTasksFlag      bool
    bashCompletionNamespacesFlag bool
)
```

4. **Execution Flags**
```go
var (
    execFlag        bool     // Execute command
    fileFlag        bool     // Use file input
    prefixFlag      bool     // Add output prefix
    parallelFlag    bool     // Run in parallel
    privilegedFlag  bool     // Run with privileges
    ptyFlag         bool     // Use PTY
    SSHConfigFlag   bool     // Use SSH config
)
```

## Core Functions

### 1. Resource Initialization

```go
func initResources() {
    // Initialize system flags and resources
    // Set up default values and configurations
}
```

### 2. Command Execution Flow

The main execution flow follows these steps:

1. **Initialization**
   - Parse command line arguments
   - Load configuration files
   - Set up environment

2. **Configuration Processing**
   - Load user configuration
   - Apply overrides
   - Validate settings

3. **Command Execution**
   - Parse command
   - Select appropriate handler
   - Execute command
   - Handle output

4. **Cleanup**
   - Release resources
   - Close connections
   - Report status

## Integration Points

### 1. Lua Integration
The core module integrates with Lua through:
- Loading Lua scripts
- Executing Lua functions
- Managing Lua state
- Handling Lua errors

### 2. SSH Configuration
Manages SSH configuration through:
- SSH config file generation
- Connection parameters
- Authentication methods
- Proxy settings

### 3. Terminal Handling
Provides terminal management via:
- PTY allocation
- Output formatting
- Color support
- Interactive features

## Best Practices

1. **Configuration Management**
   - Use override files for local changes
   - Keep sensitive data in separate files
   - Use environment variables for dynamic values
   - Document configuration changes

2. **Command Execution**
   - Handle errors appropriately
   - Provide meaningful error messages
   - Use appropriate verbosity levels
   - Implement proper timeout handling

3. **Resource Management**
   - Clean up resources properly
   - Handle connection timeouts
   - Manage memory usage
   - Monitor system resources

4. **Security Considerations**
   - Validate user input
   - Handle credentials securely
   - Use proper permissions
   - Implement access controls

## Error Handling

The system uses the following error handling mechanisms:

1. **Exit Codes**
```go
const (
    ExitErr = 1  // Standard error exit code
)
```

2. **Error Types**
   - Configuration errors
   - Connection errors
   - Authentication errors
   - Execution errors

3. **Error Recovery**
   - Graceful degradation
   - Fallback options
   - Clean resource cleanup
   - Error reporting

## Example Usage

1. **Basic Command Execution**
```bash
essh hostname command
```

2. **Parallel Execution**
```bash
essh -p web-* uptime
```

3. **Using Configuration**
```bash
essh -f config.lua task-name
```

## Dependencies

The core module depends on several external packages:
```go
import (
    "github.com/fatih/color"           // Terminal color support
    "github.com/kardianos/osext"       // Executable path handling
    "github.com/yuin/gopher-lua"       // Lua integration
    "github.com/charmbracelet/bubbles" // Terminal UI components
)
```
