# Scripts
The **Script** library provides functions that access to script environments and internal state.

---

## getscripthash

> [!NOTE]
> This is an implementation detail, as a regular scripter you may ignore this!
> 
> Please hash the compressed and encrypted bytecode, do not decrypt and decompress bytecode and then hash it, you will fail our tests!
> Additionally, this will ensure compability between executors who do practice sUNC.

Returns a `SHA384` hash of the script bytecode encoded in base64. This function should work with all classes that inherit from `BaseScript`.
```luau
getscripthash(script: BaseScript): string
```

### Parameters
- `script` - The script the function should obtain hash of.

### Examples

```luau
local scriptHash = getscripthash(game.Players.LocalPlayer.Character.Animate)
print(scriptHash) -- Should return an non-changing SHA384 hash
```
