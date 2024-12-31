# Signal
Functions that allow interaction with RBXSignals.
---

## getconnections

returns the connections of a specific signal.

```luau
getconnections(signal: RBXScriptSignal): {Connection}
```

### Parameters

- `signal`: The signal whose connections you want to retrieve.

### Example

```luau
local connections = getconnections(workspace.Part.Touched)

for _, connection in ipairs(connections) do
    connection:Disable()
end
```
---
