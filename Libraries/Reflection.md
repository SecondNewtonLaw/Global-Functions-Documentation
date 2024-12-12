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
- `instance` - Instance that the property will be read from.
- `propertyName` - Name of the property that will be read.

### Example
```luau
local part = Instance.new("Part")
print(gethiddenproperty(part, "Name")) -- Returns "Part", false
print(gethiddenproperty(part, "DataCost")) -- Returns 20, true
```

---
