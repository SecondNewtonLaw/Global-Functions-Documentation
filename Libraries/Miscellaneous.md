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
