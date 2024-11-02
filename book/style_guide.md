# Best Practices

This page is a working document collecting syntax guidelines and best practices we have discovered so far.
The goal of this document is to eventually land on a canonical Nushell code style, but for now it is still work in
progress and _subject to change_.

There may be exceptions or extensions to these guidelines which are not yet documented.

As with all aspects of Nushell, we welcome discussion and contributions.

Keep in mind that these guidelines are not required in external repositories (not ours). You are, of course, free
to use any formatting style you wish. For Nushell documentation and repositories, we will eventually try to converge on
the recommendations here, but for now a variety of styles from multiple authors are present.

Note: In this document, escape sequences should not be interpreted literally, unless directed to do so. In other words,
treat something like `\n` like the new line character and not a literal slash followed by `n`.

## Formatting

### Basic Spacing

- A single space _IS RECOMMENDED_ before and after pipe (`|`) symbols, operators, commands, subcommands, their options and arguments, except before/after a token at the beginning/end of a line.
- Multiple consecutive spaces _ARE NOT RECOMMENDED_ unless they are part of a string or table literal.
- A single blank line _IS RECOMMENDED_ at the end of a file.
- Trailing whitespace at the end of a line is _NOT RECOMMENDED_
- Whitespace between a record key name and the key-value separator `:` _IS NOT RECOMMANDED_
- A single space following the key-value separator `:` _IS RECOMMENDED_

::: details Examples

```nu
# Correct
'Hello, Nushell! This is a gradient.' | ansi gradient --fgstart '0x40c9ff' --fgend '0xe81cff'

# Incorrect - Two spaces after the pipe symbol:
'Hello, Nushell! This is a gradient.' |  ansi gradient --fgstart '0x40c9ff' --fgend '0xe81cff'

# Correct (See "short-form" assignments and records below for more info)
let colors = {foreground: "blue, background: "white"}

# Incorrect - Extra space before separator
let colors = {foreground : "blue, background : "white"}

# Incorrect - No space after separator
let colors = {foreground:"blue, background:"white"}
```

:::

### String Quoting

Nushell, of course, offers an extensive set of string quoting options. In general, the following forms are preferred:

- For most strings, either single-quotes or double-quotes are acceptible, but it _IS RECOMMENDED_ that usage be consistent within any given file.

- Using single-quotes _IS RECOMMENDED WHEN_ there are literal backslash characters in the string and no backslashes should be interpreted as escapes. This is commonly the case in Regex expressions.

  ::: details Example

  ```nu
  # Correct - Using single quotes allows backslash to be used easily in a regex
  'String contains one or more digits 4' =~ '\d'

  # Incorrect - Double-quotes requires additional escaping of backslash
  'String contains one or more digits 4' =~ "\\d"
  ```

  :::

- Using bareword strings _IS RECOMMENDED FOR_ simple lists of strings. Otherwise, quoted strings _ARE RECOMMENDED WHEN_ strings in the list contain spaces or other characters that require additional quotes.

  ::: details Examples

  ```nu
  # Correct
  [item1 item2 item3]

  # Correct
  [
      'Ford F-150'
      'Peugeot 508'
      'Nissan Pulsar'
      'Mazda2'
  ]

  # Incorrect - "Placeholder" should be quoted for consistency
  [
      'Ford F-150'
      'Peugeot 508'
      'Nissan Pulsar'
      Mazda2
  ]

  # Incorrect
  # TODO
  ```

  :::

- Using bareword strings _IS RECOMMENDED FOR_ record key names when there are no special characers that require other quotes. Quotes _ARE RECOMMENDED_ around record values which are strings.

  ::: details Example

  ```nu
  # Correct
  {key1: "Value1", key2: null}

  # Correct - Parser requires A|V be quoted
  {"A|V": "5|10"}

  # Incorrect - "Value1" string is unquoted
  {key1: Value1, key2: null}

  # Incorrect - Key names are quoted
  {'key1': 'Value1', 'key2': null}
  ```

  :::

- Bareword strings _MAY_ be used for "strings masquerading as enums", at least until and unless Nushell implements enums.

  ::: details Example

  ```nu
  # Correct
  ls | where type = dir
  # Also correct
  ls | where type = "dir"
  ```

  :::

