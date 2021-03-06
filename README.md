# Crimpex
Creating an md5 hash of a number, string, array, or hash in Elixir

Documentation can be generated with [ExDoc](https://github.com/elixir-lang/ex_doc)
and published on [HexDocs](https://hexdocs.pm). Once published, the docs can
be found at [https://hexdocs.pm/crimpex](https://hexdocs.pm/crimpex).

[![Build Status](https://travis-ci.org/)](https://travis-ci.org/)

Crimpex is an implementation of [Crimp](https://github.com/BBC-News/crimp) in Elixir, however as of the initial 0.1 there is a known difference in the outputs between the original Ruby implementation and this Elixir implementation when using nested hash inputs.

Please see the [bbc-news/crimp](https://github.com/BBC-News/crimp) repo for more details.

## Implementations

[![Crimp Lua](https://img.shields.io/badge/Crimp-Lua-00007C.svg)](https://github.com/bbc-news/crimpua)
[![Crimp Ruby](https://img.shields.io/badge/Crimp-Ruby-CC342D.svg)](https://github.com/bbc-news/crimp)

## Installation

If [available in Hex](https://hex.pm/docs/publish), the package can be installed
by adding `crimpex` to your list of dependencies in `mix.exs`:

```elixir
def deps do
  [
    {:crimpex, "~> 0.1.0"}
  ]
end
```

## Usage

```elixir
Crimpex.signature({a = {b = 1}}) -- prints "ac13c15d07e5fa3992fc6b15113db900"
```

## Implementation Details

Under the hood Crimpex annotates the passed data structure to a nested array of primitives (strings, numbers, booleans, nils, etc.) and a single byte to indicate the type of the primitive:

|  Type   | Byte |
|   :-:   |  :-: |
| String  |  `S` |
| Number  |  `N` |
| Boolean |  `B` |
| nil     |  `_` |
| Array   |  `A` |
| Hash    |  `H` |

You can verify it using the `#notation` function:

```elixir
Notation.notate(%{a: 1})
"1NaSAH"
```

Before signing Crimpex, uses the `#notation` function to transform the return value of a map to a string.

```elixir
Notation.notate(%{ a: %{ b: "c" } })
"aSbScSAHAH"
```

Please note the List and Map keys are sorted before signing.

```elixir
Notation.notate([3, 1, 2])
"1N2N3NA"
```

key/value tuples get sorted as well.

```elixir
Notation.notate([ b: 3 ])
"3NbSAH"
```

## Changelog

### 0.1.1

Fix for mixed encodings of strings inside lists

### 0.1.0

Initial release.

## Testing

For testing we use ExUnit built into Elixir itself

```sh
mix test
```

The project has also not been pushed to Hex as of yet so will need to be added as a git dependency.