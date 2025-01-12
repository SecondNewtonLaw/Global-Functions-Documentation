# Signals

Functions that allow interaction with RBXScriptSignals and RBXScriptConnections.

---

## getconnections

> [!NOTE]
> Passing a C-Signal into **getconnections** should return `Function` and `Thread` as nil, due to them not being accessible.

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
| `Function` | function? | The function bound to this connection. Nil when `ForeignState` is true, or `LuaConnection` is false. |
| `Thread` | thread? | The thread that created the connection. Nil when `ForeignState` is true, or `LuaConnection` is false. |

| Method | Description |
| ----- | ----------- |
| `Fire(...: any): ()` | Fires this connection with the provided arguments. |
| `Defer(...: any): ()` | [Defers](https://devforum.roblox.com/t/beta-deferred-lua-event-handling/1240569) an event to connection with the provided arguments. |
| `Disconnect(): ()` | Disconnects the connection from the function. |
| `Disable(): ()` | Prevents the connection from firing. |
| `Enable(): ()` | Enables the connection, allowing it to fire. |

### Parameters

- `signal` - The signal whose connections you want to retrieve.

### Examples

```luau
local DummyFolder = Instance.new("Folder")
DummyFolder.ChildAdded:Connect(function() return "Triggered" end)
local connection = getconnections(DummyFolder.ChildAdded)[1] -- First connection in the returned table

print(`{connection.Function()}, {type(connection.Thread)}`) -- Output: Triggered, thread
```

```luau
local CConnection = getconnections(game.Players.LocalPlayer.Idled)[1]
print(`{CConnection.Function}, {CConnection.Thread}`) -- Output: nil, nil
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
> hooksignal cannot intercept `C-connections` or `ForeignState` connections

Intercepts signal calls, invoking a callback for each Lua connection with an info table and arguments. The original connection runs if the callback returns true.

```luau
function hooksignal(signal: RBXScriptSignal<...any>, callback: function)
```

### Parameters

- `signal` - The signal to be hooked
- `callback` - The new callback triggered by hooksignal, returning true to allow or false/nil to block the original signal connection

### Examples

```luau
TODO
```

---

## restoresignal

restores a signal's original behavior after it has been hooked.

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

restoresignal(part.Touched) -- the signals original behavior is now restored via restoresignal
```
