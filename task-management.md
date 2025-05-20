# Task Management

## Overview

The Task management system in ESSH provides a powerful way to automate SSH-based operations. Tasks can be defined with dependencies, run in parallel, and integrate with the host management system.

## Task Structure

Tasks in ESSH are defined with properties that control their execution and behavior. They can be configured to run on specific hosts, with specific drivers, and with various execution options.

## Key Features

### 1. Task Definition
- Name and description
- Command specification
- Host targeting
- Driver selection
- Dependency management

### 2. Execution Options
- Sequential execution
- Parallel execution
- Error handling
- Output capture
- Environment variables

### 3. Integration
- Host system integration
- Driver system integration
- Registry integration
- Lua scripting support

## Task Types

1. **Command Tasks**
   - Direct command execution
   - Script execution
   - Remote file operations

2. **Composite Tasks**
   - Task dependencies
   - Task groups
   - Sequential execution
   - Parallel execution

## Usage Examples

1. Basic Task:
```lua
-- Define a basic system update task that runs on multiple hosts
task "update" {
    description = "Update system packages",
    -- Use glob pattern to match multiple hosts
    -- Here it will match 'web-1', 'web-prod', etc.
    targets = ["web-*"],  
    
    -- Multi-line script using heredoc syntax
    -- Commands will run sequentially
    script = [[
        sudo apt update
        sudo apt upgrade -y
    ]]
}
```

2. Task with Dependencies:
```lua
-- Define a deployment task that depends on other tasks
task "deploy" {
    description = "Deploy application",
    -- These tasks must complete successfully before
    -- this task will run, in the order specified
    depends = ["build", "backup"],
    
    -- Only run on hosts tagged as 'production'
    targets = ["production"],
    
    -- Run sequentially on each target host
    -- (false means don't run in parallel)
    parallel = false
}
```

3. Parallel Task:
```lua
-- Define a task that runs simultaneously on all hosts
task "check-status" {
    description = "Check service status across all servers",
    -- Special target 'all' means run on every defined host
    targets = ["all"],
    
    -- parallel = true means run on all targets simultaneously
    -- This is good for independent operations like status checks
    parallel = true,
    
    -- Single command execution
    script = "systemctl status nginx"
}
```

## Best Practices

1. Use descriptive task names
2. Document task purpose and requirements
3. Properly handle task dependencies
4. Use parallel execution when appropriate
5. Implement proper error handling
6. Use task groups for organization
7. Test tasks in development environment first

## Task Execution Flow

1. Task validation
2. Dependency resolution
3. Host selection
4. Driver preparation
5. Command generation
6. Execution
7. Output handling
8. Status reporting

## Error Handling

Tasks can be configured to handle errors in different ways:
- Stop on first error
- Continue despite errors
- Custom error handlers
- Error reporting and logging
