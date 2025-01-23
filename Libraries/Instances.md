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
function getcallbackvalue(object: Instance, property: string): (...any) -> (...any)?
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
function fireclickdetector(object: Instance<ClickDetector>, distance: number?): ()
```

Selectable Events: 'MouseClick', 'RightMouseClick', 'MouseHoverEnter', 'MouseHoverLeave'.

### Parameters

- `object` - The ClickDetector to trigger
- `distance?` - Distance to trigger the ClickDetector from

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

> [!NOTE]
> It's not recommended to implement this function in luau. Doing so will expose you to easy detections

Triggers a `ProximityPrompt` instantly, regardless of distance or duration.

```luau
function fireproximityprompt(object: Instance<ProximityPrompt>): ()
```

### Parameters

- `object` - The ProximityPrompt to fire

### Example

```luau
local DummyPart = Instance.new("Part", workspace)
local DummyProximityPrompt = Instance.new("ProximityPrompt")
DummyProximityPrompt.Parent = DummyPart

DummyProximityPrompt.Triggered:Connect(function()
    print("Triggered")
end)

fireproximityprompt(DummyProximityPrompt) -- Output: Triggered
```

---

## firetouchinterest

> [!NOTE]
> It's not recommended to implement this function in luau. Doing so will expose you to easy detections

Triggers a `Touched` event on a `BasePart` with the other wanted part.

```luau
function firetouchinterest(part: Instance<BasePart>, part2: Instance<BasePart>, toggle: number): ()
```

### Parameters

- `part` - The part initiating the touch
- `part2` - The part to be touched
- `toggle` - Determines the touching event trigger
    - `1` - Starts the **Touched** event
    - `0` - Ends the **Touched** event

### Example

```luau
local DummyPart = Instance.new("Part")
DummyPart.CFrame = CFrame.new(0, -200, 0)
DummyPart.Anchored = true
DummyPart.Parent = workspace

DummyPart.Touched:Connect(function(arg1)
    print(arg1:IsDescendantOf(game.Players.LocalPlayer.Character))
end)

firetouchinterest(game.Players.LocalPlayer.Character.Head, DummyPart, 1)
task.wait(0.5)
firetouchinterest(game.Players.LocalPlayer.Character.Head, DummyPart, 0) -- Output: true
```
