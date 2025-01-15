Functions that allow **inspection/modification/creation** of Luau closures

---

## newcclosure

> [!WARNING]
> Many executors are implementing this function using `coroutine` functions in Lua; these implementations won't pass sUNC checks.
>
> The wrapped function should be yieldable (meaning the function can call `task.wait` for example)

This function takes in a Lua closure and wraps it into a C closure.

When the returned function is called, the original Lua closure is called, and arguments are passed to the original closure, and then the original closure returned arguments are passed to the caller of the C closure.

```luau
function newcclosure(functionToWrap: function, debugName: string?): function
```

### Parameters

- `functionToWrap` - A Lua closure (function) to be wrapped.
- `debugName` - (Optional) A debug name for the wrapped function. If not provided, the name will be blank.

### Examples

```luau
local originalFunction = function(...)
    return ...
end

print(iscclosure(originalFunction)) -- Prints false

local wrappedFunction = newcclosure(originalFunction, "sUNC")

print(iscclosure(wrappedFunction)) -- Prints true

local functionResults = wrappedFunction("hello world")
print(functionResults) -- Prints "hello world"

print(debug.info(wrappedFunction, "n")) -- Prints "sUNC"
```

```luau
local functionThatYields = newcclosure(function()
   task.wait(5)
   print("hello world")
end)

functionThatYields() -- Should print "hello world" after 5 seconds
```

```luau
newcclosure(print) -- Should error because print is already C closure (function)
```

---

## iscclosure

Checks if a given function is a C closure (implemented in C/C++).

```luau
function iscclosure(func: function): boolean
```

### Parameters

- `func` - The function to check.

### Examples

```luau
local function ExecutorLuaClosure()
    print("This is an Exeucotr Lua Closure")
end

local ExecutorCClosure = newcclosure(function()
    print("This is an Executor C Closure
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
function islclosure(func: function): boolean
```

### Parameters

- `func` - The function to check.

### Examples

```luau
local function ExecutorLuaClosure()
    print("This is an Exeucotr Lua Closure")
end

local ExecutorCClosure = newcclosure(function()
    print("This is an Executor C Closure
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
function isexecutorclosure(func: function): boolean
```

### Parameters

- `func` - The function to check.

### Examples

```luau
local function ExecutorLuaClosure()
    print("This is an Exeucotr Lua Closure")
end

local ExecutorCClosure = newcclosure(function()
    print("This is an Executor C Closure
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
> The new function must have the same environment as the old one.
> 
> Hooking the new function must not affect the old one.

```luau
function clonefunction(func: function): function
```

### Parameters

- `func` - The function to clone.

### Examples

```luau
local function oldfunction()
    return "hello world!"
end

local newfunction = clonefunction(oldfunction)
print(oldfunction == newfunction) --should return false
print(newfunction()) --should print "hello world!"
```

---

## hookfunction

Hooks a function and returns the original unhooked function.

> [!Note]
> The hook shouldn't have more upvalues than the function you want to hook.                                                                         
> All closure pairs should be supported, otherwise it will not pass the test.

```luau
function hookfunction<T>(function_to_hook: T, function_hook: (...any) -> (...any)): T
```

### Parameters

- `function_to_hook` - The function that will be hooked
- `function_hook` - The function that will be used as a hook

### Examples

```luau
local function DummyFunction(): string
    print("I am NOT hooked!")
end

local function DummyHook(): string
    print("I am hooked!")
end

DummyFunction() -- prints: "I am NOT hooked!"

local OldFunction = hookfunction(DummyFunction, DummyHook)

DummyFunction() -- prints: "I am hooked!"
OldFunction() -- prints: "I am NOT hooked!"
```
