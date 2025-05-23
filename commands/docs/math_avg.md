---
title: math avg
categories: |
  math
version: 0.104.0
math: |
  Returns the average of a list of numbers.
usage: |
  Returns the average of a list of numbers.
editLink: false
contributors: false
---
<!-- This file is automatically generated. Please edit the command in https://github.com/nushell/nushell instead. -->

# `math avg` for [math](/commands/categories/math.md)

<div class='command-title'>Returns the average of a list of numbers.</div>

## Signature

```> math avg {flags} ```


## Input/output types:

| input          | output   |
| -------------- | -------- |
| duration       | duration |
| filesize       | filesize |
| list\<duration\> | duration |
| list\<filesize\> | filesize |
| list\<number\>   | number   |
| number         | number   |
| range          | number   |
| record         | record   |
| table          | record   |
## Examples

Compute the average of a list of numbers
```nu
> [-50 100.0 25] | math avg
25
```

Compute the average of a list of durations
```nu
> [2sec 1min] | math avg
31sec
```

Compute the average of each column in a table
```nu
> [[a b]; [1 2] [3 4]] | math avg
╭───┬───╮
│ a │ 2 │
│ b │ 3 │
╰───┴───╯
```
