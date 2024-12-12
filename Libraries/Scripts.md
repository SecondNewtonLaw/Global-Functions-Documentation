# Scripts
The **Script** library provides functions that access to script environments and internal state.

---

## getscripthash

> [!NOTE]
> This is an implementation detail, as a regular scripter you may ignore this!
> 
> Please hash the compressed and encrypted bytecode, do not decrypt and decompress bytecode and then hash it, you will fail our tests!


Returns an base64 encoded string of the hashed script bytecode using `SHA384`, this function should support all inheritants of `BaseScript` class
```luau
getscripthash(script: BaseScript): (string)
```

### Parameters
- `script` - The script the function should obtain hash of.

### Examples

```luau
local scriptHash = getscripthash(game.Players.LocalPlayer.Character.Animate)
print(scriptHash) -- Should return an non-changing SHA384 hash
```
