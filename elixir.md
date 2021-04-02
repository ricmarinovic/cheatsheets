---
title: Elixir
category: Elixir
tags: [Featured, WIP]
updated: 2021-02-12
weight: -5
---

Intro
-------------------------------------

Mix
-------------------------------------

```
mix help --search phx
```

Strings
-------------------------------------

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

Releases
-------------------------------------

- [Documentation](https://hexdocs.pm/mix/Mix.Tasks.Release.html)

Deployment
-------------------------------------

- [Kubernetes and the Erlang VM: orchestration on the large and the small](https://blog.plataformatec.com.br/2019/10/kubernetes-and-the-erlang-vm-orchestration-on-the-large-and-the-small/)
