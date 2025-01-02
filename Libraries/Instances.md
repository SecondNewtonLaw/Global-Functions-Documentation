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
> To test your **gethui** implementation's functionality it's recommended you run [sUNC](https://discord.gg/EsfbAZJpzp).

Returns a hidden UI container that minimalizes most detection methods.

```luau
function gethui(): Folder
```

### Example

```luau
local UI = Instance.new("ScreenGui")
UI.Parent = gethui()
```

---

## setrbxclipboard

Copies the provided `rbxm`, `rbxmx` model data, or `in-game instance` to the Studio client's clipboard.

```luau
function setrbxclipboard(data: string | Instance): boolean
```

### Parameters

- `data` - The data to copy to the clipboard, if it is a string it must be a path to the `rbxm` or `rbxmx` file in your workspace, if it is an instance it must be a path to that instance in-game

### Example

```luau
local Part = Instance.new("Part", workspace)
setrbxclipboard(Part) -- you can now paste this part into your roblox studio client workspace
```

```luau
local data = readfile("model.rbxm") -- model.rbxm is a rbxm file in your executor's workspace
setrbxclipboard(data)
```
