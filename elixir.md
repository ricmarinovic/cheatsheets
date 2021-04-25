---
title: Elixir
category: Elixir
tags: [Featured, WIP]
updated: 2021-04-25
weight: -5
---

Intro
-------------------------------------

Control flow
-------------------------------------

### Comprehensions

```elixir
for x <- [1, 2, 3], y <- [2, 3, 4], rem(y, 2) == 0 do
  x * y
end
#=> [2, 4, 4, 8, 6, 12]
```

```elixir
for <<r::8, g::8, b::8 <- pixels>>, do: {r, g, b}
```


#### into

```elixir
for <<c <- " hello world ">>, c != ?\s, into: "", do: <<c>>
#=> "helloworld"
```


#### uniq

```elixir
for x <- [1, 1, 2, 3], uniq: true, do: x * 2
#=> [2, 4, 6]
```

#### reduce

```elixir
for <<x <- "AbCabCABc">>, x in ?a..?z, reduce: %{} do
  acc -> Map.update(acc, <<x>>, 1, & &1 + 1)
end
#=> %{"a" => 1, "b" => 2, "c" => 1}
```

### `with`

```elixir
with {:ok, map} <- do_something do
  on_success
end
```

Enum
-------------------------------------

Stream
-------------------------------------

### cycle

```elixir
Stream.cycle([1, 2, 3])
|> Enum.take(5)
#=> [1, 2, 3, 4, 5]
```

### repeatedly

```elixir
Stream.repeatedly(&:rand.uniform/0)
|> Enum.take(3)
#=> [0.6148287886796542, 0.3662689171313852, 0.6596815231166095]
```

### iterate

```elixir
Stream.iterate(0, &(&1 + 1))
|> Enum.take(5)
#=> [0, 1, 2, 3, 4]
```

### unfold

```elixir
Stream.unfold(5, fn
  0 -> nil
  n -> {n, n - 1}
end) |> Enum.to_list()
#=>[5, 4, 3, 2, 1]
Stream.unfold({0, 1}, fn {f1, f2} -> {f1, {f2, f1 + f2}} end) |> Enum.take(7)
#=> [0, 1, 1, 2, 3, 5, 8]
```

### resource

```elixir
Stream.resource(
  fn -> File.open!("sample") end,
  fn file ->
    case IO.read(file, :line) do
      data when is_binary(data) -> {[data], file}
      _ -> {:halt, file}
    end,
    fn file -> File.close(file) end
)
```


Strings
-------------------------------------

### Binaries

```elixir
<< 12.3::big-signed-float-size(32) >>
<< a::binary-size(32) >>
```


#### Options

- binary
- bitstring
- bits
- bytes
- float
- integer
- utf8
- utf16
- utf32

#### Qualifiers

- `size(n)` (in bits)
- `signed` or `unsigned`
- endianess: `big`, `little` or `native`


### Using IOLists

```elixir
list = ["abc", "def"]
IO.iodata_to_binary(list)
#=> "abcdef"
```

- [Elixir and IO Lists](https://www.bignerdranch.com/blog/elixir-and-io-lists-part-1-building-output-efficiently/)
- [Elixir RAM and the Template of Doom](https://www.evanmiller.org/elixir-ram-and-the-template-of-doom.html)
- [A short guide to refc binaries](https://medium.com/@mentels/a-short-guide-to-refc-binaries-f13f9029f6e2)

Gettext
-------------------------------------

- [Documentation](https://hexdocs.pm/gettext/Gettext.html)
- [Using Gettext to internationalize a Phoenix application](https://blog.plataformatec.com.br/2016/03/using-gettext-to-internationalize-a-phoenix-application/)

Debugging
-------------------------------------

### pry

```elixir
require IEx; IEx.pry
```

### breakpoints

| command | description |
| :------------- | :------------- |
| binding | shows all variables in scope |
| break!(fun) | define a new breakpoint for `Mod.fun/arity` |
| breaks | prints all breakpoints and their IDs |
| continue | continue the code execution |
| respawn | starts a new shell |
| whereami | show the file name, line and the code being executed |

Random Numbers
-------------------------------------

Timer
-------------------------------------

```elixir
:timer.send_interval(time, message)
```

```elixir
:timer.send_after(time, message)
```

```elixir
:timer.sleep(time)
Process.sleep(time)
```



Releases
-------------------------------------

- [Documentation](https://hexdocs.pm/mix/Mix.Tasks.Release.html)

Deployment
-------------------------------------

- [Kubernetes and the Erlang VM: orchestration on the large and the small](https://blog.plataformatec.com.br/2019/10/kubernetes-and-the-erlang-vm-orchestration-on-the-large-and-the-small/)
