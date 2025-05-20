# ESSH - Enhanced SSH Tool

## Overview

ESSH is an enhanced SSH command-line tool built in Go that provides advanced SSH management capabilities. It extends the standard SSH functionality with features like host management, task automation, and driver customization.

## Key Components

The application is structured into several main components:

1. **Core (cmd/essh)**
   - Entry point of the application
   - Manages command-line interface
   - Initializes the ESSH environment

2. **Host Management (essh/host.go)**
   - Defines SSH host configurations
   - Manages host properties and tags
   - Handles SSH connection hooks
   - Supports hierarchical host definitions

3. **Task Management (essh/task.go)**
   - Defines automated tasks
   - Manages task execution
   - Handles task dependencies
   - Supports parallel execution

4. **Driver System (essh/driver.go)**
   - Provides extensible SSH connection handling
   - Manages connection properties
   - Supports custom connection engines
   - Handles template-based command generation

5. **Registry System (essh/registry.go)**
   - Manages global state
   - Handles component registration
   - Provides lookup capabilities

## Project Structure

```
├── cmd/essh/          # Main application entry point
├── essh/             # Core package containing main functionality
├── lib/              # Additional Lua libraries and resources
├── support/          # Support packages (colors, helpers, etc.)
├── test/             # Test files and scripts
└── website/          # Documentation website content
```

## Key Features

- Host management with tags and properties
- Task automation with dependencies
- Custom SSH drivers
- Lua scripting support
- Before/After connection hooks
- Parallel execution support
- SSH config generation
- Shell completion (Zsh/Bash)
- Color output support
