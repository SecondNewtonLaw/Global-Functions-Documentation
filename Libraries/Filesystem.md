# Filesystem

Functions that provide read and write access to a files in an executor's workspace.

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
setclipboard(readfile("thing.txt")) -- your clipboard will now contain the string from thing.txt
```
