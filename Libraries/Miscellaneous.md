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

---

## setfflag

> [!NOTE]
> You can find a list of all Fast Flags [here](https://raw.githubusercontent.com/MaximumADHD/Roblox-FFlag-Tracker/main/PCDesktopClient.json)

Sets a specified flag to the given value.

```luau
function setfflag(flag: string, value: string): ()
```

### Parameters
- `flag` - the flag to change
- `value` - the value you want to set the flag to

### Example

```luau
setfflag("BacktraceLogSize", "400") -- sets the backtracelogsize fflag to 400
```

---

## setfpscap

Sets the in-game FPS cap. If set to 0, the FPS cap is disabled.

```luau
function setfpscap(fps: number): ()
```

### Parameters
- `fps` - The FPS cap.

### Example

```luau
setfpscap(0) -- Unlocks the FPS cap
```

---

## request

Sends an HTTP request with the given options, yielding until the request is finished, and returns the response.

```luau
function request(options: HttpRequest): HttpResponse
```

### Request

| Field | Type | Description |
| ----- | ---- | ----------- |
| `Url` | string | The URL for the request. |
| `Method` | string | The HTTP method to use. Can be `GET`, `POST`, `PATCH`, or `PUT`. |
| `Body` | string? | The body of the request. |
| `Headers` | table? | A table of headers. |
| `Cookies` | table? | A table of cookies. |

### Response

| Field | Type | Description |
| ----- | ---- | ----------- |
| `Body` | string | The body of the response. |
| `StatusCode` | number | The number status code of the response. |
| `StatusMessage` | string | The status message of the response. |
| `Success` | boolean | Whether or not the request was successful. |
| `Headers` | table | A dictionary of headers. |

### Headers

The executor provides the following headers for identification on a web server:

| Header | Description |
| ------ | ----------- |
| `PREFIX-User-Identifier` | A string unique to each user, and does not change if the script executor is used across computers. |
| `PREFIX-Fingerprint` | The hardware identifier of the user. |
| `User-Agent` | The name and version of the executor. |

### Parameters

- `options` - The options to use.

### Example
```luau
local response = request({
	Url = "http://example.com/",
	Method = "GET",
})

print(response.StatusCode .. " - " .. response.StatusMessage) --> 200 - HTTP/1.1 200 OK
```
