# `filtergc`

`#!luau filtergc` allows you to retrieve specific garbage-collected values from Luau's memory, using fine-tuned filters.

This function is most often used to find game-defined functions or internal tables by matching constants, keys, metatables, and more. It behaves similarly to [`#!luau getgc`](../getgc.md), but offers more control over what gets returned.

```luau
function filtergc(
    filterType: "function" | "table",
    filterOptions: FunctionFilterOptions | TableFilterOptions,
    returnOne: boolean
):
    (...any) -> (...any) | { [any]: any } | { (...any) -> (...any) | { [any]: any } }
```

---

## Parameters

| Parameter         | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| `#!luau filterType`   | The type of value to search for.         |
| `#!luau filterOptions` | A set of rules used to match functions or tables. [See below.](#filter-option-types)               |
| `#!luau returnOne?`   | If `#!luau true`, returns the first match, instead of a table of matches.         |

---

## Filter option types

Each filter type has its own valid fields:

- See [Function Filters](./FunctionFilterOptions.md) for matching functions by name, constants, upvalues, and more.
- See [Table Filters](./TableFilterOptions.md) for matching tables by keys, values, metatables, and more.

---

## Notes

- Garbage-collected values must still be referenced by a live thread to be found.
- Some filters (like `Constants` or `Hash`) do not apply to C functions.
