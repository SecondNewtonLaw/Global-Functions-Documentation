# Filesystem

Functions that provide read and write access to a files in an executor's workspace.

> [!WARNING]
> **Filesystem** functions should restrict access to read and write potentially harmful file types like `.exe` or `.bat`

> [!NOTE]
> If a file or folder does not exist and it is checked via `isfolder`/`isfile` it should return false

---

## writefile

Writes data to a specified file path.

```luau
function writefile(path: string, data: string): ()
```

### Parameters
- `path` - path to the file that will be wrote to
- `data` - the data to be written into the file

### Example

```luau
writefile("thing.txt", "Hello, world!") -- thing.txt is written with the data "Hello, world!"
setclipboard(readfile("thing.txt")) -- your clipboard will now contain the string "Hello, world!" from thing.txt
```

---

## readfile

Retrieves the content of the file at the specified path.

```luau
function readfile(path: string): string
```

### Parameters
- `path` - path to the file that will be read

### Example

```luau
writefile("thing.txt", "Hello, world!")
setclipboard(readfile("thing.txt")) -- your clipboard will now contain the string "Hello, world!" from thing.txt
```

---

## listfiles

Provides a list of files and folders within a specified directory.

```luau
function listfiles(path: string): {string}
```

### Parameters
- `path` - path to the directory

### Examples

```luau
-- second is a folder under the folder that is in the listfiles parameter; Folder --> Second
writefile("folder/second/thing1.txt", "what")
writefile("folder/second/thing2.txt", "what ok")
for _, file in ipairs(listfiles("folder/second") do
    print(file) -- thing1 and thing2 will be outputted into the console
end
```

---

## isfile

Determines if the specified path is a file.

```luau
function isfile(path: string): boolean
```

### Parameters
- `path` - The path to check

### Example

```luau
makefolder("thing")
writefile("thing/real.txt", "Hello, World!")
print(isfile("thing")) -- Output: False
print(isfile("thing/real.txt")) -- Output: True
```

---

## appendfile

Appends data to the end of the file at the specified path, creating the file if it doesn't already exist.

```luau
function appendfile(path: string, contents: string): ()
```

### Parameters
- `path` - Path to the file you will append data to
- `contents` - The content to append

### Example

```luau
writefile("items.txt", "List of Items:\n")

for _, child in ipairs(game:GetService("ReplicatedStorage"):GetChildren()) do
    if child.ClassName ~= "" then
        appendfile("items.txt", child.ClassName .. "\n")
    end
end
```

---

## delfile

Deletes the file at the specified path.

```luau
function delfile(path: string): ()
```

### Parameters

- `path` - Path to the file you will delete

### Example

```luau
writefile("thing.txt", "Hello, world!")
delfile("thing.txt") -- thing.txt is now removed from the workspace and shouldn't exist
print(isfile("thing.txt")) -- Output: False
```

---

## loadfile

Generates a chunk from the file at the given path, using the global environment. Returns the chunk or nil with an error message.

```luau
function loadfile(path: string): ()
```

### Parameters

- `path` - the file to generate a chunk from

### Example

```luau
writefile("name.lua", "local name = ...; return 'My name is, ' .. name")
local func, err = loadfile("name.lua")
local output = assert(func, err)("Alice")
print(output)  -- Output: My name is, Alice
```

---

## getcustomasset

Returns a content URL `(e.g., rbxasset://)` that can be used with UI elements, sounds, meshes, and more. Internally, files are copied to the game's content directory.

```luau
function getcustomasset(path: string, noCache: boolean): string
```

### Parameters

- `path` - The path to the asset.
- `noCache` - Determines whether the asset should be cached or not.

### Example

```luau
TBD
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

---

## makefolder

Creates a folder at the specified path if it doesn't already exist.

```luau
function makefolder(path: string): ()
```

### Parameters

- `path` - The location where you want to create the folder.

### Example

```luau
makefolder("thingy")
print(isfolder("thingy"))) -- Output: True
```

--- 

## isfolder

Determines if the specified path is a folder.

```luau
function isfolder(path: string): boolean
```

### Parameters

- `path` - The path to check

### Example

```luau
writefile("thingy.txt", "sunc")
print(isfolder("thingy.txt"))) -- Output: False
```

---

## delfolder

Deletes the folder at the specified path.

```luau
function delfolder(path: string): ()
```

### Parameters

- `path` - Path to the folder you will delete

### Example

```luau
makefolder("thingy")
delfolder("thingy")
print(isfolder("thingy")) -- Output: False | it does not exist anymore so it returned false
```
