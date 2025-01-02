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
