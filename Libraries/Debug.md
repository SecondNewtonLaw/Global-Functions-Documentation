# Debug

Functions that allow us to get **more control** over Luau functions.

---

## debug.getconstants

Returns the constants of the specified Lua function. Should error on C closure (functions) because they have no constants.

```luau
function debug.getconstants(func: function): { [number]: number | string | boolean | nil }
```

### Parameter

- `func` - The Lua function the constants would be obtained from.

### Example

```luau
local function DummyFunction()
    local dummyString = "foo bar"
    string.split(dummyString, " ")
end

local Constants = debug.getconstants(DummyFunction)
for ConstantIndex, Constant in Constants do
    print(`[{ConstantIndex}]: {Constant}`)
end

-- Output:
-- [1]: "string"
-- [2]: "split"
-- [4]: "foo bar"
-- [5]: " "
-- Optimization Level: 1, Debug Level: 1
```

```luau
print(debug.getconstants(print)) -- Should error due to being a C closure (function)
```

---

## debug.getconstant

Returns the constant at the specified index. If there is no constant at the specified index, `nil` will be returned instead.

```luau
function debug.getconstant(func: (...any) -> (...any), index: number): number | string | boolean | nil
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

local Result = debug.getconstant(DummyFunction, 2)
print(Result) -- Output: string

-- Optimization Level: 1, Debug Level: 1
```

```luau
local function DummyFunction()
    local DummyString = "foo bar"
    string.split(DummyString, " ")
end

local Result = debug.getconstant(DummyFunction, 3)
print(Result) -- Output: nil

-- Optimization Level: 1, Debug Level: 1
```

```luau
print(debug.getconstant(print)) -- Should error due to being a C closure (function)
```

---

## debug.setconstant

Sets the wanted constant at the specified index. An error will be returned if the index is invalid.

```luau
function debug.setconstant(func: (...any) -> (...any), index: number, value: number | string | boolean): ()
```

### Parameters

- `func` - The Lua function whose constant would be set
- `index` - The position of the constant
- `value` - New constant replacing the old one

### Example

```luau
local function DummyFunction()
    print(game.Name)
end

debug.setconstant(DummyFunction, 4, "Players")

DummyFunction() -- Output: Players
-- Optimization Level: 1, Debug Level: 1
```

---

## debug.getupvalues

Returns the upvalues of the specified function. `nil` will be returned if there is none.

```luau
function debug.getupvalues(func: (...any) -> (...any)): { [number]: any }
```

### Parameter

- `func` - The Lua function the upvalues would be obtained from.

### Examples

```luau
local Var1 = false
local Var2 = "Hi"
local function DummyFunction()
    Var1 = true
    Var2..=", hello"
end

for UpvalIndex, UpvalValue in pairs(debug.getupvalues(DummyFunction)) do
    print(UpvalIndex, UpvalValue)
end

-- Output:
-- 1 false
-- 2 Hi
```

```luau
local Var1 = false
local function DummyFunction()
    print(Var1)
end

print(next(debug.getupvalues(DummyFunction))) -- Output: nil
```

```luau
print(debug.getupvalues(print)) -- Should error due to `print` being a C closure
```

---

## debug.getupvalue

Returns the upvalue at the specified index. An error should occur if the index is invalid.

```luau
function debug.getupvalue(func: (...any) -> (...any), index: number): any
```

### Parameters

- `func` - The Lua function the upvalue would be obtained from.
- `index` - The position of the wanted upvalue.

### Examples

```luau
local Up1 = function() print("Hello from up") end

local function DummyFunction()
    Up1()
end

local Upvalue = debug.getupvalue(DummyFunction, 1)
Upvalue() -- Output: Hello from up
```

```luau
local function DummyFunction() end

debug.getupvalue(DummyFunction, 0) -- Should error on this line
```

```luau
debug.getupvalue(print, 1) -- Should error due to invalid index and C closure passage
```

---

## debug.setupvalue

Replaces the upvalue at the specified index. An error should occur if the index is invalid.

```luau
function debug.setupvalue(func: (...any) -> (...any), index: number, value: any): ()
```

### Parameters

- `func` - The Lua function whose upvalue would be set.
- `index` - The position of the wanted upvalue.
- `value` - New upvalue replacing the old one

### Example

```luau
local Up = 90

local function DummyFunction()
    Up += 1
    print(Up)
end

DummyFunction() -- Output: 91
debug.setupvalue(DummyFunction, 1, 99)
DummyFunction() -- Output: 100
```

---

## debug.getstack

Returns all used values in the provided stack level

```luau
function debug.getstack(level: number, index: number?): any | { [number]: any  }
```

### Parameters

- `level` - The call stack
- `index?` - The position of the values inside the call stack

### Examples

```luau
-- level 1 (stack)
local Count = 0
local function RecursiveFunction() -- Entering a level deeper, meaning we can call using level 2 in this function
    Count += 1
    if Count > 6 then return end -- We'll stop at 6 to not retrieve useless information for our example
    local a = 29
    local b = true
    local f = "Example"
    a += 1
    b = false
    f..="s"
    print(debug.getstack(1, Count))
    RecursiveFunction()
end

RecursiveFunction()
-- Output:
-- 30
-- false
-- Examples
-- function: 0x...  <-- print function
-- function: 0x...  <-- getstack function
-- 1  <-- argument provided to getstack function
```

```luau
local function DummyFunction() return "Hello" end
local Var = 5
Var += 1

return (function()
    print(debug.getstack(2, 1)()) -- Output: Hello
    print(debug.getstack(2, 2)) -- Output: 6
end)()
```

---

## debug.setstack

Sets a value in the stack at the specified index.

```luau
function debug.setstack(level: number, index: number, value: any): ()
```

### Parameters

- `level` - The call stack.
- `index` - The position of the values inside the call stack.
- `value` - The new value to set at the specified position.

### Examples

```luau
error(debug.setstack(1, 1, function() -- Replace error with our function
    return function()
        print("Replaced")
    end
end))() -- Output: Replaced
```

```luau
local OuterValue = 10

local function InnerFunction()
    OuterValue += 9
    debug.setstack(2, 1, 100)
end 
InnerFunction()

print(OuterValue) -- Output: 100
```

---

## debug.getprotos

Returns all the functions defined in the provided function

```luau
function debug.getprotos(func: (...any) -> (...any)): { [number]: (...any) -> (...any) }
```

### Parameter

- `func` - The function to obtain the protos from

### Example

```luau
local function DummyFunction0()
    local function DummyFunction1() end
    local function DummyFunction2() end
end

for IndexProto, ValueProto in pairs(debug.getprotos(DummyFunction0)) do
    print(IndexProto, debug.info(ValueProto, "n"))
end

-- Output:
-- DummyFunction1
-- DummyFunction2
```
