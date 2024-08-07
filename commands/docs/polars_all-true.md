---
title: polars all-true
categories: |
  dataframe
version: 0.96.0
dataframe: |
  Returns true if all values are true.
usage: |
  Returns true if all values are true.
---
<!-- This file is automatically generated. Please edit the command in https://github.com/nushell/nushell instead. -->

# `polars all-true` for [dataframe](/commands/categories/dataframe.md)

<div class='command-title'>Returns true if all values are true.</div>

## Signature

```> polars all-true {flags} ```


## Input/output types:

| input | output |
| ----- | ------ |
| any   | any    |

## Examples

Returns true if all values are true
```nu
> [true true true] | polars into-df | polars all-true
╭───┬──────────╮
│ # │ all_true │
├───┼──────────┤
│ 0 │ true     │
╰───┴──────────╯

```

Checks the result from a comparison
```nu
> let s = ([5 6 2 8] | polars into-df);
    let res = ($s > 9);
    $res | polars all-true
╭───┬──────────╮
│ # │ all_true │
├───┼──────────┤
│ 0 │ false    │
╰───┴──────────╯

```