- Using raw strings _IS RECOMMENDED WHEN_ the text contains other quotation marks within it and interpolation is not needed.

  ::: details Example

  ```nu
  # Correct
  r#'We said, "Shouldn't you use a raw string here?"'#

  # Incorrect
  "We said, \"Shouldn't you use a raw string here?\""
  ```

  :::

### Short-form vs. Long-form

Many Nushell constructs should be formatted differently depending on their length or complexity. In general, the "short-form" will fit on one line, while the "long-form" will require multiple lines.

In general, in short-form:

- Opening-bracket characters such as `{`, `[`, and `(` _SHOULD_ be preceded by a space when not at the beginning of a line _AND_ not preceeded by another opening-bracket character.
- The corresponding closing-bracket characters _SHOULD_ be followed by a space when not at the end of a line _AND_ not followed by another closing-bracket character.

  ::: details Example

  ```nu
  # TODO
  (1 + 2) * 3
  # Incorrect
  (1 + 2 ) * 3
  ```

  :::

#### Lists

- Short-form _IS RECOMMENDED FOR_ simple, unnested lists:

  ```nu
  [item1, item2, item3]
  ```

  In short-form lists:

  - A space after the opening bracket `[` _IS NOT RECOMMENDED_
  - A space before the closing bracket `]` _IS NOT RECOMMENDED_
  - Delimiting list items using a comma, following by a space _IS RECOMMENDED_.

  ::: details Example

  ```nu
  # Correct - Space-delimited list items
  [Bash, Zsh, Fish, Nushell]

  # Incorrect - Space-delimited list
  # Incorrect - Also a space after the [ and before the ]
  [ Bash Zsh Fish Nushell ]
  ```

  :::

- Long-form _IS RECOMMENDED FOR_ lists with longer items, records, or other (nested) lists

  ::: details Example

  ```nu
  [
      {name: "Lila", status: "applied"}
      {name: "Luca", status: "processing"}
      {name: "Rania", status: "accepted"}
  ]
  ```

  :::

  In long-form lists:

  - A newline after the opening bracket `]` _IS RECOMMENDED_.
  - A newline before the closing bracket `]` _IS RECOMMENDED_.
  - Delimiting list-items using a line-break _IS RECOMMENDED_.
  - Indenting each list-item _IS RECOMMENDED_.
  - Aligning the closing bracket `]` with the first character of line with the matching open-bracket `[`

  ::: details Examples

  ```nu
  # Correct
  [
      'Ford F-150'
      'Peugeot 508'
      'Nissan Pulsar'
  ]

  # Incorrect - No identation
  [
  'Ford F-150'
  'Peugeot 508'
  'Nissan Pulsar'
  ]

  # Incorrect - Opening and closing bracket on same line as a list-item
  # Incorrect - Closing bracket not properly aligned
  [ 'Ford F-150'
    'Peugeot 508'
    'Nissan Pulsar' ]

  # Incorrect - List items are not delimited by newlines
  [
      'Ford F-150' 'Peugeot 508' 'Nissan Pulsar'
  ]
  ```

  :::

#### Records

