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
