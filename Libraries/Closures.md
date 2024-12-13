# Closures

Functions that allow **modification/creation** of Luau closures (functions)

---

## newcclosure

> [!WARNING]
> Many executors are implementing this function using `coroutine` functions in Lua; these implementations won't pass sUNC checks.
>
> The wrapped function should be yieldable (meaning the function can call `task.wait` for example)

This function takes in a Lua closure (function) and wraps it into a C closure (function).

When the returned function is called, the original Lua closure is called, and arguments are passed to the original closure (function), and then the original closure (function) returned arguments are passed to the caller of the C closure (function).

```luau
newcclosure(functionToWrap: (...any) -> (...any), wrappedFunctionName: string?): (...any) -> (...any)
```

### Parameters

- `functionToWrap` - A Lua closure (function) to be wrapped.
- `wrappedFunctionName` (optional) - A custom name for the wrapped function. If not provided, the name will be blank.

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

This function checks if a given function is a C closure (implemented in C/C++) or a Lua closure (implemented in Lua).

```luau
iscclosure(func: (...any) -> (...any)): boolean
```

### Parameters

- `func` - The function to check.

### Returns

- `true` if the function is a C closure.
- `false` if the function is a Lua closure.

### Examples

```luau
local luaFunction = function()
    print("This is a Lua function")
end

local cFunction = print

print(iscclosure(luaFunction)) -- Output: false
print(iscclosure(cFunction)) -- Output: true
```

### Use Cases

A primary use case for `iscclosure` is when working with functions like `hookfunction`. You might want to create a hook with the same closure type as the original function:

```luau
local function hookFunction(functionToHook, hook)
    if iscclosure(functionToHook) then
        hook = newcclosure(hook, debug.info(hook, "n"))
    end
    
    return hookfunction(functionToHook, hook)
end
```

In this example, `iscclosure` helps determine whether to wrap the `hook` function as a C closure using `newcclosure` to match the original function's type. This can be important for maintaining consistency and avoiding unexpected behavior when hooking functions.
