# Debug

Functions that allow us to get **more control** over Luau functions.

---

## debug.getconstants

Returns the constants of the specified Lua function. Should error on C closure (functions) because they have no constants.

```luau
debug.getconstants(func: function): { [number]: number | string | nil }
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
debug.getconstant(func: function, index: number): number | string | nil
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

---

## debug.getupvalues

Returns the upvalues of the specified function. `nil` will be returned if there is none.

```luau
debug.getupvalues(func: function): { [number]: any }
```

### Parameters

- `func` - The Lua function the upvalues would be obtained from.

### Examples

```luau
local var1 = false
local var2 = "Hi"
local function DummyFunction()
    var1 = true
    var2..=", hello"
end

for upvalIndex, upvalValue in pairs(debug.getupvalues(DummyFunction)) do
    print(upvalIndex, upvalValue)
end

-- Output:
-- 1 false
-- 2 "Hi"
```

```luau
local var1 = false
local function DummyFunction()
    print(var1)
end

print(next(debug.getupvalues(DummyFunction)))

-- Output:
-- nil
```

```luau
print(debug.getupvalues(print)) -- Should error due to being a C closure (function)
```

## debug.getupvalue

Returns the upvalue at the specified index. An error should occur if the index is invalid.

```luau
debug.getupvalue(func: function, index: number): any
```

### Parameters

- `func` - The Lua function the upvalue would be obtained from.
- `index` - The position of the wanted upvalue.

### Examples

```luau
local up1 = function() print("Hello from up") end

local function DummyFunction()
    up1()
end

local upvalue = debug.getupvalue(DummyFunction, 1)
upvalue()

-- Output:
-- "Hello from up"
```

```luau
local function DummyFunction() end

debug.getupvalue(DummyFunction, 0) -- Should error on this line
```

```luau
debug.getupvalue(print, 1) -- Should error due to invalid index and C closure passage
```
