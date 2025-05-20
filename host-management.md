# Host Management

## Overview

The Host management system in ESSH provides a flexible way to define and manage SSH hosts. It supports advanced features like tagging, hierarchical definitions, and connection hooks.

## Host Structure

```go
type Host struct {
    Name                 string
    Description          string
    Props                map[string]string
    HooksBeforeConnect   []interface{}
    HooksAfterConnect    []interface{}
    HooksAfterDisconnect []interface{}
    Hidden               bool
    Tags                 []string
    SSHConfig            map[string]string
    Registry             *Registry
    Group                *Group
    LValues             map[string]lua.LValue
    Parent              *Host
    Child               *Host
}
```

## Key Features

### 1. Properties
- Custom properties via `Props` map
- SSH configuration via `SSHConfig` map
- Description for documentation
- Hidden flag for internal hosts

### 2. Tags
- Multiple tags per host
- Tag-based filtering
- Group organization

### 3. Connection Hooks
- Before Connect hooks
- After Connect hooks
- After Disconnect hooks

### 4. Hierarchical Structure
- Parent-child relationships
- Property inheritance
- Configuration overrides

### 5. Lua Integration
- Custom Lua values storage
- Runtime configuration
- Dynamic property evaluation

## Usage Examples

1. Basic Host Definition:
```lua
host "web-server" {
    Description = "Main web server",
    Hostname = "192.168.1.100",
    User = "admin",
    Tags = {"production", "web"}
}
```

2. Using Hooks:
```lua
host "database" {
    Description = "Database server",
    Hostname = "db.example.com",
    BeforeConnect = function(host)
        -- Custom logic before connection
    end
}
```

3. Hierarchical Hosts:
```lua
host "base-server" {
    User = "admin",
    Port = "22"
}

host "web-1" {
    Parent = "base-server",
    Hostname = "web1.example.com"
}
```

## Best Practices

1. Use meaningful names for hosts
2. Always include descriptions
3. Use tags for organization
4. Implement necessary hooks
5. Use inheritance for common configurations
6. Keep sensitive data in separate files
