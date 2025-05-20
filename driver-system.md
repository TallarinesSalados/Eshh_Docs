# Driver System

## Overview

The Driver system in ESSH provides a flexible and extensible way to customize how SSH connections are handled. It allows for custom connection engines and template-based command generation.

## Driver Structure

```go
type Driver struct {
    Name     string
    Props    map[string]interface{}
    Engine   func(*Driver) (string, error)
    Registry *Registry
    Group    *Group
    LValues  map[string]lua.LValue
    Parent   *Driver
    Child    *Driver
}
```

## Key Features

### 1. Custom Engines
- Customizable connection handling
- Template-based command generation
- Support for different SSH implementations

### 2. Properties
- Flexible property storage
- Runtime configuration
- Template variable support

### 3. Lua Integration
- Custom Lua value storage
- Dynamic property evaluation
- Script-based configuration

### 4. Inheritance
- Parent-child relationships
- Property inheritance
- Configuration overrides

## Default Driver

ESSH includes a default driver that provides standard SSH functionality. The default driver can be extended or replaced with custom implementations.

### Default Driver Features
- Standard SSH command generation
- SSH config file support
- ProxyCommand support
- Environment variable handling

## Creating Custom Drivers

1. Basic Driver Definition:
```lua
-- Define a basic SSH driver that uses a custom SSH config file
driver "custom" {
    -- The engine function returns the final SSH command to be executed
    -- {ssh_config} and {host} are template variables that will be replaced
    -- at runtime with actual values
    engine = function(driver)
        return [[ssh -F {ssh_config} {host}]]
    end
}
```

2. Advanced Driver with Properties:
```lua
-- Define an advanced driver with custom properties and behavior
driver "advanced" {
    -- Define driver-specific properties that can be accessed
    -- within the engine function or other hooks
    Props = {
        timeout = 30,        -- Connection timeout in seconds
        compression = true   -- Enable SSH compression
    },
    -- Custom engine that can use the defined properties
    engine = function(driver)
        -- Access properties using driver.Props
        local cmd = "ssh"
        if driver.Props.compression then
            cmd = cmd .. " -C"  -- Add compression flag
        end
        cmd = cmd .. string.format(" -o ConnectTimeout=%d", driver.Props.timeout)
        return cmd .. " {host}"
    end
}
```

## Best Practices

1. Use meaningful driver names
2. Document driver properties
3. Handle errors appropriately
4. Use templating for command generation
5. Consider security implications
6. Test with different SSH configurations

## Driver Selection

Drivers can be selected:
1. Per host
2. Per task
3. Globally via configuration
4. Through command-line options
