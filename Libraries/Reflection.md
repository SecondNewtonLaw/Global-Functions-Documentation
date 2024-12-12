# Reflection
TODO: Description for reflection

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
print(gethiddenproperty(part, "Name")) -- Returns "Part", false
print(gethiddenproperty(part, "DataCost")) -- Returns 20, true
```

---

## sethiddenproperty
> [!WARNING]
> It appears that many executors fail to implement some or all property types, Mainly `SharedString` and `SystemAddress`

> [!NOTE]
> Developers should not rely on just `setscriptable` to make this function work

Sets the hidden, non-scriptable property's value no matter its type, such as `BinaryString`, `SharedString` and `SystemAddress`.

Avoids detections and errors that can happen by just using `setscriptable` to set the property
```luau
sethiddenproperty(instance: Instance, propertyName: string, propertyValue: any): ()
```

### Parameters
- `instance` - The instance that contains the property.
- `propertyName` - The name of the property to be assigned.
- `propertyValue` - The value to which the property should be set.

### Example
```luau
local part = Instance.new("Part")
print(gethiddenproperty(part, "DataCost")) -- Returns 20, true
sethiddenproperty(part, "DataCost", 100) -- Sets "DataCost" to 100
print(gethiddenproperty(part, "DataCost")) -- Returns 100, true
```

---
