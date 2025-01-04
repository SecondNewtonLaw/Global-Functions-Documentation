# Instances

The **Instance** library allows interaction with game objects.

---

## getinstances

> [!NOTE]
> **getinstances** should be able to return instances outside of `game`

Returns a list of all instances referenced by the client.

```luau
function getinstances(): { [number]: Instance }
```

### Example

```luau
local DummyPart = Instance.new("Part")

for indexInstance, valueInstance in pairs(getnilinstances()) do
    if valueInstance == DummyPart then
        print(`Found the wanted nil instance: {DummyPart}`)
    end
end
```

---

## getnilinstances

Returns a list of instances that aren't descendants of a service provider.

```luau
function getnilinstances(): { [number]: Instance }
```

### Example

```luau
local DummyPart = Instance.new("Part")
DummyPart.Parent = nil

for indexNil, valueNil in pairs(getnilinstances()) do
    if valueNil == DummyPart then
        print(`Found the wanted nil instance: {DummyPart}`)
    end
end
```

---

## cloneref

Returns a copy of the Instance where the copy should not be equal to the original Instance it was cloned from.

```luau
function cloneref(object: Instance): Instance
```

### Parameters

- `object` - The Instance to clone.

### Example

```luau
local ClonedPlayer = cloneref(game:GetService("Players").LocalPlayer)
local Player = game:GetService("Players").LocalPlayer
print(Player == ClonedPlayer) -- Output: False
```

---

## gethui

> [!NOTE]
> This is an implementation detail; as a regular scripter, you may ignore this!
> The container in which the elements sit in, should not be findable directly. For example, a descendant for loop through CoreGui shouldn't find the container. Although if found directly, the container's parent should be clonereffed.

> If you're going for a different approach from the one above, make sure the container is able to be found in the registry

Returns a hidden UI container that minimalizes most detection methods.

```luau
function gethui(): Instance
```

### Example

```luau
local UI = Instance.new("ScreenGui")
UI.Parent = gethui()
print(gethui().ScreenGui) -- Output: "ScreenGui"
```

---

## getcallbackvalue

Returns the function assigned to an object's callback property, which is otherwise inaccessible through standard indexing.

```luau
function getcallbackvalue(object: Instance, property: string): function?
```

### Parameters

- `object` - The object to get the callback property from.
- `property` - The name of the callback property.

### Example

```luau
local DummyBindableFunction = Instance.new("BindableFunction")

DummyBindableFunction.OnInvoke = function()
    print("Callback")
end

getcallbackvalue(DummyBindableFunction, "OnInvoke")() -- Output: Callback
```

---

## fireclickdetector

> [!NOTE]
> You can read more about the ClickDetector events [here](https://create.roblox.com/docs/reference/engine/classes/ClickDetector)

Triggers a specified event on a `ClickDetector`. If not provided, the distance parameter defaults to **zero**, and the event parameter defaults to **MouseClick**.

```luau
function fireclickdetector(object: ClickDetector, distance: number?, event: string?): ()
```

Selectable Input Events: 'MouseClick', 'RightMouseClick', 'MouseHoverEnter', 'MouseHoverLeave'.

### Parameters

- `object` - The ClickDetector to trigger
- `distance` - Optional distance to trigger the ClickDetector from
- `event` - Optional input event to specify

### Example

```luau
local Part = Instance.new("Part", workspace)
local ClickDetector = Instance.new("ClickDetector", Part)

ClickDetector.RightMouseClick:Connect(function()
	print("Hello, World!")
end)

fireclickdetector(ClickDetector, 5, "RightMouseClick") -- Output: Hello, World!
```
