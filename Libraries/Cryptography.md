# Cryptography

Functions for encrypting and decrypting data.

---

## crypt.base64encode

Encodes a string with Base64 encoding.

```luau
function crypt.base64encode(data: string): string
```

### Parameters

- `data` - The data to encode.

### Example

```luau
local base64 = crypt.base64encode("Hello, World!")
local raw = crypt.base64decode(base64)

print(base64) --> SGVsbG8sIFdvcmxkIQ==
print(raw) --> Hello, World!
```
