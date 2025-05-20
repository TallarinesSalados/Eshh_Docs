# Runners System

## Overview

The Runners system in ESSH handles the execution of tasks and commands across local and remote hosts. It manages parallel execution, output handling, and error management.

## Key Components

### Task Runner
```go
func runTask(config string, task *Task, args []string, L *lua.LState) error
```

The Task Runner is responsible for:
- Task preparation and validation
- Argument handling
- Remote execution coordination
- Output management
- Error handling

## Execution Types

### 1. Local Execution
- Command execution on the local system
- Environment variable handling
- Working directory management
- Output capture and formatting

### 2. Remote Execution
- SSH-based command execution
- Parallel execution across hosts
- Connection management
- Output aggregation

### 3. Mixed Execution
- Combined local and remote tasks
- Dependency coordination
- State management
- Error propagation

## Key Features

### 1. Task Preparation
- Environment setup
- Argument processing
- Dependency validation
- Resource allocation

### 2. Execution Control
- Parallel execution
- Sequential execution
- Timeout management
- Process management

### 3. Output Handling
- Stream management
- Color output
- Error formatting
- Log management

### 4. Error Management
- Error detection
- Error propagation
- Recovery handling
- Status reporting

## Usage Examples

### 1. Basic Task Execution
```lua
-- Simple task that runs a single command
-- This is useful for basic operations or testing
task "simple" {
    -- Direct command string execution
    -- The command will run in the default shell
    script = "echo 'Hello, World!'"
}
```

### 2. Parallel Remote Execution
```lua
-- Task that runs simultaneously on multiple hosts
task "parallel" {
    -- Target all hosts that match the pattern 'web-*'
    targets = ["web-*"],
    
    -- Enable parallel execution across all target hosts
    -- This significantly speeds up execution for independent operations
    parallel = true,
    
    -- Function-style script allows for more complex logic
    script = function(task)
        -- Run the uptime command on each target host
        -- Results will be collected asynchronously
        task:run("uptime")
    end
}
```

### 3. Mixed Execution
```lua
-- Task that combines local and remote operations
task "deploy" {
    -- Preparation phase runs locally
    prepare = function(task)
        -- Build the application locally before deployment
        local build = task:run("npm build")
        if build.exitStatus ~= 0 then
            error("Build failed: " .. build.stderr)
        end
    end,
    
    -- Main script runs on remote hosts
    script = function(task)
        -- Copy built files and restart the service
        task:run("rsync -avz ./dist/ /var/www/app/")
        task:run("systemctl restart app")
    end
}
```

## Best Practices

### 1. Task Design
- Keep tasks focused
- Handle errors properly
- Use appropriate timeouts
- Document dependencies

### 2. Performance
- Use parallel execution when appropriate
- Manage resource usage
- Monitor execution time
- Cache when possible

### 3. Error Handling
- Validate inputs
- Check exit codes
- Handle timeouts
- Provide error context

### 4. Output Management
- Use appropriate verbosity
- Format output clearly
- Preserve error messages
- Handle large outputs

## Execution Flow

1. **Task Initialization**
   - Load task configuration
   - Process arguments
   - Setup environment
   - Validate requirements

2. **Preparation Phase**
   - Run prepare functions
   - Check dependencies
   - Allocate resources
   - Setup connections

3. **Execution Phase**
   - Run main task logic
   - Handle remote execution
   - Manage parallel tasks
   - Monitor progress

4. **Cleanup Phase**
   - Release resources
   - Close connections
   - Cleanup temporary files
   - Report status

## Advanced Features

### 1. Execution Hooks
```lua
task "with-hooks" {
    before = function(task)
        -- Pre-execution setup
    end,
    after = function(task)
        -- Post-execution cleanup
    end
}
```

### 2. Resource Management
```lua
task "managed" {
    init = function(task)
        -- Resource allocation
        return function()
            -- Resource cleanup
        end
    end
}
```

### 3. Output Control
```lua
task "formatted" {
    script = function(task)
        task:setOutput({
            format = "json",
            quiet = false,
            color = true
        })
    end
}
```

## Integration Points

The Runners system integrates with:
1. Task Management System
2. Host Management System
3. Driver System
4. Registry System
5. Lua Runtime
