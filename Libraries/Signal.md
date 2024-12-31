# Signal

Functions that allow interaction with RBXSignals.

---

## getconnections

Returns the connections of a specific signal.

```luau
function getconnections(signal: RBXScriptSignal): {Connection}
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
| `Disconnect(): ()` | Disconnects the connection. |
| `Disable(): ()` | Prevents the connection from firing. |
| `Enable(): ()` | Allows the connection to fire if it was previously disabled. |

### Parameters

- `signal` The signal whose connections you want to retrieve.

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
function firesignal(signal: RBXScriptSignal)
```

### Parameters

- `signal` The signal to fire

### Example

```luau
local function func()
    return "those who know"
end

local firebind = Instance.new("BindableEvent")
local result

firebind.Event:Connect(function()
    result = func()
end)

print(firesignal(firebind.Event) or result) -- prints "those who know"
```
