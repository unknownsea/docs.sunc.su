# `crypt.base64encode`

`#!luau crypt.base64encode` encodes a string with [Base64](https://en.wikipedia.org/wiki/Base64) encoding.

```luau
function crypt.base64encode(data: string): string
```

## Parameters

| Parameter | Description |
|-----------|-------------|
| `#!luau data` | The string to encode. |

---

## Example

```luau
print(crypt.base64encode("DummyString\0\2")) -- Output: RHVtbXlTdHJpbmcAAg==
```
