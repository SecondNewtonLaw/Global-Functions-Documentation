# Debug
---

## debug.getconstants
> [!Warning]
> C Functions shouldn't be allowed to be passed through this function for security purposes. 

Returns the constants of the specified function.
```luau
debug.getconstants(func: (...any) -> (...any)): { [number]: any }
```

### Parameter
- `func` - Wanted function the constants would be obtained from.
### Example
```luau
local function DummyFunction()
    local dummyString = "foo bar"
    string.split(dummyString, " ")
end

local constants = debug.getconstants(DummyFunction)
for constantIndex, constant in constants do
    print(`[{constantIndex}]: {constant}`)
end

-- Optimization Level: 1, Debug Level: 1
-- Output:
-- [1]: "string"
-- [2]: "split"
-- [4]: "foo bar"
-- [5]: " "
```
