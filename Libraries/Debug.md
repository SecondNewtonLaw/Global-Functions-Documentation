# Debug
Functions that allow us to get **more control** over Luau functions.

---

## debug.getconstants

Returns the constants of the specified Lua function. Should error on C closure (functions) because they have no constants.
```luau
debug.getconstants(func: (...any) -> (...any)): { [number]: number | string | nil }
```

### Parameters
- `func` - The Lua function the constants would be obtained from.

### Examples
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

```luau
print(debug.getconstants(print)) -- Should error due to being a C closure (function)
```
---

## debug.getconstant

Returns the constant at the specified index. If there is no constant at the specified index, `nil` will be returned instead.

```luau
debug.getconstant(func: (...any) -> (...any), index: number): number | string | nil
```

### Parameters
- `func` - The Lua function the constant would be obtained from.
- `index` - Position of the wanted constant.

### Examples
```luau
local function DummyFunction()
    local dummyString = "foo bar"
    string.split(dummyString, " ")
end

local result = debug.getconstant(DummyFunction, 2)
print(result)

-- Optimization Level: 1, Debug Level: 1
-- Output:
-- string
```

```luau
local function DummyFunction()
    local dummyString = "foo bar"
    string.split(dummyString, " ")
end

local result = debug.getconstant(DummyFunction, 3)
print(result)

-- Optimization Level: 1, Debug Level: 1
-- Output:
-- nil
```


```luau
print(debug.getconstant(print)) -- Should error due to being a C closure (function)
```
