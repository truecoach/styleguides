# Elixir Style Guides

## Table of Contents
1.  [Whitespace](#whitespace)
1.  [Syntax](#syntax)
    1. [Functions](#functions)
    1. [Conditional Expressions](#conditional-expressions)
    1. [Pipelines](#pipelines)
    1. [Modules](#modules)
1.  [Naming](#naming)

## Whitespace

* Use two-space indentation and spaces around operators, after commas, colons and semicolons.

```
# bad
answer=1-3

def square(a) when a>0,do: a * a

Enum.map(["cat","dog"],fn(value)->IO.puts(value)end)

# good
answer = 1 - 3

def square(a) when a > 0, do: a * a

Enum.map(["cat", "dog"], fn(value) -> IO.puts(value) end)
```

* **Do not** put spaces around matched pairs like brackets, parentheses, etc.

```
# bad
{ :ok, result } = { :ok, 13 }

[ a, b, [ c ] ] = [ 1, 2, [ 3 ] ]

%{ favorite: color } = %{ favorite: "red" }

def iterate( [ head | tail ] ), do: iterate(tail)

# good
{:ok, result} = {:ok, 13}

[a, b, [c]] = [1, 2, [3]]

%{favorite: color} = %{favorite: "red"}

def iterate([head | tail]), do: iterate(tail)
```

* Use empty lines between function `def`s...

```
def mark_as_read(email) do
  # ...
end

def mark_as_unread(email) do
  # ...
end

def send, do: # ...
```

* ...but put function `def`s that match for the same function together.

```
def sum(nil), do: {:error, "No Value"}
def sum([]), do: 0
def sum([head | tail]) do
  head + sum(tail)
end
```


## Syntax

### Functions

* Use parentheses when there are arguments in a function definition. **Do not** use parentheses when there
  are no arguments.

```
# bad
def greet name message do
  ...
end

# bad
def respond() do
  ...
end

# good
def greet(name, message) do
  ...
end

# good
def respond do
  ...
end
```
...however, favor parenthesis for null arity function calls, when the function is in the local scope.


```elixir
# bad
result = operation

# good
result = operation()
```

* Use `do:` for single line function definitions.

```
# bad
def iterate([]) do
 :ok
end

# good
def iterate([]), do: :ok
```

* Use parentheses in function calls when passing parameters, especially inside a pipeline.

```
# bad
convert_to_celsius 32

# REALLY bad
# parses as convert_to_celsius(32 |> format)
convert_to_celsius 32 |> format

# good
convert_to_celsius(32)

# good
convert_to_celsius(32) |> format
```

### Conditional Expressions

* Never use `do:` for multi-line `if/unless` statements. Instead, use
  the `do ... end` format.

```
# bad
if price < 100 do:
  # multiple
  # lines
  # of code

# good
if price > 100 do
  # multiple
  # lines
  # of code
end
```

* Use `do:` for single line `if/unless` statements.

```
if price == 100, do: # do stuff
```

* Use `true` or an atom as the last (catch-all) condition of a `cond` statement
(An atom evaluates to a truthy value). Suggested atoms are `:else`, `:default` or `:otherwise`.

```
cond do
  1 + 1 == 4 ->
    "Close, but no"
  1 + 1 == 3 ->
    "Getting warmer"
  true ->
    "Sure"
end

cond do
  1 + 1 == 4 ->
    "Close, but no"
  1 + 1 == 3 ->
    "Getting warmer"
  :else ->
    "Sure"
end
```

### Pipelines

* Use the pipeline operator, `|>`, to chain **multiple** functions together.

```
# bad
some_string |> String.downcase

# good
String.downcase(some_string)
```

* Use a single level of indentation for multi-line pipelines.

```
# bad
some_string
  |> String.downcase
  |> String.strip

# good
some_string
|> String.downcase
|> String.strip
```

* Use **bare** variables in the first part of a function chain.

```
# bad
String.strip(some_string)
|> String.downcase
|> String.codepoints

# good
some_string
|> String.strip
|> String.downcase
|> String.codepoints
```

* Always start a new line for each pipeline in a function chain.

```
# bad
some_string |> String.strip |> String.downcase

# good
some_string
|> String.strip
|> String.downcase
|> String.codepoints
```

* When assigning the result of a pipeline to a variable, the `variable =` should stand on its own, with an indented pipeline below.

# bad
result = some_string
|> String.strip
|> String.downcase
|> String.codepoints

# good
result =
  some_string
  |> String.strip
  |> String.downcase
  |> String.codepoints

### Modules

* Use one module per file unless the module is only used internally by
another module (such as a test).

* No newline after defmodule, starting with `@moduledoc`, followed by `use`'s, `import`'s, and `alias`'s

```
defmodule Exam do
  @moduledoc """
  Short one line summary, ending in period.

  More details as necessary after newline.
  """

  use GenServer
  import App.Router.Helpers
  alias App.Repo

  def start_link do
    ...
  end
end
```

## Naming

* Use `snake_case` for atoms, functions and variables.

* Use `CamelCase` for modules.

* Use a question mark rather than a leading `is_` when naming a function
  that returns a boolean value.

```
# bad
def is_acid?(ph) do
  # ph > 7
end

# good
def acid?(ph) do
  # ph < 7
end
```
