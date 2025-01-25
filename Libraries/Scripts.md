# Scripts

The **Script** library provides functions that access to script environments and internal state.

---

## getscriptbytecode

> [!NOTE]
> This function should return nil if the script has no bytecode.
> We encourage this behavior, as it's easier for people to check for nil, rather than each executor having its own output.

```luau
function getscriptbytecode(script: LocalScript | ModuleScript | Script): string | nil
```

### Parameter

- `script` - The `Script`, `LocalScript` or `ModuleScript` the bytecode should be obtained from.

### Example

```luau
local AnimateScriptBytecode = getscriptbytecode(game.Players.LocalPlayer.Character.Animate)
print(AnimateScriptBytecode) -- Returns a string with the bytecode.

print(getscriptbytecode(Instance.new("LocalScript"))) -- Output: nil
```

---

## getscripthash

> [!NOTE]
> This is an implementation detail; as a regular scripter, you may ignore this!
> Please hash the compressed and encrypted bytecode; do not decrypt and decompress the bytecode and then hash it!

Returns a `SHA384` hash represented in hex of the module/script's bytecode. This function should work with `LocalScript`, `ModuleScript`, and `Script` instances that have RunContext set to Client.

```luau
function getscripthash(script: LocalScript | ModuleScript | Script): string
```

### Parameter

- `script` - The module/script the function should obtain a hash of.

### Example

```luau
local scriptHash = getscripthash(game.Players.LocalPlayer.Character.Animate)
print(scriptHash) -- Should return a non-changing SHA384 hash in hex representation
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

This function creates a new closure (function) from the module/script's bytecode. The game does not use the function you will get; it's mostly used it to retrieve constants.

This should work with `LocalScript`, `ModuleScript`, and `Script` instances that have RunContext set to Client.

> [!NOTE]
> This function should return nil, an empty string or error if the script has no bytecode.
> We encourage this behavior, as it's easier for people to check for nil or an empty string, rather than each executor having its own output.

### Parameter

- `script` - The module/script the function should create a closure out of.

### Example

```luau
local AnimScript = game.Players.LocalPlayer.Character.Animate
print(type(getscriptclosure(AnimScript))) -- Output: function

print(getscriptclosure(Instance.new("LocalScript"))) -- Output: nil, "" or error
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
function getsenv(script: LocalScript | ModuleScript | Script): { [string]: any }
```

### Parameter

- `script` - The module/script the function gets the globals table of.

### Examples

```luau
local scriptEnv = getsenv(game.Players.LocalPlayer.Character.Animate)
print(type(scriptEnv.onSwimming)) -- Output: function

print(getsenv(Instance.new("LocalScript"))) -- Should error with something along the lines of "script not running"
```

---

## getscripts

> [!NOTE]
> This is an implementation detail; as a regular scripter, you may ignore this!
>
> This function is recommended to be implemented by using the `RBX::Lua::InstanceBridge::Push` function.

Returns a table of all instances that inherit the `BaseScript` class; this list should include `LocalScript`, `ModuleScript`, and any `Script` with the RunContext set to Client.
This table should also include scripts or modules that are parented to nil.

```luau
function getscripts(): { [number]: LocalScript | ModuleScript | Script }
```

### Example

```luau
local DummyScript = Instance.new("LocalScript")

for indexScript, valueScript in pairs(getscripts()) do
    if valueScript == DummyScript then
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

Returns a table of all instances like `getscripts` does but only returns modules and scripts that are currently running.
Should also include all `Script` instances with RunContext set to Client that are running.

```luau
function getrunningscripts(): { [number]: LocalScript | ModuleScript | Script }
```

### Example

```luau
local DummyScript = Instance.new("LocalScript", game.Players.LocalPlayer.Character)
local DummyScript2 = Instance.new("LocalScript")

pcall(require, DummyScript)

for indexScript, valueScript in pairs(getrunningscripts()) do
    if valueScript == DummyScript then
        print(`Found the running script {DummyScript}`)
    elseif valueScript == DummyScript2 then
        print(`Found the non-running script: {DummyScript2}`)
    end
end
```

---

## getloadedmodules

Returns a table of all ModuleScripts that are currently running.

```luau
function getloadedmodules(): { [number]: ModuleScript }
```

### Example

```luau
local DummyModule = Instance.new("ModuleScript")
local DummyModule2 = Instance.new("ModuleScript")

pcall(require, DummyModule)

for indexModule, valueModule in pairs(getloadedmodules()) do
    if valueModule == DummyModule then
        print(`Found the loaded {DummyModule}`)
    elseif valueModule == DummyModule2 then
        print(`Found the unloaded module: {DummyModule2}`)
    end
end
```

---

## getcallingscript

Returns the script associated with the current thread. This function is useful for determining which script is currently executing the Luau code.

```luau
function getcallingscript(): (LocalScript | ModuleScript | Script)
```

### Example

```luau
local _
_ = hookmetamethod(game, "__index", function(t, k)
    if not checkcaller() then
        local callingScript = getcallingscript() -- Should return a foreign script

        warn("__index called from script:", callingScript:GetFullName())

        hookmetamethod(game, "__index", _)
        return _(t, k)
    end
end)

local currentScript = getcallingscript()
print(currentScript.Name) -- Prints the name of the script
```
