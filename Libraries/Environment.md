# Environment

These functions allow to **modify/access** our executor environment and Roblox environment.

---

## getgenv

Provides a table containing all executor functions, serving as the shared environment for all scripts executed by the executor.

```luau
getgenv(): { [any]: any }
```

### Examples

```luau
getgenv().test = "hello world"
print(test) -- Should print "hello world" in current script and all future executed scripts
```

---

## getrenv

Returns the environment table that all game scripts use. Can be used to access functions that game scripts use.

```luau
getrenv(): { [any]: any }
```

### Examples

```luau
getrenv().game = nil -- Now no game script can access game
```

---

## getgc

Returns a table with all Lua values that aren't dead (meaning they are referenced by active scripts).

By default, it excludes tables; you can use `includeTables` to also get tables.

```luau
getgc(includeTables: boolean?): { [number]: userdata | table | function }
```

### Parameters

- `includeTables` - (Optional) Whether the output table should also include tables

### Examples

```luau
local DummyTable = {}
local function DummyFunction() end
task.wait(0.05) -- Step a bit

for garbageIndex, garbageValue in pairs(getgc()) do
    if garbageValue == DummyFunction then
        print(`Found function: {DummyFunction}`)
    elseif garbageValue == DummyTable then
        print(`Found table?: {DummyTable}`) -- This shouldn't print
    end
end
```

```luau
local DummyTable = {}
local function DummyFunction() end
task.wait(0.05) -- Step a bit

for garbageIndex, garbageValue in pairs(getgc(true)) do
    if garbageValue == DummyFunction then
        print(`Found function: {DummyFunction}`) -- Both should print
    elseif garbageValue == DummyTable then
        print(`Found table: {DummyTable}`) -- Both should print
    end
end
```

---

## filtergc

> [!NOTE]
> Values are compared using `bit-for-bit equality`, meaning NaN equals NaN and -0 is not equal to 0. This enables you to search for NaN or -0 as specific values.

Searches for Lua values that are currently referenced and match the specified criteria.

```luau
function filtergc(type: string | function | table, options: table, return_one?: bool)
```

### Table:

| Key            | Description                                                                 | Default   |
| -------------- | --------------------------------------------------------------------------- | --------- |
| `Keys`         | If not empty, only include tables with keys corresponding to all values in this table | `nil`     |
| `Values`       | If not empty, only include tables with values corresponding to all values in this table | `nil`     |
| `KeyValuePairs`| If not empty, only include tables with keys/value pairs corresponding to all values in this table | `nil`     |
| `Metatable`    | If not empty, only include tables with the metatable passed                | `nil`     |

### Function:

| Key        | Description                                                                        | Default |
| ---------- | ---------------------------------------------------------------------------------- | ------- |
| `Name`     | If not empty, only include functions with this name                                | `nil`   |
| `Constants`| If not empty, only include functions with constants that match all values in this table | `nil`   |
| `Upvalues` | If not empty, only include functions with upvalues that match all values in this table | `nil`   |
| `IgnoreExecutor`| If `false`, do not ignore Executor functions | `true` |   

### Parameters

- `type` - specifies the type of Lua value to search for
- `options` - criteria used to filter the search results based on the specified type
- `return_one` - A boolean that returns only the first match when true; otherwise, all matches are returned.

### Examples

```luau
local function myfunc()
    return "aaaaa" .. uv
end

print(filtergc('function', {
    IgnoreExecutor = false,
    Name = "myfunc"
}, true)
```
