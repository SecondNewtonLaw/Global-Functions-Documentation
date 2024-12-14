# Closures

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
newcclosure(functionToWrap: function, debugName: string?): function
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
iscclosure(func: function): boolean
```

### Parameters

- `func` - The function to check.

### Examples

```luau
local luaFunction = function()
    print("This is a Lua function")
end

local cFunction = print

print(iscclosure(luaFunction)) -- Output: false
print(iscclosure(cFunction)) -- Output: true
```

---

## islclosure

Checks if a given function is a L closure (implemented in Lua).

```luau
islclosure(func: function): boolean
```

### Parameters

- `func` - The function to check.

### Examples

```luau
local luaFunction = function()
    print("This is a Lua function")
end

local cFunction = print

print(islclosure(luaFunction)) -- Output: true
print(islclosure(cFunction)) -- Output: false
```

---

## isexecutorclosure

Checks if a given function is the executor's closure.

```luau
isexecutorclosure(func: function): boolean
```

### Parameters

- `func` - The function to check.

### Examples

```luau
local executorClosure = function()
    print("This is a Lua function")
end

local standardLuauClosure = print

local executorClosure2 = iscclosure

print(isexecutorclosure(executorClosure)) -- Output: true
print(isexecutorclosure(standardLuauClosure)) -- Output: false
print(isexecutorclosure(executorClosure2)) -- Output: true
```
