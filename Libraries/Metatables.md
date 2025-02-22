# Metatables

The **Metatable** library allows interaction with metatables.

---

## getrawmetatable

> [!NOTE]
> **getrawmetatable** ignores the __metatable field.

Returns the metatable of **object**

```luau
function getrawmetatable(object: Instance | table): { Instance | function | string }
```

### Example

```luau
for i,v in getrawmetatable(game) do
    print(i, v)
end
```
