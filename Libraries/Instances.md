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
> It's not recommended to implement this function in luau. Doing so will expose you to easy detections

Triggers a specified event on a `ClickDetector`. The event parameter defaults to **MouseClick** if not defined. Not providing the distance will default to infinite.

```luau
function fireclickdetector(object: ClickDetector, distance: number?, event: string?): ()
```

Selectable Events: 'MouseClick', 'RightMouseClick', 'MouseHoverEnter', 'MouseHoverLeave'.

### Parameters

- `object` - The ClickDetector to trigger
- `distance?` - Distance to trigger the ClickDetector from
- `event?` - Input event to specify

### Example

```luau
local ClickDetector = Instance.new("ClickDetector")

ClickDetector.MouseClick:Connect(function()
    print("Fired")
end)

fireclickdetector(ClickDetector, 32) -- This will not output
```

```luau
local ClickDetector = Instance.new("ClickDetector")

ClickDetector.MouseClick:Connect(function()
    print("Fired")
end)

fireclickdetector(ClickDetector, 31) -- Output: Fired
```

---

## fireproximityprompt

Triggers a `ProximityPrompt` with a specified number of times, with an option to ignore its hold duration.

```luau
function fireproximityprompt(object: ProximityPrompt, amount: number?, skip: boolean?): ()
```

### Parameters

- `object` - The ProximityPrompt to fire
- `amount` - Number of times to fire the specified ProximityPrompt
- `skip` - Determines if the ProximityPrompt should ignore the hold duration

### Example

```luau
local Part = Instance.new("Part")
local ProximityPrompt = Instance.new("ProximityPrompt", Part)

ProximityPrompt.Triggered:Connect(function()
	Part.Name = "Triggered"
end)

fireproximityprompt(ProximityPrompt, 1, true) -- Fires the proximityprompt once and instantly does it by specifying skip as `true`

repeat task.wait() until Part.Name == "Triggered"

print("Part is now named Triggered!")
```
