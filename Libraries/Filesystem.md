# Filesystem

Functions that provide read and write access to a files in an executor's workspace.

> [!WARNING]
> Filesystem functions should not be given access to read or write any files such as `.exe` or `.bat`, any malicious file types should be disallowed

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
