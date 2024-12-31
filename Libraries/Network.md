# Network

---

## isnetworkowner

Determines if your client is the network owner of a part, returning true or false accordingly.

```luau
function isnetworkowner(part: BasePart): ()
```

### Parameters

- `part` - The part for which to check if you have network ownership.

### Example

```luau
local part = Instance.new("Part")
print(isnetworkowner(part)) -- Output: true
```

---
