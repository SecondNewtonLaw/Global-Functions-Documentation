# Scripts
The **Script** library provides functions that access to script environments and internal state.

---

## getscriptbytecode

Returns the module/script's decompressed and decrypted bytecode to be used for decompiling. This function should work with `LocalScript`, `ModuleScript`, and `Script` instances that have RunContext set to Client.
```luau
getscriptbytecode(script: LocalScript | ModuleScript | Script): string
```

### Parameter
- `script` - The module/script the bytecode should be obtained from.

### Examples
```luau
local scriptBytecode = getscriptbytecode(game.Players.LocalPlayer.Character.Animate)
print(scriptBytecode) -- Should return a string with the bytecode
```

```luau
-- Should error due to having no bytecode or being unable to decompress the bytecode
print(getscriptbytecode(Instance.new("LocalScript")))
```
---

## getscripthash

> [!NOTE]
> This is an implementation detail; as a regular scripter, you may ignore this!
> 
> Please hash the compressed and encrypted bytecode; do not decrypt and decompress the bytecode and then hash it!
> Additionally, this will ensure compatibility between executors who do practice sUNC.

Returns a `SHA384` hash represented in hex of the module/script's bytecode. This function should work with `LocalScript`, `ModuleScript`, and `Script` instances that have RunContext set to Client.
```luau
getscripthash(script: LocalScript | ModuleScript | Script): string
```

### Parameters
- `script` - The module/script the function should obtain a hash of.

### Examples

```luau
local scriptHash = getscripthash(game.Players.LocalPlayer.Character.Animate)
print(scriptHash) -- Should return an non-changing SHA384 hash in hex
```

```luau
print(getscripthash(Instance.new("LocalScript"))) -- Should error due to having no bytecode
```
---

## getscriptclosure

> [!NOTE]
> This is an implementation detail; as a regular scripter, you may ignore this!
>
> The closure you return needs to be callable (meaning it can be called without erroring), or you will fail our tests! This means the closure needs to have properly set capabilities and identity and a proper environment with the script global!
> 
> For the environment, make a new table having __index set to the table L->global->mainthread->gt, and for the script global, just push the script instance and set it on the table.
>
> For capabilities, make sure to check if the script/module is a RobloxScript and set identity and capabilities accordingly.

This function creates a new closure (function) from the module/script's bytecode. The game does not use the function you will get; you should primarily use it to retrieve constants.

This should work with `LocalScript`, `ModuleScript`, and `Script` instances that have RunContext set to Client.
```luau
getscriptclosure(script: LocalScript | ModuleScript | Script): function
```

### Parameters
- `script` - The module/script the function should create a closure out of.

### Examples
```luau
local testScript = game.Players.LocalPlayer.Character.Animate
print(getscriptclosure(testScript)) -- Returns an function
getscriptclosure(testScript)() -- Shouldn't error
```

```luau
print(getscriptclosure(Instance.new("LocalScript"))) -- Should error for not having any bytecode
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
getsenv(script: LocalScript | ModuleScript | Script): { [string]: any }
```

### Parameters
- `script` - The module/script the function gets the globals table of.

### Examples
```luau
local scriptEnv = getsenv(game.Players.LocalPlayer.Character.Animate)
print(scriptEnv) -- Should return a table
```

```luau
print(getsenv(Instance.new("LocalScript"))) -- Should error with "This script isn't running"
```
---

## getscripts

> [!NOTE]
> This is an implementation detail; as a regular scripter, you may ignore this!
>
> This function should be implemented by using the `RBX::Lua::InstanceBridge::Push` function; any other implementation will fail our tests.

Returns a table of all instances that inherit the `BaseScript` class; this list should include `LocalScript`, `ModuleScript`, and any `Script` with the RunContext set to Client.
This table should also include scripts or modules that are parented to nil.
```luau
getscripts(): { LocalScript | ModuleScript | Script }
```

### Examples
```luau
local allScriptsAndModules = getscripts()
for _, v in next, allScriptsAndModules do
  print(v.ClassName) -- Should print LocalScript, ModuleScript or Script
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
getrunningscripts(): { LocalScript | ModuleScript | Script }
```

### Examples
```luau
local allRunningScriptsAndModules = getrunningscripts()
for _, v in next, allRunningScriptsAndModules do
  print(v.ClassName) -- Should print LocalScript, ModuleScript or Script
end
```
---

## getloadedmodules

Returns a table of all ModuleScripts that are currently running.
```luau
getloadedmodules(): { ModuleScript }
```
