# Scripts
The **Script** library provides functions that access to script environments and internal state.

---

## getscriptbytecode

Returns the module/script's decompressed and decrypted bytecode to be used for decompiling. This function should work with `LocalScript`, `ModuleScript` and `Script` instances that have RunContext set to Client.
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

## getscripthash

> [!NOTE]
> This is an implementation detail, as a regular scripter you may ignore this!
> 
> Please hash the compressed and encrypted bytecode, do not decrypt and decompress bytecode and then hash it, you will fail our tests!
> Additionally, this will ensure compability between executors who do practice sUNC.

Returns a `SHA384` hash of the module/script bytecode encoded in base64. This function should work with `LocalScript`, `ModuleScript` and `Script` instances that have RunContext set to Client.
```luau
getscripthash(script: LocalScript | ModuleScript | Script): string
```

### Parameters
- `script` - The module/script the function should obtain hash of.

### Examples

```luau
local scriptHash = getscripthash(game.Players.LocalPlayer.Character.Animate)
print(scriptHash) -- Should return an non-changing SHA384 hash
```

```luau
print(getscripthash(Instance.new("LocalScript"))) -- Should error due to having no bytecode
```
