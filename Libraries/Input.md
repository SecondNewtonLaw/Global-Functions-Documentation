# Input

The **Input** library allows you to simulate user inputs.

---

## isrbxactive

> [!NOTE]
> For any of the other input functions to work isrbxactive **must** be true

Indicates if the game's window is currently in focus.

```luau
function isrbxactive(): boolean
```

### Example

```luau
if isrbxactive() then
	print("the window is active")
end
```

---

## mouse1click

```luau
function mouse1click(): ()
```

Dispatches a left mouse button click.

---

## mouse1press

```luau
function mouse1press(): ()
```

Dispatches a left mouse button press.

---

## mouse1release

```luau
function mouse1release(): ()
```

Dispatches a left mouse button release.

---

## mouse2click

```luau
function mouse2click(): ()
```

Dispatches a right mouse button click.

---

## mouse2press

```luau
function mouse2press(): ()
```

Dispatches a right mouse button press.

---

## mouse2release

```luau
function mouse2release(): ()
```

Dispatches a right mouse button release.

---

## mousemoveabs

Moves the mouse cursor to the given absolute position on the screen.

```luau
function mousemoveabs(x: number, y: number): ()
```

### Parameters

- `x` - The x-coordinate of the mouse cursor
- `y` - The y-coordinate of the mouse cursor

### Example

Move the mouse in a zigzag pattern across the screen:

```luau
local direction = 1

for i = 0, 100 do
    local x = (i / 100) * workspace.CurrentCamera.ViewportSize.X
    local y = workspace.CurrentCamera.ViewportSize.Y / 2 + math.sin(i / 10) * 100 * direction
    mousemoveabs(x, y)

    if i % 25 == 0 then
        direction = -direction
    end
    
    task.wait(0.05)
end
```

---

## mousemoverel

Shifts the mouse cursor by the given relative distance.

```luau
function mousemoverel(x: number, y: number): ()
```

### Parameters

- `x` - The x-coordinate of the mouse cursor
- `y` - The y-coordinate of the mouse cursor

### Example

Moves the cursor in a small circle:

```luau
for i = 0, 20 do
	local x = math.sin(i / 20 * math.pi * 2)
	local y = math.cos(i / 20 * math.pi * 2)
	mousemoverel(x * 100, y * 100)
	task.wait(0.05)
end```
