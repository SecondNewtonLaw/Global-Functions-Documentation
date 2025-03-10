# Reflection

Functions that allow to **modify/access** hidden/non-scriptable properties of Instances.

---

## gethiddenproperty

> [!WARNING]
> Many executors may implement this function by using `setscriptable`, which is discouraged due to detection vectors and/or limitations coming with `setscriptable` itself.

Returns the hidden, non-scriptable properties value no matter its type, such as `BinaryString`, `SharedString` and `SystemAddress`. A boolean will also be returned, indicating if the property is hidden or not.


```luau
gethiddenproperty(instance: Instance, propertyName: string): (any, boolean)
```

### Parameters

- `instance` - The instance that contains the property.
- `propertyName` - The name of the property to be read.

### Example

```luau
local part = Instance.new("Part")
print(gethiddenproperty(part, "Name")) -- Output: "Part", false
print(gethiddenproperty(part, "DataCost")) -- Output: 20, true
```

---

## sethiddenproperty

> [!WARNING]
> Many executors may implement this function by using `setscriptable`, which is discouraged due to detection vectors and/or limitations coming with `setscriptable` itself.


Sets the hidden, non-scriptable property's value no matter its type, such as `BinaryString`, `SharedString` and `SystemAddress`. A boolean will also be returned, indicating if the property is hidden or not.

Avoids detections and errors that can happen by just using `setscriptable` to set the property

```luau
sethiddenproperty(instance: Instance, propertyName: string, propertyValue: any): boolean
```

### Parameters

- `instance` - The instance that contains the property.
- `propertyName` - The name of the property to be assigned.
- `propertyValue` - The value to which the property should be set.

### Example

```luau
local part = Instance.new("Part")
print(gethiddenproperty(part, "DataCost")) -- Output: 20, true
sethiddenproperty(part, "DataCost", 100)
print(gethiddenproperty(part, "DataCost")) -- Output: 100, true
```

---

## checkcaller

Determines whether the function was called from the executor's thread or not.

```luau
function checkcaller(): boolean
```

### Example

```luau
local FromCaller
local _; _ = hookmetamethod(game, "__namecall", function(...)
    if FromCaller ~= true then
        FromCaller = checkcaller()
    end
    return _(...)
end)

task.wait(0.09) -- Step a bit
hookmetamethod(game, "__namecall", _)

print(FromCaller) -- Output: false
print(checkcaller()) -- Output: true
```

---

## setthreadidentity

Sets the current thread's identity to the wanted value

```luau
function setthreadidentity(id: number): ()
```

### Parameters

- `id` - The wanted identity to change to

### Example

```luau
setthreadidentity(2)
print(game.CoreGui) -- Throws an error
setthreadidentity(8)
print(pcall(Instance.new, "Player")) -- Output: true Player
```

---

## getthreadidentity

Gets the current thread's identity

```luau
function getthreadidentity(): number
```

### Example

```luau
task.defer(function() setthreadidentity(2); print(getthreadidentity()) end)
setthreadidentity(3)
print(getthreadidentity())

-- Output: 
-- 3
-- 2
```
