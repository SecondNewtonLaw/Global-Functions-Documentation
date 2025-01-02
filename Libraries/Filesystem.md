# Filesystem

Functions that provide read and write access to a files in an executor's workspace.

> [!WARNING]
> **Filesystem** functions should restrict access to potentially harmful file types like `.exe` or `.bat`.

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
