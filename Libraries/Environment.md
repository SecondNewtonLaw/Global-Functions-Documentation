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
getgc(includeTables?: boolean): { [number]: userdata | table | function }
```

### Parameters

- `includeTables` - Whether the output table should also include tables

### Examples

```luau
local DummyTable = {}
local function DummyFunction() end
task.wait(0.05) -- Step a bit

for garbageIndex, garbageValue in pairs(getgc()) do
    if garbageValue == DummyFunction then
        print(`Found function: {DummyFunction}`)
    elseif garbageValue == DummyTable then
        print(`Found table?: {DummyTable}`) --This shouldn't print
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
