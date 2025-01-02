# Miscellaneous

The **miscellaneous** functions are a collection of functions that have no designated category.

---

## identifyexecutor

Returns the name and version of the current executor, first string contains the executor's identifier and the second contains the version of the executor.

```luau
function identifyexecutor(): (string, string)
```

### Example

```luau
local execName, execVersion = identifyexecutor()
print(execName, execVersion) -- Output: "YourName 0.0.1"
```
---

## setclipboard

Copies a string or Instance or table of Instances to the clipboard.

```luau
function setclipboard(data: string | number | Instance | table)
```

### Parameters

- `data` - The data you want to go onto your clipboard

### Example

```luau
setclipboard(game:GetService("Players").LocalPlayer.Name) -- Your clipboard should contain your username
```

---

## messagebox

A wrapper around Microsoft's [MessageBoxA](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-messageboxa).

```luau
function messagebox(text: string, caption: string, flags: number): number
```

### Parameters
- `text` - The text to display in the message box
- `caption` - The caption of the message box
- `flags` - The flags to use

### Example

```luau
local MB_ICONINFORMATION = 0x00000040
local MB_OKCANCEL = 0x00000001
local MB_DEFBUTTON1 = 0x00000000

local IDOK = 0x00000001
local IDCANCEL = 0x00000002

local input = messagebox(
    "Do you want to proceed with the operation?",
    "Confirmation",
    bit32.bor(MB_ICONINFORMATION, MB_OKCANCEL, MB_DEFBUTTON1)
)

if input == IDOK then
    print("OK")
elseif input == IDCANCEL then
    print("Cancelled")
end
```

---

## queue_on_teleport

Schedules the specified script to run after the player has teleported to another place.

```luau
function queue_on_teleport(code: string): ()
```

### Parameters
- `code` - The code to schedule

### Example
```luau
queue_on_teleport([[print("Hello, World!")]]) -- On the next teleport "Hello, World!" will be outputted into the console
```
