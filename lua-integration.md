# Lua Integration

## Overview

ESSH provides deep integration with Lua, allowing for powerful scripting capabilities throughout the system. The Lua integration enables dynamic configuration, custom logic, and extensible functionality.

## Components

### 1. Lua State Initialization

```go
func InitLuaState(L *lua.LState) {
    // Register custom types
    registerHostClass(L)
    registerTaskClass(L)
    registerDriverClass(L)
    registerHostQueryClass(L)
    registerRegistryClass(L)
    registerGroupClass(L)
    
    // Global functions
    L.SetGlobal("host", L.NewFunction(esshHost))
    L.SetGlobal("task", L.NewFunction(esshTask))
    L.SetGlobal("driver", L.NewFunction(esshDriver))
    L.SetGlobal("group", L.NewFunction(esshGroup))
}
```

### 2. Built-in Modules

ESSH includes several pre-loaded Lua modules:
- `json` - JSON encoding/decoding
- `fs` - File system operations
- `yaml` - YAML processing
- `template` - Template rendering
- `http` - HTTP client functionality
- `env` - Environment variable access
- `aws` - AWS integration
- `mdns` - mDNS service discovery
- `watch` - File watching
- `crypto` - Cryptographic operations
- `time` - Time manipulation
- `base64` - Base64 encoding/decoding
- `db` - Database operations
- `storage` - Persistent storage

## Key Features

### 1. Custom Types
- Host type for SSH host management
- Task type for automation
- Driver type for connection handling
- Query types for filtering
- Registry type for state management
- Group type for organization

### 2. Global Functions
- `host()` - Host definition
- `task()` - Task creation
- `driver()` - Driver configuration
- `group()` - Group management

### 3. Extension Points
- Custom module loading
- Function overriding
- Event hooks
- Custom commands

## Usage Examples

1. Host Definition:
```lua
-- Define a web server with custom initialization
host "web-server" {
    -- Basic SSH connection properties
    Hostname = "example.com",
    User = "admin",
    
    -- init function runs when the host is first loaded
    -- Use it for dynamic configuration or validation
    init = function(host)
        -- Example: Set additional properties based on environment
        if os.getenv("ENVIRONMENT") == "production" then
            host.Port = "22"
            host.Tags = {"production", "web"}
        else
            host.Port = "2222"
            host.Tags = {"staging", "web"}
        end
    end
}
```

2. Task with Custom Logic:
```lua
-- Define a deployment task with preparation and error handling
task "deploy" {
    -- prepare runs before the main script
    -- Use it for environment setup and validation
    prepare = function(task)
        -- Check if git is installed
        local git_check = task:run("which git")
        if git_check.exitStatus ~= 0 then
            error("Git is not installed on the system")
        end
    end,
    
    -- Main script with error handling and deployment logic
    script = function(task)
        -- Execute git pull and check result
        local result = task:run("git pull")
        if result.exitStatus == 0 then
            -- Successful pull, proceed with deployment
            task:run("npm install")
            task:run("npm run build")
            task:run("systemctl restart app")
        else
            -- Handle git pull failure
            error("Failed to pull latest code: " .. result.stderr)
        end
    end
}
```

3. Using Built-in Modules:
```lua
-- Import required modules
local json = require "json"   -- JSON encoding/decoding
local fs = require "fs"       -- File system operations
local yaml = require "yaml"   -- YAML processing

-- Define a task to convert YAML config to JSON
task "config" {
    script = function(task)
        -- Read YAML configuration file
        local data = yaml.read("config.yml")
        if not data then
            error("Failed to read config.yml")
        end
        
        -- Convert to JSON string with proper formatting
        local json_str = json.encode(data, {
            indent = true,  -- Pretty print
            sort_keys = true  -- Consistent output
        })
        
        -- Write to new JSON file
        local ok = fs.write("config.json", json_str)
        if not ok then
            error("Failed to write config.json")
        end
    end
}
```

## Best Practices

1. **Script Organization**
   - Keep scripts modular
   - Use clear naming
   - Comment complex logic
   - Handle errors properly

2. **Performance**
   - Minimize script complexity
   - Cache computed values
   - Use efficient algorithms
   - Profile when necessary

3. **Security**
   - Validate input data
   - Handle sensitive information carefully
   - Use secure modules
   - Implement proper access controls

4. **Maintenance**
   - Document custom scripts
   - Version control scripts
   - Test script functionality
   - Keep dependencies updated

## Debugging

### 1. Error Handling
```lua
local ok, err = pcall(function()
    -- Your code here
end)
if not ok then
    print("Error:", err)
end
```

### 2. Logging
```lua
local log = require "log"
log.info("Starting task...")
log.error("Failed to connect:", err)
```

### 3. Inspection
```lua
local inspect = require "inspect"
print(inspect(someTable))
```

## Integration Examples

### 1. AWS Integration
```lua
local aws = require "aws"
task "ec2-list" {
    script = function()
        local instances = aws.ec2:describeInstances()
        -- Process instances
    end
}
```

### 2. Database Operations
```lua
local db = require "db"
task "db-backup" {
    script = function()
        local conn = db.connect("mysql", connectionString)
        -- Perform backup
    end
}
```

### 3. File Watching
```lua
local watch = require "watch"
watch.add("/path/to/config", function(event)
    -- Handle file changes
end)