Note: Some [Basic spacing](#basic-spacing) guidelines above apply to both short-form and long-form records

- Short-form _IS RECOMMENDED FOR_ short records, typically those with only one or two fields

  ```nu
  {key1: "Value1", key2: "Value2"}
  ```

  In short-form records:

  - A space after the opening bracket `{` _IS NOT RECOMMENDED_.
  - A space before the closing bracket `}` _IS NOT RECOMMENDED_.
  - Delimiting fields using a comma, followed by a single space, _IS RECOMMENDED_.

  ::: details Example

  ```nu
  # Correct
  {name: "Lila", status: "applied"}

  # Incorrect - Extra space after/before opening and closing brackets
  { name: "Lila", status: "applied" }

  # Incorrect - No comma in delimiter
  {name: "Lila" status: "applied"}

  # Incorrect - No space after comma in delimiter
  {name: "Lila",status: "applied"}
  ```

- Long-form _IS RECOMMENDED_ for longer records, either with more fields or with longer content.
- Long-form _IS ALLOWED_ in all cases for records.

  ```nu
  {
      name: "Lila"
      status: "applied"
      date-applied: 2026-10-11
      date-of-decision: null
  }

  # or even

  {
    foreground-color: "blue"
  }
  ```

  In long-form records:

  - A linebreak after the opening bracket `{` _IS RECOMMENDED_.
  - A linebreak before the closing bracket `}` _IS RECOMMENDED_.
  - Delimiting fields using a line-break _IS RECOMMENDED_.
  - Indenting each field _IS RECOMMENDED_.

  ::: details Examples

  ```nu
  # Correct
  {
    foreground: "white"
    background: "red"
  }

  # Incorrect - Missing linebreak after opening bracket
  # Incorrect - Missing linebreak before closing bracket
  { foreground: "white"
    background: "red" }

  # Incorrect - Fields are not indented
  {
  foreground: "white"
  background: "red"
  }
  ```

  :::

#### Closures

- Short-form _IS RECOMMENDED_ for short, simple (typically two or fewer commands) closures:

  ```nu
  ls | where {|file| $file.modified > ((date now) - 1day)}
  ```

  In short-form closures:

  - Using a space after the opening bracket _IS NOT RECOMMENDED WHEN_ there is a parameter list.
  - Using a space after the opening bracket _IS RECOMMENDED WHEN_ there is not a parameter list.
  - Using a space before the closing bracket _IS RECOMMENDED_
  - Using a single space after the `|parameter list|` _IS RECOMMENDED_
  - Defining an empty closure as `{||}` _IS RECOMMENDED_

  ::: details Example

  ```nu
  # Correct
  1..10 | reduce {|item acc| $item + $acc }

  # Correct
  1..10 | each { $in / 2 }

  # Incorrect - No space after parameter list
  1..10 | reduce {|item acc|$item + $acc }

  # Incorrect - No space before closing bracket
  1..10 | reduce {|item acc| $item + $acc}

  # Incorrect - Extra space before parameter list
  1..10 | reduce { |item acc| $item + $acc }

  # Incorrect - Missing spaces after/before brackets
  1..10 | each {$in / 2}
  ```

  :::

- Long-form _IS RECOMMENDED_ for longer or more complex closures:

  ```nu
  [3953 14212 14213] | each {|issue|
      let url = "https://api.github.com/repos/nushell/nushell/issues"
      | path join ($issue | into string)
      http get $url | get title
  }
  ```

  In long-form closures:

  - Using a space after the opening bracket _IS NOT RECOMMENDED WHEN_ there is a parameter list.
  - Using a line-break after the opening bracket _IS RECOMMENDED WHEN_ there is _NOT_ a parameter list.
  - Using a line-break after a `|parameter list|` _IS RECOMMENDED_
  - Using a line-break before the closing bracket _IS RECOMMENDED_
  - Indenting the "first" line of the closure (the line _after_ the opening bracket and parameter list) by one level _IS RECOMMENDED_
  - Aligning the closing bracket with the first charcter of the line on which the closure's opening bracket appeared _IS RECOMMENDED_
  - When a parameter list becomes too complex, it _IS RECOMMENED_ that each parameter be split onto its own line

  ::: details Examples

  ```nu
  # Correct
  [3953 14212 14213] | each {|issue|
      let url = "https://api.github.com/repos/nushell/nushell/issues"
      | path join ($issue | into string)
      http get $url | get title
  }

  # Incorrect - Space before the parameter list
  # Incorrect - Closing bracket is not aligned with first character of the first line
  [3953 14212 14213] | each { |issue|
      let url = "https://api.github.com/repos/nushell/nushell/issues"
      | path join ($issue | into string)
      http get $url | get title }
  ```

  :::

#### Blocks

- Short-form _IS RECOMMENDED_ for short, simple blocks where the total length does not exceed 80 characters.

  ```nu
  if $f.type == dir { "  n/a" } else { $f.size }
  ```

- Long-form _IS RECOMMENDED_ for longer or more complex blocks. Naturally, most all `def` blocks will be long-form:

  ```nu
  def "str replace-map" [ pattern_map:record, ...cellpath:any] {
      let table = $in
      $pattern_map | transpose -d pattern replacement
      | reduce -f $table {|pattern_replacement,table|
            let pattern = $'\$($pattern_replacement.pattern)'
            let replacement = $pattern_replacement.replacement
            $table | str replace -r -a $pattern $replacement ...$cellpath
        }
  }
  ```

#### Pipelines

- Short-form _IS RECOMMENDED WHEN_ the pipeline is less than ~80 characters and all its elements are short-form.

  ```nu
  ls | where size > 1MiB
  ```

  In short-form pipelines:

  - A single space on either size of the pipe symbol _IS RECOMMENDED_

  ::: details Example

  ```nu
  # Correct
  ls | where type == dir

  # Incorrect - No spaces
  ls|where type == dir

  # Incorrect - Extra spaces
  ls  |  where type == dir
  ```

  :::

- A pipeline with any long-form elements will, of course, require long form.
- Long-form _IS RECOMMENDED WHEN_ the pipeline is more than ~80 characters

  ```nu
  ls
  | update size {|f| if $f.type == dir { "  n/a" } else { $f.size }}
  | sort-by -c {|a b| ($a.name | str downcase) < ($b.name | str downcase)}
  | sort-by type
  | select name size
  ```

  In long-form pipelines:

  - Placing the continuing pipe symbol at the beginning of the line _IS RECOMMENDED_. This is also known as "wall of pipes" form.

    Note: At present, Nushell does not include a default binding for creating a new-line. It is recommended that you create a
    keybinding for either <kbd>Shift</kbd>+<kbd>Enter</kbd> or <kbd>Alt</kbd>+<kbd>Enter</kbd> (whichever is not bound by your terminal)
    to easily move to the next line when creating a multi-line pipeline.

  - Aligning the pipe symbol with the first character of the previous line _IS RECOMMENDED_.

  - A single space _IS RECOMMENDED_ after the pipe symbol on each line.

  - Short groups of related commands _MAY_ be grouped together in short-form on their own lines.

    In the following example,
    the `where` filter is grouped with its input, and "flattening the values" can be thought of as a single step in the pipeline.
    It's also possible to consider the text operations on the last two lines to be a single step, but again, this is an _OPTIONAL_
    recommentation left to the author's judgement.

    ```nu
    scope commands | where {
      get signatures
      | values | flatten
      | get syntax_shape
      | to text
      | str contains "block"
    }
    ```

#### Assignments

- Short-form _IS RECOMMEND_ for short, simple assignments.

  ```nu
  let type = "widget"
  # or
  let result = http get $endpoint
  ```

  In short-form assignments:

  - Using a subexpression for the assignment is _OPTIONAL WHEN_ it may improve readability in longer expressions.

  ::: details Example

  ```nu
  # Correct
  let foo = r#'Hello, World'#
  let id = try {$result.id | into int}
  let issue = http get https://api.github.com/repos/nushell/nushell/issues/3953
  let issue = (http get https://api.github.com/repos/nushell/nushell/issues/3953)

  # Incorrect - Unnecessary subexpression
  let foo = (r#'Hello, World'#)
  let id = (try {$result.id | into int})
  ```

  :::

- Long-form _IS RECOMMEND_ for longer or more complex assignments. Naturally, any assignment which includes a long-form element will also be long-form.

  ```nu
  let backup_names = (
    ls | where type == file
    | each {|file|
        let parts = $file.name | path parse
        let ext = if $parts.extension == "" { "" } else { $".($parts.extension)" }
        $"($parts.stem)--backup($ext)"
      }
  )
  ```

  In long-form assignments:

  - Using a multi-line subexpression for the assignment _IS RECOMMENDED_
  - Placing the opening `(` paranthesis on the line with the assignment operator _IS RECOMMENDED_
  - A linebreak immediately following the opening paranthesis _IS RECOMMENDED_
  - Increasing the indentation by one level starting with the next line after the `(` _IS RECOMMENDED_
  - Aligning the `)` closing paranthese with the first character of the line on which the `(` opening
    paranthese appeared _IS RECOMMENDED_

  ::: details Examples

  ```nu
  # Correct
  let backup_names = (
    ls | where type == file
    | each {|file|
        let parts = $file.name | path parse
        let ext = if $parts.extension == "" { "" } else { $".($parts.extension)" }
        $"($parts.stem)--backup($ext)"
      }
  )

  # Incorrect - No linebreak after (
  # Incorrect - Closing ) is not aligned properly
  # Incorrect - Subexpression is not indented one level
  let backup_names = ( ls | where type == file
  | each {|file|
      let parts = $file.name | path parse
      let ext = if $parts.extension == "" { "" } else { $".($parts.extension)" }
      $"($parts.stem)--backup($ext)"
    })
  ```

  :::

#### Subexpressions

- Short-form _IS RECOMMENDED_ for short, simple expressions
- Long-form _IS RECOMMENDED_ for longer or more complex expressions:

In-short form subexpressions:

- Whitespace before the opening `(` and after the closing `)` _IS RECOMMENDED_ when not at the beginning or end of the line
- Whitespace after the opening `(` or before the closing `)` _IS NOT RECOMMENDED_

For long-form subexpressions, see the [Assignments](#assignments) section above.

#### Closure and Block Parameters

- Short-form _IS RECOMMENDED WHEN_ there are short parameter lists. Typically, this will be determined by:

  1. The base number of parameters
  2. The number of parameters with type definitions
  3. The number of parameters with default values

  ```nu
  def add [a:int, b:int] {
    $a + $b
  }
  ```

  In short-form parameters:

  - Whitespace after the opening `|` (for closure parameters) or `[` (for block parameters) _IS NOT RECOMMENDED_.
  - Whitespace before the closing `|` or `]` _IS NOT RECOMMENDED_.
  - Separating parameters using a comma, followed by a single space, _IS RECOMMENDED_.

    ::: details Examples

    ```nu
    # Correct
    def add [a:int, b:int] {
      $a + $b
    }

    # Correct
    do {|a:int, b:int| $a + $b } 42 42

    # Incorrect - Missing comma between parameters
    def add [a:int b:int] {
      $a + $b
    }

    # Incorrect - Extra whitespace after and before the opening and closing |
    do {| a:int, b:int | $a + $b } 42 42
    ```

    :::

- Long-form _IS RECOMMENDED WHEN_ there are type definitions or default values.
- Long-form _IS REQUIRED_ by the parser when there are documentation comments for parameters.

## Naming Conventions

### Abbreviations and Acronyms

Abbreviations and acroynms _SHOULD_ only be used when they are well-known or commonly userd.

::: details Example

```nu
# Correct
query-user --id 123

# Incorrect
qry-usr --id 123
```

:::

### Case

#### Commands

Kebab-case _IS RECOMMENDED_ for command names with multiple-words.

::: details Example

```nu
# Correct
fetch-user --id 123

# Incorrect
fetch_user --id 123
fetchUser --id 123
```

:::

See also [Naming Commands](custom_commands.md#naming-commands).

#### Sub-Commands

Sub commands are commands that are logically grouped under a parent command and separated by a space.
**It's recommended to** use kebab-case for the sub-command name.

Correct:

```nu
date now

date list-timezone

def "login basic-auth" [username: string password: string] {
    # ...
}
```

See also [Naming Sub-Commands](custom_commands.md#subcommands).

#### Flags

**It's recommended to** use kebab-case for flag names.

Correct:

```nu
def greet [name: string, --all-caps] {
    # ...
}
```

Incorrect:

```nu
def greet [name: string, --all_caps] {
    # ...
}
```

::: tip
Notice that the name used to access the flag is accessed by replacing the dash with an underscore in the resulting
variable name.

See [Flags](custom_commands.md#flags).
:::

#### Variables and Command Parameters

**It's recommended to** use snake_case for variable names, including command parameters.

Correct:

```nu
let user_id = 123

def fetch-user [user_id: int] {
  # ...
}
```

Incorrect:

```nu
let user-id = 123
let userId = 123

def fetch-user [user-id: int] {
  # ...
}
```

#### Environment Variables

**It's recommended to** use SCREAMING_SNAKE_CASE for environment variable names.

Correct:

```nu
$env.ENVIRONMENT_CODE = "prod"

$env.APP_VERSION = "1.0.0"
```

Incorrect:

```nu
$env.ENVIRONMENT-CODE = "prod"

$env.app_version = "1.0.0"
```

## Options and Parameters of Custom Commands

- **It's recommended to** keep count of all positional parameters less than or equal to 2, for remaining inputs use options. Assume that command can expect source and destination parameter, like `mv`: source and target file or directory.
- **It's recommended to** use positional parameters unless they can't be used due to rules listed here or technical restrictions.
  For instance, when there are several kinds of optional parameters (but at least one parameter should be provided)
  use options. Great example of this is `ansi gradient` command where at least foreground or background must be passed.
- **It's recommended to** provide both long and short options.

## Documentation

- **It's recommended to** provide documentation for all exported entities (like custom commands) and their
  inputs (like custom command parameters and options).
