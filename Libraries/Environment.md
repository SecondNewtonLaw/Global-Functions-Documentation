# Environment

These functions allow to **modify/access** our executor environment and Roblox environment.

---

## getgenv

Provides a table containing all executor functions, serving as the shared environment for all scripts executed by the executor.

```luau
getgenv(): { any }
```

### Example

```luau
getgenv().test = "hello world"
print(test) -- Should print "hello world" in current script and all future executed scripts
```

---

## getrenv

Returns the environment table that all game scripts use. Can be used to access functions that game scripts use.

```luau
getrenv(): { any }
```

### Example

```luau
getrenv().game = nil -- Now no game script can access game
```

---

## getgc

Returns a table with all Lua values that aren't dead (meaning they are referenced by active scripts).

By default, it excludes tables; you can use `includeTables` to also get tables.

```luau
getgc(include_tables: boolean?): { { any } | (...any) -> (...any) }
```

### Parameter

- `include_tables?` - Whether the output table should also include tables

### Examples

```luau
local DummyTable = {}
local function DummyFunction() end
task.wait(0.05) -- Step a bit

for GarbageIndex, GarbageValue in pairs(getgc()) do
    if GarbageValue == DummyFunction then
        print(`Found function: {DummyFunction}`)
    elseif GarbageValue == DummyTable then
        print(`Found table?: {DummyTable}`) -- This shouldn't print
    end
end
```

```luau
local DummyTable = {}
local function DummyFunction() end
task.wait(0.05) -- Step a bit

for GarbageIndex, GarbageValue in pairs(getgc(true)) do
    if GarbageValue == DummyFunction then
        print(`Found function: {DummyFunction}`) -- Both should print
    elseif GarbageValue == DummyTable then
        print(`Found table: {DummyTable}`) -- Both should print
    end
end
```

---

## filtergc

Similar to `getgc`, will return Lua values that are being referenced and match the specified criteria.

```luau
function filtergc(filter_type: "function" | "table", filter_options: FunctionFilterOptions | TableFilterOptions, return_one: boolean?): (...any) -> (...any) | { [any]: any }
```

### Table filter options:

| Key            | Description                                                                                       | Default |
| -------------- | ------------------------------------------------------------------------------------------------- | ------- |
| `Keys`         | If not empty, only include tables with keys corresponding to all values in this table.             |  `nil`  |
| `Values`       | If not empty, only include tables with values corresponding to all values in this table.           |  `nil`  |
| `KeyValuePairs`| If not empty, only include tables with keys/value pairs corresponding to all values in this table. |  `nil`  |
| `Metatable`    | If not empty, only include tables with the metatable passed.                                       |  `nil`  |

### Function filter options:

| Key             | Description                                                                             | Default |
| --------------- | --------------------------------------------------------------------------------------- | ------- |
| `Name`          | If not empty, also include functions with this name.                                     |  `nil`  |
| `IgnoreExecutor`| If true, also include functions made in the executor.                                    |  `true` |
| `Hash`          | If not empty, also include functions with the specified hash of their bytecode.                  |  `nil`  |
| `Constants`     | If not empty, also include functions with constants that match all values in this table. |  `nil`  |
| `Upvalues`      | If not empty, also include functions with upvalues that match all values in this table.  |  `nil`  |

### Parameters

- `filter_type` - specifies the type of Lua value to search for.
- `filter_options` - criteria used to filter the search results based on the specified type.
- `return_one?` - A boolean that returns only the first match when true; otherwise, all matches are returned.

> [!NOTE]
> Executing this examples multiple times in a short period of time may result in false negatives.

### Examples - Function

Usage of `return_one?`:
```luau
local function DummyFunction() end

local Retrieved = filtergc('function', {Name = "DummyFunction", IgnoreExecutor = false}, true)
print(type(Retrieved)) -- Output: function
```

```luau
local function DummyFunction() end

local Retrieved = filtergc('function', {Name = "DummyFunction", IgnoreExecutor = false}) -- Returns a table
print(type(Retrieved[1])) -- Output: function
```

## Usage of `options` parameter

Usage of `IgnoreExecutor` and `Hash`:
```luau
local function DummyFunction() return "Hello" end
local FuncHash = getfunctionhash(DummyFunction)
local Retrieved = filtergc("function", {Hash = FuncHash, IgnoreExecutor = false}, true)
print(getfunctionhash(Retrieved) == FuncHash) -- Output: true
print(Retrieved == DummyFunction) -- Output: true
```

Usage of `Constants` and `Upvalues`:
```luau
local up = 5
local function DummyFunction() 
    up+=1
    print(game.Players.LocalPlayer)
end

local Retrieved = filtergc('function', { 
    Constants = {
        "print", "game", "Players", "LocalPlayer", 1
    },
    Upvalues = {5},
    IgnoreExecutor = false
}, true)

print(Retrieved == DummyFunction) -- Output: true
```

---

### Examples - Table

Usage of `Keys` and `Values`:
```lua
local DummyTable = { ["DummyKey"] = "" }

local Retrieved = filtergc('table', {
    Keys = { "DummyKey" },
    IgnoreExecutor = false
}, true)

print(Retrieved == DummyTable) -- Output: true
```

Usage of `KeyValuePairs`:
```luau
local DummyTable = { ["DummyKey"] = "DummyValue" }

local Retrieved = filtergc('table', {
    KeyValuePairs = { ["DummyKey"] = "DummyValue" },
    IgnoreExecutor = false
}, true)

print(Retrieved == DummyTable) -- Output: true
```
