# Scripts

The **Script** library provides functions that access to script environments and internal state.

---

## getscriptbytecode

> [!NOTE]
> This function should throw an **error** if the script has no bytecode.
> We encourage this behavior, as it's easier for people to pcall, rather than each executor having its own output.

```luau
function getscriptbytecode(script: Script | LocalScript | ModuleScript): string
```

### Parameter

- `script` - The `Script`, `LocalScript` or `ModuleScript` the bytecode would be obtained from.

### Example

```luau
local AnimateScriptBytecode = getscriptbytecode(game.Players.LocalPlayer.Character.Animate)
print(AnimateScriptBytecode) -- Returns a string with the bytecode.

print(getscriptbytecode(Instance.new("LocalScript"))) -- Throws an error
```

---

## getscripthash

> [!NOTE]
> This is an implementation detail; as a regular scripter, you may ignore this!
> 
> Please hash the compressed and encrypted bytecode; do not decrypt and decompress the bytecode and then hash it!
> Additionally, this function should throw an **error** if the script has no bytecode to hash.
> We encourage this behavior, as it's easier for people to pcall, rather than each executor having its own output.

Returns a `SHA384` hash represented in hex of the module/script's bytecode. This function should work with `LocalScript`, `ModuleScript`, and `Script` instances that have RunContext set to Client.

```luau
function getscripthash(script: Script | LocalScript | ModuleScript): string
```

### Parameter

- `script` - The `Script`, `LocalScript` or `ModuleScript` the function should obtain a hash of.

### Example

```luau
local ScriptHash = getscripthash(game.Players.LocalPlayer.Character.Animate)
print(ScriptHash) -- Should return a non-changing SHA384 hash in hex representation

print(getscripthash(Instance.new("LocalScript"))) -- Throws an error
```

---

## getscriptclosure

> [!NOTE]
> This is an implementation detail; as a regular scripter, you may ignore this!
> 
> The closure you return needs to be callable (meaning it can be called without erroring). This means the closure needs to have properly set capabilities and identity and a proper environment with the script global!
>
> For the environment, make a new table having __index set to the table L->global->mainthread->gt, and for the script global, just push the script instance and set it on the table.
>
> For capabilities, make sure to check if the script/module is a RobloxScript, and set identity and capabilities accordingly.
> 
> This function should throw an **error** if the script has no bytecode.
> We encourage this behavior, as it's easier for people to pcall, rather than each executor having its own output.

This function creates a new closure (function) from the module/script's bytecode. The game does not use the function you will get; it's mostly used to retrieve constants.

This should work with `LocalScript`, `ModuleScript`, and `Script` instances that have RunContext set to Client.

```luau
function getscriptclosure<A..., R...>(script: Script | LocalScript | ModuleScript): (A...) -> R...
```

### Parameter

- `script` - The `Script`, `LocalScript` or `ModuleScript` the function should create a closure out of.

### Example

```luau
local AnimateScript = game.Players.LocalPlayer.Character.Animate
print(getscriptclosure(AnimateScript)) -- Output: function 0x...

print(getscriptclosure(Instance.new("LocalScript"))) -- Throws an error
```

---

## getsenv

> [!NOTE]
> This is an implementation detail; as a regular scripter, you may ignore this!
>
> To implement this function, you should be finding the script's Lua state and returning its L->gt table; any other approach will result in our tests failing.
>
> The reason it should be implemented this way is because game devs can set their `script` global to `nil` and break other approaches.

Gives you the globals table of a running module/script (meaning all variables not defined as local in the script).
This should work with `LocalScript`, `ModuleScript`, and `Script` instances that have RunContext set to Client.

```luau
function getsenv(script: Script | LocalScript | ModuleScript): { [any]: any }
```

### Parameter

- `script` - The module/script the function gets the globals table of.

### Example

```luau
local scriptEnv = getsenv(game.Players.LocalPlayer.Character.Animate)
print(scriptEnv.onSwimming) -- Output: function 0x...

print(getsenv(Instance.new("LocalScript"))) -- Throws an error
```

---

## getscripts

