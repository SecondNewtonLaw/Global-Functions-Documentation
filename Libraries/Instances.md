# Instances

The **Instance** library allows interaction with game objects.

---

## getinstances

> [!NOTE]
> **getinstances** should be able to return instances outside of `game`

Returns a list of all instances referenced by the client.

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

---

## getnilinstances

Returns a list of instances that aren't descendants of a service provider.

```luau
function getnilinstances(): {Instance}
```

### Example

```luau
for _, object in ipairs(getnilinstances()) do
	if object:IsA("BasePart") then
		print(object, "is a BasePart")
	end
end
```

---

## gethui

> [!NOTE]
> **gethui** should not be equal to `CoreGui` and should be hidden from visibility.

Returns a hidden UI container that minimalizes most detection methods.

```luau
function gethui(): Folder
```

### Example

```luau
local UI = Instance.new("ScreenGui")
UI.Parent = gethui()
```
