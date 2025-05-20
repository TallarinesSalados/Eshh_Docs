# Groups System

## Overview

The Groups system in ESSH provides a way to organize and manage collections of hosts, tasks, and drivers. It supports hierarchical organization and shared properties across components.

## Group Structure

```go
type Group struct {
    Type    GroupType
    Hosts   map[string]*Host
    Tasks   map[string]*Task
    Drivers map[string]*Driver
    LValues map[string]lua.LValue
}
```

## Group Types

Groups can be of different types:
- Host Groups (`GroupTypeHosts`)
- Task Groups (`GroupTypeTasks`)
- Driver Groups (`GroupTypeDrivers`)

## Key Features

### 1. Component Organization
- Logical grouping of related components
- Namespace management
- Hierarchical organization
- Component isolation

### 2. Property Sharing
- Shared configuration
- Common properties
- Group-level defaults
- Property inheritance

### 3. Management Functions
- Component registration
- Group-level operations
- Bulk configuration
- Group-wide updates

### 4. Lua Integration
- Script-based group management
- Dynamic group configuration
- Custom group properties
- Group-level scripting

## Usage Examples

1. Creating a Host Group:
```lua
group "web-servers" {
    hosts = {
        "web-1",
        "web-2",
        "web-3"
    },
    Props = {
        environment = "production",
        datacenter = "us-east"
    }
}
```

2. Task Group Definition:
```lua
group "deployment" {
    tasks = {
        "build",
        "test",
        "deploy"
    },
    Props = {
        notify = true,
        timeout = 300
    }
}
```

3. Driver Group:
```lua
group "custom-ssh" {
    drivers = {
        "ssh-with-proxy",
        "ssh-with-tunnel"
    }
}
```

## Best Practices

1. **Logical Grouping**
   - Group related components together
   - Use meaningful group names
   - Maintain clear group hierarchies
   - Document group purposes

2. **Property Management**
   - Use group properties efficiently
   - Avoid property conflicts
   - Document shared properties
   - Use inheritance carefully

3. **Organization**
   - Keep groups focused
   - Avoid deep nesting
   - Use clear naming conventions
   - Maintain group documentation

4. **Integration**
   - Coordinate with other systems
   - Handle group events properly
   - Manage group lifecycles
   - Monitor group health

## Group Operations

### Registration
```lua
-- Register components with a group
group:RegisterHost(host)
group:RegisterTask(task)
group:RegisterDriver(driver)
```

### Property Management
```lua
-- Set group properties
group.Props = {
    environment = "production",
    location = "datacenter-1"
}
```

### Bulk Operations
```lua
-- Perform operations on all group members
for _, host in pairs(group.Hosts) do
    -- Perform operation
end
```

## Integration Points

The Group system integrates with:
1. Host Management System
2. Task Management System
3. Driver System
4. Registry System
5. Lua Runtime
