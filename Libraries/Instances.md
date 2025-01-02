# Instances

The **Instance** library allows interaction with game objects.

---

## getinstances

> [!NOTE]
> **getinstances** should be able to return instances outside of `game`

Returns a list of all Instances referenced by the client.

```luau
function getinstances(): {Instance}
```

### Example

```luau
for _, instance in ipairs(getinstances()) do
    if instance.ClassName == "Model" then
        print(instance.Name) -- will print all instances with that is a Model
    end
end
```
