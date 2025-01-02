# Signals

Functions that allow interaction with RBXScriptSignals and RBXScriptConnections.

---

## getconnections

Returns the connections of a specific signal.

```luau
function getconnections(signal: RBXScriptSignal<...any>): {Connection}
```

### Connection

| Field | Type | Description |
| ----- | ---- | ----------- |
| `Enabled` | boolean | Whether the connection can receive events. |
| `ForeignState` | boolean | Whether the function was connected by a foreign Luau state (i.e. CoreScripts). |
| `LuaConnection` | boolean | Whether the connection was created in Luau code. |
| `Function` | function? | The function bound to this connection. Nil when `ForeignState` is true. |
| `Thread` | thread? | The thread that created the connection. Nil when `ForeignState` is true. |

| Method | Description |
| ----- | ----------- |
| `Fire(...: any): ()` | Fires this connection with the provided arguments. |
| `Defer(...: any): ()` | [Defers](https://devforum.roblox.com/t/beta-deferred-lua-event-handling/1240569) an event to connection with the provided arguments. |
| `Disconnect(): ()` | Disconnects the connection from the function. |
| `Disable(): ()` | Prevents the connection from firing. |
| `Enable(): ()` | Enables the connection, allowing it to fire. |

### Parameters

- `signal` - The signal whose connections you want to retrieve.

### Example

```luau
local connections = getconnections(workspace.Part.Touched)

for _, connection in ipairs(connections) do
    connection:Disable()
end
```
---

## firesignal

Fires a signal's Lua connections.

```luau
function firesignal(signal: RBXScriptSignal<...any>, ...: any?)
```

### Parameters

- `signal` - The signal to fire
- `...?` - The wanted arguments to pass into the fired connections 

### Example

```luau
local part = Instance.new("Part")
part.ChildAdded:Connect(function(arg1)
    print(typeof(arg1))
end)

firesignal(part.ChildAdded) -- Output: nil
firesignal(part.ChildAdded, workspace) -- Output: Instance
```

---

## hooksignal

> [!NOTE]
> hooksignal cannot intercept `C connections` or `CoreScript Lua connections`

Intercepts signal calls, invoking a callback for each Lua connection with an info table and arguments. The original connection runs if the callback returns true.

```luau
function hooksignal(signal: RBXScriptSignal<...any>, callback: function)
```

### Parameters

- `signal` - The signal to be hooked
- `callback` - The new callback triggered by hooksignal, returning true to allow or false/nil to block the original signal connection

### Example

```luau
local part = Instance.new("Part")
part.Changed:Connect(function(prop)
    print(prop .. " changed?")
end)
hooksignal(part.Changed, function(info, prop)
    print(info.Connection) -- the connection object.
    print(info.Function) -- the original function. Not available for waiting connections.
    print(info.Index) -- the position of this connection in part.Changed at the time this callback is executed. Not available for waiting connections.
    print(prop)
    return true, "Hooked"
end)
part.Name = "NewName"
```

---

## restoresignal

restores a signal's original behavior after it has been modified.

```luau
function restoresignal(signal: RBXScriptSignal<...any>)
```

### Parameters

- `signal` - The signal to be restored

### Example

```luau
local hook = hooksignal(workspace.Part.Touched, function(info, ...)
    print("Touched signal from part")
    return true
end)

part.Touched:Fire()

restoresignal(part.Touched) -- the signals original behavior is now restored via restoresignal

part.Touched:Fire() -- back to its original behavior
```
