# `request`

`#!luau request` sends a [HTTP request](https://en.wikipedia.org/wiki/HTTP) to the given URL using the provided configuration table. It yields until the request is complete and returns a structured response.

```luau
type RequestOptions = {
    Url: string,
    Method: "GET" | "POST" | "PUT" | "PATCH" | "DELETE",
    Body: string?,
    Headers: { [string]: string }?,
    Cookies: { [string]: string }?
}

type Response = {
    Body: string,
    StatusCode: number,
    StatusMessage: string,
    Success: boolean,
    Headers: { [string]: string }
}

function request(options: RequestOptions): Response
```

## Parameters

| Parameter         | Description                                 |
|-------------------|---------------------------------------------|
| `#!luau options`   | A table of fields defining the HTTP request. |

### `#!luau RequestOptions` Fields

| Field         | Type         | Description                                                |
|---------------|--------------|------------------------------------------------------------|
| `#!luau Url`     | `string`     | The target URL.                                             |
| `#!luau Method`  | `string`     | The HTTP [method](https://en.wikipedia.org/wiki/HTTP#Request_methods) (`GET`, `POST`, `PATCH`, or `PUT`).         |
| `#!luau Body`    | `string?`    | (Optional) The request payload.                             |
| `#!luau Headers` | `{ [string]: string }?`     | (Optional) Dictionary of HTTP [headers](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields).                      |
| `#!luau Cookies` | `{ [string]: string }?`     | (Optional) Dictionary of [cookies](https://en.wikipedia.org/wiki/HTTP_cookie).                           |

### `#!luau Response` Fields

| Field              | Type       | Description                              |
|--------------------|------------|------------------------------------------|
| `#!luau Body`          | `string`           | The returned response body.                                                                |
| `#!luau StatusCode`    | `number`           | The numeric HTTP [status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).   |
| `#!luau StatusMessage` | `string`           | The human-readable status description.                                                     |
| `#!luau Success`       | `boolean`          | Whether the request was successful.                                                        |
| `#!luau Headers`       | `{ [string]: string }`    | Dictionary of response [headers](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields).|

### Automatically added Headers

Executors will attach this unique header automatically:

<!-- recommendation: standardise headers to be X-User-Identifier and X-Fingerprint -->
<!-- the 'X' prefix is standard for custom HTTP headers, as defined in RFC822 (https://datatracker.ietf.org/doc/html/rfc822) -->

| Header                     | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| `PREFIX-User-Identifier`   | Unique user ID that stays consistent across devices for the same user.     |
| `PREFIX-Fingerprint`       | Hardware-bound identifier (HWID) of the client's machine.                           |
| [`User-Agent`](https://en.wikipedia.org/wiki/User-Agent_header)           | Executor name and version string.                                          |

---

## Examples

### Example 1

```luau title="Basic GET request with fingerprint lookup" linenums="1"
local response = request({
    Url = "http://httpbin.org/get",
    Method = "GET",
})

local decoded = game:GetService("HttpService"):JSONDecode(response.Body)
local retrieved_fingerprint

for key in pairs(decoded.headers) do
    if key:match("Fingerprint") then
        retrieved_fingerprint = key
        break
    end
end

print(response.StatusCode)         -- Output: 200
print(response.Success)            -- Output: true
print(retrieved_fingerprint)        -- Output: PREFIX-Fingerprint
```

### Example 2

```luau title="Basic POST request with payload" linenums="1"
local response = request({
    Url = "http://httpbin.org/post",
    Method = "POST",
    Body = "Example"
})

print(response.StatusMessage)                               -- Output: OK
print(response.StatusCode)                                  -- Output: 200
print(game:GetService("HttpService"):JSONDecode(response.Body).data) -- Output: Example
```