> [!NOTE]
> This is an implementation detail; as a regular scripter, you may ignore this!
>
> This function is recommended to be implemented by using the `RBX::Lua::InstanceBridge::push` function.
> Returns a table of all instances that inherit the `BaseScript` class; this list should include `LocalScript`, `ModuleScript`, and any `Script` with the RunContext set to Client.
> This table should also include scripts or modules that are parented to nil.

```luau
function getscripts(): { Script | LocalScript | ModuleScript }
```

### Example

```luau
local DummyScript = Instance.new("LocalScript")

for _, ValueScript in pairs(getscripts()) do
    if (ValueScript == DummyScript) then
        print(`Found the script {DummyScript}`)
    end
end
```

---

## getrunningscripts

> [!NOTE]
> This is an implementation detail; as a regular scripter, you may ignore this!
>
> To check whether a module/script is running, you would use the same method you use for getting the script/module's Lua state in `getsenv`.
> 
> Returns a table of all instances like `getscripts` does but only returns modules and scripts that are currently running.
> Should also include all `Script` instances with RunContext set to Client that are running.

```luau
function getrunningscripts(): { LocalScript | ModuleScript | Script }
```

### Example

```luau
local DummyScript = game.Players.LocalPlayer.Character.Animate
local DummyScript2 = Instance.new("LocalScript")

for _, ValueScript in pairs(getrunningscripts()) do
    if (ValueScript == DummyScript) then
        print(`Found the running script {DummyScript}`)
    elseif (ValueScript == DummyScript2) then
        print("Should never happen")
    end
end
```

---

## getloadedmodules

Returns a table of all ModuleScripts that are currently running.

```luau
function getloadedmodules(): { ModuleScript }
```

### Example

```luau
local DummyModule = Instance.new("ModuleScript")
local DummyModule2 = Instance.new("ModuleScript")

pcall(require, DummyModule)

for _, ValueModule in pairs(getloadedmodules()) do
    if ValueModule == DummyModule then
        print(`Found the loaded {DummyModule}`)
    elseif ValueModule == DummyModule2 then
        print("Should never happen")
    end
end
```

---

## getcallingscript

> [!NOTE]
> This is an implementation detail; as a regular scripter, you may ignore this!
> 
> Do not make this function simply grab the script global from the current thread, this is easily breakable if the game devs set their script global to other or nil,
> instead use the instance from the Lua State userdata.

Returns the script associated with the current thread. This function is useful for determining which script is currently executing the Luau code.

```luau
function getcallingscript(): LocalScript | ModuleScript | Script
```

### Example

```luau
local Old; Old = hookmetamethod(game, "__index", function(t, k)
    if not checkcaller() then
        local callingScript = getcallingscript() -- Should return a foreign script

        warn("__index called from script:", callingScript:GetFullName())

        hookmetamethod(game, "__index", old)
        return old(t, k)
    end
end)

print(getcallingscript().Name)
```

---

## getthreadidentity

Returns the executor's current identity

```luau
function getthreadidentity(): number
```

### Example

```luau
setthreadidentity(3)
print(getthreadidentity()) -- Output: 3
setthreadidentity(7)
print(getthreadidentity()) -- Output: 7
```

---

## setthreadidentity

Sets the executor's identity and capabilities matching that identity

```luau
function setthreadidentity(input: number): ()
```

### Parameter

- `input` - The wanted identity to set to

### Examples

```luau
local Success, Result = pcall(function()
	setthreadidentity(0)
	return game.DataCost
end)

print(Success) -- Output: false
print(Result) -- Output: The current thread cannot read 'DataCost'...
```

```luau
local Success, Result = pcall(function()
	setthreadidentity(8)
	return Instance.new("Player")
end)

print(Success) -- Output: true
print(typeof(Result)) -- Output: Instance
print(Result.AccountAge) -- Output: 0
```


---

## loadstring

Compiles the given string, and returns it runnable in a function.

```luau
function loadstring<A...>(src: string, chunkname: string?): ((A...) -> any | nil, string?)
```

### Parameters

- `src` - The source to compile.
- `chunkname` - Name of the chunk.

### Examples

```luau
loadstring([[
    Placeholder = {"Example"}
]])()

print(Placeholder[1]) -- Output: Example
```

```luau
local Func, Err = loadstring("Example = ", "CustomName")

print(`{Func}, {Err}`) -- Output: nil, [string "CustomName"]:1: Expected identifier when parsing expression, got <eof>
```
