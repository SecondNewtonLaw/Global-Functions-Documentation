Functions that allow **inspection/modification/creation** of Luau closures

---

## newcclosure

> [!WARNING]
> Many executors are implementing this function using `coroutine` functions in Lua; these implementations won't pass sUNC checks.
>
> The wrapped function should be yieldable (meaning that the function should be able to call `task.wait`, for example)

This function takes in a function and wraps it into a C closure.

When the returned function is called, the original Lua closure is called, and arguments are passed to the original closure, and then the original closure returned arguments are passed to the caller of the C closure.

```luau
function newcclosure(function_to_wrap: (...any) -> (...any), debug_name: string?): (...any) -> (...any)
```

### Parameters

- `function_to_wrap` - A function to be wrapped.
- `debug_name?` - (Optional) A debug name for the wrapped function. If not provided, the name will be blank.

### Examples

```luau
local OriginalFunction = function(...)
    return ...
end

print(iscclosure(OriginalFunction)) -- Output: false

local WrappedFunction = newcclosure(OriginalFunction, "sUNC")

print(iscclosure(WrappedFunction)) -- Output: true

local FunctionResults = WrappedFunction("Hello")
print(FunctionResults) -- Output: Hello

print(debug.info(WrappedFunction, "n")) -- Output: sUNC
```

```luau
local FunctionThatYields = newcclosure(function()
    print("Before")
    task.wait(1.5)
    print("After")
end)

FunctionThatYields()
-- Output:
-- Before
-- yield for 1.5 seconds
-- After
```

---

## iscclosure

Checks if a given function is a C closure (implemented in C/C++).

```luau
function iscclosure(func: (...any) -> (...any)): boolean
```

### Parameters

- `func` - The function to check.

### Examples

```luau
local function ExecutorLuaClosure()
    print("This is an Executor Lua Closure")
end

local ExecutorCClosure = newcclosure(function()
    print("This is an Executor C Closure")
end)

local StandardCClosure = print
local GlobalExecutorClosure = filtergc

print(iscclosure(ExecutorCClosure)) -- Output: true
print(iscclosure(GlobalExecutorClosure)) -- Output: true
print(iscclosure(StandardCClosure)) -- Output: true

print(iscclosure(ExecutorLuaClosure)) -- Output: false
```

---

## islclosure

Checks if a given function is a L closure (implemented in Lua).

```luau
function islclosure(func: (...any) -> (...any)): boolean
```

### Parameters

- `func` - The function to check.

### Examples

```luau
local function ExecutorLuaClosure()
    print("This is an Exeucotr Lua Closure")
end

local ExecutorCClosure = newcclosure(function()
    print("This is an Executor C Closure")
end)

local StandardCClosure = print

print(islclosure(ExecutorLuaClosure)) -- Output: true

print(islclosure(StandardCClosure)) -- Output: false
print(islclosure(ExecutorCClosure)) -- Output: false
```

---

## isexecutorclosure

Checks if a given function is the executor's closure.

```luau
function isexecutorclosure(func: (...any) -> (...any)): boolean
```

### Parameters

- `func` - The function to check.

### Examples

```luau
local function ExecutorLuaClosure()
    print("This is an Executor Lua Closure")
end

local ExecutorCClosure = newcclosure(function()
    print("This is an Executor C Closure")
end)

local StandardCClosure = print
local GlobalExecutorClosure = filtergc

print(isexecutorclosure(ExecutorLuaClosure)) -- Output: true
print(isexecutorclosure(ExecutorCClosure)) -- Output: true
print(isexecutorclosure(GlobalExecutorClosure)) -- Output: true

print(isexecutorclosure(StandardCClosure)) -- Output: false
```

---

## clonefunction

Creates and returns a new function that has the same behaviour as the passed function.
> [!NOTE]
> The cloned function must have the same environment as the original.
> 
> Any sort of modification to the original shouldn't affect the clone. Meaning that stuff like hooking the original will not affect the clone.

```luau
function clonefunction(func: (...any) -> (...any)): (...any) -> (...any)
```

### Parameters

- `func` - The function to clone.

### Examples

```luau
local function Old()
    print("Hello")
end

local Clone = clonefunction(Old)

print(debug.info(Clone, "l")) -- Output: 1
print(debug.info(Clone, "n")) -- Output: old
print(Clone == Old) -- Output: false
print(getfenv(Clone) == getfenv(Old)) -- Output: true
```

---

## hookfunction

Hooks a function with another wanted function, and returns the original unhooked function.

> [!Note]
> The hook shouldn't have more upvalues than the function you want to hook.
>                       
> All possible hooking closure pairs should be supported (NC = newcclosure):

| Function | Hook |
| -------- | ---- |
|     L    |  L   |
|     L    |  C   |
|     L    |  NC  |
|     C    |  C   |
|     C    |  L   |
|     C    |  NC  |
|     NC   |  NC  |
|     NC   |  L   |
|     NC   |  C   |

```luau
function hookfunction<T>(function_to_hook: T, function_hook: (...any) -> (...any)): T
```

### Parameters

- `function_to_hook` - The function that will be hooked
- `function_hook` - The function that will be used as a hook

### Examples

```luau
local function DummyFunction()
    print("I am NOT hooked!")
end

local function DummyHook()
    print("I am hooked!")
end

DummyFunction() -- Output: I am NOT hooked!

local OldFunction = hookfunction(DummyFunction, DummyHook)

DummyFunction() -- Output: I am hooked!
OldFunction() -- Output: I am NOT hooked!
```
