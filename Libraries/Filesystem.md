# Filesystem

Functions that provide read and write access to a files in an executor's workspace.

> [!WARNING]
> Filesystem functions should not be given access to read or write any files such as `.exe` or `.bat`, any malicious file types should be disallowed

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
