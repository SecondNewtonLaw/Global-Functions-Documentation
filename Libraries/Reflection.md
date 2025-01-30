# Reflection

Functions that allow to **modify/access** hidden/non-scriptable properties of Instances.

---

## gethiddenproperty

> [!WARNING]
> It appears that many executors fail to implement some or all property types, Mainly `SharedString` and `SystemAddress`

> [!NOTE]
> Developers should not rely on just `setscriptable` to make this function work

Returns the hidden, non-scriptable properties value no matter its type, such as `BinaryString`, `SharedString` and `SystemAddress` and a boolean indicating if the property is hidden.

Avoids detections and errors that can happen by just using `setscriptable` to read the property

```luau
gethiddenproperty(instance: Instance, propertyName: string): (any, boolean)
```

### Parameters

- `instance` - The instance that contains the property.
- `propertyName` - The name of the property to be read.

### Example

```luau
local part = Instance.new("Part")
print(gethiddenproperty(part, "Name")) -- Returns "Part", false [Not Hidden]
print(gethiddenproperty(part, "DataCost")) -- Returns 20, true [Hidden]
```

---

## sethiddenproperty

> [!WARNING]
> It appears that many executors fail to implement some or all property types, Mainly `SharedString` and `SystemAddress`

> [!NOTE]
> Developers should not rely on just `setscriptable` to make this function work

Sets the hidden, non-scriptable property's value no matter its type, such as `BinaryString`, `SharedString` and `SystemAddress` and returns a boolean indicating if the property is hidden.

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
print(gethiddenproperty(part, "DataCost")) -- Returns 20, true [Hidden]
sethiddenproperty(part, "DataCost", 100) -- Returns true [Hidden]
print(gethiddenproperty(part, "DataCost")) -- Returns 100, true [Hidden]
```

---

## checkcaller

Determines whether the function was called from the executor's thread.

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
